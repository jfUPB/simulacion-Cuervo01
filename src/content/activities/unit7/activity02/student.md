## 1. Exploración e investigación
### Conceptos clave:

- **Engine:** Es el motor físico que gestiona la simulación. Se encarga de actualizar la física de todos los objetos, detectar colisiones, aplicar gravedad y resolver las interacciones.

- **World:** Es el “contenedor” donde viven todos los cuerpos físicos. Está vinculado al engine y contiene tanto los cuerpos dinámicos como los estáticos.

- **Bodies:** Son los objetos físicos dentro del mundo. Pueden ser:

Bodies.rectangle(x, y, w, h)

Bodies.circle(x, y, r)

Bodies.polygon(x, y, sides, radius) Se les pueden aplicar propiedades como fricción, masa, restitución, etc.

- **Constraint:** Permite unir dos cuerpos con una especie de "resorte" o "barra". Se usa para simular conexiones físicas como bisagras, resortes o cadenas.

- **MouseConstraint:** Permite que el usuario interactúe con los objetos usando el mouse. Detecta los cuerpos debajo del cursor y les aplica fuerzas cuando se arrastran.

## Experimento 1: Mundo con gravedad, cajas y círculos
Este experimento incluye un mundo con gravedad, cuerpos dinámicos (cajas y círculos) que caen y colisionan, y cuerpos estáticos (suelo).
  ```Javascript
  let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies,
    Mouse = Matter.Mouse,
    MouseConstraint = Matter.MouseConstraint;
  let engine;
  let world;
  let shapes = [];
  let particles = [];
  let ground;
  let mConstraint;

  function setup() {
    createCanvas(600, 400);
    engine = Engine.create();
    world = engine.world;

  // Crear un suelo estático
  ground = Bodies.rectangle(300, height - 10, width, 20, { isStatic: true });
  World.add(world, ground);

  // Crear constraint para el mouse
  const canvasMouse = Mouse.create(canvas.elt);
  canvasMouse.pixelRatio = pixelDensity();
  const options = {
    mouse: canvasMouse
  };
  mConstraint = MouseConstraint.create(engine, options);
  World.add(world, mConstraint);

  textAlign(CENTER);
  textSize(16);
  }

  function mousePressed() {
    // Crear formas estilizadas
    const shapeType = random(["star", "hexagon", "ellipse"]);
    const size = random(30, 70);
    const x = mouseX;
    const y = mouseY;
  
    let shape;
    if (shapeType === "star") {
      shape = Bodies.polygon(x, y, 5, size);
    } else if (shapeType === "hexagon") {
      shape = Bodies.polygon(x, y, 6, size);
    } else {
      shape = Bodies.circle(x, y, size / 2);
    }
  
    shapes.push({ body: shape, type: shapeType });
    World.add(world, shape);
  
    // Crear partículas mágicas con colores contrastantes
    for (let i = 0; i < 5; i++) {
      particles.push({
        x: x,
        y: y,
        size: random(5, 15),
        color: color(random(50, 255), random(50, 255), random(50, 255)),
        alpha: 255,
        speedX: random(-1, 1),
        speedY: random(-1, 1)
      });
    }
  }
  
  function draw() {
    // Fondo con un gradiente oscuro y vibrante
    background(lerpColor(color(30, 30, 30), color(80, 0, 100), abs(sin(frameCount * 0.01))));
  
    Engine.update(engine);
  
    // Instrucciones
    fill(255);
    noStroke();
    text("Haz clic para crear formas con contraste", width / 2, 30);
  
    // Dibujar partículas mágicas
    for (let particle of particles) {
      particle.x += particle.speedX;
      particle.y += particle.speedY;
      particle.alpha -= 2;
      fill(particle.color.levels[0], particle.color.levels[1], particle.color.levels[2], particle.alpha);
      noStroke();
      ellipse(particle.x, particle.y, particle.size);
    }
  
    // Limpiar partículas viejas
    particles = particles.filter(p => p.alpha > 0);
  
    // Dibujar formas estilizadas con colores contrastantes
    for (let item of shapes) {
      const pos = item.body.position;
      const angle = item.body.angle;
      const shapeType = item.type;

    push();
    translate(pos.x, pos.y);
    rotate(angle);

    if (shapeType === "star") {
      fill(lerpColor(color(255, 50, 50), color(255, 200, 50), cos(frameCount * 0.05)));
      noStroke();
      beginShape();
      for (let i = 0; i < 10; i++) {
        const r = i % 2 === 0 ? 30 : 15;
        const angle = PI / 5 * i;
        vertex(cos(angle) * r, sin(angle) * r);
      }
      endShape(CLOSE);
    } else if (shapeType === "hexagon") {
      fill(lerpColor(color(50, 255, 50), color(50, 50, 255), sin(frameCount * 0.03)));
      stroke(0);
      strokeWeight(2);
      beginShape();
      for (let i = 0; i < 6; i++) {
        const angle = TWO_PI / 6 * i;
        vertex(cos(angle) * 30, sin(angle) * 30);
      }
      endShape(CLOSE);
    } else {
      fill(lerpColor(color(255, 150, 255), color(100, 50, 255), cos(frameCount * 0.01)));
      stroke(255);
      strokeWeight(1);
      ellipse(0, 0, item.body.circleRadius * 2);
    }

    pop();
  }

  // Dibujar el suelo con un color fuerte
  fill(100, 50, 255, 200); // Intenso y definido
  noStroke();
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, width, 20);
  }


  ```
