# tch-the-ball
First version of the game
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Catch the Ball</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      background: #f0f0f0;
      font-family: sans-serif;
      text-align: center;
    }
    canvas {
      background: #222;
      display: block;
      margin: 0 auto;
    }
    #score {
      position: absolute;
      top: 10px;
      left: 50%;
      transform: translateX(-50%);
      color: white;
      font-size: 20px;
      z-index: 1;
    }
  </style>
</head>
<body>
  <div id="score">Score: 0</div>
  <canvas id="gameCanvas" width="400" height="600"></canvas>
  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");
    const scoreDisplay = document.getElementById("score");

    let ball = {
      x: Math.random() * 360 + 20,
      y: -20,
      radius: 20,
      dy: 3
    };

    let paddle = {
      x: 150,
      y: 560,
      width: 100,
      height: 10,
      dx: 5
    };

    let score = 0;

    document.addEventListener("keydown", (e) => {
      if (e.key === "ArrowLeft") paddle.x -= paddle.dx;
      else if (e.key === "ArrowRight") paddle.x += paddle.dx;
    });

    function drawBall() {
      ctx.beginPath();
      ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2);
      ctx.fillStyle = "red";
      ctx.fill();
      ctx.closePath();
    }

    function drawPaddle() {
      ctx.fillStyle = "white";
      ctx.fillRect(paddle.x, paddle.y, paddle.width, paddle.height);
    }

    function updateGame() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      drawBall();
      drawPaddle();

      ball.y += ball.dy;

      if (
        ball.y + ball.radius > paddle.y &&
        ball.x > paddle.x &&
        ball.x < paddle.x + paddle.width
      ) {
        score++;
        scoreDisplay.textContent = "Score: " + score;
        resetBall();
      } else if (ball.y > canvas.height) {
        score = 0;
        scoreDisplay.textContent = "Score: 0";
        resetBall();
      }

      requestAnimationFrame(updateGame);
    }

    function resetBall() {
      ball.x = Math.random() * 360 + 20;
      ball.y = -20;
    }

    updateGame();
  </script>
</body>
</html>
