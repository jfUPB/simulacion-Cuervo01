### Código:

```javascript
let r1 = 125;
let r2 = 125;
let m1 = 10;
let m2 = 10;
let a1 = Math.PI / 2;
let a2 = Math.PI / 2;
let a1_v = 0;
let a2_v = 0;
let g = 1;
let px2, py2;
let buffer;

function setup() {
  createCanvas(600, 600);
  buffer = createGraphics(width, height);
  buffer.background(20);
  background(20);
}

function draw() {
  background(20, 50);
  image(buffer, 0, 0, width, height);

  let num1 = -g * (2 * m1 + m2) * sin(a1);
  let num2 = -m2 * g * sin(a1 - 2 * a2);
  let num3 = -2 * sin(a1 - a2) * m2;
  let num4 = a2_v * a2_v * r2 + a1_v * a1_v * r1 * cos(a1 - a2);
  let den = r1 * (2 * m1 + m2 - m2 * cos(2 * a1 - 2 * a2));
  let a1_a = (num1 + num2 + num3 * num4) / den;

  let num5 = 2 * sin(a1 - a2);
  let num6 = a1_v * a1_v * r1 * (m1 + m2) + g * (m1 + m2) * cos(a1) + a2_v * a2_v * r2 * m2 * cos(a1 - a2);
  let den2 = r2 * (2 * m1 + m2 - m2 * cos(2 * a1 - 2 * a2));
  let a2_a = (num5 * num6) / den2;

  a1_v += a1_a;
  a2_v += a2_a;
  a1 += a1_v;
  a2 += a2_v;

  // Inspirado en la simulación de referencia, aplicamos conservación de energía
  a1_v *= 0.9998; // Reducción mínima de energía
  a2_v *= 0.9998;

  // Introducir perturbaciones periódicas para mantener variabilidad
  if (frameCount % 300 === 0) {
    a1_v += random(-0.02, 0.02);
    a2_v += random(-0.02, 0.02);
  }

  let x1 = width / 2 + r1 * sin(a1);
  let y1 = 200 + r1 * cos(a1);
  let x2 = x1 + r2 * sin(a2);
  let y2 = y1 + r2 * cos(a2);

  stroke(255, 150, 0);
  strokeWeight(4);
  line(width / 2, 200, x1, y1);
  fill(255, 0, 150);
  ellipse(x1, y1, m1 * 3);
  line(x1, y1, x2, y2);
  fill(0, 255, 150);
  ellipse(x2, y2, m2 * 3);

  buffer.stroke(random(100, 255), random(100, 255), 255, 150);
  buffer.strokeWeight(2);
  if (frameCount > 1) {
    buffer.line(px2, py2, x2, y2);
  }
  px2 = x2;
  py2 = y2;
}

```


### Link: https://editor.p5js.org/Cuervo01/sketches/dYRi1ZC2l
