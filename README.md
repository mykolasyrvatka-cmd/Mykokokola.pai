// Spara som flappy.js och kör med `node flappy.js`

const readline = require('readline');

const rows = 10;
const cols = 20;

let bird = 5; // startposition
let pipeX = cols - 1;
let pipeGap = Math.floor(Math.random() * (rows - 4)) + 2;
let score = 0;

readline.emitKeypressEvents(process.stdin);
process.stdin.setRawMode(true);
process.stdin.on('keypress', (str, key) => {
    if (key.name === 'space') {
        bird -= 2; // hoppa
    }
    if (key.ctrl && key.name === 'c') process.exit();
});

function draw() {
    console.clear();
    for (let r = 0; r < rows; r++) {
        let line = '';
        for (let c = 0; c < cols; c++) {
            if (c === 2 && r === bird) line += '🐦'; // fågel
            else if (c === pipeX && (r < pipeGap || r > pipeGap + 2)) line += '█'; // pipe
            else line += ' ';
        }
        console.log(line);
    }
    console.log('Score:', score);
}

function gameLoop() {
    bird += 1; // gravitation
    pipeX -= 1; // rör rör sig

    if (pipeX < 0) {
        pipeX = cols - 1;
        pipeGap = Math.floor(Math.random() * (rows - 4)) + 2;
        score++;
    }

    // kollision
    if (bird < 0 || bird >= rows || (pipeX === 2 && (bird < pipeGap || bird > pipeGap + 2))) {
        console.clear();
        console.log('Game Over! Score:', score);
        process.exit();
    }

    draw();
}

setInterval(gameLoop, 200);
