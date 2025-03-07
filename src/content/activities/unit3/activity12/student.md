## Desafíos Encontrados

- **Modelado de las interacciones gravitatorias:** Implementar la interacción entre los cuerpos fue un reto, ya que la fuerza gravitatoria es inversamente proporcional al cuadrado de la distancia. Fue necesario ajustar los cálculos para evitar movimientos demasiado bruscos o colisiones inesperadas.

- **Persistencia de los cuerpos en pantalla:** En un inicio, los cuerpos desaparecían después de un tiempo. Para solucionar esto, implementé rebotes en los bordes de la pantalla, asegurando que los cuerpos siempre permanecieran visibles y en movimiento.

- **Interactividad con el usuario:** Diseñar una respuesta interactiva a la acción del usuario, como la repulsión al hacer clic, requirió ajustes en la dirección y magnitud de la fuerza aplicada para que el efecto fuera intuitivo y fluido.

- **Representación visual abstracta:** Inspirarse en las esculturas cinéticas de Alexander Calder y traducirlas a una simulación en p5.js fue un desafío creativo. Opté por líneas de colores cambiantes que conectan los cuerpos según su proximidad, creando una composición visual dinámica y en constante evolución.

## Lecciones Aprendidas

- **Importancia del ajuste de parámetros:** Experimentar con diferentes valores de fuerzas, velocidades y umbrales de conexión fue clave para lograr un comportamiento natural y equilibrado en la simulación.

- **Depuración a través de pruebas iterativas:** Realizar pruebas constantes me permitió detectar y corregir problemas como la desaparición de los cuerpos y la falta de fluidez en los movimientos.

- **Mejora de la experiencia del usuario:** Agregar elementos interactivos hizo que la simulación fuera más atractiva. Incorporar una respuesta visual y dinámica a la acción del usuario enriquecí la experiencia.

## Ejemplos Concretos

Problema: Los cuerpos desaparecían después de un tiempo.

Solución: Se agregaron rebotes en los bordes y restricciones en la posición para evitar que salieran del lienzo.

Problema: Las líneas de conexión eran poco visibles y a veces no aparecían.

Solución: Se aumentó el rango de conexión y se aplicaron colores variables para mejorar la visibilidad.
