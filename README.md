<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Fine Sculpting Game - Levels</title>
<style>
  body { 
    margin: 0; 
    background: #111; 
    display: flex; 
    justify-content: center; 
    align-items: center; 
    height: 100vh; 
    overflow: hidden;
    color: white;
    font-family: sans-serif;
  }
  canvas { 
    background: #222; 
    touch-action: none;
    border: 2px solid #333;
    border-radius: 12px;
  }
  #hud {
    position: absolute;
    top: 10px;
    width: 100%;
    text-align: center;
    z-index: 10;
  }
  #score, #timer, #level {
    font-size: 1.3em;
    margin: 5px;
    display: inline-block;
  }
  #resetBtn {
    display: block;
    margin: 10px auto;
    padding: 6px 12px;
    font-size: 1em;
    cursor: pointer;
  }
</style>
</head>
<body>
<div id="hud">
  <span id="level">Level: 1</span>
  <span id="timer">25s</span>
  <span id="score">Score: 0</span>
  <button id="resetBtn">Reset Level</button>
</div>
<canvas id="canvas" width="400" height="400"></canvas>

<script>
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
const timerDisplay = document.getElementById('timer');
const scoreDisplay = document.getElementById('score');
const levelDisplay = document.getElementById('level');
const resetBtn = document.getElementById('resetBtn');

const centerX = canvas.width/2;
const centerY = canvas.height/2;

let baseShapeRadius = 80;
let numPoints = 80;
let shapePoints = [];
let baseRotationSpeed = 0.02;
let rotation = 0;
let isTouching = false;
let fingerX = 0;
let fingerY = 0;

let score = 0;
let totalScore = 0;
let timeLeft = 25;
let timerStarted = false;
let level = 1;
let pulseAlpha = 0;

let levelRingRadius = 90; // visible ring behind shape

// Particles
let particles = [];

function initShape(level) {
    shapePoints = [];
    for(let i=0;i<numPoints;i++){
        let angle = (i/numPoints)*Math.PI*2;
        let r = baseShapeRadius + Math.random()*30; 
        shapePoints.push({angle: angle, radius: r});
    }
}

function resetLevel() {
    rotation = 0;
    score = 0;
    timeLeft = 25;
    timerStarted = false;
    pulseAlpha = 0;
    levelRingRadius = 90; // keep ring size fixed at start
    initShape(level);
    timerDisplay.textContent = timeLeft + "s";
    scoreDisplay.textContent = "Score: " + totalScore;
}

initShape(level);

// Pointer events
canvas.addEventListener('pointerdown', (e)=>{
    isTouching = true;
    const rect = canvas.getBoundingClientRect();
    fingerX = e.clientX - rect.left;
    fingerY = e.clientY - rect.top;
    if(!timerStarted) startTimer();
});

canvas.addEventListener('pointermove', (e)=>{
    if(isTouching){
        const rect = canvas.getBoundingClientRect();
        fingerX = e.clientX - rect.left;
        fingerY = e.clientY - rect.top;
    }
});

canvas.addEventListener('pointerup', ()=>{ isTouching = false; });

resetBtn.addEventListener('click', ()=>{
    resetLevel();
});

// Timer
function startTimer(){
    timerStarted = true;
    const countdown = setInterval(()=>{
        if(timeLeft <= 0){
            clearInterval(countdown);
            timerDisplay.textContent = "Done";
            isTouching = false;
        } else {
            timeLeft--;
            timerDisplay.textContent = timeLeft + "s";
        }
    },1000);
}

// Circularity score (1+)
function circularityScore(){
    let avg = shapePoints.reduce((a,b)=>a+b.radius,0)/numPoints;
    let variance = shapePoints.reduce((a,b)=>a+Math.abs(b.radius-avg),0)/numPoints;
    let rawScore = Math.max(1, Math.floor(1000 - variance*15));
    return rawScore;
}

// Particle system
function spawnParticle(x,y){
    particles.push({
        x: x,
        y: y,
        vx: (Math.random()-0.5)*1,
        vy: (Math.random()-0.5)*1,
        life: 20 + Math.random()*10
    });
}

function updateParticles(){
    particles.forEach((p,i)=>{
        p.x += p.vx;
        p.y += p.vy;
        p.life--;
        if(p.life <=0) particles.splice(i,1);
    });
}

function drawParticles(){
    ctx.fillStyle = 'rgba(0,255,255,0.5)';
    particles.forEach(p=>{
        ctx.beginPath();
        ctx.arc(p.x,p.y,2,0,Math.PI*2);
        ctx.fill();
    });
}

