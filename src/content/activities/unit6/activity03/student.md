## Análisis del Algoritmo de Flocking
### Objetivo General del Algoritmo
El algoritmo de Flocking simula el comportamiento colectivo de grupos de agentes como pájaros o peces, que se mueven de forma fluida y coordinada siguiendo reglas locales. Cada “boid” actúa de forma individual, pero el conjunto genera un comportamiento emergente, como si existiera una inteligencia grupal.

### Reglas del Flocking
Cada agente (boid) sigue tres reglas fundamentales:

#### 1. Separación (Separation)
Objetivo: Evitar colisiones y mantener una distancia mínima con sus vecinos.

Lógica: El boid revisa cuáles vecinos están demasiado cerca. Por cada uno, calcula un vector que se aleje de él, y suma estos vectores. Finalmente, normaliza y ajusta la fuerza.

Fuerza resultante: Un vector de dirección que lo empuja lejos de sus vecinos más cercanos.

```javascript


// Ejemplo de fragmento en p5.js
separate(boids) {
  let steering = createVector();
  let total = 0;
  for (let other of boids) {
    let d = dist(this.position.x, this.position.y, other.position.x, other.position.y);
    if (other != this && d < this.perceptionRadius) {
      let diff = p5.Vector.sub(this.position, other.position);
      diff.div(d); // Más fuerte si están muy cerca
      steering.add(diff);
      total++;
    }
  }
  if (total > 0) {
    steering.div(total);
    steering.setMag(this.maxSpeed);
    steering.sub(this.velocity);
    steering.limit(this.maxForce);
  }
  return steering;
}

```
#### 2. Alineación (Alignment)
Objetivo: Moverse en la misma dirección que el promedio de sus vecinos.

Lógica: El boid promedia las velocidades de sus vecinos cercanos y se ajusta para alinearse con esa dirección.

Fuerza resultante: Un vector de dirección hacia la velocidad promedio local.

```javascript


align(boids) {
  let steering = createVector();
  let total = 0;
  for (let other of boids) {
    let d = dist(this.position.x, this.position.y, other.position.x, other.position.y);
    if (other != this && d < this.perceptionRadius) {
      steering.add(other.velocity);
      total++;
    }
  }
  if (total > 0) {
    steering.div(total);
    steering.setMag(this.maxSpeed);
    steering.sub(this.velocity);
    steering.limit(this.maxForce);
  }
  return steering;
}
```

#### 3. Cohesión (Cohesion)
Objetivo: Mantenerse cerca del grupo, evitando aislarse.

Lógica: El boid calcula el centro de masa de sus vecinos y genera una fuerza que lo dirige hacia ese punto.

Fuerza resultante: Un vector de dirección hacia la posición promedio de los vecinos.

```javascript
cohesion(boids) {
  let steering = createVector();
  let total = 0;
  for (let other of boids) {
    let d = dist(this.position.x, this.position.y, other.position.x, other.position.y);
    if (other != this && d < this.perceptionRadius) {
      steering.add(other.position);
      total++;
    }
  }
  if (total > 0) {
    steering.div(total);
    steering.sub(this.position);
    steering.setMag(this.maxSpeed);
    steering.sub(this.velocity);
    steering.limit(this.maxForce);
  }
  return steering;
}
```
### Parámetros Clave
Parámetro	Descripción
- **perceptionRadius**	Distancia dentro de la cual los boids consideran a otros como vecinos.
- **maxSpeed**	Velocidad máxima a la que puede moverse un boid.
- **maxForce**	Fuerza máxima que puede aplicar para cambiar su dirección.
- **separationWeight**	Peso relativo de la fuerza de separación.
- **alignmentWeight**	Peso relativo de la alineación.
- **cohesionWeight**	Peso relativo de la cohesión.

### Descripción del experimento de modificación
En esta modificación del sistema de n-cuerpos inspirado en las esculturas cinéticas de Alexander Calder, se introdujeron elementos visuales y comportamentales que elevan la experiencia a un plano más artístico y sensorial. El objetivo fue transformar la simulación en una obra de arte generativo en tiempo real, fusionando ciencia, diseño y expresión estética.

Cambios principales realizados
Rastro visual con transparencia (alpha trail):
Se incorporó un efecto de estela mediante el uso de una capa semitransparente en el fondo (background con alpha). Esto permite que los movimientos de los cuerpos dejen un rastro, generando una sensación de fluidez y continuidad en el tiempo, como si cada instante quedara parcialmente congelado en la escena.

Paleta de colores vibrante y dinámica:
Se asignaron colores brillantes y contrastantes a cada cuerpo, haciendo uso del espacio de color HSB para lograr combinaciones armónicas y saturadas. Esto resalta la individualidad de cada cuerpo y aporta una dimensión visual rica al conjunto.