## Experimento 2: Restricción de Cuerpo (Constraint)
En este segundo experimento, vamos a crear dos cuerpos conectados mediante una restricción.

```javascript
// Importar módulos desde Matter.js
let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies,
    Constraint = Matter.Constraint,
    Mouse = Matter.Mouse,
    MouseConstraint = Matter.MouseConstraint;

let engine;
let world;
let body1, body2, constraint;
let ground;
let mConstraint;

let backgroundParticles = [];
let circleColor1, circleColor2;
let lineColor;
let rotationAngle1 = 0;
let rotationAngle2 = 0;

function setup() {
  createCanvas(800, 600);
  engine = Engine.create();
  world = engine.world;

  // Crear un suelo estático
  ground = Bodies.rectangle(400, height - 10, width, 20, { isStatic: true });
  World.add(world, ground);

  // Crear límites alrededor del canvas
  const leftWall = Bodies.rectangle(0, height / 2, 20, height, { isStatic: true });
  const rightWall = Bodies.rectangle(width, height / 2, 20, height, { isStatic: true });
  const topWall = Bodies.rectangle(width / 2, 0, width, 20, { isStatic: true });
  World.add(world, [leftWall, rightWall, topWall]);

  // Crear los dos cuerpos
  body1 = Bodies.circle(300, 100, 40, { restitution: 0.9 });
  body2 = Bodies.circle(500, 100, 40, { restitution: 0.9 });
  World.add(world, [body1, body2]);

  // Crear una restricción (constraint) entre los dos cuerpos
  constraint = Constraint.create({
    bodyA: body1,
    bodyB: body2,
    stiffness: 0.05,
    length: 200
  });
  World.add(world, constraint);

  // Configurar el MouseConstraint para interactividad
  const canvasMouse = Mouse.create(canvas.elt);
  canvasMouse.pixelRatio = pixelDensity();
  const options = {
    mouse: canvasMouse
  };
  mConstraint = MouseConstraint.create(engine, options);
  World.add(world, mConstraint);

  // Colores vibrantes y contrastantes
  circleColor1 = color(255, 50, 150);
  circleColor2 = color(50, 255, 200);
  lineColor = color(200, 255, 50);

  // Crear partículas para un fondo animado
  for (let i = 0; i < 200; i++) {
    backgroundParticles.push({
      x: random(width),
      y: random(height),
      size: random(2, 5),
      alpha: random(50, 150),
      speed: random(0.5, 1.5)
    });
  }
}

function draw() {
  // Fondo animado con partículas
  background(20);
  noStroke();
  for (let p of backgroundParticles) {
    fill(100, 150, 255, p.alpha);
    ellipse(p.x, p.y, p.size);
    p.y += p.speed;
    if (p.y > height) p.y = 0;
  }

  Engine.update(engine);

  // Rotaciones dinámicas
  rotationAngle1 += 0.05;
  rotationAngle2 -= 0.05;

  // Dibujar los círculos con estilos brillantes
  push();
  translate(body1.position.x, body1.position.y);
  rotate(rotationAngle1);
  fill(circleColor1);
  stroke(255, 200, 200);
  strokeWeight(4);
  ellipse(0, 0, body1.circleRadius * 2 + sin(frameCount * 0.1) * 10);
  pop();

  push();
  translate(body2.position.x, body2.position.y);
  rotate(rotationAngle2);
  fill(circleColor2);
  stroke(200, 255, 255);
  strokeWeight(4);
  ellipse(0, 0, body2.circleRadius * 2 + sin(frameCount * 0.1) * 10);
  pop();

  // Dibujar la línea de conexión con efectos de pulso
  strokeWeight(6 + sin(frameCount * 0.1) * 2);
  stroke(lineColor);
  line(body1.position.x, body1.position.y, body2.position.x, body2.position.y);

  // Dibujar el suelo con gradiente
  noStroke();
  for (let i = 0; i < 20; i++) {
    fill(100, 50 + i * 10, 255 - i * 10, 180);
    rectMode(CENTER);
    rect(ground.position.x, ground.position.y, width, 20 - i);
  }
}
```

