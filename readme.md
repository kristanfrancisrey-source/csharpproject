<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Endless Lane Runner</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    background: #222;
    color: white;
    font-family: Arial, sans-serif;
    text-align: center;
}

h1 {
    margin: 15px 0;
}

.info {
    display: flex;
    justify-content: center;
    gap: 30px;
    font-size: 18px;
    margin-bottom: 10px;
}

canvas {
    width: 95%;
    max-width: 400px;
    height: auto;
    background: #444;
    border: 3px solid white;
    touch-action: none;
}

.controls {
    margin: 12px;
}

button {
    padding: 14px 22px;
    margin: 5px;
    border: none;
    border-radius: 8px;
    background: #2196F3;
    color: white;
    font-size: 17px;
    font-weight: bold;
}

button:active {
    background: #0b65a5;
}

#start {
    background: #28a745;
}

#restart {
    background: #e53935;
}

.description {
    max-width: 400px;
    margin: 15px auto;
    padding: 10px;
    font-size: 14px;
}
</style>
</head>

<body>

<h1>ENDLESS LANE RUNNER</h1>

<div class="info">
    <span>Time: <b id="time">45</b>s</span>
    <span>Score: <b id="score">0</b></span>
</div>

<canvas id="game" width="400" height="550"></canvas>

<div class="controls">
    <button onclick="move(-1)">LEFT</button>
    <button onclick="jump()">JUMP</button>
    <button onclick="move(1)">RIGHT</button>
</div>

<div>
    <button id="start" onclick="startGame()">START GAME</button>
    <button id="restart" onclick="restartGame()">RESTART</button>
</div>

<div class="description">
    <p>Swipe left or right to change lanes. Jump over obstacles and collect gold coins.</p>
    <p>Get the highest score before the 45-second timer runs out!</p>
</div>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

let playerLane = 1;
let playerY = 450;
let jumpHeight = 0;
let jumping = false;

let score = 0;
let time = 45;
let objects = [];

let running = false;
let gameOver = false;

let startTime = 0;
let lastSpawn = 0;
let speed = 5;

function move(direction) {
    if (!running) return;

    playerLane = Math.max(
        0,
        Math.min(2, playerLane + direction)
    );
}

function jump() {
    if (running && !jumping) {
        jumping = true;
        jumpHeight = 110;
    }
}

function spawnObject() {
    let lane = Math.floor(Math.random() * 3);
    let coin = Math.random() < 0.4;

    objects.push({
        lane: lane,
        y: -30,
        coin: coin,
        collected: false
    });
}

function drawGame() {
    ctx.clearRect(0, 0, 400, 550);

    // Road
    ctx.fillStyle = "#333";
    ctx.fillRect(50, 0, 300, 550);

    // Lane lines
    ctx.strokeStyle = "white";
    ctx.lineWidth = 4;
    ctx.setLineDash([25, 20]);

    ctx.beginPath();
    ctx.moveTo(150, 0);
    ctx.lineTo(150, 550);
    ctx.moveTo(250, 0);
    ctx.lineTo(250, 550);
    ctx.stroke();

    ctx.setLineDash([]);

    // Player
    let x = 100 + playerLane * 100;

    ctx.fillStyle = "#2196F3";
    ctx.fillRect(
        x - 18,
        playerY - jumpHeight,
        36,
        50
    );

    // Objects
    objects.forEach(obj => {
        let ox = 100 + obj.lane * 100;

        if (obj.coin && !obj.collected) {
            ctx.fillStyle = "gold";
            ctx.beginPath();
            ctx.arc(ox, obj.y, 12, 0, Math.PI * 2);
            ctx.fill();
        } else if (!obj.coin) {
            ctx.fillStyle = "orange";
            ctx.fillRect(ox - 18, obj.y, 36, 35);
        }
    });

    // Start screen
    if (!running && !gameOver) {
        ctx.fillStyle = "white";
        ctx.font = "24px Arial";
        ctx.fillText("ENDLESS RUNNER", 90, 250);
        ctx.font = "17px Arial";
        ctx.fillText("Press START GAME", 115, 285);
    }
}

function update() {
    if (!running) return;

    let elapsed = Math.floor(
        (Date.now() - startTime) / 1000
    );

    time = Math.max(0, 45 - elapsed);

    document.getElementById("time").textContent = time;

    if (time <= 0) {
        endGame();
        return;
    }

    // Increase difficulty over time
    speed = 5 + Math.floor((45 - time) / 10);

    let spawnDelay = Math.max(400, 900 - (45 - time) * 10);

    if (Date.now() - lastSpawn > spawnDelay) {
        spawnObject();
        lastSpawn = Date.now();
    }

    objects.forEach(obj => {
        obj.y += speed;

        if (
            obj.lane === playerLane &&
            obj.y > playerY - 20 &&
            obj.y < playerY + 40
        ) {
            if (obj.coin && !obj.collected) {
                score += 10;
                obj.collected = true;
            } 
            else if (!obj.coin && !jumping) {
                endGame();
            }
        }
    });

    objects = objects.filter(obj => obj.y < 570);

    // Jump animation
    if (jumping) {
        jumpHeight -= 7;

        if (jumpHeight <= 0) {
            jumpHeight = 0;
            jumping = f
