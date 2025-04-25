### 1. Flujo de trabajo
Para integrar Matter.js en p5.js, comencé importando las librerías y configurando el motor de física dentro de setup(). En esa función, también inicialicé el mundo, el motor de física, el suelo y el mouse constraint.
En draw(), actualizo el motor con Engine.update(engine) para que los cuerpos físicos se comporten de manera realista. Después, dibujo el fondo animado tipo nebulosa, las partículas (efecto visual), y luego cada letra usando su posición y ángulo obtenidos desde Matter.js.

### 2. Representación visual vs. simulación física
Cada letra del texto "CAER" fue convertida en un cuerpo físico rectangular (con Bodies.rectangle) y le asigné una propiedad personalizada llamada .text que guarda la letra específica.
Para representarlas visualmente en el canvas, uso translate() y rotate() en base a la posición y ángulo del cuerpo, y luego dibujo el texto con text(). El reto fue mantener la sincronización visual con la física, ya que un pequeño error en las transformaciones puede hacer que el texto “flote” o se vea desfasado respecto al cuerpo físico.

### 3. Creación de formas complejas
Aunque las letras son visualmente distintas, en la simulación física se usan cajas rectangulares para simplificar. En este caso no se buscó una forma compleja con vértices, sino que se representó cada letra sobre un cuerpo simple. Esto facilitó el proceso y evitó problemas de colisión más complejos, aunque limita la fidelidad física respecto a la forma real de cada letra.
Crear formas más complejas sería posible con Bodies.fromVertices, pero requiere mucho más trabajo y precisión.

### 4. Física para la semántica
El uso de física fue muy efectivo para reforzar el significado de la palabra "CAER". Las letras literalmente caen por efecto de la gravedad y rebotan, lo que le da una dimensión visual y cinética al significado.
Este enfoque funciona especialmente bien para palabras relacionadas con movimiento, peso, inestabilidad, impacto o transformación.
Por otro lado, palabras abstractas como "amor" o "memoria" podrían ser más difíciles de traducir a un comportamiento físico directo sin una metáfora visual clara.

### 5. Potencial exploratorio
La combinación de p5.js y Matter.js abre muchas posibilidades. Desde instalaciones interactivas hasta visualizaciones de datos con movimiento físico, pasando por juegos o arte generativo.
Particularmente me interesa seguir explorando el uso de partículas, reacciones físicas al sonido o al mouse, y crear experiencias donde el texto o los elementos visuales "sientan" el entorno. La física no solo añade realismo, sino que también puede aportar poética visual y narrativa interactiva.


```javascript

let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies,
    Body = Matter.Body,
    Mouse = Matter.Mouse,
    MouseConstraint = Matter.MouseConstraint;

let engine;
let world;
let letters = [];
let ground;
let mConstraint;
let letterIndex = 0;

let prevMouseX = 0;
let prevMouseY = 0;
let particles = [];
let bgTime = 0;

function setup() {
  createCanvas(800, 600);
  engine = Engine.create();
  world = engine.world;
  engine.world.gravity.y = 1;

  ground = Bodies.rectangle(width / 2, height - 10, width, 20, { isStatic: true });
  World.add(world, ground);

  let canvasMouse = Mouse.create(document.body);
  canvasMouse.pixelRatio = pixelDensity();
  let options = {
    mouse: canvasMouse
  };
  mConstraint = MouseConstraint.create(engine, options);
  World.add(world, mConstraint);
}

function mousePressed() {
  let word = "CAER";
  let positions = [200, 300, 400, 500];

  let x = positions[int(random(positions.length))];
  let letter = Bodies.rectangle(x, 0, 50, 50, {
    restitution: 0.9,
    frictionAir: 0.02
  });
  letter.text = word[letterIndex];
  World.add(world, letter);
  letters.push(letter);

  // Sparkle particles
  for (let i = 0; i < 25; i++) {
    particles.push({
      x: x,
      y: 0,
      vx: random(-2, 2),
      vy: random(-2, 2),
      alpha: 255,
      size: random(2, 5),
      hue: random(180, 360)
    });
  }

  letterIndex = (letterIndex + 1) % word.length;
}

function drawNebulaBackground() {
  bgTime += 0.01;
  for (let y = 0; y < height; y += 2) {
    let inter = map(y, 0, height, 0, 1);
    let r = 30 + 20 * sin(bgTime + y * 0.01);
    let g = 20 + 40 * cos(bgTime + y * 0.015);
    let b = 60 + 30 * sin(bgTime + y * 0.02);
    stroke(r, g, b, 180);
    line(0, y, width, y);
  }
}

function draw() {
  drawNebulaBackground();
  Engine.update(engine);

  let dx = mouseX - prevMouseX;
  let dy = mouseY - prevMouseY;
  let speed = sqrt(dx * dx + dy * dy);

  if (speed > 15) {
    for (let letter of letters) {
      let fx = random(-0.02, 0.02);
      let fy = random(-0.02, 0.02);
      Body.applyForce(letter, letter.position, { x: fx, y: fy });
    }
  }

  // Draw sparkle particles
  noStroke();
  for (let p of particles) {
    fill(`hsla(${p.hue}, 100%, 75%, ${p.alpha / 255})`);
    ellipse(p.x, p.y, p.size);
    p.x += p.vx;
    p.y += p.vy;
    p.alpha -= 4;
  }
  particles = particles.filter(p => p.alpha > 0);

  // Draw letters
  for (let letter of letters) {
    push();
    translate(letter.position.x, letter.position.y);
    rotate(letter.angle);
    textAlign(CENTER, CENTER);
    let pulse = sin(frameCount * 0.2 + letter.position.x) * 5;
    textSize(48 + pulse);
    fill(255);
    stroke(255, 100, 255);
    strokeWeight(3);
    drawingContext.shadowColor = 'rgba(255, 150, 255, 0.8)';
    drawingContext.shadowBlur = 25;
    text(letter.text, 0, 0);
    pop();
  }

  // Ground
  noStroke();
  fill(50, 50, 80, 100);

  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, width, 20);

  prevMouseX = mouseX;
  prevMouseY = mouseY;
}

```
