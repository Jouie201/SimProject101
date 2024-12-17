const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

let player = {
    x: canvas.width / 2 - 50, // Center the tank based on the new width
    y: canvas.height - 120, // Adjust the y position to fit the larger tank
    width: 100, // Increased width
    height: 100, // Increased height
    speed: 5,
    image: new Image() // Create a new image object
};

player.image.src = 'tank.jpg'; // Load the tank image

let bullets = [];
let bulletSpeed = 3;
let enemies = [];
let enemySpeed = 1;
let score = 0;
let gameOver = false;

class Enemy {
    constructor(x, y) {
        this.x = x;
        this.y = y;
        this.width = 50;
        this.height = 50;
        this.image = new Image(); // Create a new image object for the enemy
        this.image.src = 'enemy1.png'; // Load the enemy image
    }

    update() {
        this.y += enemySpeed; // Move downwards
    }

    draw() {
        ctx.drawImage(this.image, this.x, this.y, this.width, this.height); // Draw the enemy image
    }
}

function drawPlayer() {
    ctx.drawImage(player.image, player.x, player.y, player.width, player.height); // Draw the tank image
}

function drawBullets() {
    ctx.fillStyle = 'red';
    bullets.forEach(bullet => {
        ctx.fillRect(bullet.x, bullet.y, bullet.width, bullet.height);
    });
}

function drawEnemies() {
    enemies.forEach(enemy => {
        enemy.draw();
    });
}

function updateBullets() {
    bullets.forEach((bullet, bulletIndex) => {
        bullet.y -= bulletSpeed;
        if (bullet.y < 0) {
            bullets.splice(bulletIndex, 1); // Remove bullet if it goes off screen
        }
    });
}

function updateEnemies() {
    enemies.forEach((enemy, enemyIndex) => {
        enemy.update();
        if (enemy.y > canvas.height) {
            gameOver = true; // Set game over if an enemy reaches the bottom
        }
    });
}

function checkCollisions() {
    bullets.forEach((bullet, bulletIndex) => {
        enemies.forEach((enemy, enemyIndex) => {
            if (bullet.x < enemy.x + enemy.width &&
                bullet.x + bullet.width > enemy.x &&
                bullet.y < enemy.y + enemy.height &&
                bullet.y + bullet.height > enemy.y) {
                bullets.splice(bulletIndex, 1); // Remove bullet
                enemies.splice(enemyIndex, 1); // Remove enemy
                score++; // Increment score
            }
        });
    });
}

function drawGameOver() {
    ctx.fillStyle = 'black';
    ctx.font = '40px Arial';
    ctx.fillText('Game Over!', canvas.width / 2 - 100, canvas.height / 2 - 20);
    ctx.font = '20px Arial';
    ctx.fillText(`Final Score: ${score}`, canvas.width / 2 - 70, canvas.height / 2 + 20);
}

function gameLoop() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    drawPlayer();
    drawBullets();
    drawEnemies();
    updateBullets();
    updateEnemies();
    checkCollisions();

    ctx.fillStyle = 'black';
    ctx.font = '20px Arial';
    ctx.fillText(`Score: ${score}`, 10, 20); // Display score

    if (gameOver) {
        drawGameOver(); // Draw game over message
    } else {
        requestAnimationFrame(gameLoop);
    }
}

document.addEventListener('keydown', (event) => {
    if (event.key === 'ArrowLeft' && player.x > 0) {
        player.x -= player.speed;
    } else if (event.key === 'ArrowRight' && player.x < canvas.width - player.width) {
        player.x += player.speed;
    } else if (event.key === ' ') { // Space bar to shoot
        bullets.push({ x: player.x + player.width / 2 - 2.5, y: player.y, width: 5, height: 20 });
    }
});

// Spawn enemies at regular intervals
setInterval(() => {
    const enemyX = Math.random() * (canvas.width - 50);
    enemies.push(new Enemy(enemyX, 0)); // Spawn enemy at the top
}, 1000); // Adjust the interval as needed

// Start the game loop
gameLoop();
