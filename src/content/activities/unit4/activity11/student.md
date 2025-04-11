### Código:
```javascript
let particles = [];
let gravity = 0.1;
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
  } else if (key === 'G' || key === 'g') {
    gravity += 0.02; // Aumenta la gravedad más suavemente
  }
}

function mousePressed() {
  // Reaparecen partículas al hacer clic
  for (let i = 0; i < 300; i++) { // Reduce el número de partículas añadidas para menor caos
    particles.push(new Particle(random(width), random(height)));
  }
}

class Particle {
  constructor(x = random(width), y = random(height)) {
    this.origin = createVector(x, y);
    this.angle = random(TWO_PI);
    this.aVelocity = random(-0.03, 0.03); // Reduce la variación de velocidad angular inicial
    this.aAcceleration = 0;
    this.length = random(80, 120); // Mantiene longitudes más consistentes
    this.size = random(3, 6);
    this.damping = random(0.98, 1.01); // Reduce el rango de amortiguación
  }

  update() {
    // Simulación del péndulo con oscilación más estable
    this.aAcceleration = (-1 * gravity / this.length) * sin(this.angle);
    this.aVelocity += this.aAcceleration;
    this.angle += this.aVelocity;
    this.aVelocity *= this.damping; // Amortiguación menos extrema
  }

  display() {
    let col1 = colorPalette[currentPalette][0];
    let col2 = colorPalette[currentPalette][1];
    let posX = this.origin.x + this.length * sin(this.angle * noise(frameCount * 0.005)); // Reduce la influencia del ruido
    let posY = this.origin.y + this.length * cos(this.angle * noise(frameCount * 0.005));
    
    fill(lerpColor(col1, col2, posX / width));
    ellipse(posX, posY, this.size);
  }
}
```

### Link: https://editor.p5js.org/Cuervo01/sketches/NWPfNiP-f
