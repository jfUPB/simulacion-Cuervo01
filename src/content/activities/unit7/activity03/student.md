let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies,
    Body = Matter.Body,
    Mouse = Matter.Mouse,
    MouseConstraint = Matter.MouseConstraint;

let engine;
let world;
let letters = [];
let ground;
let mConstraint;
let letterIndex = 0;

let prevMouseX = 0;
let prevMouseY = 0;
let particles = [];
let bgTime = 0;

function setup() {
  createCanvas(800, 600);
  engine = Engine.create();
  world = engine.world;
  engine.world.gravity.y = 1;

  ground = Bodies.rectangle(width / 2, height - 10, width, 20, { isStatic: true });
  World.add(world, ground);

  let canvasMouse = Mouse.create(document.body);
  canvasMouse.pixelRatio = pixelDensity();
  let options = {
    mouse: canvasMouse
  };
  mConstraint = MouseConstraint.create(engine, options);
  World.add(world, mConstraint);
}

function mousePressed() {
  let word = "CAER";
  let positions = [200, 300, 400, 500];

  let x = positions[int(random(positions.length))];
  let letter = Bodies.rectangle(x, 0, 50, 50, {
    restitution: 0.9,
    frictionAir: 0.02
  });
  letter.text = word[letterIndex];
  World.add(world, letter);
  letters.push(letter);

  // Sparkle particles
  for (let i = 0; i < 25; i++) {
    particles.push({
      x: x,
      y: 0,
      vx: random(-2, 2),
      vy: random(-2, 2),
      alpha: 255,
      size: random(2, 5),
      hue: random(180, 360)
    });
  }

  letterIndex = (letterIndex + 1) % word.length;
}

function drawNebulaBackground() {
  bgTime += 0.01;
  for (let y = 0; y < height; y += 2) {
    let inter = map(y, 0, height, 0, 1);
    let r = 30 + 20 * sin(bgTime + y * 0.01);
    let g = 20 + 40 * cos(bgTime + y * 0.015);
    let b = 60 + 30 * sin(bgTime + y * 0.02);
    stroke(r, g, b, 180);
    line(0, y, width, y);
  }
}

function draw() {
  drawNebulaBackground();
  Engine.update(engine);

  let dx = mouseX - prevMouseX;
  let dy = mouseY - prevMouseY;
  let speed = sqrt(dx * dx + dy * dy);

  if (speed > 15) {
    for (let letter of letters) {
      let fx = random(-0.02, 0.02);
      let fy = random(-0.02, 0.02);
      Body.applyForce(letter, letter.position, { x: fx, y: fy });
    }
  }

  // Draw sparkle particles
  noStroke();
  for (let p of particles) {
    fill(`hsla(${p.hue}, 100%, 75%, ${p.alpha / 255})`);
    ellipse(p.x, p.y, p.size);
    p.x += p.vx;
    p.y += p.vy;
    p.alpha -= 4;
  }
  particles = particles.filter(p => p.alpha > 0);

  // Draw letters
  for (let letter of letters) {
    push();
    translate(letter.position.x, letter.position.y);
    rotate(letter.angle);
    textAlign(CENTER, CENTER);
    let pulse = sin(frameCount * 0.2 + letter.position.x) * 5;
    textSize(48 + pulse);
    fill(255);
    stroke(255, 100, 255);
    strokeWeight(3);
    drawingContext.shadowColor = 'rgba(255, 150, 255, 0.8)';
    drawingContext.shadowBlur = 25;
    text(letter.text, 0, 0);
    pop();
  }

  // Ground
  noStroke();
  fill(50, 50, 80, 100);
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, width, 20);

  prevMouseX = mouseX;
  prevMouseY = mouseY;
}
