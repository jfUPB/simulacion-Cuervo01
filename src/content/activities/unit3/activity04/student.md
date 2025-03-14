### 1. ¿Dónde está el marco Motion 101 en el código?
   
El marco Motion 101 está presente en la manera en que se estructuran los cálculos de movimiento en la clase Mover. En particular, la función update() sigue los principios de Motion 101, que establece que el movimiento en un sistema digital se basa en tres elementos clave:

Posición 
Velocidad 
Aceleración 
En el código, estos tres elementos se relacionan de la siguiente manera:

```javascript


update() {
    this.velocity.add(this.acceleration);  // Se actualiza la velocidad con la aceleración
    this.velocity.limit(this.topSpeed);    // Se limita la velocidad para evitar que crezca indefinidamente
    this.position.add(this.velocity);      // Se actualiza la posición con la nueva velocidad
}
```

Esto sigue el flujo típico de Motion 101: posición → velocidad → aceleración. La aceleración afecta la velocidad, y la velocidad afecta la posición, permitiendo que el objeto simule un movimiento realista.

### 2. En la unidad anterior definíamos la aceleración mediante algún algoritmo. ¿Cuáles eran?
En la unidad anterior, la aceleración se definía manualmente en función de diferentes algoritmos. Algunos ejemplos comunes eran:

Aceleración constante: Se asignaba un valor fijo a la aceleración.

```javascript
this.acceleration = createVector(0.01, 0);
```
Aceleración aleatoria: Se generaban valores aleatorios para la aceleración en cada cuadro, simulando un movimiento errático.


```javascript
this.acceleration = p5.Vector.random2D();
this.acceleration.mult(0.5);  // Se escala para que no sea muy intensa
```
Aceleración hacia el mouse: Se calculaba la aceleración como un vector dirigido hacia la posición del mouse.


```javascript
let mouse = createVector(mouseX, mouseY);
let direction = p5.Vector.sub(mouse, this.position);
direction.setMag(0.2);  // Se establece la magnitud de la aceleración
this.acceleration = direction;
```
Estos algoritmos permitían definir manualmente cómo se comportaba la aceleración, en lugar de calcularla basándose en fuerzas externas.

### 3. En esta unidad vamos a calcular la aceleración. ¿Qué tiene que ver esto con las leyes de Newton?
Ahora que vamos a calcular la aceleración en lugar de definirla directamente, entramos en el territorio de las Leyes del Movimiento de Newton, en especial la Segunda Ley de Newton:

*F = m ⋅ a*

Esto significa que la aceleración de un objeto (a) es el resultado de la fuerza (F) aplicada sobre él, dividida por su masa (m). En código, esto se traduce a:

```javascript
applyForce(force) {
    let f = p5.Vector.div(force, this.mass);  // F = m * a  →  a = F / m
    this.acceleration.add(f);
}
```

En este modelo, en lugar de asignar arbitrariamente un valor de aceleración, calculamos la aceleración como una consecuencia de las fuerzas aplicadas. Esto es mucho más realista y nos permite simular efectos como la gravedad, la fricción o el viento.

