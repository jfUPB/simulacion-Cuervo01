## Para qué sirve el método mag()?

El método mag() sirve para calcular la magnitu de un vector 2D

## Para qué sirve el método magSq()?

El método magSq() calcula la magnitud del vector al cuadrado

### ¿Cuál es más eficiente?

Si hablamos de rendimiento computacional, es más eficiente el método magSq(), ya que calcula el cuadrado 
de la magnitud del vector sin necesidad de calcular la raíz cuadrada. Por otro lado, mag() calcula la 
magnitud real del vector.

## ¿Para qué sirve el método normalize()?

Se utiliza para escalar el vector y que tenga una magnitud de 1 sin cambiar su dirección.

## ¿Para qué sirve el método dot()?

El método dot() se usa para calcular el producto cruz de dos vectores.

## ¿Cuál es la interpretación geométrica del producto cruz de dos vectores?

El producto cruz de dos vectores en el espacio tridimensional genera un nuevo vector. Geométricamente, el
nuevo vector apunta en una dirección que sigue la regla de la mano derecha: si alineas el pulgar con el 
primer vector y el índice con el segundo, el dedo medio apunta en la dirección del vector resultante.

La magnitud del vector, mide la perpendicularidad de los vectores originales entre sí; si son paralelos,
el producto cruz es 0.

## ¿Para que te puede servir el método dist()?

El método dist() calcula la distancia entre dos puntos en un espacio, sea 2D o 3D.

## Para qué sirven los métodos normalize() y limit()?

**Normalize()**  Escala el vector de manera que su magnitud sea 1.

**limit()**  Restringe la magnitud de un vector a un valor máximo especificado
