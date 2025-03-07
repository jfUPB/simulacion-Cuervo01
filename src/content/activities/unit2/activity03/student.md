## ¿Qué resultado esperas obtener?

Al ver el código a simple vista, espero que haya un canvas color gris, y pareciera que el código dibuja algo sobre él 
que cambia de posición y que la consola imprima la frase "only once". 

## ¿Qué obtuviste?

Lo que se obtiene al poner el código en p5.js es un canvas gris y un texto que arroja la consola "Only once. Pero no 
se evidencia nada dibujado sobre el canvas.

## Revisar conceptos:

**Paso por valor:** Se aplica a tipos de datos como números, cadenas y booleanos. Cuando éste pasa a una función, se pasa 
una copia del valor original.

### Ejemplo

```javascript

function increment(x) {
    x++;
    console.log(x); // Imprime 6
}

let a = 5;
increment(a);
console.log(a); // Imprime 5, el valor original no ha cambiado

``` 

**Paso por referencia:** Se aplica a objetos y arrays. Cuando éste [asa a una función, se pasa una referencia al objeto original,
es decir que, cualquier cambio hecho dentro de la función, afeta también al original.

### Ejemplo

```javascript

let position;

function setup() {
    createCanvas(400, 400);
    posicion = createVector(6,9);
    playingVector(posicion);
    noLoop();
}

function playingVector(v){
    v.x = 20;
    v.y = 30;
}

function draw() {
    background(220);
    console.log("Only once");
}

```

## ¿Qué tipo de paso se está realizando en el código? 

El código realiza un paso por referencia, ya que el vector position es un objeto que pasa a una función. Por lo que cualquier 
modificación que se haga dentro de ella, afecta al objeto original. 

## ¿Qué aprendiste? 

Apréndí cada uno de los conceptos de paso por valor y paso por referencia y como diferenciarlos
