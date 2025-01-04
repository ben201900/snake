# snake<!DOCTYPE html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>贪吃蛇游戏</title>
    <link rel="stylesheet" href="styles.css">
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }

        #game-container {
            text-align: center;
        }

        #game-canvas {
            background-color: #fff;
            border: 1px solid #000;
        }

        #score {
            margin-top: 10px;
            font-size: 20px;
        }
    </style>
</head>
<body>
    <div id="game-container">
        <canvas id="game-canvas" width="400" height="400"></canvas>
        <div id="score">得分: 0</div>
    </div>
    <script type="module">
        class Snake {
            constructor() {
                this.body = [{ x: 10, y: 10 }];
                this.direction = { x: 1, y: 0 };
            }

            move() {
                const head = { x: this.body[0].x + this.direction.x, y: this.body[0].y + this.direction.y };
                this.body.unshift(head);
                this.body.pop();
            }

            changeDirection(newDirection) {
                this.direction = newDirection;
            }

            grow() {
                const tail = this.body[this.body.length - 1];
                this.body.push({ x: tail.x, y: tail.y });
            }

            checkCollision() {
                const head = this.body[0];
                for (let i = 1; i < this.body.length; i++) {
                    if (head.x === this.body[i].x && head.y === this.body[i].y) {
                        return true;
                    }
                }
                return false;
            }
        }

        const canvas = document.getElementById('game-canvas');
        const ctx = canvas.getContext('2d');
        const scoreElement = document.getElementById('score');
        const snake = new Snake();
        let score = 0;
        let food = { x: Math.floor(Math.random() * 20), y: Math.floor(Math.random() * 20) };

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draw snake
            ctx.fillStyle = 'green';
            snake.body.forEach(segment => {
                ctx.fillRect(segment.x * 20, segment.y * 20, 20, 20);
            });

            // Draw food
            ctx.fillStyle = 'red';
            ctx.fillRect(food.x * 20, food.y * 20, 20, 20);
        }

        function update() {
            snake.move();

            // Check if snake eats the food
            if (snake.body[0].x === food.x && snake.body[0].y === food.y) {
                snake.grow();
                score++;
                scoreElement.textContent = `得分: ${score}`;
                food = { x: Math.floor(Math.random() * 20), y: Math.floor(Math.random() * 20) };
            }

            // Check for collisions
            if (snake.checkCollision() || snake.body[0].x < 0 || snake.body[0].x >= 20 || snake.body[0].y < 0 || snake.body[0].y >= 20) {
                alert('游戏结束');
                document.location.reload();
            }
        }

        function gameLoop() {
            update();
            draw();
            setTimeout(gameLoop, 100);
        }

        window.addEventListener('keydown', (e) => {
            switch (e.key) {
                case 'ArrowUp':
                    snake.changeDirection({ x: 0, y: -1 });
                    break;
                case 'ArrowDown':
                    snake.changeDirection({ x: 0, y: 1 });
                    break;
                case 'ArrowLeft':
                    snake.changeDirection({ x: -1, y: 0 });
                    break;
                case 'ArrowRight':
                    snake.changeDirection({ x: 1, y: 0 });
                    break;
            }
        });

        gameLoop();
    </script>
</body>
</html>
