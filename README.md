<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Fine Sculpting Game</title>
<style>
  body { margin: 0; background: #111; display: flex; justify-content: center; align-items: center; height: 100vh; }
  canvas { background: #222; touch-action: none; }
</style>
</head>
<body>
<canvas id="canvas" width="400" height="400"></canvas>
<script>
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

const centerX = canvas.width/2;
const centerY = canvas.height/2;

let shapeRadius = 80;
let numPoints = 80;
let shapePoints = [];

// Create initial irregular shape
for(let i=0;i<numPoints;i++){
  let angle = (i/numPoints)*Math.PI*2;
  let r = shapeRadius + Math.random()*20;
  shapePoints.push({angle: angle, radius: r});
}

let rotation = 0;
let isTouching = false;
let fingerX = 0;
let fingerY = 0;

canvas.addEventListener('pointerdown', (e)=>{
    isTouching = true;
    const rect = canvas.getBoundingClientRect();
    fingerX = e.clientX - rect.left;
    fingerY = e.clientY - rect.top;
});

canvas.addEventListener('pointermove', (e)=>{
    if(isTouching){
        const rect = canvas.getBoundingClientRect();
        fingerX = e.clientX - rect.left;
        fingerY = e.clientY - rect.top;
    }
});

canvas.addEventListener('pointerup', ()=>{
    isTouching = false;
});

// Main loop
function draw(){
    ctx.clearRect(0,0,canvas.width,canvas.height);

    // Update rotation
    rotation += 0.01;

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
    ctx.fillStyle = '#555';
    ctx.fill();
    ctx.strokeStyle = '#fff';
    ctx.stroke();

    // Draw blade slightly above finger
    if(isTouching){
        let bladeX = fingerX;
        let bladeY = fingerY - 30; // blade floats 30px above finger
        ctx.beginPath();
        ctx.moveTo(bladeX, bladeY);
        ctx.lineTo(bladeX, bladeY - 60); // extend blade upward
        ctx.strokeStyle = '#0f0';
        ctx.lineWidth = 4;
        ctx.stroke();

        // Sculpt shape using only tip
        let tipX = bladeX;
        let tipY = bladeY - 60;
        for(let i=0;i<numPoints;i++){
            let p = shapePoints[i];
            let x = centerX + Math.cos(p.angle + rotation)*p.radius;
            let y = centerY + Math.sin(p.angle + rotation)*p.radius;
            let dx = tipX - x;
            let dy = tipY - y;
            let dist = Math.sqrt(dx*dx + dy*dy);
            if(dist < 15){ // small radius for fine sculpt
                p.radius -= 1; // shave off a little
                if(p.radius < 10) p.radius = 10; // don't collapse completely
            }
        }
    }

    requestAnimationFrame(draw);
}

draw();
</script>
</body>
</html>
