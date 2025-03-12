## Link: https://editor.p5js.org/Cuervo01/sketches/p9Ab776OG

## Código:

```javascript
let vehicle;

function setup() {
  createCanvas(600, 400);
  vehicle = new Vehicle(width / 2, height / 2);
}

function draw() {
  background(220);

  vehicle.update();
  vehicle.display();
}

function keyPressed() {
  if (keyCode === LEFT_ARROW) {
    vehicle.rotate(-0.1); // Gira a la izquierda
  } else if (keyCode === RIGHT_ARROW) {
    vehicle.rotate(0.1); // Gira a la derecha
  } else if (keyCode === UP_ARROW) {
    vehicle.thrust(); // Avanza en la dirección actual
  }
}

class Vehicle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.angle = 0; // Ángulo de rotación del vehículo
  }

  rotate(angleChange) {
    this.angle += angleChange; // Cambia la orientación del vehículo
  }

  thrust() {
    // Calcula la dirección en la que se debe mover basado en el ángulo actual
    let force = p5.Vector.fromAngle(this.angle);
    force.mult(0.1); // Pequeña aceleración en esa dirección
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0); // Reinicia la aceleración para la próxima iteración

    // Mantener el vehículo dentro de la pantalla
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) this.position.y = 0;
    if (this.position.y < 0) this.position.y = height;
  }

  display() {
    push();
    translate(this.position.x, this.position.y);
    rotate(this.angle);

    fill(127);
    stroke(0);
    strokeWeight(2);

    // Dibujar un triángulo como vehículo
    beginShape();
    vertex(10, 0);
    vertex(-10, -5);
    vertex(-10, 5);
    endShape(CLOSE);

    pop();
  }
}

```

## Captura:

https://github.com/user-attachments/assets/14ebe8d3-ab87-4531-adc5-f92d3552bbc4

