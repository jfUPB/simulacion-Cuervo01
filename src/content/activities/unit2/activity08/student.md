## ¿Qué pasa si...?

¿Que pasa si hago que el objeto cambie de color y su aceleración en función de la distancia que tiene
con el mouse?

## Hipótesis

Pienso que debería cambiar de color a medida que se acerque o se aleje el cursor. Si se acerca es azul,
y si se aleja es rojo. Y tambien, si se acera al mouse, disminuye su velocidad, y si se aleja, aumenta.

## Implementación


```javascript
let pos;
let velocity;

function setup() {
  createCanvas(640, 360);
  pos = createVector(100, 100);
  velocity = createVector(2.5, 5);
}

function draw() {
  background(255);

  // Calcular la distancia entre el ratón y el objeto
  let mouse = createVector(mouseX, mouseY);
  let distance = p5.Vector.dist(pos, mouse);

  // Ajustar la velocidad en función de la distancia
  let direction = p5.Vector.sub(mouse, pos);
  direction.normalize();
  direction.mult(distance / 50);
  velocity = direction;

  // Añadir la velocidad a la ubicación
  pos.add(velocity);

  // Calcular el color basado en la distancia
  let colorValue = map(distance, 0, width, 0, 255);
  colorValue = constrain(colorValue, 0, 255);

  // Dibujar el círculo en la nueva ubicación con el color calculado
  stroke(0);
  fill(colorValue, 100, 255 - colorValue);
  ellipse(pos.x, pos.y, 16, 16);
}

```

## ¿Qué pasó? 

Su color y aceleración cambia con respecto a la distancia que tenga del cursor. Si se acerca al cursor
se pone azul, y si se aleja, va cambiando a rojo.

## ¿Por qué?

Utilizé la función map() para asignar la distancia del objeto a un rango de valores de color. y constrain() 
asegura que el valor se mantenga dentro de los límites apropiados. Y para la aceleración, calculé la dirección
hacia el cursor y la normalicé, ajusté la magnitud de la aceleración en función de l.a distancia dividida por 
un factor.

## Conclusión

Al integrar tanto el cambio de color como la aceleración basados en la distancia al cursor del ratón, logré una 
experiencia interactiva dinámica. El objeto no solo reacciona a la posición del cursor, sino que también lo hace
de una manera visualmente atractiva y con un movimiento fluido y realista. 
