## 1. Fricción
La fricción es una fuerza que se opone al movimiento de un objeto. La fricción depende de la velocidad del objeto y generalmente es proporcional a la magnitud de la velocidad pero en dirección opuesta.

Código de la simulación de fricción:
```javascript

class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(2, 2);
    this.acceleration = createVector(0, 0);
    this.mass = 10;
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  applyFriction() {
    let friction = this.velocity.copy();
    friction.mult(-1);
    friction.normalize();
    let normal = 1;  // Suponemos que la fuerza normal es 1
    let mu = 0.1;  // Coeficiente de fricción
    friction.mult(mu * normal);
    this.applyForce(friction);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
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

  mover.applyFriction();
  mover.update();
  mover.display();
}
```

Explicación:
**Fricción:** La fricción es modelada como una fuerza que se opone a la dirección del movimiento del objeto. Se calcula multiplicando la normal (suponiendo que es 1) por el coeficiente de fricción. Luego, se aplica la fricción usando el método applyForce().

## 2. Resistencia del aire y de fluidos
La resistencia del aire y de fluidos es una fuerza que se opone al movimiento de un objeto y es proporcional al cuadrado de la velocidad del objeto.

Código de la simulación de resistencia del aire:

```javascript

class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(2, 2);
    this.acceleration = createVector(0, 0);
    this.mass = 10;
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  applyAirResistance() {
    let resistance = this.velocity.copy();
    resistance.mult(-1);
    let c = 0.02;  // Coeficiente de resistencia
    let speed = this.velocity.mag();
    resistance.normalize();
    resistance.mult(c * speed * speed);
    this.applyForce(resistance);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
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

  mover.applyAirResistance();
  mover.update();
  mover.display();
}
```
##Explicación: 
Resistencia del aire: La resistencia del aire es modelada como una fuerza que se opone al movimiento del objeto y es proporcional al cuadrado de la velocidad del objeto. La magnitud de la resistencia se calcula multiplicando el coeficiente de resistencia por el cuadrado de la velocidad, y se aplica la fuerza resultante.

## 3. Atracción gravitacional
La atracción gravitacional entre dos objetos depende de sus masas y la distancia entre ellos.

Código de la simulación de atracción gravitacional:
```javascript

class Mover {
  constructor(m, x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.mass = m;
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  attract(mover) {
    let force = p5.Vector.sub(this.position, mover.position);
    let distance = constrain(force.mag(), 5, 25);
    force.normalize();
    let strength = (0.4 * this.mass * mover.mass) / (distance * distance);
    force.mult(strength);
    mover.applyForce(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  display() {
    stroke(0);
    fill(175);
    ellipse(this.position.x, this.position.y, 16, 16);
  }
}

let mover1;
let mover2;

function setup() {
  createCanvas(640, 360);
  mover1 = new Mover(20, width / 2 - 100, height / 2);
  mover2 = new Mover(10, width / 2 + 100, height / 2);
}

function draw() {
  background(255);

  mover1.attract(mover2);
  mover2.attract(mover1);

  mover1.update();
  mover2.update();

  mover1.display();
  mover2.display();
}
```
Enlace a la simulación de atracción gravitacional

Explicación:
