## Proceso generativo (Algoritmo)
El sistema se basa en un conjunto de partículas autónomas que se mueven influenciadas por un flow field dinámico. Este campo se modifica constantemente a partir de las propiedades del audio y la interacción del usuario. Se implementan técnicas como el ruido Perlin, modulación de fuerzas y aleatoriedad controlada para lograr una visualización fluida, impredecible y sensible al sonido.

Técnicas aplicadas:
Flow field influenciado por ruido Perlin, donde la dirección de los vectores se altera según la información del FFT (frecuencias graves y agudas).

Sistema de partículas que se dispersan y atraen según la amplitud.

Uso de aleatoriedad controlada para condiciones iniciales: posiciones, formas y colores únicos por ejecución.

Conexiones visuales temporales entre partículas para generar texturas y patrones.

Inputs y su efecto:
Amplitud: controla la cantidad de partículas y la fuerza de dispersión. A mayor volumen, mayor expansión visual.

FFT:

Graves: alteran la rotación y dirección del flow field.

Agudos: incrementan la velocidad de las partículas y generan conexiones efímeras entre ellas.

MouseX: cambia la paleta de colores y el comportamiento del campo de flujo.

MouseY: ajusta la opacidad de los elementos visuales.

Teclas 1-5: alternan modos de visualización (puntos, líneas, manchas, redes, espirales).

## Naturaleza generativa
La visualización está pensada para que nunca se repita exactamente igual, incluso con la misma música. Esto se logra mediante:

Aleatoriedad controlada en las condiciones iniciales.

Dinamismo constante del flow field modulado por el audio.

Interacción activa del usuario, que altera parámetros visuales en tiempo real.

El comportamiento impredecible, pero coherente, da como resultado una obra generativa genuina, viva y en constante evolución.

## Outputs visuales
Elementos visuales generados:
Partículas que se desplazan suavemente, con formas variables.

Conexiones o líneas que emergen temporalmente entre partículas cercanas cuando los agudos están activos.

Manchas, texturas o espirales según el modo seleccionado por el usuario.

Propiedades dinámicas:
Movimiento: guiado por el flow field y modulado por el FFT.

Tamaño: responde a la amplitud.

Color: influenciado por la posición del mouse.

Opacidad: cambia con MouseY y momentos de menor volumen.

Interconexión: aparece con mayor energía en las frecuencias agudas.

