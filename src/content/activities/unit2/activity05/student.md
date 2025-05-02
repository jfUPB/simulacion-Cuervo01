## Código

```javascript

let t = 0; // Factor de interpolación

function setup() {
    createCanvas(100, 100);
}

function draw() {
    background(200);

    let v0 = createVector(50, 50);
    let v1 = createVector(30, 0);
    let v2 = createVector(0, 30);
    
    // Movimiento del vector morado entre rojo y azul
    t = (sin(frameCount * 0.05) + 1) / 2; // Oscila entre 0 y 1
    let v3 = p5.Vector.lerp(v1, v2, t);
    
    // Color dinámico basado en t
    let colorLerp = lerpColor(color('red'), color('blue'), t);

    // Vector verde desde la punta del rojo hasta la punta del azul
    let v4 = p5.Vector.sub(v2, v1); 

    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v3, colorLerp); // Vector morado con color dinámico
    drawArrow(p5.Vector.add(v0, v1), v4, 'green'); // Vector verde
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

