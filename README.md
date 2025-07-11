<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1" />
<title>Naruto 2D Basic Mobile</title>
<style>
  body, html {
    margin: 0; padding: 0; overflow: hidden; touch-action: none;
    background: #87ceeb;
    user-select: none;
  }
  #game {
    position: relative;
    width: 100vw;
    height: 100vh;
    background: url('https://i.imgur.com/9y8Xm9R.png') no-repeat center bottom;
    background-size: cover;
  }
  #naruto {
    position: absolute;
    bottom: 50px;
    left: 50px;
    width: 50px;
    height: 70px;
    background: url('https://i.imgur.com/Yj0kA1U.png') no-repeat center;
    background-size: contain;
  }
  #controls {
    position: absolute;
    bottom: 20px;
    width: 100%;
    display: flex;
    justify-content: center;
    gap: 20px;
  }
  .btn {
    width: 60px;
    height: 60px;
    background: rgba(255,255,255,0.7);
    border-radius: 50%;
    font-size: 30px;
    text-align: center;
    line-height: 60px;
    user-select: none;
    touch-action: none;
    box-shadow: 0 0 5px #333;
  }
</style>
</head>
<body>
  <div id="game">
    <div id="naruto"></div>
    <div id="controls">
      <div class="btn" id="leftBtn">&#8592;</div>
      <div class="btn" id="jumpBtn">&#8593;</div>
      <div class="btn" id="rightBtn">&#8594;</div>
    </div>
  </div>

<script>
  const naruto = document.getElementById('naruto');
  const game = document.getElementById('game');

  let posX = 50;
  let posY = 50; // for jumping
  let jumping = false;
  let velocityY = 0;
  const gravity = 1.5;
  const groundLevel = 50;

  const moveSpeed = 5;

  let leftPressed = false;
  let rightPressed = false;

  // Controls
  document.getElementById('leftBtn').addEventListener('touchstart', e => { e.preventDefault(); leftPressed = true; });
  document.getElementById('leftBtn').addEventListener('touchend', e => { e.preventDefault(); leftPressed = false; });

  document.getElementById('rightBtn').addEventListener('touchstart', e => { e.preventDefault(); rightPressed = true; });
  document.getElementById('rightBtn').addEventListener('touchend', e => { e.preventDefault(); rightPressed = false; });

  document.getElementById('jumpBtn').addEventListener('touchstart', e => { e.preventDefault(); jump(); });

  function jump() {
    if (!jumping) {
      jumping = true;
      velocityY = -20;
    }
  }

  function gameLoop() {
    if (leftPressed) {
      posX = Math.max(0, posX - moveSpeed);
    }
    if (rightPressed) {
      posX = Math.min(game.clientWidth - naruto.clientWidth, posX + moveSpeed);
    }

    if (jumping) {
      posY += velocityY;
      velocityY += gravity;

      if (posY >= groundLevel) {
        posY = groundLevel;
        jumping = false;
        velocityY = 0;
      }
    }

    naruto.style.left = posX + 'px';
    naruto.style.bottom = posY + 'px';

    requestAnimationFrame(gameLoop);
  }

  gameLoop();
</script>
</body>
</html>
