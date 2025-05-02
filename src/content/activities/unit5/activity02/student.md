 ## Ejemplo 4.2: an Array of Particles.
 ### Código:

 ```javascript
let particles = [];

function setup() {
  createCanvas(600, 600);
  for (let i = 0; i < 100; i++) {
    particles.push(new Particle());
  }
  noStroke();
}

function draw() {
  background(20, 30, 50, 50); // Fondo dinámico con opacidad para estelas
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    p.update();
    p.display();

    // Eliminar partículas cuando se agote su vida útil
    if (p.lifespan <= 0) {
      particles.splice(i, 1);
    }
  }

  // Generar nuevas partículas para mantener el flujo
  if (frameCount % 10 === 0) {
    particles.push(new Particle());
  }
}

class Particle {
  constructor() {
    this.x = width / 2 + random(-50, 50); // Distribución inicial más variable
    this.y = height / 2 + random(-50, 50);
    this.vx = randomGaussian(0, 2);
    this.vy = randomGaussian(0, 2);
    this.size = abs(randomGaussian(5, 3));
    this.lifespan = 255;
    this.color = color(random(100, 255), random(100, 255), random(255)); // Color aleatorio
  }

  update() {
    this.x += this.vx;
    this.y += this.vy;
    this.lifespan -= 2;

    // Rebote en los bordes
    if (this.x < 0 || this.x > width) this.vx *= -1;
    if (this.y < 0 || this.y > height) this.vy *= -1;
  }

  display() {
    fill(this.color.levels[0], this.color.levels[1], this.color.levels[2], this.lifespan);
    ellipse(this.x, this.y, this.size);
  }
}
```
#### Link: https://editor.p5js.org/Cuervo01/sketches/shP8rutWG

##  Ejemplo 4.4: a System of Systems.
### Código:

```javascript
class System {
  constructor() {
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle());
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.update();
      p.display();

      // Eliminar partículas si su vida útil termina
      if (p.lifespan <= 0) {
        this.particles.splice(i, 1);
      }
    }
  }
}

class Particle {
  constructor() {
    this.pos = createVector(random(width), random(height)); // Posición aleatoria inicial
    this.size = random(20, 40); // Tamaño más grande para patrón geométrico
    this.lifespan = 200 + random(55);
    this.color = color(random(0, 255), random(100, 255), random(200, 255)); // Colores neón vibrantes
    this.angle = random(TWO_PI); // Ángulo inicial para rotación
    this.angularVelocity = random(-0.05, 0.05); // Velocidad de giro
  }

  update() {
    // Movimiento aleatorio con rotación
    this.pos.x += random(-2, 2);
    this.pos.y += random(-2, 2);
    this.angle += this.angularVelocity;

    // Reducir vida útil
    this.lifespan -= 2;
  }

  display() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.angle);

    // Forma hexagonal
    strokeWeight(2);
    stroke(this.color);
    fill(0, this.lifespan * 0.5); // Brillo interno con opacidad
    polygon(0, 0, this.size, 6); // Dibujar hexágono
    pop();
  }
}

function polygon(x, y, radius, npoints) {
  let angle = TWO_PI / npoints;
  beginShape();
  for (let a = 0; a < TWO_PI; a += angle) {
    let sx = x + cos(a) * radius;
    let sy = y + sin(a) * radius;
    vertex(sx, sy);
  }
  endShape(CLOSE);
}

let systems = [new System()];

function setup() {
  createCanvas(600, 600);
  background(10, 10, 30); // Fondo oscuro moderno
}

function draw() {
  background(10, 10, 30, 10); // Fondo dinámico con leves rastros
  for (let system of systems) {
    system.addParticle();
    system.run();
  }
}

function mousePressed() {
  // Crear un nuevo sistema de partículas al presionar el mouse
  systems.push(new System());
}

```
#### Link: https://editor.p5js.org/Cuervo01/sketches/Du-vroxM5

## Ejemplo 4.5: a Particle System with Inheritance and Polymorphism.

### Código: 

```javascript
let particleSystem;

function setup() {
  createCanvas(600, 600);
  particleSystem = new ParticleSystem();
}

function draw() {
  background(10, 10, 20, 30); // Fondo translúcido para rastros
  particleSystem.addParticle();
  particleSystem.run();
}

class BaseParticle {
  constructor() {
    this.position = createVector(random(width), random(height)); // Posición inicial aleatoria
    this.velocity = createVector(random(-1, 1), random(-1, 1)); // Movimiento lento
    this.acceleration = createVector(0, 0.02); // Gravedad simulada
    this.lifespan = 200 + random(55);
    this.size = random(10, 20);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 3;
  }

  display() {
    fill(255, this.lifespan);
    noStroke();
    ellipse(this.position.x, this.position.y, this.size); // Forma predeterminada: círculo
  }
}

class ColoredParticle extends BaseParticle {
  constructor() {
    super();
    this.color = color(random(50, 255), random(50, 255), random(255), this.lifespan);
    this.shape = random(["circle", "square", "triangle"]); // Seleccionar forma aleatoria
  }

  display() {
    push();
    translate(this.position.x, this.position.y);
    fill(this.color);
    noStroke();

    // Mostrar diferentes formas según el tipo seleccionado
    if (this.shape === "circle") {
      ellipse(0, 0, this.size); // Círculo
    } else if (this.shape === "square") {
      rectMode(CENTER);
      rect(0, 0, this.size, this.size); // Cuadrado
    } else if (this.shape === "triangle") {
      triangle(
        -this.size / 2, this.size / 2,  // Esquina inferior izquierda
        this.size / 2, this.size / 2,   // Esquina inferior derecha
        0, -this.size / 2               // Punta superior
      );
    }
    pop();
  }
}

class ParticleSystem {
  constructor() {
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new ColoredParticle());
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.update();
      p.display();
      if (p.lifespan <= 0) {
        this.particles.splice(i, 1); // Eliminar partículas "muertas"
      }
    }
  }
}
```
#### Link: https://editor.p5js.org/Cuervo01/sketches/SkiUYrRU8

