#### Diferencia entre distribución uniforme y no uniforme

- En la distribucion uniforme, todos los valores dentro de un rango, tienen la misma probabilidad de ocurrir.
 Es decir, la probabilidad de que se mueva a la derecha o a la izquierda, es la misma.

- En la distribución no uniforme, algunos valores tienen más probabilidades de salir, por lo que favorece el
 avance en esa direccion en vez de retroceder

### Código modificado:

```javascript
function setup() {
  createCanvas(100, 100);
  background(200);
  describe('Movimiento aleatorio favorecido hacia la derecha');
}

function draw() {
  // Estilo de los círculos.
  noStroke();
  fill(0, 10);

  // Distribución uniforme entre 0 y 100.
  let x = random(100);
  let y = 25;
  circle(x, y, 5);

  // Distribución normal centrada en 75 con desviación estándar pequeña (favorece a la derecha).
  x = randomGaussian(75, 10);  // Media en 75 (más cerca del lado derecho).
  y = 50;
  circle(x, y, 5);

  // Otra distribución normal centrada en 80 para favorecer aún más la derecha.
  x = randomGaussian(80, 10);  // Media en 80 (más cerca del borde derecho).
  y = 75;
  circle(x, y, 5);
}
```

### Resultado:
![image](src/assets/RandomWalk.png)

