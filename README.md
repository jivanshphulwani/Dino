<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Dino Jump Game with Cats</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap');

  :root {
    --color-bg: #f0f0f3;
    --color-ground: #d7d7d7;
    --color-dino: #4b5563;
    --color-obstacle: #111827;
    --color-cat: #8b5cf6; /* violetish for cat */
    --color-text: #374151;
    --color-shadow: rgba(0,0,0,0.1);
    --font-primary: 'Poppins', sans-serif;
  }

  * {
    box-sizing: border-box;
  }

  body {
    margin: 0;
    background: var(--color-bg);
    font-family: var(--font-primary);
    color: var(--color-text);
    display: flex;
    justify-content: center;
    align-items: flex-start;
    height: 100vh;
    padding: 2rem;
    user-select: none;
  }

  .container {
    max-width: 600px;
    width: 100%;
    text-align: center;
  }

  h1 {
    font-weight: 700;
    font-size: 3rem;
    margin-bottom: 0.25rem;
    color: var(--color-dino);
  }

  p.instructions {
    font-weight: 600;
    color: #6b7280;
    margin-bottom: 1.5rem;
  }

  .instructions small {
    display: block;
    margin-top: 0.5rem;
    color: #9ca3af;
    font-weight: 500;
    font-style: italic;
  }

  .game-area {
    position: relative;
    width: 100%;
    height: 180px;
    background: white;
    border-radius: 1rem;
    box-shadow: 0 10px 15px var(--color-shadow);
    overflow: hidden;
  }

  .ground {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 40px;
    background: var(--color-ground);
    box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
  }

  .dino {
    position: absolute;
    bottom: 40px;
    left: 60px;
    width: 40px;
    height: 40px;
    background: var(--color-dino);
    border-radius: 8px 8px 0 0;
    box-shadow: 0 4px 8px var(--color-shadow);
    transition: bottom 0.2s ease;
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

  /* Obstacle styles */
  .obstacle {
    position: absolute;
    bottom: 40px;
    width: 24px;
    height: 40px;
    background: var(--color-obstacle);
    border-radius: 4px;
    box-shadow: 0 4px 6px var(--color-shadow);
  }

  /* Cat obstacle style */
  .cat-obstacle {
    position: absolute;
    bottom: 40px;
    width: 24px;
    height: 30px;
    background: var(--color-cat);
    border-radius: 20% 20% 50% 50% / 60% 60% 30% 30%; /* catbits rounded for ears and body */
    box-shadow: 0 6px 10px rgba(139, 92, 246, 0.5);
  }

  /* Add small "ears" pseudo elements for cat shape */
  .cat-obstacle::before, .cat-obstacle::after {
    content: "";
    position: absolute;
    top: 2px;
    width: 7px;
    height: 8px;
    background: var(--color-cat);
    border-radius: 60% 60% 20% 20% / 70% 70% 30% 30%;
    box-shadow: 0 3px 6px rgba(139, 92, 246, 0.4);
  }
  .cat-obstacle::before {
    left: 2px;
    transform: rotate(-20deg);
  }
  .cat-obstacle::after {
    right: 2px;
    transform: rotate(20deg);
  }

  .scoreboard {
    margin-top: 1rem;
    font-size: 1.25rem;
    font-weight: 700;
  }

  .game-over {
    position: absolute;
    top: 40%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: rgba(243, 244, 246, 0.95);
    padding: 2rem 3rem;
    border-radius: 1rem;
    box-shadow: 0 12px 24px rgba(0,0,0,0.15);
    font-weight: 700;
    font-size: 1.5rem;
    color: #ef4444;
    user-select: none;
    display: none;
    z-index: 10;
  }

  button.restart-button {
    margin-top: 1rem;
    background: #4f46e5;
    color: white;
    border: none;
    padding: 0.75rem 1.5rem;
    border-radius: 0.75rem;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  button.restart-button:hover,
  button.restart-button:focus {
    background: #4338ca;
    outline: none;
  }

  @media (max-width: 480px) {
    h1 {
      font-size: 2.25rem;
    }

    .game-area {
      height: 150px;
    }

    .dino {
      width: 32px;
      height: 32px;
      left: 40px;
      bottom: 32px;
    }

    .obstacle, .cat-obstacle {
      width: 18px;
      height: 32px;
      bottom: 32px;
    }
  }
</style>
</head>
<body>
  <div class="container" role="main">
    <h1>Dino Jump Game with Cats</h1>
    <p class="instructions">
      Press <strong>Space</strong> or <strong>Click</strong> to jump over obstacles. Avoid the blocks and the cats!
      <small>Cat obstacles are purple colored with "ears".</small>
    </p>
    <div class="game-area" aria-label="Game area with running dino, cats, and obstacles" tabindex="0">
      <div class="ground"></div>
      <div class="dino" aria-live="polite" aria-atomic="true" aria-label="Dinosaur character"></div>
      <div class="game-over" role="alert" aria-live="assertive">
        Game Over!<br />
        <button class="restart-button" aria-label="Restart game">Restart</button>
      </div>
    </div>
    <div class="scoreboard" aria-label="Score board" aria-live="polite" aria-atomic="true">Score: 0</div>
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

      // Create obstacle element, randomly cat or block
      function createObstacle() {
        const isCat = Math.random() < 0.4; // 40% chance cat obstacle
        const obs = document.createElement('div');
        if (isCat) {
          obs.classList.add('cat-obstacle');
          // Cats are a bit shorter and narrower in height style
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
            // Reset obstacle
            obs.x = gameArea.clientWidth + 30;
            // Randomly choose new obstacle type on reset
            const isCat = Math.random() < 0.4;
            obs.type = isCat ? 'cat' : 'block';
            obs.element.className = ''; // reset classes
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

      function checkCollision(obstacle) {
        // Dino rectangle
        const dinoRect = {
          left: 60,
          bottom: dinoBottom,
          top: dinoBottom + dino.offsetHeight,
          right: 60 + dino.offsetWidth
        };
        // Obstacle rectangle
        const obstacleRect = {
          left: obstacle.x,
          bottom: groundHeight,
          top: groundHeight + obstacle.element.offsetHeight,
          right: obstacle.x + obstacle.width
        };

        // Check horizontal overlap
        const horizontalCollision = !(dinoRect.right < obstacleRect.left || dinoRect.left > obstacleRect.right);
        // Check vertical overlap
        const verticalCollision = !(dinoRect.bottom > obstacleRect.top || dinoRect.top < obstacleRect.bottom);

        return horizontalCollision && verticalCollision;
      }

      function jump() {
        if (!canJump || gameOver) return;
        jumpVelocity = jumpStrength;
        canJump = false;
        dino.classList.add('jumping');
        setTimeout(() => {
          dino.classList.remove('jumping');
        }, 600);
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
      }

      function restartGame() {
        // Reset variables
        score = 0;
        scoreboard.textContent = 'Score: 0';
        dinoBottom = groundHeight;
        jumpVelocity = 0;
        canJump = true;
        gameOver = false;
        gameOverPanel.style.display = 'none';
        dino.style.bottom = dinoBottom + 'px';
        // Reset obstacle positions randomly and types
        obstacles.forEach((obs, i) => {
          obs.x = gameArea.clientWidth + 100 + i * 300;
          obs.element.style.right = (gameArea.clientWidth - obs.x) + 'px';
          // Randomly set obstacle type
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
        // Restart game loop
        animationFrameId = requestAnimationFrame(gameLoop);
      }

      function setupGame() {
        // Create initial obstacles spaced apart
        for(let i = 0; i < 2; i++) {
          createObstacle();
          obstacles[i].x = gameArea.clientWidth + 100 + i * 300;
          obstacles[i].element.style.right = (gameArea.clientWidth - obstacles[i].x) + 'px';
        }
        // Start the loop
        animationFrameId = requestAnimationFrame(gameLoop);
      }

      // Event listeners
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


