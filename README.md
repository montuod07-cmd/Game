<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pong Game</title>
    <style>
        canvas {
            background: #000;
            display: block;
            margin: auto;
        }
    </style>
</head>
<body>
    <canvas id="pong" width="800" height="400"></canvas>
    <script>
        const canvas = document.getElementById('pong');
        const ctx = canvas.getContext('2d');

        // Create the pong paddle
        const paddleWidth = 10, paddleHeight = 100;
        const playerPaddle = {
            x: 0,
            y: canvas.height / 2 - paddleHeight / 2,
            width: paddleWidth,
            height: paddleHeight,
            color: '#FFF',
        };

        const aiPaddle = {
            x: canvas.width - paddleWidth,
            y: canvas.height / 2 - paddleHeight / 2,
            width: paddleWidth,
            height: paddleHeight,
            color: '#FFF',
        };

        // Create the pong ball
        const ball = {
            x: canvas.width / 2,
            y: canvas.height / 2,
            radius: 10,
            speed: 4,
            velocityX: 4,
            velocityY: 4,
            color: '#FFF',
        };

        // Draw everything
        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = playerPaddle.color;
            ctx.fillRect(playerPaddle.x, playerPaddle.y, playerPaddle.width, playerPaddle.height);
            ctx.fillStyle = aiPaddle.color;
            ctx.fillRect(aiPaddle.x, aiPaddle.y, aiPaddle.width, aiPaddle.height);
            ctx.fillStyle = ball.color;
            ctx.beginPath();
            ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2, false);
            ctx.fill();
            ctx.closePath();
        }

        // Update game
        function update() {
            ball.x += ball.velocityX;
            ball.y += ball.velocityY;

            // AI paddle movement
            if (aiPaddle.y < ball.y - aiPaddle.height / 2) {
                aiPaddle.y += 4;
            } else {
                aiPaddle.y -= 4;
            }

            // Collision detection
            if (ball.y + ball.radius > canvas.height || ball.y - ball.radius < 0) {
                ball.velocityY = -ball.velocityY;
            }

            if (ball.x < 0 || ball.x > canvas.width) {
                // Reset ball position
                ball.x = canvas.width / 2;
                ball.y = canvas.height / 2;
                ball.velocityX = -ball.velocityX;
            }

            // Paddle collision
            if (ball.x - ball.radius < playerPaddle.x + playerPaddle.width &&
                ball.y > playerPaddle.y && ball.y < playerPaddle.y + playerPaddle.height) {
                ball.velocityX = -ball.velocityX;
            }

            if (ball.x + ball.radius > aiPaddle.x &&
                ball.y > aiPaddle.y && ball.y < aiPaddle.y + aiPaddle.height) {
                ball.velocityX = -ball.velocityX;
            }
        }

        // Game loop
        function loop() {
            draw();
            update();
            requestAnimationFrame(loop);
        }

        // Start the game
        loop();
    </script>
</body>
</html>
