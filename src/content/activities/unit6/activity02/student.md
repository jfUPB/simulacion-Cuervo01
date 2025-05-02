### 1. Estructura del campo de flujo

El campo de flujo en el ejemplo de The Nature of Code se almacena en una estructura de datos basada en una cuadrícula de vectores. En este caso, se implementa como un array 2D donde cada celda contiene un vector de dirección.

Estructura de datos: Un array bidimensional (this.field en la clase FlowField).

Elemento de la estructura: Cada celda de la cuadrícula contiene un vector de dirección.

Generación de los vectores: Se usa el ruido Perlin (noise()) para generar variaciones suaves en los vectores de flujo, lo que permite que los agentes sigan caminos más orgánicos en lugar de moverse en patrones rígidos.

### 2. Comportamiento del agente

Los agentes utilizan el campo de flujo para determinar su movimiento de la siguiente manera:

Localización del vector correspondiente: Cada agente mapea su posición actual a la cuadrícula del campo de flujo utilizando la función floor(), de modo que encuentra el índice correcto dentro de la matriz de vectores.

Obtención del vector de dirección: Una vez identificado el índice de la cuadrícula, el agente obtiene el vector de dirección de esa celda.

Cálculo de la fuerza de dirección:

Se toma el vector del campo y se convierte en la fuerza deseada.

Se resta la velocidad actual del agente de la fuerza deseada para obtener la "fuerza de dirección".

Se limita la magnitud de la fuerza de dirección usando maxforce para evitar cambios bruscos en la trayectoria.

### 3. Parámetros clave identificados

Resolución del campo de flujo: Determina el tamaño de cada celda en la cuadrícula y, por lo tanto, la cantidad de vectores.

Velocidad máxima (maxspeed) de los agentes: Controla la rapidez con la que pueden moverse.

Fuerza máxima (maxforce): Regula la aceleración y el cambio de dirección de los agentes.

### 4. Modificación experimental

Para analizar el impacto en el comportamiento de los agentes, realicé la siguiente modificación en el código:

Modificación realizada

Se realizaron tres cambios principales para evitar que los agentes se alineen en una hilera: primero, se modificó la generación del campo de flujo usando noise() con un desplazamiento temporal (frameCount * 0.005), lo que evita que los vectores sean estáticos y genera trayectorias más dinámicas. Segundo, se introdujo la función applyRandomDisruption(), que cada 60 frames aplica una ligera variación aleatoria a la dirección de los agentes, evitando que sigan exactamente el mismo camino. Tercero, se añadió variabilidad en la maxspeed y maxforce de cada agente, asegurando que no se comporten de manera idéntica y promoviendo trayectorias más orgánicas y dispersas.

```javascript
let cols, rows;
let resolution = 20;
let flowfield;
let vehicles = [];

function setup() {
  createCanvas(600, 600);
  cols = floor(width / resolution);
  rows = floor(height / resolution);
  flowfield = new Array(cols * rows);

  for (let i = 0; i < 100; i++) {
    vehicles.push(new Vehicle(random(width), random(height)));
  }
}

function draw() {
  background(240);
  generateFlowField();

  for (let vehicle of vehicles) {
    vehicle.follow(flowfield);
    vehicle.applyRandomDisruption(); // Nueva función para evitar alineación
    vehicle.update();
    vehicle.display();
  }
}

function generateFlowField() {
  for (let y = 0; y < rows; y++) {
    for (let x = 0; x < cols; x++) {
      let index = x + y * cols;

      // Nueva fórmula con ruido Perlin y variaciones temporales
      let angle = noise(x * 0.1, y * 0.1, frameCount * 0.005) * TWO_PI * 2;
      let v = p5.Vector.fromAngle(angle);
      v.setMag(1);
      flowfield[index] = v;
    }
  }
}

class Vehicle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D();
    this.acceleration = createVector(0, 0);
    this.maxspeed = random(2.5, 4.5); // Variación en la velocidad
    this.maxforce = random(0.05, 0.15); // Variación en la fuerza
  }

  follow(flowfield) {
    let x = floor(this.position.x / resolution);
    let y = floor(this.position.y / resolution);
    let index = x + y * cols;
    if (index >= 0 && index < flowfield.length) {
      let force = flowfield[index].copy();
      this.applyForce(force);
    }
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  applyRandomDisruption() {
    // Cada cierto tiempo, se agrega una leve variación en la dirección
    if (frameCount % 60 === 0) {
      let disruption = p5.Vector.random2D().mult(0.2);
      this.applyForce(disruption);
    }
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);

    // Wrapping
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) this.position.y = 0;
    if (this.position.y < 0) this.position.y = height;
  }

  display() {
    fill(50);
    noStroke();
    ellipse(this.position.x, this.position.y, 4, 4);
  }
}

```

### Efecto observado

Esta modificación creó un patrón más predecible en la dirección de los agentes, con trayectorias que tendían a formar remolinos y bucles en lugar de caminos suavemente curvados. En comparación con la versión original basada en noise(), los agentes mostraron un comportamiento más estructurado y repetitivo.

Se adjunta una captura de pantalla/GIF que muestra la diferencia en el movimiento.
