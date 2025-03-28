## 1. Gestión de creación, desaparición y memoria de partículas
Las partículas se crean dinámicamente dentro de un límite de cantidad (máximo 200 en mi última versión).

Se eliminan de la lista cuando su tiempo de vida llega a cero (lifespan < 0), evitando acumulación innecesaria y mejorando el rendimiento.

La memoria se gestiona asegurando que solo las partículas activas están en el array, usando splice() para remover las inactivas.

## 2. Aplicación del marco Motion 101
Movimiento básico: Cada partícula tiene posición, velocidad y aceleración.

Actualización de estado: La velocidad se suma a la posición en cada update().

Aplicación de fuerzas: Factores como el sonido afectan la aceleración y la velocidad.

## 3. Aplicación de fuerzas externas
Gravedad simulada: La aceleración negativa en y hace que las partículas suban y luego caigan.

Ruido Perlin: Usado en WaveParticle para agregar oscilaciones orgánicas.

Interacción con el micrófono: La fuerza del sonido afecta tamaño, velocidad y color de las partículas.

## 4. Uso de herencia y polimorfismo
Herencia: WaveParticle y ReactiveParticle heredan de Particle, reutilizando código base.

Polimorfismo: Cada subclase redefine applySound() y update(), modificando su comportamiento sin cambiar la estructura base.
