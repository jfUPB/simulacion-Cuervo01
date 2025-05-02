### Cambios realizados:

No fue posible hacerlo con la letra R, por lo que se hizo con el micrófono. Si capta un sonido fuerte, se dispersan las partículas 

### Código:

```javascript
let particles = [];
let attractor;
let noiseOffset = 0;
let audioContext, analyser, dataArray;
let noiseThreshold = 0.5; // Umbral para ignorar ruido ambiente

function setup() {
  createCanvas(windowWidth, windowHeight);
  for (let i = 0; i < 200; i++) {
    particles.push(new Particle(random(width), random(height)));
  }
  attractor = createVector(width / 2, height / 2);
  
  // Configuración del micrófono
  navigator.mediaDevices.getUserMedia({ audio: true })
    .then(stream => {
      audioContext = new (window.AudioContext || window.webkitAudioContext)();
      let source = audioContext.createMediaStreamSource(stream);
      analyser = audioContext.createAnalyser();
      analyser.fftSize = 256;
      source.connect(analyser);
      dataArray = new Uint8Array(analyser.frequencyBinCount);
    })
    .catch(err => {
      console.error('Error al acceder al micrófono', err);
    });
}

function draw() {
  background(0, 20);
  
  attractor.set(mouseX, mouseY);
  
  let disperseForce = 0;
  if (analyser) {
    analyser.getByteFrequencyData(dataArray);
    let maxVolume = Math.max(...dataArray) / 256;
    
    if (maxVolume > noiseThreshold) { // Ignorar ruido ambiente
      disperseForce = (maxVolume - noiseThreshold) * 50;
    }
  }
  
  for (let p of particles) {
    p.applyForces(disperseForce);
    p.update();
    p.show();
  }
}

class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(1, 3));
    this.acc = createVector(0, 0);
    this.maxSpeed = 5;
  }
  
  applyForces(disperseForce) {
    let noiseForce = createVector(noise(noiseOffset) * 2 - 1, noise(noiseOffset + 100) * 2 - 1);
    noiseForce.mult(0.5);
    noiseOffset += 0.01;
    
    let attraction = p5.Vector.sub(attractor, this.pos);
    let distance = p5.Vector.dist(attractor, this.pos);
    attraction.setMag(map(distance, 0, width, 5, 0.1));
    
    let randomAccel = p5.Vector.random2D().mult(0.2);
    
    this.acc.add(noiseForce);
    this.acc.add(attraction);
    this.acc.add(randomAccel);
    
    if (disperseForce > 0) {
      let disperseDir = p5.Vector.random2D().mult(disperseForce);
      this.vel.add(disperseDir);
    }
  }
  
  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }
  
  show() {
    noStroke();
    fill(255, 150);
    ellipse(this.pos.x, this.pos.y, 5);
  }
}
```

### Captura 


https://github.com/user-attachments/assets/9c6b3cea-7828-4cca-831b-1955b573c5d7