function draw(){
    ctx.clearRect(0,0,canvas.width,canvas.height);
    
    // Draw level ring
    ctx.beginPath();
    ctx.arc(centerX,centerY,levelRingRadius,0,Math.PI*2);
    ctx.strokeStyle = 'rgba(255,255,255,0.3)';
    ctx.lineWidth = 2;
    ctx.stroke();

    // Dynamic rotation slowdown
    let rotationSpeed = baseRotationSpeed;
    if(isTouching && timeLeft>0){
        let minDist = Infinity;
        let tipX, tipY;
        let bladeLength = 60;
        let bladeAngle = -Math.PI/12; // start tilt ~15 degrees
        let bladeX1 = fingerX;
        let bladeY1 = fingerY - 40;
        let bladeX2 = bladeX1 + Math.cos(bladeAngle)*bladeLength;
        let bladeY2 = bladeY1 + Math.sin(bladeAngle)*bladeLength;
        tipX = bladeX2;
        tipY = bladeY2;
        for(let i=0;i<numPoints;i++){
            let p = shapePoints[i];
            let x = centerX + Math.cos(p.angle + rotation)*p.radius;
            let y = centerY + Math.sin(p.angle + rotation)*p.radius;
            let dx = tipX - x;
            let dy = tipY - y;
            let dist = Math.sqrt(dx*dx + dy*dy);
            if(dist < minDist) minDist = dist;
        }
        if(minDist < 15) rotationSpeed = baseRotationSpeed * (minDist/15);
    }

    rotation += rotationSpeed;

    // Draw shape
    ctx.beginPath();
    for(let i=0;i<numPoints;i++){
        let p = shapePoints[i];
        let x = centerX + Math.cos(p.angle + rotation)*p.radius;
        let y = centerY + Math.sin(p.angle + rotation)*p.radius;
        if(i===0) ctx.moveTo(x,y);
        else ctx.lineTo(x,y);
    }
    ctx.closePath();

    // Glow pulse effect when near perfect
    if(score > 950){
        pulseAlpha = Math.min(0.7, pulseAlpha + 0.02);
        ctx.shadowColor = 'rgba(0,255,255,'+pulseAlpha+')';
        ctx.shadowBlur = 20;
    } else {
        pulseAlpha = 0;
        ctx.shadowBlur = 0;
    }

    ctx.fillStyle = 'rgba(85,85,85,0.8)'; // translucent
    ctx.fill();
    ctx.strokeStyle = '#fff';
    ctx.lineWidth = 2;
    ctx.stroke();
    ctx.shadowBlur = 0;

    // Blade and carving
    if(isTouching && timeLeft>0){
        let bladeLength = 60;
        let bladeAngle = -Math.PI/12; // tilt
        let bladeX1 = fingerX;
        let bladeY1 = fingerY - 40;
        let bladeX2 = bladeX1 + Math.cos(bladeAngle)*bladeLength;
        let bladeY2 = bladeY1 + Math.sin(bladeAngle)*bladeLength;

        ctx.beginPath();
        ctx.moveTo(bladeX1, bladeY1);
        ctx.lineTo(bladeX2, bladeY2);
        ctx.strokeStyle = '#0f0';
        ctx.lineWidth = 4;
        ctx.stroke();

        let tipX = bladeX2;
        let tipY = bladeY2;
        for(let i=0;i<numPoints;i++){
            let p = shapePoints[i];
            let x = centerX + Math.cos(p.angle + rotation)*p.radius;
            let y = centerY + Math.sin(p.angle + rotation)*p.radius;
            let dx = tipX - x;
            let dy = tipY - y;
            let dist = Math.sqrt(dx*dx + dy*dy);
            if(dist < 8){ // fine tip
                p.radius -= 0.5;
                if(p.radius < 10) p.radius = 10;
                spawnParticle(tipX,tipY);
            }
        }

        score = circularityScore();
        totalScore += Math.floor(score/100); // accumulative score beyond 1000
        scoreDisplay.textContent = "Score: " + totalScore;
    }

    updateParticles();
    drawParticles();

    // Level up if shape fits within level ring
    let shapeMaxRadius = Math.max(...shapePoints.map(p=>p.radius));
    if(shapeMaxRadius <= levelRingRadius){
        level++;
        levelDisplay.textContent = "Level: " + level;
        levelRingRadius = Math.max(30, levelRingRadius - 5); // shrink ring
        resetLevel();
    }

    requestAnimationFrame(draw);
}

draw();
</script>
</body>
</html>
