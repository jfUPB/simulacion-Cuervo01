##Cuerpos y fuerzas gravitacionales:

Cada cuerpo es un objeto con posición, velocidad y aceleración, representado mediante la clase Body.
Se implementó una fuerza de atracción entre el cursor y los cuerpos, simulando una interacción gravitatoria inversamente proporcional a la distancia (siguiendo la ley de la gravedad).
También se incluyó una función de repulsión cuando se presiona el mouse, cambiando su dirección y color, lo que agrega interactividad.

##Conexiones dinámicas:

Se dibujan líneas de colores entre los cuerpos cuando están a una distancia menor a 250 píxeles.
Las líneas varían en grosor y color, generando un efecto visual dinámico similar a una estructura en constante cambio.

##Evitar que los cuerpos desaparezcan:

Se corrigió un problema donde los cuerpos podían salir del lienzo y desaparecer, agregando rebotes en los bordes de la pantalla.
Ahora, cuando un cuerpo toca un borde, su dirección se invierte y se mantiene dentro de los límites.

```javascript

let bodies = [];
let G = 1;

function setup() {
  createCanvas(windowWidth, windowHeight);
  for (let i = 0; i < 15; i++) {
    bodies.push(new Body(random(width), random(height), random(10, 30)));
  }
}

function draw() {
  background(20, 20, 30, 100);
  for (let body of bodies) {
    body.update();
  }
  drawConnections();
}

function mousePressed() {
  for (let body of bodies) {
    body.repel(mouseX, mouseY);
  }
}

function drawConnections() {
  for (let i = 0; i < bodies.length; i++) {
    for (let j = i + 1; j < bodies.length; j++) {
      let d = dist(bodies[i].pos.x, bodies[i].pos.y, bodies[j].pos.x, bodies[j].pos.y);
      if (d < 250) {
        stroke(random(100, 255), random(100, 255), random(100, 255), 150);
        strokeWeight(random(1, 4));
        line(bodies[i].pos.x, bodies[i].pos.y, bodies[j].pos.x, bodies[j].pos.y);
      }
    }
  }
}

class Body {
  constructor(x, y, r) {
    this.pos = createVector(x, y);
    this.vel = createVector(random(-2, 2), random(-2, 2));
    this.acc = createVector(0, 0);
    this.r = r;
    this.color = color(random(100, 255), random(100, 255), random(100, 255));
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update() {
    let mouseVec = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouseVec, this.pos);
    let distance = constrain(dir.mag(), 5, 200);
    dir.normalize();
    let strength = (G * this.r) / (distance * distance);
    dir.mult(strength);
    this.applyForce(dir);
    
    this.vel.add(this.acc);
    this.vel.limit(5);
    this.pos.add(this.vel);
    this.acc.mult(0);
    
    // Evita que los cuerpos desaparezcan
    if (this.pos.x < 0 || this.pos.x > width) {
      this.vel.x *= -1;
    }
    if (this.pos.y < 0 || this.pos.y > height) {
      this.vel.y *= -1;
    }
    
    this.pos.x = constrain(this.pos.x, 0, width);
    this.pos.y = constrain(this.pos.y, 0, height);
  }

  repel(mx, my) {
    let mouseVec = createVector(mx, my);
    let dir = p5.Vector.sub(this.pos, mouseVec);
    dir.setMag(10);
    this.applyForce(dir);
    this.color = color(random(100, 255), random(100, 255), random(100, 255));
  }
}

```

#### Link

https://editor.p5js.org/Cuervo01/sketches/p9Ab776OG

#### Captura






https://github.com/user-attachments/assets/fd34749b-8e80-477b-8817-ac71e7fee6ab

