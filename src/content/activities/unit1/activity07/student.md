###Explicación de la figura 0.4

Izquierda (ruido Perlin): Se observa una curva suave y continua, sin cambios bruscos. Esto significa que los valores cercanos en el tiempo tienen valores similares, lo que genera una variación gradual y fluida.
Derecha (ruido aleatorio tradicional): Se ve una gráfica con cambios bruscos y sin patrón, ya que cada punto se elige de forma completamente aleatoria.

```javascript
let xStart = 0; // Punto de inicio en el ruido Perlin

function setup() {
  createCanvas(600, 400);
}

function draw() {
  background(30);
  stroke(255);
  noFill();
  
  let xOff = xStart; // Variable para recorrer el ruido
  
  beginShape();
  for (let x = 0; x < width; x++) {
    let y = noise(xOff) * height; // Altura según el ruido Perlin
    vertex(x, y);
    xOff += 0.01; // Incremento suave para cambios progresivos
  }
  endShape();
  
  xStart += 0.01; // Hace que la onda se mueva lentamente
}

```
Se utilizó noise(xOff) para generar valores suaves en y.
Recorrela pantalla con un for para construir la línea.
xOff aumenta poco a poco para evitar saltos bruscos.
xStart mueve la onda en el tiempo.



![image](/src/assets/RuidoPerlin.png)
