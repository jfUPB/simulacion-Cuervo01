Problema detectado
En el método applyForce(force), la aceleración se iguala directamente a la fuerza recibida:


```javascript

applyForce(force) {
  this.acceleration = force;
}

```
Esto hace que la última fuerza aplicada sea la única que afecte al objeto en cada frame, ignorando las demás.

Solución
En lugar de sobrescribir la aceleración, se debe acumular la suma de todas las fuerzas y luego aplicarla a la aceleración correctamente usando la ecuación:
a= ∑F/m
​
Si asumimos que la masa m=1, la aceleración simplemente será la suma de las fuerzas. Modificamos el método applyForce() para que acumule las fuerzas en cada frame:

```javascript
applyForce(force) {
  this.acceleration.add(force);
}
```

```javascript


let mover;

class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    this.acceleration.add(force); // Se suman todas las fuerzas
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0); // Se reinicia la aceleración después de aplicarla
  }

  display() {
    fill(255);
    ellipse(this.position.x, this.position.y, 20, 20);
  }
}

function setup() {
  createCanvas(400, 400);
  mover = new Mover();
}

function draw() {
  background(0);

  // Definimos fuerzas
  let gravity = createVector(0, 0.1);
  let wind = createVector(0.05, 0);

  // Aplicamos fuerzas al objeto
  mover.applyForce(gravity);
  mover.applyForce(wind);

  mover.update();
  mover.display();
}

```
- Se acumulan las fuerzas en applyForce(force), en lugar de sobrescribir la aceleración.
- Se aplica la aceleración en update() sumándola a la velocidad y la posición.
- Se resetea la aceleración (this.acceleration.mult(0)) después de aplicarla, para que no se arrastre de un frame a otro sin nuevas fuerzas.
