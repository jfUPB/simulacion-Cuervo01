## Código

```javascript
function setup() {
    createCanvas(400, 400);
}

function draw() {
    background(220);

    let base = createVector(mouseX, mouseY); // La base sigue al mouse
    let scaleFactor = map(mouseX, 0, width, 0.5, 2); // Escala según la posición del mouse

    let v1 = createVector(100 * scaleFactor, 0);
    let v2 = createVector(0, 100 * scaleFactor);
    let v3 = p5.Vector.lerp(v1, v2, 0.5);

    drawArrow(base, v1, 'red');
    drawArrow(base, v2, 'blue');
    drawArrow(base, v3, 'purple');
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```
## Solución del problema
Primero creé un vector base que sigue la posición del mouse, lo que permite que los vectores se dibujen 
desde donde está el mouse. Luego, modifiqué el código para definir un factor de escala, basado en la 
posición del mouse en el eje X utilizando map().
