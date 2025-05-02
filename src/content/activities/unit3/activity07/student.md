## Problema 

El planteamiento actual tiene un problema significativo debido a la manera en que se manipula el vector force dentro de la función applyForce(force). Dado que force es un objeto de la clase p5.Vector y se pasa por referencia, cualquier cambio realizado a este vector afectará al objeto original que se pasó como argumento. Esto implica que cuando se divide force por 10, se está modificando directamente el vector original, lo cual no es deseado.

Imagina que aplicas dos fuerzas, wind y gravity, sobre un objeto de la siguiente manera:

```javascript

mover.applyForce(wind);
mover.applyForce(gravity);
```

Al llamar a applyForce(wind), wind se divide por 10, modificando el vector original wind. Luego, al llamar a applyForce(gravity), gravity también se divide por 10, modificando el vector original gravity. Esto significa que los vectores originales wind y gravity no se mantendrán intactos para futuras operaciones o frames, lo cual no es el comportamiento esperado.

## Solución Propuesta:

Para evitar este problema, es mejor crear una copia del vector force dentro de la función applyForce y luego realizar las operaciones necesarias en esa copia. Así, los vectores originales no se verán afectados.

```javascript
class Mover {
  constructor() {
    this.position = createVector(0, 0);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.mass = 10;  // Asumir que la masa es 10
  }

  applyForce(force) {
    let f = force.copy();  // Crear una copia del vector force
    f.div(this.mass);  // Dividir la copia por la masa del objeto
    this.acceleration.add(f);  // Sumar la copia modificada a la aceleración
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);  // Reiniciar la aceleración
  }

  display() {
    stroke(0);
    fill(175);
    ellipse(this.position.x, this.position.y, 16, 16);
  }
}

let mover;

function setup() {
  createCanvas(640, 360);
  mover = new Mover();
}

function draw() {
  background(255);

  let wind = createVector(0.1, 0);
  let gravity = createVector(0, 0.1 * mover.mass);

  mover.applyForce(wind);
  mover.applyForce(gravity);
  mover.update();
  mover.display();
}
```
