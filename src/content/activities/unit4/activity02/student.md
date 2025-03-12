## Analisis del primer código

### ¿Qué está pasando en esta simulación? ¿Cuál es la interacción?

En esta simulación, los elementos gráficos giran alrededor del centro de la pantalla. La interacción se da a través de la función rotate(), que cambia el ángulo de los elementos en cada frame, produciendo un efecto de rotación.

### Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?

Se hace para facilitar la rotación de los elementos alrededor de un punto central. Si el origen estuviera en la esquina superior izquierda (0,0), la rotación se haría en relación con esa posición, lo que complicaría la visualización.

### ¿Cuál es la relación entre el sistema de coordenadas y la función rotate()?

La función rotate() rota el sistema de coordenadas en torno al origen actual. Como el origen se traslada al centro de la pantalla con translate(width/2, height/2), la rotación ocurre alrededor de este punto en lugar de la esquina superior izquierda.

### ¿Por qué parece que los elementos gráficos se dibujan en la posición (0,0)?

Porque después de hacer translate(), el punto (0,0) se encuentra en el centro de la pantalla. Todos los elementos se dibujan en relación con este nuevo origen.

### ¿Por qué, aunque en cada frame se hace lo mismo, los elementos gráficos rotan?

Porque antes de dibujar cada frame, se aplica una rotación con rotate(), que afecta la orientación de todos los elementos dibujados en el nuevo sistema de coordenadas.

## Análisis del segundo código

### ¿Qué se hace en el marco Motion 101?

Se establece un marco de referencia para controlar el movimiento de un objeto en el espacio. Se calcula su dirección mediante un vector de velocidad y se usa este vector para determinar el ángulo de rotación.

### ¿Qué hace la función heading()?
Devuelve el ángulo de dirección del vector de velocidad en radianes. Este ángulo es el que se usa en rotate(angle), asegurando que el objeto apunte en la dirección en la que se mueve.
### ¿Qué hacen las funciones push() y pop()?
push() guarda el estado actual del sistema de coordenadas.
pop() restaura el estado anterior del sistema de coordenadas.
Esto permite que las transformaciones como translate() y rotate() solo afecten el objeto en cuestión, sin modificar otros elementos en la pantalla.
### ¿Qué hace rectMode(CENTER)?
Cambia el modo de dibujo de rectángulos para que las coordenadas dadas sean el centro del rectángulo en lugar de la esquina superior izquierda. Esto facilita la rotación, ya que el punto de referencia del rectángulo está en su centro.

### ¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad?

El ángulo de rotación es el mismo que el ángulo del vector de velocidad. Como el vector de velocidad indica la dirección del movimiento, usar rotate(angle) alinea el objeto con esta dirección.

### Dibujo conceptual:
Si el vector de velocidad apunta hacia la derecha, el ángulo será 0 radianes.
Si apunta hacia arriba, el ángulo será -π/2 radianes.
La traslación mueve el objeto a su posición actual y la rotación lo alinea con el vector de movimiento.

