
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Coin Collection Game</title>
<style>
* { box-sizing: border-box; }
body {
    margin: 0;
    font-family: Arial, sans-serif;
    text-align: center;
    background: #172b3a;
    color: white;
}
h1 { color: gold; }
#info {
    display: flex;
    justify-content: center;
    gap: 30px;
    font-size: 20px;
    margin: 10px;
}
#game {
    position: relative;
    width: 330px;
    height: 450px;
    margin: 15px auto;
    overflow: hidden;
    background: #354b43;
    border: 5px solid white;
    border-radius: 12px;
}
.lane {
    position: absolute;
    height: 100%;
    width: 33.33%;
    border-right: 2px dashed #ddd;
}
.lane:nth-child(1) { left: 0; }
.lane:nth-child(2) { left: 33.33%; }
.lane:nth-child(3) { left: 66.66%; border: none; }

#player {
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    font-size: 38px;
    z-index: 3;
}
.item {
    position: absolute;
    font-size: 30px;
    z-index: 2;
}
#message {
    position: absolute;
    top: 40%;
    width: 100%;
    font-size: 22px;
    font-weight: bold;
    z-index: 5;
}
button {
    padding: 12px 22px;
    margin: 5px;
    border: none;
    border-radius: 8px;
    background: gold;
    color: #222;
    font-weight: bold;
    cursor: pointer;
}
button:hover { background: orange; }
.controls { margin: 10px; }
</style>
</head>
<body>

<h1>🪙 COIN RUNNER</h1>

<div id="info">
    <span>Score: <b id="score">0</b></span>
    <span>Time: <b id="time">45</b>s</span>
</div>

<div id="game">
    <div class="lane"></div>
    <div class="lane"></div>
    <div class="lane"></div>
    <div id="player">🏃</div>
    <div id="message">Collect coins and avoid obstacles!</div>
</div>

<button onclick="startGame()">START GAME</button>
<button onclick="restartGame()">RESTART</button>

<div class="controls">
    <p>⬅ LEFT | RIGHT ➡ | SPACE = JUMP</p>
    <p>🪙 Coin = 10 Points | ⏱ 45 Seconds</p>
</div>

<script>
const game = document.getElementById("game");
const player = document.getElementById("player");
const message = document.getElementById("message");

let lane = 1;
let score = 0;
let time = 45;
let speed = 3;
let jumping = false;
let running = false;
let items = [];
let timer, gameLoop, spawnLoop;

function updatePlayer() {
    player.style.left = (lane * 33.33 + 16.66) + "%";
}

function moveLeft() {
    if (running && lane > 0) {
        lane--;
        updatePlayer();
    }
}

function moveRight() {
    if (running && lane < 2) {
        lane++;
        updatePlayer();
    }
}

function jump() {
    if (!running || jumping) return;
    jumping = true;
    player.style.bottom = "100px";
    setTimeout(() => {
        player.style.bottom = "20px";
        jumping = false;
    }, 600);
}

document.addEventListener("keydown", e => {
    if (e.key === "ArrowLeft") moveLeft();
    if (e.key === "ArrowRight") moveRight();
    if (e.code === "Space") {
        e.preventDefault();
        jump();
    }
});

function createItem() {
    if (!running) return;

    let coin = Math.random() < 0.6;
    let item = document.createElement("div");
    item.className = "item";
    item.textContent = coin ? "🪙" : "🚧";

    let itemLane = Math.floor(Math.random() * 3);
    item.style.left = (itemLane * 33.33 + 8) + "%";
    item.style.top = "-35px";

    game.appendChild(item);
    items.push({element: item, lane: itemLane, y: -35, coin});
}

function updateGame() {
    if (!running) return;

    items.forEach((item, index) => {
        item.y += speed;
        item.element.style.top = item.y + "px";

        if (item.y > 350 && item.y < 415 &&
            item.lane === lane) {

            if (item.coin) {
                score += 10;
                document.getElementById("score").textContent = score;
            } else if (!jumping) {
                endGame("GAME OVER!");
            }

            item.element.remove();
            items.splice(index, 1);
        } else if (item.y > 460) {
            item.element.remove();
            items.splice(index, 1);
        }
    });

    speed += 0.0008;
}

function startGame() {
    if (running) return;

    if (time <= 0) restartGame();

    running = true;
    message.style.display = "none";

    timer = setInterval(() => {
        time--;
        document.getElementById("time").textContent = time;
        if (time <= 0) endGame("TIME'S UP!");
    }, 1000);

    gameLoop = setInterval(updateGame, 20);
    spawnLoop = setInterval(createItem, 850);
}

function endGame(text) {
    if (!running) return;
    running = false;

    clearInterval(timer);
    clearInterval(gameLoop);
    clearInterval(spawnLoop);

    message.innerHTML = text + "<br>Final Score: " + score;
    message.style.display = "block";
}

function restartGame() {
    clearInterval(timer);
    clearInterval(gameLoop);
    clearInterval(spawnLoop);

    items.forEach(item => item.element.remove());
    items = [];

    score = 0;
    time = 45;
    speed = 3;
    lane = 1;
    jumping = false;
    running = false;

    player.style.bottom = "20px";
    updatePlayer();

    document.getElementById("score").textContent = 0;
    document.getElementById("time").textContent = 45;

    message.innerHTML = "Collect coins and avoid obstacles!";
    message.style.display = "block";
}

updatePlayer();
</script>
</body>
</html>

