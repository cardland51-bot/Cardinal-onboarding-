<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Fine Sculpting Game</title>
<style>
  body { 
    margin: 0; 
    background: #111; 
    display: flex; 
    justify-content: center; 
    align-items: center; 
    height: 100vh; 
    flex-direction: column; 
    color: #0f0; 
    font-family: monospace;
  }
  canvas { 
    background: #222; 
    touch-action: none; 
    border-radius: 12px; 
    margin-top: 10px;
  }
  #controls {
    display: flex;
    justify-content: center;
    gap: 10px;
    align-items: center;
  }
  button {
    background: #0f0;
    color: #000;
    border: none;
    padding: 10px 20px;
    border-radius: 8px;
    font-size: 16px;
    cursor: pointer;
    font-weight: bold;
  }
  button:active {
    transform: scale(0.98);
  }
  #timer, #score {
    font-size: 18px;
  }
</style>
</head>
<body>
<div id="controls">
  <button id="resetBtn">RESET</button>
  <button id="modeBtn">FREE SCULPT: OFF</button>
  <span id="timer">Time: 25s</span>
  <span id="score">Score: 0</span>
</div>
<canvas id="canvas" width="400" height="400"></canvas>

<script>
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
const resetBtn = document.getElementById('resetBtn');
const modeBtn = document.getElementById('modeBtn');
const timerDisplay = document.getElementById('timer');
const scoreDisplay = document.getElementById('score');

const centerX = canvas.width / 2;
const centerY = canvas.height / 2;

let shapeRadius = 80;
let numPoints = 80;
let shapePoints = [];
let rotation = 0;
let isTouching = false;
let fingerX = 0;
let fingerY = 0;
let score = 0;
let timeLeft = 25;
let timerActive = true;
let freeMode = false;

function initShape() {
  shapePoints = [];
  for (let i = 0; i < numPoints; i++) {
    let angle = (i / numPoints) * Math.PI * 2;
    let r = shapeRadius + Math.random() * 20;
    shapePoints.push({ angle: angle, radius: r });
  }
  score = 0;
  timeLeft = 25;
  timerActive = !freeMode;
}

initShape();

// Touch controls
canvas.addEventListener('pointerdown', (e) => {
  isTouching = true;
  const rect = canvas.getBoundingClientRect();
  fingerX = e.clientX - rect.left;
  fingerY = e.clientY - rect.top;
});

canvas.addEventListener('pointermove', (e) => {
  if (isTouching) {
    const rect = canvas.getBoundingClientRect();
    fingerX = e.clientX - rect.left;
    fingerY = e.clientY - rect.top;
  }
});

canvas.addEventListener('pointerup', () => {
  isTouching = false;
});

resetBtn.addEventListener('click', () => {
  initShape();
});

modeBtn.addEventListener('click', () => {
  freeMode = !freeMode;
  modeBtn.textContent = `FREE SCULPT: ${freeMode ? 'ON' : 'OFF'}`;
  initShape();
});

// Timer countdown
function startTimer() {
  setInterval(() => {
    if (timerActive && !freeMode && timeLeft > 0) {
      timeLeft--;
      timerDisplay.textContent = `Time: ${timeLeft}s`;
      if (timeLeft === 0) {
        timerActive = false;
      }
    }
  }, 1000);
}
startTimer();

// Calculate how circular the shape is
function calculateScore() {
  let avg = shapePoints.reduce((a, b) => a + b.radius, 0) / shapePoints.length;
  let variance = shapePoints.reduce((a, b) => a + Math.pow(b.radius - avg, 2), 0) / shapePoints.length;
  let circularity = Math.max(0, 1000 / (variance + 1));
  if (timerActive || freeMode) score += circularity * 0.001;
  scoreDisplay.textContent = `Score: ${score.toFixed(1)}`;
}

// Draw loop
function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  rotation += 0.01;

  // Draw rotating shape
  ctx.beginPath();
  for (let i = 0; i < numPoints; i++) {
    let p = shapePoints[i];
    let x = centerX + Math.cos(p.angle + rotation) * p.radius;
    let y = centerY + Math.sin(p.angle + rotation) * p.radius;
    if (i === 0) ctx.moveTo(x, y);
    else ctx.lineTo(x, y);
  }
  ctx.closePath();
  ctx.fillStyle = '#555';
  ctx.fill();
  ctx.strokeStyle = '#fff';
  ctx.lineWidth = 1.5;
  ctx.stroke();

  // Draw blade slightly above finger
  if (isTouching) {
    let bladeX = fingerX;
    let bladeY = fingerY - 30;
    ctx.beginPath();
    ctx.moveTo(bladeX, bladeY);
    ctx.lineTo(bladeX, bladeY - 60);
    ctx.strokeStyle = '#0f0';
    ctx.lineWidth = 4;
    ctx.stroke();

    // Sculpt shape using tip
    let tipX = bladeX;
    let tipY = bladeY - 60;
    for (let i = 0; i < numPoints; i++) {
      let p = shapePoints[i];
      let x = centerX + Math.cos(p.angle + rotation) * p.radius;
      let y = centerY + Math.sin(p.angle + rotation) * p.radius;
      let dx = tipX - x;
      let dy = tipY - y;
      let dist = Math.sqrt(dx * dx + dy * dy);
      if (dist < 15) {
        p.radius -= 0.6;
        if (p.radius < 10) p.radius = 10;
      }
    }
  }

  calculateScore();
  requestAnimationFrame(draw);
}

draw();
</script>
</body>
</html>
