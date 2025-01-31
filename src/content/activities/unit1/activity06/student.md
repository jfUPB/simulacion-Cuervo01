## Lévy flight

Es un patron de movimiento en el cual el objeto se mueve en pasos aleatorios, con una distibución 
que sigue una ley de potencia, dando, en su mayoría, pasos pequeños y algunos pasos muy grandes. 

```javascript
let numSteps = 500; // Número de pasos
let alpha = 1.5; // Parámetro de la distribución de Lévy
let x, y; // Posición actual
let path = []; // Almacena la trayectoria

function setup() {
  createCanvas(600, 600);
  x = width / 2;
  y = height / 2;
  path.push({ x, y });
}

function draw() {
  background(30);
  stroke(255);
  noFill();
  
  // Dibujar trayectoria
  beginShape();
  for (let p of path) {
    vertex(p.x, p.y);
  }
  endShape();

  if (path.length < numSteps) {
    let angle = random(TWO_PI);
    let stepLength = (1 / pow(random(), 1 / alpha)) * 10; // Generar paso Lévy
    x += cos(angle) * stepLength;
    y += sin(angle) * stepLength;
    
    // Mantener dentro de los límites
    x = constrain(x, 0, width);
    y = constrain(y, 0, height);
    
    path.push({ x, y });
  } else {
    noLoop(); // Detener cuando alcance el número de pasos
  }
}

```

![image](https://github.com/user-attachments/assets/d9d2d37d-f225-47af-ab67-ebee127316c0)
