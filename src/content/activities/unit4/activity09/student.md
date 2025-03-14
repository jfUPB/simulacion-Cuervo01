### Código:
```javascript


let mass1 = 20; // Masa del primer objeto
let mass2 = 20; // Masa del segundo objeto
let k1 = 0.2; // Constante del resorte 1
let k2 = 0.2; // Constante del resorte 2
let restLength1 = 100; // Longitud en reposo del resorte 1
let restLength2 = 100; // Longitud en reposo del resorte 2
let pos1, pos2; // Posiciones de las masas
let vel1 = 0, vel2 = 0; // Velocidades de las masas (inicialmente pequeñas)
let accel1 = 0, accel2 = 0; // Aceleraciones de las masas
let damping = 0.98; // Factor de amortiguación para reducir la velocidad

function setup() {
  createCanvas(800, 400);
  pos1 = createVector(width / 2, height / 2 - restLength1);
  pos2 = createVector(width / 2, height / 2);
}

function draw() {
  background(255);

  // Calcular la fuerza del resorte 1
  let force1 = -k1 * (pos1.y - (height / 2));
  accel1 = force1 / mass1;
  vel1 += accel1;
  vel1 *= damping; // Aplicar amortiguación
  pos1.y += vel1;

  // Calcular la fuerza del resorte 2
  let force2 = -k2 * (pos2.y - pos1.y - restLength2);
  accel2 = force2 / mass2;
  vel2 += accel2;
  vel2 *= damping; // Aplicar amortiguación
  pos2.y += vel2;

  // Dibujar el sistema
  stroke(0);
  strokeWeight(2);

  // Resorte 1
  line(width / 2, height / 2, pos1.x, pos1.y);
  fill(127);
  ellipse(pos1.x, pos1.y, 20, 20);

  // Resorte 2
  line(pos1.x, pos1.y, pos2.x, pos2.y);
  fill(127);
  ellipse(pos2.x, pos2.y, 20, 20);
}
```

### Link: 

https://editor.p5js.org/Cuervo01/sketches/WxUFkc6Xa

### Captura: 

https://github.com/user-attachments/assets/3a4c7f5c-32cc-4a15-98f5-1cbfad1079b3



