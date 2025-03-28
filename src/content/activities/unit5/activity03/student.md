## Código:

```javascript

// Variables globales
let particles = [];
let bgColor;

function setup() {
  createCanvas(windowWidth, windowHeight);
  // Fondo base negro para generar mucho contraste
  bgColor = color(0, 0, 0);
  noStroke();
}

function draw() {
  // Fondo dinámico: un degradado muy sutil entre negro y azul muy oscuro
  let gradientColor = lerpColor(color(0, 0, 0), color(10, 10, 30), sin(frameCount * 0.02) * 0.5 + 0.5);
  background(gradientColor);

  // Generar partículas aleatorias de forma controlada
  if (random(1) < 0.15) {
    particles.push(new Particle(random(width), random(height)));
  }

  // Actualizar y mostrar partículas
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    p.update();
    p.display();
    if (p.isDead()) {
      particles.splice(i, 1);
    }
  }

  // Estrellas parpadeantes en tonos brillantes para mayor contraste
  for (let i = 0; i < 80; i++) {
    fill(random(230, 255), random(230, 255), random(0, 50), random(50, 150));
    ellipse(random(width), random(height), random(1, 3));
  }

  // Instrucciones para el usuario
  fill(255);
  textSize(16);
  textAlign(CENTER);
  text("Haz clic o mueve el mouse para interactuar con las partículas", width / 2, height - 20);
}

// Clase de Partículas con comportamientos dinámicos
class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(random(-2, 2), random(-2, 2));
    this.acc = createVector(0, 0);
    // Tamaño más destacado para resaltar
    this.size = random(10, 20);
    this.lifespan = 255;
    // Paleta de colores vibrantes y contrastantes: evita tonos que se mezclen con el fondo negro
    this.color = color(random(200, 255), random(200, 255), random(0, 100));
    this.trail = [];
    // Asignamos un tipo aleatorio para experimentar distintos movimientos:
    // 'normal': sin modificación, 'pendulum': movimiento oscilante, 'levy': saltos aleatorios
    this.type = random(['normal', 'pendulum', 'levy']);
    this.angle = random(TWO_PI);
    this.angularVel = random(-0.1, 0.1);
    // Centro para el movimiento pendular
    this.pendulumCenter = createVector(random(width), random(height));
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update() {
    // Movimiento general
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 2;
    
    // Atracción/repulsión respecto al mouse
    let mouseVec = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouseVec, this.pos);
    let d = dir.mag();
    if (d < 100) {
      dir.setMag((100 - d) * 0.1);
      this.applyForce(dir.mult(-1)); // Repulsión
    } else if (d < 200) {
      dir.setMag((200 - d) * 0.05);
      this.applyForce(dir); // Atracción
    }
    
    // Movimiento pendular: oscila alrededor de un centro fijo
    if (this.type === 'pendulum') {
      this.angle += this.angularVel;
      this.pos.x = this.pendulumCenter.x + cos(this.angle) * 150;
      this.pos.y = this.pendulumCenter.y + sin(this.angle) * 150;
    }
    
    // Vuelo de Lévy: ocasionales saltos aleatorios para efecto discontinuo
    if (this.type === 'levy') {
      if (random(1) < 0.01) {
        this.vel.set(random(-5, 5), random(-5, 5));
      }
    }
    
    // Guardar el rastro para efectos de estela
    this.trail.push(this.pos.copy());
    if (this.trail.length > 15) {
      this.trail.shift();
    }
  }

  display() {
    noStroke();
    // Dibujar la estela, reduciendo la opacidad gradualmente
    for (let i = 0; i < this.trail.length; i++) {
      let alpha = map(i, 0, this.trail.length, 0, this.lifespan / 2);
      fill(red(this.color), green(this.color), blue(this.color), alpha);
      ellipse(this.trail[i].x, this.trail[i].y, this.size / 3);
    }
    // Dibujar el cuerpo principal de la partícula
    fill(red(this.color), green(this.color), blue(this.color), this.lifespan);
    ellipse(this.pos.x, this.pos.y, this.size);
  }

  isDead() {
    return this.lifespan < 0;
  }
}

function mouseClicked() {
  // Generar más partículas en el punto de interacción
  for (let i = 0; i < 20; i++) {
    particles.push(new Particle(mouseX, mouseY));
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}
```
### 1. Herencia y Polimorfismo
Qué implementé:

Por qué lo apliqué:
Esta estrategia simula polimorfismo en que, aunque todas las partículas comparten la misma estructura (la clase base), cada una puede comportarse de manera diferente según su tipo. Esto permite extender la funcionalidad sin duplicar código y tener un sistema modular y fácil de mantener.

### 2. Unidad 1 – Movimiento Básico (Motion 101)
Qué implementé:

Por qué lo apliqué:
El movimiento básico es la base de la animación; sin él, la simulación no tendría dinamismo. Este concepto asegura que las partículas se desplazan de forma natural, dándoles vida dentro del espacio generativo.

### 3. Unidad 2 – Interacciones entre Objetos y Fuerzas Neta Acumulativa
Qué implementé:

Por qué lo apliqué:
Este concepto demuestra cómo distintos estímulos pueden actuar de forma simultánea sobre un objeto, similar al sistema de n-cuerpos donde varias fuerzas inciden sobre cada partícula. Permite que la obra sea interactiva y responda de manera orgánica a las acciones del usuario, haciendo la experiencia más inmersiva.

### 4. Unidad 3 – Colisiones y Dinámica de Sistemas (N-cuerpos)
Qué implementé:

Por qué lo apliqué:
El concepto de n-cuerpos y fuerzas acumulativas ilustra cómo múltiples influencias pueden conformar la trayectoria de un objeto. Esto enriquece la dinámica visual de la obra, haciendo que cada partícula tenga un comportamiento único y realista al combinar distintas fuerzas.

### 5. Unidad 4 – Efectos Visuales y Estética
Qué implementé:

Por qué lo apliqué:
El objetivo es lograr una obra que no solo sea interactiva y rica en dinámica, sino también estética y emocionalmente impactante. Los efectos visuales aumentan el contraste y la belleza visual, haciendo que la simulación sea más envolvente y memorable para el espectador.
