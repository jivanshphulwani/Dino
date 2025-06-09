<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1" />
<title>Dino Jump Game with Cats - Responsive</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap');

  /* CSS Variables for colors and fonts */
  :root {
    --color-bg: #ffffff;
    --color-ground: #d7d7d7;
    --color-dino: #4b5563;
    --color-obstacle: #111827;
    --color-cat: #8b5cf6; /* violetish for cat */
    --color-text: #6b7280;
    --color-heading: #111827;
    --color-shadow: rgba(0,0,0,0.06);
    --font-primary: 'Poppins', sans-serif;
  }

  /* Reset and box sizing */
  *, *::before, *::after {
    box-sizing: border-box;
  }

  body {
    margin: 0;
    background: var(--color-bg);
    color: var(--color-text);
    font-family: var(--font-primary);
    display: flex;
    justify-content: center;
    padding: 2.5rem 1rem 3rem;
    min-height: 100vh;
  }

  /* Container with max width and vertical spacing */
  .container {
    width: 100%;
    max-width: 600px;
    text-align: center;
    user-select: none;
    padding: 0 1rem;
  }

  /* Heading styles */
  h1 {
    font-weight: 700;
    font-size: 3.75rem;
    line-height: 1.1;
    color: var(--color-heading);
    margin-bottom: 0.5rem;
  }

  /* Instructions paragraph */
  p.instructions {
    font-weight: 600;
    font-size: 1.125rem;
    color: var(--color-text);
    margin-top: 0;
    margin-bottom: 1.75rem;
    max-width: 460px;
    margin-left: auto;
    margin-right: auto;
  }
  p.instructions small {
    display: block;
    margin-top: 0.5rem;
    font-weight: 500;
    color: #a1a1aa;
    font-style: italic;
  }

  /* Game area responsive container */
  .game-area {
    position: relative;
    background: var(--color-bg);
    border-radius: 0.75rem;
    box-shadow: 0 8px 20px var(--color-shadow);
    user-select: none;
    /* Maintain aspect ratio 600x180 roughly => 3.33:1 */
    width: 100%;
    max-width: 600px;
    aspect-ratio: 20 / 6;
    overflow: hidden;
    margin-left: auto;
    margin-right: auto;
  }

  /* Ground */
  .ground {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 40px;
    background: var(--color-ground);
    box-shadow: inset 0 2px 5px rgba(0,0,0,0.05);
    border-radius: 0 0 0.75rem 0.75rem;
  }

  /* Dino styles */
  .dino {
    position: absolute;
    bottom: 40px;
    left: 10%;
    width: 40px;
    height: 40px;
    background: var(--color-dino);
    border-radius: 0.5rem 0.5rem 0 0;
    box-shadow: 0 4px 14px rgba(75, 85, 99, 0.2);
    transition: bottom 0.25s cubic-bezier(0.4, 0, 0.2, 1);
    transform-origin: bottom center;
  }
  .dino.jumping {
    animation: jump-animation 600ms ease forwards;
  }
  @keyframes jump-animation {
    0% { bottom: 40px; }
    30% { bottom: 110px; }
    60% { bottom: 110px; }
    100% { bottom: 40px; }
  }

  /* Obstacle block */
  .obstacle {
    position: absolute;
    bottom: 40px;
    width: 24px;
    height: 40px;
    background: var(--color-obstacle);
    border-radius: 0.5rem;
    box-shadow: 0 6px 8px rgba(17, 24, 39, 0.15);
  }

  /* Cat obstacle styles */
  .cat-obstacle {
    position: absolute;
    bottom: 40px;
    width: 24px;
    height: 30px;
    background: var(--color-cat);
    border-radius: 20% 20% 50% 50% / 60% 60% 30% 30%;
    box-shadow: 0 8px 14px rgba(139, 92, 246, 0.3);
  }
  .cat-obstacle::before, .cat-obstacle::after {
    content: "";
    position: absolute;
    top: 2px;
    width: 7px;
    height: 8px;
    background: var(--color-cat);
    border-radius: 60% 60% 20% 20% / 70% 70% 30% 30%;
    box-shadow: 0 3px 7px rgba(139, 92, 246, 0.35);
  }
  .cat-obstacle::before {
    left: 2px;
    transform: rotate(-20deg);
  }
  .cat-obstacle::after {
    right: 2px;
    transform: rotate(20deg);
  }

  /* Scoreboard */
  .scoreboard {
    margin-top: 1.5rem;
    font-size: 1.375rem;
    font-weight: 700;
    color: var(--color-heading);
  }

  /* Game over panel */
  .game-over {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: #fafafa;
    padding: 2.5rem 3rem;
    border-radius: 0.75rem;
    box-shadow: 0 12px 28px rgba(0,0,0,0.1);
    font-weight: 700;
    font-size: 1.5rem;
    color: #d14343;
    user-select: none;
    display: none;
    z-index: 20;
    max-width: 90%;
    text-align: center;
  }

  button.restart-button {
    margin-top: 1.25rem;
    background: #4338ca;
    color: white;
    border: none;
    padding: 0.75rem 1.75rem;
    border-radius: 0.75rem;
    font-size: 1.125rem;
    font-weight: 700;
    cursor: pointer;
    transition: background-color 0.25s ease;
  }
  button.restart-button:hover,
  button.restart-button:focus {
    background: #3730a3;
    outline: none;
  }

  /* Responsive tweaks for smaller widths */
  @media (max-width: 480px) {
    .container {
      padding: 0 1rem;
    }
    h1 {
      font-size: 2.5rem;
      margin-bottom: 0.75rem;
    }
    p.instructions {
      font-size: 1rem;
      margin-bottom: 1.25rem;
      max-width: 100%;
    }
    .dino {
      width: 32px;
      height: 32px;
      left: 8%;
      bottom: 32px;
    }
    .obstacle,
    .cat-obstacle {
      width: 18px;
      height: 28px;
      bottom: 32px;
    }
    .scoreboard {
      font-size: 1.125rem;
      margin-top: 1rem;
    }
    .game-area {
      aspect-ratio: 20 / 6;
      height: auto;
    }
  }
