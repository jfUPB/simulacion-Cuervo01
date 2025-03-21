### Código:
```javascript
let particles = [];
let noiseScale = 0.01;
let colorPalette;
let currentPalette = 0;

function setup() {
  createCanvas(600, 600);
  noStroke();
  colorPalette = [
    [color(255, 100, 200), color(100, 200, 255)],
    [color(255, 200, 100), color(100, 255, 100)],
    [color(200, 100, 255), color(255, 255, 100)]
  ];
  for (let i = 0; i < 500; i++) {
    particles.push(new Particle());
  }
}

function draw() {
  background(10, 30, 50, 20);

  for (let p of particles) {
    p.update();
    p.display();
  }
}

function keyPressed() {
  if (key === 'C' || key === 'c') {
    currentPalette = (currentPalette + 1) % colorPalette.length; // Cambia la paleta de colores
  } else if (key === 'L' || key === 'l') {
    particles = []; // Limpia todas las partículas
  }
}

function mousePressed() {
  // Reaparecen partículas al hacer clic
  for (let i = 0; i < 500; i++) {
    particles.push(new Particle(random(width), random(height)));
  }
}

class Particle {
  constructor(x = random(width), y = random(height)) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.maxSpeed = random(2, 5);
    this.size = random(2, 6);
  }

  update() {
    // Movimiento fluido basado en el ruido
    let angle = noise(this.pos.x * noiseScale, this.pos.y * noiseScale) * TWO_PI * 4;
    let force = p5.Vector.fromAngle(angle);
    this.acc.add(force);

    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);

    // Reaparece al cruzar los bordes
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0) this.pos.y = height;
  }

  display() {
    let col1 = colorPalette[currentPalette][0];
    let col2 = colorPalette[currentPalette][1];
    fill(lerpColor(col1, col2, this.pos.x / width));
    ellipse(this.pos.x, this.pos.y, this.size);
  }
}
```

### Link: https://editor.p5js.org/Cuervo01/sketches/NWPfNiP-f
