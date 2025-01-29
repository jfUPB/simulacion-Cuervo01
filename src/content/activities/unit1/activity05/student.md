### Ejemplo de distribución normal

Se usa la función rect(x, y, tamano, tamano) para dibujar cuadrados. Los cuadrados siguen la misma lógica de distribución en x (concentrados en el centro del canvas y distribuidos de manera normal) y se distribuyen verticalmente de manera aleatoria usando y.

#### Código:
```javascript
function setup() {
  createCanvas(400, 200);
  background(255);
  noStroke();
  
  // Parámetros para la distribución normal
  let media = width / 2; // La media será el centro del canvas
  let desviacionEstandar = 50; // Desviación estándar controlando la dispersión
  
  // Visualización de la distribución
  for (let i = 0; i < 1000; i++) {
    // Generar un número aleatorio con distribución normal
    let x = randomGaussian(media, desviacionEstandar); 
    let y = random(height); // Distribuir verticalmente
    let tamano = 10; // Tamaño de los cuadrados
    
    // Dibuja cuadrados donde la posición X depende de la distribución normal
    if (x >= 0 && x <= width) { // Asegurarse de que el valor esté dentro del canvas
      fill(0, 100, 255, 50); // Color de los cuadrados
      rect(x, y, tamano, tamano); // Dibuja el cuadrado
    }
  }
}

```

![Distribución normal](/src/assets/DistribucionNormal.png)