</style>
</head>
<body>
  <div class="container" role="main">
    <h1>Dino Jump Game with Cats</h1>
    <p class="instructions" id="instructions">
      Press <strong>Space</strong> on keyboard or <strong>Tap</strong> on screen to jump over obstacles. Avoid the blocks and the cats!
      <small>Cat obstacles appear as purple shapes with little "ears".</small>
    </p>
    <div class="game-area" role="region" aria-label="Game area with running dinosaur and obstacles" tabindex="0">
      <div class="ground"></div>
      <div class="dino" aria-live="polite" aria-atomic="true" aria-label="Dinosaur character"></div>
      <div class="game-over" role="alert" aria-live="assertive" aria-hidden="true">
        Game Over!<br />
        <button class="restart-button" aria-label="Restart game">Restart</button>
      </div>
    </div>
    <div class="scoreboard" aria-live="polite" aria-atomic="true" aria-label="Score board">Score: 0</div>
  </div>

  <script>
    (function() {
      const gameArea = document.querySelector('.game-area');
      const dino = document.querySelector('.dino');
      const groundHeight = 40;
      const gravity = 0.6;
      const jumpStrength = 12;
      let obstacles = [];
      let gameSpeed = 6;
      let animationFrameId;
      let score = 0;
      let gameOver = false;
      let jumpVelocity = 0;
      let dinoBottom = groundHeight;
      let canJump = true;

      const scoreboard = document.querySelector('.scoreboard');
      const gameOverPanel = document.querySelector('.game-over');
      const restartButton = document.querySelector('.restart-button');

      // Create an obstacle element, either cat or block
      function createObstacle() {
        const isCat = Math.random() < 0.4; // 40% chance cat obstacle
        const obs = document.createElement('div');
        if (isCat) {
          obs.classList.add('cat-obstacle');
          obs.style.height = '30px';
        } else {
          obs.classList.add('obstacle');
          obs.style.height = '40px';
        }
        obs.style.right = '-30px';
        gameArea.appendChild(obs);
        obstacles.push({
          element: obs,
          x: gameArea.clientWidth + 30,
          width: obs.offsetWidth,
          type: isCat ? 'cat' : 'block'
        });
      }

      function resetObstacle(obs) {
        obs.x = gameArea.clientWidth + 30;
        obs.element.style.right = '-30px';
      }

      function updateObstacles() {
        for(let i = 0; i < obstacles.length; i++) {
          let obs = obstacles[i];
          obs.x -= gameSpeed;
          if (obs.x + obs.width < 0) {
            // Reset obstacle position and type
            obs.x = gameArea.clientWidth + 30;
            const isCat = Math.random() < 0.4;
            obs.type = isCat ? 'cat' : 'block';
            obs.element.className = '';
            if (isCat) {
              obs.element.classList.add('cat-obstacle');
              obs.element.style.height = '30px';
              obs.width = obs.element.offsetWidth;
            } else {
              obs.element.classList.add('obstacle');
              obs.element.style.height = '40px';
              obs.width = obs.element.offsetWidth;
            }
          }
          obs.element.style.right = (gameArea.clientWidth - obs.x) + 'px';

          if (checkCollision(obs)) {
            endGame();
          }
        }
      }

      // Axis aligned rectangle collision detection
      function checkCollision(obstacle) {
        const dinoRect = {
          left: 60,
          bottom: dinoBottom,
          top: dinoBottom + dino.offsetHeight,
          right: 60 + dino.offsetWidth
        };
        const obstacleRect = {
          left: obstacle.x,
          bottom: groundHeight,
          top: groundHeight + obstacle.element.offsetHeight,
          right: obstacle.x + obstacle.width
        };
        const horizontalCollision = !(dinoRect.right < obstacleRect.left || dinoRect.left > obstacleRect.right);
        const verticalCollision = !(dinoRect.bottom > obstacleRect.top || dinoRect.top < obstacleRect.bottom);

        return horizontalCollision && verticalCollision;
      }

      function jump() {
        if (!canJump || gameOver) return;
        jumpVelocity = jumpStrength;
        canJump = false;
        dino.classList.add('jumping');
        setTimeout(() => dino.classList.remove('jumping'), 600);
      }

      function updateDino() {
        if (dinoBottom > groundHeight || jumpVelocity > 0) {
          dinoBottom += jumpVelocity;
          jumpVelocity -= gravity;
          if (dinoBottom <= groundHeight) {
            dinoBottom = groundHeight;
            canJump = true;
          }
          dino.style.bottom = dinoBottom + 'px';
        }
      }

      function updateScore() {
        if (!gameOver) {
          score++;
          scoreboard.textContent = 'Score: ' + score;
        }
      }

      function gameLoop() {
        if (gameOver) return;
        updateDino();
        updateObstacles();
        updateScore();
        animationFrameId = requestAnimationFrame(gameLoop);
      }

      function endGame() {
        gameOver = true;
        gameOverPanel.style.display = 'block';
        gameOverPanel.setAttribute('aria-hidden', 'false');
      }

      function restartGame() {
        score = 0;
        scoreboard.textContent = 'Score: 0';
        dinoBottom = groundHeight;
        jumpVelocity = 0;
        canJump = true;
        gameOver = false;
        gameOverPanel.style.display = 'none';
        gameOverPanel.setAttribute('aria-hidden', 'true');
        dino.style.bottom = dinoBottom + 'px';

        obstacles.forEach((obs, i) => {
          obs.x = gameArea.clientWidth + 100 + i * 300;
          obs.element.style.right = (gameArea.clientWidth - obs.x) + 'px';
          const isCat = Math.random() < 0.4;
          obs.type = isCat ? 'cat' : 'block';
          obs.element.className = '';
          if (isCat) {
            obs.element.classList.add('cat-obstacle');
            obs.element.style.height = '30px';
            obs.width = obs.element.offsetWidth;
          } else {
            obs.element.classList.add('obstacle');
            obs.element.style.height = '40px';
            obs.width = obs.element.offsetWidth;
          }
        });
        animationFrameId = requestAnimationFrame(gameLoop);
      }

      function setupGame() {
        for(let i = 0; i < 2; i++) {
          createObstacle();
          obstacles[i].x = gameArea.clientWidth + 100 + i * 300;
          obstacles[i].element.style.right = (gameArea.clientWidth - obstacles[i].x) + 'px';
        }
        animationFrameId = requestAnimationFrame(gameLoop);
      }

      // Input listeners
      window.addEventListener('keydown', e => {
        if (e.code === 'Space') {
          e.preventDefault();
          if (gameOver) {
            restartGame();
          } else {
            jump();
          }
        }
      });

      gameArea.addEventListener('click', () => {
        if (gameOver) {
          restartGame();
        } else {
          jump();
        }
      });

      restartButton.addEventListener('click', () => {
        restartGame();
      });

      setupGame();
    })();
  </script>
</body>
</html>

