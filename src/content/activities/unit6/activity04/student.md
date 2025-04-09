## Concepto inesperado
Esta pieza nace de la intención de explorar cómo pequeños organismos, como cardúmenes o bandadas, pueden parecer que fluyen con suavidad en un entorno invisible, pero que reaccionan drásticamente ante una fuerza externa. Aquí, ese entorno invisible está representado por un campo de flujo (flow field) generado por ruido Perlin, y la fuerza externa es el clic del espectador, que altera drásticamente la dirección del campo como si una corriente repentina (o una amenaza) agitara el entorno.

Lo inesperado surge del contraste entre calma y caos controlado: las partículas fluyen con elegancia, pero un solo clic puede alterar todo el sistema visual, haciéndolo parecer vivo y sensible. Así, se propone una metáfora visual sobre cómo los entornos complejos responden a pequeñas interacciones humanas.

## Adaptación del algoritmo
Este trabajo se basa en dos principios centrales:

1. Flow Field
Se genera un campo vectorial a partir del ruido Perlin 3D, con dimensiones espaciales (x, y) y una dimensión temporal (z).

Cada celda del campo guarda un vector que indica dirección y magnitud.

Este campo dirige el movimiento de las partículas, creando trazos orgánicos y suaves.

2. Simulación Flocking (inspiración, no completa)
Aunque no se usa el algoritmo de Boids completo, sí se simula el comportamiento colectivo.

Cada partícula sigue el campo y deja una estela, generando patrones visuales similares a los de enjambres, bandadas o corrientes marinas.

## Interacción inesperada
Al hacer clic con el mouse (clic izquierdo), los vectores del campo se modifican intensamente alterando las trayectorias.

Esto genera una transformación inmediata del comportamiento visual del sistema.

Visualmente, las líneas se hacen más grandes, con color saturado, y el fondo oscuro mantiene un contraste elegante.

## código:

```javascript
let cols, rows;
let scl = 20;
let inc = 0.1;
let zoff = 0;
let particles = [];
let flowfield = [];

let changeDirection = false;

function setup() {
  createCanvas(800, 600);
  colorMode(HSB, 255);
  cols = floor(width / scl);
  rows = floor(height / scl);
  flowfield = new Array(cols * rows);

  for (let i = 0; i < 300; i++) {
    particles[i] = new Particle();
  }

  background(0);
}

function draw() {
  let yoff = 0;
  for (let y = 0; y < rows; y++) {
    let xoff = 0;
    for (let x = 0; x < cols; x++) {
      let index = x + y * cols;
      let angle = noise(xoff, yoff, zoff) * TWO_PI * 4;
      if (changeDirection) {
        angle += PI / 2;
      }
      let v = p5.Vector.fromAngle(angle);
      v.setMag(1);
      flowfield[index] = v;
      xoff += inc;
    }
    yoff += inc;
  }

  zoff += changeDirection ? 0.01 : 0.001;

  for (let i = 0; i < particles.length; i++) {
    particles[i].follow(flowfield);
    particles[i].update();
    particles[i].edges();
    particles[i].show();
  }
}

function mousePressed() {
  if (mouseButton === LEFT) {
    changeDirection = true;
  }
}

function mouseReleased() {
  changeDirection = false;
}

class Particle {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.maxspeed = 2;
    this.prevPos = this.pos.copy();
    this.h = random(100, 255); // Más color, menos blanco
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxspeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  applyForce(force) {
    this.acc.add(force);
  }

  follow(vectors) {
    let x = floor(this.pos.x / scl);
    let y = floor(this.pos.y / scl);
    let index = x + y * cols;
    let force = vectors[index];
    this.applyForce(force);
  }

  show() {
    stroke(this.h, 200, 200, 60); // Más saturado, menos blanco
    strokeWeight(2); // Líneas más gruesas
    line(this.pos.x, this.pos.y, this.prevPos.x, this.prevPos.y);
    this.updatePrev();
  }

  updatePrev() {
    this.prevPos.x = this.pos.x;
    this.prevPos.y = this.pos.y;
  }

  edges() {
    if (this.pos.x > width) {
      this.pos.x = 0;
      this.updatePrev();
    }
    if (this.pos.x < 0) {
      this.pos.x = width;
      this.updatePrev();
    }
    if (this.pos.y > height) {
      this.pos.y = 0;
      this.updatePrev();
    }
    if (this.pos.y < 0) {
      this.pos.y = height;
      this.updatePrev();
    }
  }
}

```


