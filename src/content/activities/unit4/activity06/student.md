### Código:

```javascript

let amplitudeSlider, frequencySlider, phaseSlider, speedSlider;
let t = 0;

function setup() {
  createCanvas(600, 400);
  
  // Crear sliders para controlar parámetros
  amplitudeSlider = createSlider(10, 150, 75, 1);
  amplitudeSlider.position(20, height - 100);
  
  frequencySlider = createSlider(0.01, 0.2, 0.05, 0.01);
  frequencySlider.position(20, height - 80);
  
  phaseSlider = createSlider(0, TWO_PI, 0, 0.1);
  phaseSlider.position(20, height - 60);
  
  speedSlider = createSlider(0.01, 0.1, 0.02, 0.01);
  speedSlider.position(20, height - 40);
}

function draw() {
  background(255);
  
  let amplitude = amplitudeSlider.value();
  let frequency = frequencySlider.value();
  let phase = phaseSlider.value();
  let speed = speedSlider.value();
  
  translate(0, height / 2);
  
  stroke(0);
  noFill();
  beginShape();
  for (let x = 0; x < width; x++) {
    let y = amplitude * sin(TWO_PI * frequency * x + phase + t);
    vertex(x, y);
  }
  endShape();
  
  t += speed;
  
  // Mostrar valores de los sliders
  fill(0);
  noStroke();
  text(`Amplitud: ${amplitude}`, 200, height - 90);
  text(`Frecuencia: ${frequency}`, 200, height - 70);
  text(`Fase: ${phase.toFixed(2)}`, 200, height - 50);
  text(`Velocidad: ${speed}`, 200, height - 30);
}
```
### Link: https://editor.p5js.org/Cuervo01/sketches/Ump2slsPo

### Captura: 
