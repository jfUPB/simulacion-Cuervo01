#### ¿Qué pasa si el caminante tiene más probabilidad de moverse hacia la derecha que en otras direcciones?

Si le damos un sesgo hacia la derecha, esperamos que el caminante termine desplazándose
en esa dirección en promedio, en lugar de quedarse en una posición central.

## Modiicación del código:

Se cambia la manera en que xstep se elige para que haya más probabilidad de que sea 
positivo. Haciendo que el caminante nunca se mueva a la izquierda, sólo a la derecha


```javascript
step() {
  let xstep = random(0, 1);  // Se mueve solo a la derecha
  let ystep = random(-1, 1); // Mantiene movimiento libre en Y
  this.x += xstep;
  this.y += ystep;
}
```

## Resultados esperados:

- La trayectoria del caminante tenderá a moverse hacia la derecha, en lugar de un patrón aleatorio en todas direcciones.
- Puede que el camino aún tenga algunas oscilaciones en Y, pero en general debería desplazarse hacia la derecha a medida que pasa el tiempo.

## Resultados obtenidos:

![Experimento_caminante](../../src/assets/Caminante.png)

