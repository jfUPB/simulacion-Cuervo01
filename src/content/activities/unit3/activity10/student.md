## 1. Fricción
La fricción es una fuerza que se opone al movimiento de un objeto. La fricción depende de la velocidad del objeto y generalmente es proporcional a la magnitud de la velocidad pero en dirección opuesta.

### Código de la simulación de fricción:
```javascript

let moverFriccion;

function setup() {
  createCanvas(640, 360);
  moverFriccion = new MoverFriccion(300, 200);
}

function draw() {
  background(220);
  moverFriccion.applyFriction(0.05);
  moverFriccion.update();
  moverFriccion.display();
}

class MoverFriccion {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(2, 0);
  }

  applyFriction(coefficient) {
    let friction = this.velocity.copy();
    friction.mult(-1);
    friction.normalize();
    friction.mult(coefficient);
    this.velocity.add(friction);
  }

  update() {
    this.position.add(this.velocity);
  }

  display() {
    fill(255, 0, 0);
    ellipse(this.position.x, this.position.y, 20, 20);
  }
}

```

### Explicación:
**Fricción:** La fricción es modelada como una fuerza que se opone al movimiento del objeto y es proporcional a la magnitud de su velocidad. Para calcular la fuerza de fricción:

Se obtiene la velocidad del objeto.
Se normaliza la velocidad para obtener la dirección opuesta.
Se multiplica por un coeficiente de fricción constante.
Se aplica la fuerza resultante al objeto.
Esto genera un efecto en el que el objeto desacelera gradualmente hasta detenerse.

### Link: https://editor.p5js.org/Cuervo01/sketches/erqPKiJsj

## 2. Resistencia del aire y de fluidos
La resistencia del aire y de fluidos es una fuerza que se opone al movimiento de un objeto y es proporcional al cuadrado de la velocidad del objeto.

### Código de la simulación de resistencia del aire:

```javascript

let moverAire;

function setup() {
  createCanvas(640, 360);
  moverAire = new MoverAire(320, 50);
}

function draw() {
  background(220);
  moverAire.applyAirResistance(0.02);
  moverAire.update();
  moverAire.display();
}

class MoverAire {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 1);
  }

  applyAirResistance(coefficient) {
    let airResistance = this.velocity.copy();
    airResistance.mult(-1);
    airResistance.normalize();
    airResistance.mult(coefficient * this.velocity.magSq());
    this.velocity.add(airResistance);
  }

  update() {
    this.position.add(this.velocity);
  }

  display() {
    fill(0, 0, 255);
    ellipse(this.position.x, this.position.y, 20, 20);
  }
}
```
### Explicación: 
**Resistencia del aire:** La resistencia del aire es modelada como una fuerza que se opone al movimiento del objeto y es proporcional al cuadrado de la velocidad del objeto. Para calcularla:

Se obtiene la velocidad del objeto.
Se normaliza la velocidad para obtener la dirección opuesta.
Se multiplica por el cuadrado de la velocidad y por un coeficiente de resistencia.
Se aplica la fuerza resultante al objeto.
Este modelo simula cómo la resistencia del aire o de un fluido desacelera objetos en movimiento, con mayor efecto a velocidades altas.

### Link: https://editor.p5js.org/Cuervo01/sketches/aZ6D4wavV

## 3. Atracción gravitacional
La atracción gravitacional entre dos objetos depende de sus masas y la distancia entre ellos.

Código de la simulación de atracción gravitacional:
```javascript

let mover1, mover2;

function setup() {
  createCanvas(640, 360);
  mover1 = new MoverGravedad(20, width / 2 - 100, height / 2);
  mover2 = new MoverGravedad(10, width / 2 + 100, height / 2);
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

class MoverGravedad {
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
    ellipse(this.position.x, this.position.y, this.mass * 2, this.mass * 2);
  }
}
```
Enlace a la simulación de atracción gravitacional:

### Explicación:
**Atraccion gravitacional:** La atracción gravitacional es modelada como una fuerza que actúa entre dos objetos con masa y es proporcional al producto de sus masas e inversamente proporcional al cuadrado de la distancia entre ellos. Para calcularla:

Se obtiene el vector de dirección entre los dos objetos.
Se calcula la distancia y se limita a un rango mínimo y máximo para evitar valores extremos.
Se normaliza el vector de dirección.
Se calcula la magnitud de la fuerza usando la ecuación de Newton:
Se multiplica el vector de dirección por la magnitud calculada.
Se aplica la fuerza al otro objeto.
Este modelo simula cómo los objetos con masa se atraen entre sí, similar a la gravedad en la naturaleza.

### Link: https://editor.p5js.org/Cuervo01/sketches/O2r48AXmg
