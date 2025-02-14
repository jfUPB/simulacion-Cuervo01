## Código

```javascript

let t = 0; 
let scaleFactor = 3; // Aumenta el tamaño de los vectores

function setup() {
    createCanvas(300, 300); // Aumentamos el tamaño del lienzo
}

function draw() {
    background(200);

    let v0 = createVector(150, 150); // Centro del canvas
    let v1 = createVector(30, 0).mult(scaleFactor);
    let v2 = createVector(0, 30).mult(scaleFactor);
    
    // Movimiento del vector morado entre rojo y azul
    t = (sin(frameCount * 0.05) + 1) / 2;
    let v3 = p5.Vector.lerp(v1, v2, t);
    
    // Color dinámico basado en t
    let colorLerp = lerpColor(color('red'), color('blue'), t);

    // Vector verde desde la punta del rojo hasta la punta del azul
    let v4 = p5.Vector.sub(v2, v1);

    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v3, colorLerp);
    drawArrow(p5.Vector.add(v0, v1), v4, 'green');
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(4); // Flechas más gruesas
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 10; // Flechas más grandes
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```
##¿Cómo funciona lerp() y lerpColor().?

### Lerp()

Se utiliza para encontrar un punto intermedio entre dos valores

### LerpColor()

Se utiliza para interpolar entre dos colores

## ¿Cómo se dibuja una flecha usando drawArrow()?

La función dibuja una flecha en el lienzo tomando 3 parámetros:

- La posición del inicio del vector
- La dirección y magnitud del vector
- Color de la flecha

