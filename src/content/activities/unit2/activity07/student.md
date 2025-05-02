### Motion 101

Es un marco conceptual, para entender y programar movimiento por medio de vectores. Se basa en la relación
entre posición, velocidad, y aceleración para generar movimientos fluidos y realistas.

- Position: Representa la ubicación actual del objeto
- Velocity: Define el cambio de la posición en cada frame.
- Acceleration: controla la variación de la velocidad, perimitiendo efectos como aceleración y desaceleración.

## Ejemplo:

```javascript

let position;
let velocity;
let acceleration;

function setup() {
  createCanvas(600, 400);
  position = createVector(width / 2, height / 2);
  velocity = createVector(0, 0);
  acceleration = createVector(0.01, 0.01); // Pequeña aceleración
}

function draw() {
  background(220);

  // Actualizar velocidad y posición
  velocity.add(acceleration);
  position.add(velocity);

  // Dibujar la pelota
  fill(255, 0, 0);
  ellipse(position.x, position.y, 20, 20);

  // Rebote en los bordes
  if (position.x > width || position.x < 0) {
    velocity.x *= -1;
  }

  if (position.y > height || position.y < 0) {
    velocity.y *= -1;
  }
}


```
## Video del ejemplo


https://github.com/user-attachments/assets/b58d5005-fb98-442c-8c66-af24e2ed6f9b

