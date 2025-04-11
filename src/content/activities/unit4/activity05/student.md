### 1️. Relación entre r y theta con las posiciones x e y
Las coordenadas polares se basan en dos valores:

- r representa la distancia desde el origen hasta el punto.
 theta es el ángulo medido desde el eje positivo X.

Para convertir estas coordenadas a cartesianas, la posición en el eje X se obtiene multiplicando r por el coseno de theta, mientras que la posición en el eje Y se obtiene multiplicando r por el seno de theta. Esto significa que r determina la distancia del punto al centro, mientras que theta define en qué dirección se encuentra.

### 2️. ¿Qué ocurre en la segunda modificación y por qué?
En esta versión, la conversión de coordenadas usa un método que solo toma en cuenta el ángulo, sin especificar el radio. Como resultado, el punto se mueve en un círculo de radio 1, en lugar de depender de un valor de r. Esto hace que el movimiento sea menos notorio si se esperaba que el punto recorriera un área mayor. Además, hay una referencia a variables que no están definidas en esta versión, lo que podría generar errores o resultados inesperados.

### 3️. ¿Qué ocurre en la tercera modificación y por qué?
Aquí se incluye explícitamente el radio en la conversión de coordenadas. Como resultado, el punto ahora se mueve en un círculo cuyo tamaño depende del valor de r, asegurando que el comportamiento sea el esperado. Con esta corrección, la relación entre las coordenadas polares y cartesianas se mantiene correctamente, y el punto se mueve en la trayectoria adecuada.