Estética inspirada en el arte generativo:
La disposición visual y la interacción entre los cuerpos producen patrones impredecibles y orgánicos, evocando trazos abstractos y composiciones espontáneas. Cada ejecución de la simulación se convierte en una pieza única, impredecible y efímera.

Interacción con el usuario:
Se mantuvo la interacción del mouse para permitir que el usuario influya en el comportamiento del sistema, acentuando la relación entre el espectador y la obra, y potenciando su dimensión lúdica y exploratoria.

### Código modificado:
```javascript
let flock = [];
let hueOffset = 0;

function setup() {
  createCanvas(800, 600);
  colorMode(HSB, 360, 100, 100, 100);
  noStroke();
  for (let i = 0; i < 120; i++) {
    flock.push(new Boid());
  }
  background(0); // Fondo inicial oscuro
}

function draw() {
  // Fondo con alpha para dejar estelas
  fill(0, 0, 0, 10);
  rect(0, 0, width, height);

  hueOffset += 0.5;

  for (let boid of flock) {
    boid.edges();
    boid.flock(flock);
    boid.update();
    boid.show();
  }

  // Opcional: punto rojo del mouse
  fill(0, 100, 100);
  ellipse(mouseX, mouseY, 8, 8);
}

class Boid {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = p5.Vector.random2D();
    this.velocity.setMag(random(2, 4));
    this.acceleration = createVector();
    this.maxForce = 0.2;
    this.maxSpeed = 3.5;
    this.hue = random(360); // Color individual
  }

  edges() {
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) this.position.y = 0;
    if (this.position.y < 0) this.position.y = height;
  }

  align(boids) {
    let perceptionRadius = 25;
    let steering = createVector();
    let total = 0;
    for (let other of boids) {
      let d = dist(this.position.x, this.position.y, other.position.x, other.position.y);
      if (other != this && d < perceptionRadius) {
        steering.add(other.velocity);
        total++;
      }
    }
    if (total > 0) {
      steering.div(total);
      steering.setMag(this.maxSpeed);
      steering.sub(this.velocity);
      steering.limit(this.maxForce);
    }
    return steering;
  }

  separation(boids) {
    let perceptionRadius = 25;
    let steering = createVector();
    let total = 0;
    for (let other of boids) {
      let d = dist(this.position.x, this.position.y, other.position.x, other.position.y);
      if (other != this && d < perceptionRadius) {
        let diff = p5.Vector.sub(this.position, other.position);
        diff.div(d);
        steering.add(diff);
        total++;
      }
    }
    if (total > 0) {
      steering.div(total);
      steering.setMag(this.maxSpeed);
      steering.sub(this.velocity);
      steering.limit(this.maxForce);
    }
    return steering;
  }

  cohesion(boids) {
    let perceptionRadius = 25;
    let steering = createVector();
    let total = 0;
    for (let other of boids) {
      let d = dist(this.position.x, this.position.y, other.position.x, other.position.y);
      if (other != this && d < perceptionRadius) {
        steering.add(other.position);
        total++;
      }
    }
    if (total > 0) {
      steering.div(total);
      steering.sub(this.position);
      steering.setMag(this.maxSpeed);
      steering.sub(this.velocity);
      steering.limit(this.maxForce);
    }
    return steering;
  }

  seek(target) {
    let desired = p5.Vector.sub(target, this.position);
    desired.setMag(this.maxSpeed);
    let steering = p5.Vector.sub(desired, this.velocity);
    steering.limit(this.maxForce);
    return steering;
  }

  flock(boids) {
    let alignment = this.align(boids);
    let cohesion = this.cohesion(boids);
    let separation = this.separation(boids);
    let seekTarget = this.seek(createVector(mouseX, mouseY));

    alignment.mult(1.0);
    cohesion.mult(1.0);
    separation.mult(3.0);
    seekTarget.mult(0.4);

    this.acceleration.add(alignment);
    this.acceleration.add(cohesion);
    this.acceleration.add(separation);
    this.acceleration.add(seekTarget);
  }

  update() {
    this.position.add(this.velocity);
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxSpeed);
    this.acceleration.mult(0);

    // Oscilar ligeramente el color
    this.hue = (this.hue + 0.3) % 360;
  }

  show() {
    // Color con alpha
    fill((this.hue + hueOffset) % 360, 100, 100, 80);

    push();
    translate(this.position.x, this.position.y);
    rotate(this.velocity.heading());
    ellipse(0, 0, 12, 4); // Forma tipo pincel
    pop();
  }
}

```