## Ejemplo 4.6: a Particle System with Forces.
### Código: 

```javascript
let particles = [];
let gravity;
let bgColor = 0;

function setup() {
  createCanvas(600, 400);
  gravity = createVector(0, 0.1);
}

function draw() {
  // Fondo dinámico
  bgColor = (bgColor + 0.5) % 360;
  colorMode(HSB, 360, 100, 100);
  background(bgColor, 50, 30);

  particles.push(new Particle(width / 2, 50));

  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    let friction = p.vel.copy().mult(-0.02);
    p.applyForce(gravity);
    p.applyForce(friction);
    p.oscillate();
    p.update();
    p.show();
    if (p.isDead()) {
      particles.splice(i, 1);
    }
  }
}

class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(random(-1, 1), random(-2, 0));
    this.acc = createVector(0, 0);
    this.lifespan = 300; // Duración extendida
    this.angle = random(TWO_PI);
    this.trail = [];
  }

  applyForce(force) {
    this.acc.add(force);
  }

  oscillate() {
    this.pos.x += sin(this.angle) * 2;
    this.angle += 0.1;
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 1;

    // Guardar posiciones para la estela
    this.trail.push(this.pos.copy());
    if (this.trail.length > 15) {
      this.trail.shift();
    }
  }

  show() {
    noFill();
    strokeWeight(2);
    stroke(lerpColor(color(60, 100, 100, 150), color(200, 100, 100, 0), this.lifespan / 300));
    beginShape();
    for (let pos of this.trail) {
      vertex(pos.x, pos.y);
    }
    endShape();

    // Dibujar la partícula con brillo
    noStroke();
    fill(255, this.lifespan);
    ellipse(this.pos.x, this.pos.y, 12 + sin(frameCount * 0.1) * 3); // Animación del tamaño
  }

  isDead() {
    return this.lifespan <= 0;
  }
}
```
### Link: 

https://editor.p5js.org/Cuervo01/sketches/-bZLoVSi1

## Ejemplo 4.7: a Particle System with a Repeller.
### Código

```javascript
let particleSystem;
let repeller;

function setup() {
  createCanvas(600, 600);
  particleSystem = new ParticleSystem(createVector(width / 2, 50));
  repeller = new Repeller(width / 2, height / 2, 80);
}

function draw() {
  // Fondo más uniforme y suave para destacar las partículas
  background(20, 24, 45);

  repeller.display();
  particleSystem.applyRepeller(repeller);
  particleSystem.run();
}

// Clase para las partículas
class Particle {
  constructor(position) {
    this.position = position.copy();
    this.velocity = createVector(random(-1, 1), random(-3, 2));
    this.acceleration = createVector(0, 0.2);
    this.lifespan = 255;
    this.size = random(8, 14);
    this.color = color(random(200, 250), random(50, 100), random(100, 200), this.lifespan); // Tonos vivos contrastantes
    this.trail = [];
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 2;

    // Actualizar el rastro
    this.trail.push(this.position.copy());
    if (this.trail.length > 10) {
      this.trail.shift();
    }
  }

  display() {
    // Dibujar el rastro
    noFill();
    strokeWeight(3);
    stroke(lerpColor(color(255, 100, 100, 150), color(100, 255, 200, 0), this.lifespan / 255));
    beginShape();
    for (let pos of this.trail) {
      vertex(pos.x, pos.y);
    }
    endShape();

    // Dibujar la partícula
    noStroke();
    fill(this.color);
    ellipse(this.position.x, this.position.y, this.size);
  }

  isDead() {
    return this.lifespan <= 0 || this.position.y > height;
  }
}

// Sistema de partículas
class ParticleSystem {
  constructor(position) {
    this.origin = position.copy();
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin));
  }

  applyRepeller(repeller) {
    for (let p of this.particles) {
      if (repeller.collides(p)) {
        let dir = p5.Vector.sub(p.position, repeller.position);
        dir.normalize();
        p.velocity.reflect(dir).mult(1.5);
        p.position.add(dir.mult(repeller.size / 2 + p.size / 2));
      } else {
        let force = repeller.repel(p);
        p.applyForce(force);
      }
    }
  }

  run() {
    this.addParticle();
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.update();
      p.display();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

// Clase para el repeller
class Repeller {
  constructor(x, y, size) {
    this.position = createVector(x, y);
    this.strength = 600;
    this.size = size;
  }

  repel(particle) {
    let dir = p5.Vector.sub(particle.position, this.position);
    let distance = constrain(dir.mag(), 10, 100);
    dir.normalize();
    let force = -this.strength / (distance * distance);
    dir.mult(force);
    return dir;
  }

  collides(particle) {
    let d = dist(this.position.x, this.position.y, particle.position.x, particle.position.y);
    return d < this.size / 2 + particle.size / 2;
  }

  display() {
    // Repeller visualmente más destacable
    fill(250, 80, 100, 200);
    stroke(255, 100, 150);
    strokeWeight(3);
    ellipse(this.position.x, this.position.y, this.size);

    noFill();
    strokeWeight(2);
    stroke(lerpColor(color(255, 150, 100), color(100, 255, 150), sin(frameCount * 0.1) * 0.5 + 0.5));
    ellipse(this.position.x, this.position.y, this.size + sin(frameCount * 0.2) * 15);
  }
}
```
