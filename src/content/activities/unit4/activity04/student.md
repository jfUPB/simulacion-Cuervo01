### 1. ¿Qué es Motion 101?
Motion 101 es un concepto que describe cómo un objeto se mueve utilizando principios básicos de física. Se basa en la relación entre posición, velocidad y aceleración:

La aceleración cambia debido a fuerzas aplicadas.
La velocidad se ajusta según la aceleración.
La posición se actualiza en función de la velocidad.
Este marco nos ayuda a entender el movimiento de objetos en simulaciones, haciéndolo más realista.

### 2. ¿Qué modificación se hace a Motion 101 cuando se agregan fuerzas acumulativas?
Cuando se agregan fuerzas acumulativas, es necesario reiniciar la aceleración en cada frame para evitar que se sume infinitamente. Si no hacemos esto, el objeto se movería de manera incontrolable, ya que cada nueva fuerza seguiría acumulándose sin detenerse.

El proceso correcto es:

Cada frame, las fuerzas aplicadas se suman a la aceleración.
La velocidad se ajusta con la aceleración.
La posición se actualiza con la nueva velocidad.
Se reinicia la aceleración para que en el siguiente frame solo se apliquen las nuevas fuerzas.
Esto permite que la simulación mantenga un movimiento realista.

### 3. ¿Dónde está el Attractor en la simulación y cómo cambiar su color?
El Attractor es un objeto en la simulación que atrae a otros objetos con una fuerza similar a la gravedad. Normalmente está representado como un círculo en el centro de la pantalla.

Para cambiar su color, simplemente hay que modificar su apariencia en la función que lo dibuja en la pantalla. Por ejemplo, podríamos hacer que en lugar de su color original, sea rojo, azul o cualquier otro color.

### 4. ¿Cómo modificar el código para mover el Attractor con el mouse y cambiar su color cuando el cursor está sobre él?
Para lograr esto, se pueden hacer dos modificaciones:

- Detectar si el mouse está sobre el Attractor:

Se calcula la distancia entre el cursor y el centro del Attractor.
Si la distancia es menor que su radio, significa que el mouse está sobre él.

- Permitir mover el Attractor con el mouse:

Cuando Se hace clic sobre él, se activa un estado que indica que lo estamos arrastrando.
Mientras mantengamos presionado el botón del mouse, el Attractor se moverá junto con el cursor.
Cuando soltamos el mouse, el Attractor deja de seguir el cursor.

- Cambiar su color cuando el mouse está sobre él:

Si el cursor está cerca, el color del Attractor cambia (por ejemplo, a verde).
Cuando el cursor se aleja, vuelve a su color original.
