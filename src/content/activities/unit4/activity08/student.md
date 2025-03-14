### Código:
```javascript


let xSpacing = 16; // Espacio entre cada punto de la onda
let widthWave; // Ancho de la onda
let theta = 0.0; // Ángulo inicial
let amplitude = 75.0; // Altura de la onda
let period = 500.0; // Periodo de la onda (afecta la longitud de onda)
let dx; // Incremento en x basado en el periodo
let yValues; // Arreglo de valores y para la onda

function setup() {
  createCanvas(800, 400);
  widthWave = width + 16;
  dx = (TWO_PI / period) * xSpacing;
  yValues = new Array(floor(widthWave / xSpacing));
}

function draw() {
  background(255);
  calcWave();
  renderWave();
}

function calcWave() {
  // Incrementa theta para mover la onda
  theta += 0.02;

  let x = theta;
  for (let i = 0; i < yValues.length; i++) {
    yValues[i] = sin(x) * amplitude; // Calcula los valores y usando la sinusoide
    x += dx;
  }
}

function renderWave() {
  noStroke();
  fill(127);
  for (let x = 0; x < yValues.length; x++) {
    ellipse(x * xSpacing, height / 2 + yValues[x], 16, 16); // Dibuja círculos para representar la onda
  }
}
```
### Link:

https://editor.p5js.org/Cuervo01/sketches/mXfG_FObB

### Captura:


https://github.com/user-attachments/assets/7c7a4cb7-0596-440b-abad-c19f2e171073

