Para poder realizar la conversion, tuve que reemplazar las variables que almacenaba la posición 
del walker, y sustituírla por un objeto, que encapsulara ambas coordenadas.
Por otro lado, otra de las diferencias que hay, es que la posición de inicializaba otorgándole 
las coordenadas directamente a x y y. Lo cual tuve que cambiar, creando un vector e inicializando
su posición en el centro del lienzo.
Tambén se modificó el código para su movimiento utilizando .add().

```javascript
let position;

function setup() {
  createCanvas(400, 400);
  position = createVector(width / 2, height / 2); // Inicializa la posición en el centro
}

function draw() {
  background(255);

  // Dibujar el walker
  fill(0);
  ellipse(position.x, position.y, 20, 20);

  // Generar un paso en una dirección aleatoria
  let step = p5.Vector.random2D();
  step.mult(2); // Escalar el tamaño del paso

  position.add(step); // Aplicar el paso a la posición
}
```
