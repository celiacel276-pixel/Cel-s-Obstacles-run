<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Time Rush: Obstacle Challenge</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
            color: white;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            max-width: 1000px;
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        header {
            text-align: center;
            margin-bottom: 20px;
            width: 100%;
        }
        
        h1 {
            font-size: 3rem;
            margin-bottom: 10px;
            text-shadow: 0 0 10px #00ffcc;
            color: #00ffcc;
        }
        
        .subtitle {
            font-size: 1.2rem;
            opacity: 0.8;
            margin-bottom: 30px;
        }
        
        .game-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            width: 100%;
            margin-bottom: 30px;
        }
        
        .game-info {
            display: flex;
            justify-content: space-between;
            width: 800px;
            max-width: 100%;
            background: rgba(0, 0, 0, 0.5);
            padding: 15px;
            border-radius: 10px 10px 0 0;
            border-bottom: 2px solid #00ffcc;
        }
        
        .info-item {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .info-label {
            font-size: 0.9rem;
            opacity: 0.8;
        }
        
        .info-value {
            font-size: 1.5rem;
            font-weight: bold;
            color: #00ffcc;
        }
        
        .game-area {
            position: relative;
            width: 800px;
            height: 500px;
            max-width: 100%;
            background: #111;
            border: 2px solid #00ffcc;
            border-radius: 0 0 10px 10px;
            overflow: hidden;
            box-shadow: 0 0 20px rgba(0, 255, 204, 0.3);
        }
        
        #game-canvas {
            width: 100%;
            height: 100%;
        }
        
        .player {
            position: absolute;
            width: 40px;
            height: 40px;
            background: #00ffcc;
            border-radius: 50%;
            box-shadow: 0 0 15px #00ffcc;
            z-index: 10;
        }
        
        .obstacle {
            position: absolute;
            background: #ff3366;
            border-radius: 5px;
        }
        
        .finish-line {
            position: absolute;
            width: 20px;
            height: 100%;
            background: repeating-linear-gradient(
                45deg,
                #00ffcc,
                #00ffcc 10px,
                transparent 10px,
                transparent 20px
            );
            right: 0;
            top: 0;
        }
        
        .controls {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-top: 20px;
            width: 800px;
            max-width: 100%;
        }
        
        .buttons {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
        }
        
        button {
            padding: 12px 25px;
            font-size: 1.1rem;
            background: #00ffcc;
            color: #0f2027;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(0, 255, 204, 0.4);
        }
        
        button:hover {
            background: #00e6b8;
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 255, 204, 0.6);
        }
        
        button:disabled {
            background: #666;
            color: #999;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .instructions {
            background: rgba(0, 0, 0, 0.5);
            padding: 20px;
            border-radius: 10px;
            width: 800px;
            max-width: 100%;
            margin-top: 20px;
        }
        
        .instructions h2 {
            color: #00ffcc;
            margin-bottom: 10px;
            font-size: 1.5rem;
        }
        
        .instructions p {
            margin-bottom: 10px;
            line-height: 1.5;
        }
        
        .instructions ul {
            margin-left: 20px;
            margin-bottom: 15px;
        }
        
        .instructions li {
            margin-bottom: 8px;
        }
        
        .level-progress {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 8px;
            margin-top: 20px;
            width: 800px;
            max-width: 100%;
        }
        
        .level-dot {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: #333;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .level-dot.completed {
            background: #00ffcc;
            color: #0f2027;
        }
        
        .level-dot.current {
            background: #ffcc00;
            color: #0f2027;
            transform: scale(1.2);
        }
        
        .level-dot.locked {
            background: #444;
            color: #777;
            cursor: not-allowed;
        }
        
        .game-over {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.9);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 100;
            border-radius: 10px;
        }
        
        .game-over h2 {
            font-size: 3rem;
            color: #ff3366;
            margin-bottom: 20px;
        }
        
        .game-over p {
            font-size: 1.5rem;
            margin-bottom: 30px;
        }
        
        .hidden {
            display: none;
        }
        
        .mobile-controls {
            display: none;
            margin-top: 20px;
            gap: 20px;
        }
        
        .mobile-btn {
            width: 70px;
            height: 70px;
            border-radius: 50%;
            background: rgba(0, 255, 204, 0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            font-weight: bold;
            color: #0f2027;
            user-select: none;
        }
        
        @media (max-width: 850px) {
            .game-area, .game-info, .controls, .instructions, .level-progress {
                width: 95%;
            }
            
            h1 {
                font-size: 2.2rem;
            }
            
            .game-info {
                flex-wrap: wrap;
                justify-content: center;
                gap: 15px;
            }
            
            .mobile-controls {
                display: flex;
            }
        }
        
        @media (max-width: 600px) {
            .game-area {
                height: 400px;
            }
            
            .level-dot {
                width: 25px;
                height: 25px;
                font-size: 0.8rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>TIME RUSH</h1>
            <div class="subtitle">Dodge obstacles and reach the finish line before time runs out!</div>
        </header>
        
        <div class="game-container">
            <div class="game-info">
                <div class="info-item">
                    <div class="info-label">LEVEL</div>
                    <div class="info-value" id="level">1</div>
                </div>
                <div class="info-item">
                    <div class="info-label">TIME LEFT</div>
                    <div class="info-value" id="timer">30s</div>
                </div>
                <div class="info-item">
                    <div class="info-label">OBSTACLES</div>
                    <div class="info-value" id="obstacles">5</div>
                </div>
                <div class="info-item">
                    <div class="info-label">SPEED</div>
                    <div class="info-value" id="speed">Medium</div>
                </div>
                <div class="info-item">
                    <div class="info-label">LIVES</div>
                    <div class="info-value" id="lives">3</div>
                </div>
            </div>
            
            <div class="game-area">
                <canvas id="game-canvas"></canvas>
                <div id="game-over" class="game-over hidden">
                    <h2 id="result-title">TIME'S UP!</h2>
                    <p id="result-message">You didn't reach the finish line in time.</p>
                    <button id="restart-btn">TRY AGAIN</button>
                </div>
            </div>
        </div>
        
        <div class="controls">
            <div class="buttons">
                <button id="start-btn">START GAME</button>
                <button id="pause-btn" disabled>PAUSE</button>
                <button id="reset-btn">RESET LEVEL</button>
                <button id="next-btn" disabled>NEXT LEVEL</button>
            </div>
            
            <div class="mobile-controls">
                <div class="mobile-btn" id="left-btn">←</div>
                <div class="mobile-btn" id="up-btn">↑</div>
                <div class="mobile-btn" id="right-btn">→</div>
                <div class="mobile-btn" id="down-btn">↓</div>
            </div>
        </div>
        
        <div class="level-progress" id="level-progress">
            <!-- Level dots will be generated by JavaScript -->
        </div>
        
        <div class="instructions">
            <h2>How to Play</h2>
            <p>Navigate the blue player to the finish line on the right side while avoiding red obstacles.</p>
            <ul>
                <li><strong>Controls:</strong> Use arrow keys or WASD to move. On mobile, use the touch buttons.</li>
                <li><strong>Objective:</strong> Reach the finish line before time runs out.</li>
                <li><strong>Levels:</strong> 30 levels with increasing difficulty (more obstacles, less time, faster speed).</li>
                <li><strong>Lives:</strong> You start with 3 lives. Colliding with obstacles costs 1 life.</li>
                <li><strong>Time:</strong> Each level has a time limit. The timer decreases faster at higher levels.</li>
            </ul>
            <p><strong>Tip:</strong> Plan your route to avoid getting trapped by moving obstacles!</p>
        </div>
    </div>

    <script>
        // Game variables
        let currentLevel = 1;
        let gameRunning = false;
        let gamePaused = false;
        let timeLeft = 0;
        let timerInterval;
        let playerX = 50;
        let playerY = 250;
        let playerSpeed = 5;
        let lives = 3;
        let obstacles = [];
        let finishLineReached = false;
        let canvas, ctx;
        let keys = {};
        
        // Game configuration
        const TOTAL_LEVELS = 30;
        const BASE_TIME = 30; // Base time for level 1
        const BASE_OBSTACLES = 5; // Base obstacles for level 1
        
        // Level progress tracking
        let completedLevels = JSON.parse(localStorage.getItem('timeRushCompletedLevels')) || [];
        let unlockedLevels = JSON.parse(localStorage.getItem('timeRushUnlockedLevels')) || [1];
        
        // DOM elements
        const levelElement = document.getElementById('level');
        const timerElement = document.getElementById('timer');
        const obstaclesElement = document.getElementById('obstacles');
        const speedElement = document.getElementById('speed');
        const livesElement = document.getElementById('lives');
        const startBtn = document.getElementById('start-btn');
        const pauseBtn = document.getElementById('pause-btn');
        const resetBtn = document.getElementById('reset-btn');
        const nextBtn = document.getElementById('next-btn');
        const restartBtn = document.getElementById('restart-btn');
        const gameOverElement = document.getElementById('game-over');
        const resultTitle = document.getElementById('result-title');
        const resultMessage = document.getElementById('result-message');
        const levelProgressElement = document.getElementById('level-progress');
        const leftBtn = document.getElementById('left-btn');
        const upBtn = document.getElementById('up-btn');
        const rightBtn = document.getElementById('right-btn');
        const downBtn = document.getElementById('down-btn');
        
        // Initialize the game
        function init() {
            canvas = document.getElementById('game-canvas');
            ctx = canvas.getContext('2d');
            
            // Set canvas dimensions
            canvas.width = canvas.offsetWidth;
            canvas.height = canvas.offsetHeight;
            
            // Create level progress dots
            createLevelProgress();
            
            // Load current level
            loadLevel(currentLevel);
            
            // Event listeners for buttons
            startBtn.addEventListener('click', startGame);
            pauseBtn.addEventListener('click', togglePause);
            resetBtn.addEventListener('click', resetLevel);
            nextBtn.addEventListener('click', nextLevel);
            restartBtn.addEventListener('click', restartGame);
            
            // Keyboard controls
            document.addEventListener('keydown', (e) => {
                keys[e.key] = true;
                
                // Prevent arrow keys from scrolling the page
                if(['ArrowUp', 'ArrowDown', 'ArrowLeft', 'ArrowRight', ' '].includes(e.key)) {
                    e.preventDefault();
                }
            });
            
            document.addEventListener('keyup', (e) => {
                keys[e.key] = false;
            });
            
            // Mobile controls
            leftBtn.addEventListener('touchstart', () => keys['ArrowLeft'] = true);
            leftBtn.addEventListener('touchend', () => keys['ArrowLeft'] = false);
            
            upBtn.addEventListener('touchstart', () => keys['ArrowUp'] = true);
            upBtn.addEventListener('touchend', () => keys['ArrowUp'] = false);
            
            rightBtn.addEventListener('touchstart', () => keys['ArrowRight'] = true);
            rightBtn.addEventListener('touchend', () => keys['ArrowRight'] = false);
            
            downBtn.addEventListener('touchstart', () => keys['ArrowDown'] = true);
            downBtn.addEventListener('touchend', () => keys['ArrowDown'] = false);
            
            // Mouse controls for desktop testing
            leftBtn.addEventListener('mousedown', () => keys['ArrowLeft'] = true);
            leftBtn.addEventListener('mouseup', () => keys['ArrowLeft'] = false);
            
            upBtn.addEventListener('mousedown', () => keys['ArrowUp'] = true);
            upBtn.addEventListener('mouseup', () => keys['ArrowUp'] = false);
            
            rightBtn.addEventListener('mousedown', () => keys['ArrowRight'] = true);
            rightBtn.addEventListener('mouseup', () => keys['ArrowRight'] = false);
            
            downBtn.addEventListener('mousedown', () => keys['ArrowDown'] = true);
            downBtn.addEventListener('mouseup', () => keys['ArrowDown'] = false);
            
            // Handle window resize
            window.addEventListener('resize', () => {
                canvas.width = canvas.offsetWidth;
                canvas.height = canvas.offsetHeight;
                drawGame();
            });
            
            // Draw initial game state
            drawGame();
        }
        
        // Create level progress indicator
        function createLevelProgress() {
            levelProgressElement.innerHTML = '';
            
            for (let i = 1; i <= TOTAL_LEVELS; i++) {
                const levelDot = document.createElement('div');
                levelDot.className = 'level-dot';
                levelDot.textContent = i;
                
                if (completedLevels.includes(i)) {
                    levelDot.classList.add('completed');
                } else if (i === currentLevel) {
                    levelDot.classList.add('current');
                } else if (!unlockedLevels.includes(i)) {
                    levelDot.classList.add('locked');
                }
                
                if (unlockedLevels.includes(i) && !gameRunning) {
                    levelDot.addEventListener('click', () => {
                        if (currentLevel !== i) {
                            currentLevel = i;
                            loadLevel(currentLevel);
                            createLevelProgress();
                        }
                    });
                }
                
                levelProgressElement.appendChild(levelDot);
            }
        }
        
        // Load a specific level
        function loadLevel(level) {
            // Reset game state
            gameRunning = false;
            gamePaused = false;
            finishLineReached = false;
            obstacles = [];
            
            // Calculate level parameters
            const timeMultiplier = Math.max(0.7, 1 - (level - 1) * 0.03); // Time decreases by 3% per level
            const obstacleMultiplier = 1 + (level - 1) * 0.15; // Obstacles increase by 15% per level
            const speedMultiplier = 1 + (level - 1) * 0.1; // Speed increases by 10% per level
            
            // Set level parameters
            timeLeft = Math.floor(BASE_TIME * timeMultiplier);
            const obstacleCount = Math.min(30, Math.floor(BASE_OBSTACLES * obstacleMultiplier));
            playerSpeed = 5 * speedMultiplier;
            
            // Update UI
            levelElement.textContent = level;
            timerElement.textContent = `${timeLeft}s`;
            obstaclesElement.textContent = obstacleCount;
            speedElement.textContent = level < 10 ? 'Medium' : level < 20 ? 'Fast' : 'Very Fast';
            livesElement.textContent = lives;
            
            // Update button states
            startBtn.disabled = false;
            pauseBtn.disabled = true;
            nextBtn.disabled = true;
            
            // Generate obstacles
            generateObstacles(obstacleCount);
            
            // Reset player position
            playerX = 50;
            playerY = canvas.height / 2;
            
            // Draw the level
            drawGame();
        }
        
        // Generate obstacles for the level
        function generateObstacles(count) {
            const obstacleTypes = ['static', 'horizontal', 'vertical'];
            
            for (let i = 0; i < count; i++) {
                const type = obstacleTypes[Math.floor(Math.random() * obstacleTypes.length)];
                const width = 20 + Math.random() * 40;
                const height = 20 + Math.random() * 40;
                
                // Ensure obstacles don't spawn too close to the player start or finish line
                let x = 100 + Math.random() * (canvas.width - 200 - width);
                let y = 20 + Math.random() * (canvas.height - 40 - height);
                
                // Add some movement parameters for moving obstacles
                let speed = 0;
                let direction = 0;
                let moveRange = 0;
                
                if (type === 'horizontal') {
                    speed = 1 + Math.random() * 2;
                    direction = Math.random() > 0.5 ? 1 : -1;
                    moveRange = 50 + Math.random() * 100;
                } else if (type === 'vertical') {
                    speed = 1 + Math.random() * 2;
                    direction = Math.random() > 0.5 ? 1 : -1;
                    moveRange = 50 + Math.random() * 100;
                }
                
                obstacles.push({
                    x, y, width, height, type, speed, direction, moveRange,
                    startX: x,
                    startY: y
                });
            }
        }
        
        // Start the game
        function startGame() {
            if (gameRunning) return;
            
            gameRunning = true;
            gamePaused = false;
            finishLineReached = false;
            
            startBtn.disabled = true;
            pauseBtn.disabled = false;
            pauseBtn.textContent = 'PAUSE';
            
            // Start the timer
            clearInterval(timerInterval);
            timerInterval = setInterval(updateTimer, 1000);
            
            // Start game loop
            gameLoop();
        }
        
        // Toggle pause state
        function togglePause() {
            if (!gameRunning) return;
            
            gamePaused = !gamePaused;
            pauseBtn.textContent = gamePaused ? 'RESUME' : 'PAUSE';
            
            if (gamePaused) {
                clearInterval(timerInterval);
            } else {
                timerInterval = setInterval(updateTimer, 1000);
                gameLoop();
            }
        }
        
        // Reset current level
        function resetLevel() {
            clearInterval(timerInterval);
            loadLevel(currentLevel);
            gameOverElement.classList.add('hidden');
        }
        
        // Go to next level
        function nextLevel() {
            if (currentLevel < TOTAL_LEVELS) {
                currentLevel++;
                
                // Unlock the next level if not already unlocked
                if (!unlockedLevels.includes(currentLevel)) {
                    unlockedLevels.push(currentLevel);
                    localStorage.setItem('timeRushUnlockedLevels', JSON.stringify(unlockedLevels));
                }
                
                loadLevel(currentLevel);
                gameOverElement.classList.add('hidden');
                createLevelProgress();
            }
        }
        
        // Restart game after win/loss
        function restartGame() {
            lives = 3;
            currentLevel = 1;
            loadLevel(currentLevel);
            gameOverElement.classList.add('hidden');
            createLevelProgress();
        }
        
        // Update the timer
        function updateTimer() {
            if (!gameRunning || gamePaused || finishLineReached) return;
            
            timeLeft--;
            timerElement.textContent = `${timeLeft}s`;
            
            // Time's up
            if (timeLeft <= 0) {
                endGame(false, "Time's up!");
            }
        }
        
        // Main game loop
        function gameLoop() {
            if (!gameRunning || gamePaused || finishLineReached) return;
            
            updateGame();
            drawGame();
            
            // Continue game loop
            requestAnimationFrame(gameLoop);
        }
        
        // Update game state
        function updateGame() {
            // Move player based on keyboard input
            if (keys['ArrowUp'] || keys['w'] || keys['W']) {
                playerY = Math.max(20, playerY - playerSpeed);
            }
            if (keys['ArrowDown'] || keys['s'] || keys['S']) {
                playerY = Math.min(canvas.height - 20, playerY + playerSpeed);
            }
            if (keys['ArrowLeft'] || keys['a'] || keys['A']) {
                playerX = Math.max(20, playerX - playerSpeed);
            }
            if (keys['ArrowRight'] || keys['d'] || keys['D']) {
                playerX = Math.min(canvas.width - 20, playerX + playerSpeed);
            }
            
            // Update obstacles
            obstacles.forEach(obstacle => {
                if (obstacle.type === 'horizontal') {
                    obstacle.x += obstacle.speed * obstacle.direction;
                    
                    // Reverse direction if outside movement range
                    if (obstacle.x > obstacle.startX + obstacle.moveRange || 
                        obstacle.x < obstacle.startX - obstacle.moveRange) {
                        obstacle.direction *= -1;
                    }
                } else if (obstacle.type === 'vertical') {
                    obstacle.y += obstacle.speed * obstacle.direction;
                    
                    // Reverse direction if outside movement range
                    if (obstacle.y > obstacle.startY + obstacle.moveRange || 
                        obstacle.y < obstacle.startY - obstacle.moveRange) {
                        obstacle.direction *= -1;
                    }
                }
                
                // Check collision with player
                if (checkCollision(playerX, playerY, 20, obstacle.x, obstacle.y, obstacle.width, obstacle.height)) {
                    // Player hit an obstacle
                    lives--;
                    livesElement.textContent = lives;
                    
                    // Reset player position
                    playerX = 50;
                    playerY = canvas.height / 2;
                    
                    // Game over if no lives left
                    if (lives <= 0) {
                        endGame(false, "No lives left!");
                    }
                }
            });
            
            // Check if player reached finish line
            if (playerX >= canvas.width - 30) {
                finishLineReached = true;
                endGame(true, "Level Complete!");
            }
        }
        
        // Draw the game
        function drawGame() {
            // Clear canvas
            ctx.fillStyle = '#111';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // Draw finish line
            ctx.fillStyle = '#00ffcc';
            ctx.fillRect(canvas.width - 20, 0, 20, canvas.height);
            
            // Draw striped pattern on finish line
            ctx.strokeStyle = '#0f2027';
            ctx.lineWidth = 2;
            for (let i = 0; i < canvas.height; i += 20) {
                ctx.beginPath();
                ctx.moveTo(canvas.width - 20, i);
                ctx.lineTo(canvas.width, i);
                ctx.stroke();
            }
            
            // Draw obstacles
            obstacles.forEach(obstacle => {
                ctx.fillStyle = '#ff3366';
                ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
                
                // Add a subtle border
                ctx.strokeStyle = '#990033';
                ctx.lineWidth = 2;
                ctx.strokeRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
                
                // Add movement indicator for moving obstacles
                if (obstacle.type !== 'static') {
                    ctx.fillStyle = 'rgba(255, 255, 255, 0.5)';
                    ctx.beginPath();
                    
                    if (obstacle.type === 'horizontal') {
                        ctx.arc(obstacle.x + obstacle.width/2, obstacle.y - 10, 5, 0, Math.PI * 2);
                    } else {
                        ctx.arc(obstacle.x - 10, obstacle.y + obstacle.height/2, 5, 0, Math.PI * 2);
                    }
                    
                    ctx.fill();
                }
            });
            
            // Draw player
            ctx.fillStyle = '#00ffcc';
            ctx.beginPath();
            ctx.arc(playerX, playerY, 20, 0, Math.PI * 2);
            ctx.fill();
            
            // Add glow effect to player
            ctx.shadowColor = '#00ffcc';
            ctx.shadowBlur = 15;
            ctx.beginPath();
            ctx.arc(playerX, playerY, 15, 0, Math.PI * 2);
            ctx.fill();
            ctx.shadowBlur = 0;
            
            // Draw player direction indicator
            ctx.fillStyle = '#0f2027';
            ctx.beginPath();
            ctx.arc(playerX + 8, playerY, 8, 0, Math.PI * 2);
            ctx.fill();
            
            // Draw time pressure indicator (red tint when time is low)
            if (timeLeft < 10) {
                ctx.fillStyle = `rgba(255, 50, 50, ${0.3 - (timeLeft * 0.03)})`;
                ctx.fillRect(0, 0, canvas.width, canvas.height);
            }
        }
        
        // Check collision between player and obstacle
        function checkCollision(px, py, pr, ox, oy, ow, oh) {
            // Find the closest point to the circle within the rectangle
            const closestX = Math.max(ox, Math.min(px, ox + ow));
            const closestY = Math.max(oy, Math.min(py, oy + oh));
            
            // Calculate the distance between the circle's center and this closest point
            const distanceX = px - closestX;
            const distanceY = py - closestY;
            
            // If the distance is less than the circle's radius, there's a collision
            return (distanceX * distanceX + distanceY * distanceY) < (pr * pr);
        }
        
        // End the game
        function endGame(success, message) {
            gameRunning = false;
            clearInterval(timerInterval);
            
            // Update UI
            startBtn.disabled = false;
            pauseBtn.disabled = true;
            
            if (success) {
                resultTitle.textContent = "LEVEL COMPLETE!";
                resultMessage.textContent = `You finished with ${timeLeft} seconds remaining!`;
                nextBtn.disabled = false;
                
                // Mark level as completed
                if (!completedLevels.includes(currentLevel)) {
                    completedLevels.push(currentLevel);
                    localStorage.setItem('timeRushCompletedLevels', JSON.stringify(completedLevels));
                }
                
                // Unlock next level if not already unlocked
                const nextLevelNum = currentLevel + 1;
                if (nextLevelNum <= TOTAL_LEVELS && !unlockedLevels.includes(nextLevelNum)) {
                    unlockedLevels.push(nextLevelNum);
                    localStorage.setItem('timeRushUnlockedLevels', JSON.stringify(unlockedLevels));
                }
            } else {
                resultTitle.textContent = "GAME OVER";
                resultMessage.textContent = message;
                nextBtn.disabled = true;
            }
            
            gameOverElement.classList.remove('hidden');
            createLevelProgress();
        }
        
        // Initialize the game when page loads
        window.onload = init;
    </script>
</body>
</html>
