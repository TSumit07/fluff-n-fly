<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fluffy Sky Adventure - Play Now!</title>
    <meta name="description" content="A fun, cute flappy bird game with easy controls and beautiful graphics!">
    <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><circle cx='50' cy='50' r='40' fill='%23FFD700'/><circle cx='40' cy='40' r='5' fill='%23000'/><path d='M60 50 Q80 40 70 30' stroke='%23000' stroke-width='2' fill='none'/><path d='M50 60 L30 70 L40 80' fill='%23FFA500'/></svg>">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Comic Sans MS', cursive, sans-serif;
            background: linear-gradient(135deg, #56CCF2, #2F80ED);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            overflow: hidden;
            touch-action: manipulation;
        }
        
        #header {
            text-align: center;
            margin-bottom: 10px;
            color: white;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        #game-container {
            position: relative;
            width: 360px;
            height: 600px;
            background: linear-gradient(to bottom, #87CEEB, #E0F7FA);
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
            border: 4px solid #FFA500;
        }
        
        #bird {
            position: absolute;
            width: 50px;
            height: 50px;
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="30" cy="50" r="25" fill="%23FFD700"/><circle cx="25" cy="45" r="5" fill="%23000"/><path d="M60 50 Q80 40 70 30" stroke="%23000" stroke-width="2" fill="none"/><path d="M50 60 L30 70 L40 80" fill="%23FFA500"/></svg>');
            background-size: contain;
            z-index: 10;
            transition: transform 0.1s ease-out;
            filter: drop-shadow(2px 2px 3px rgba(0, 0, 0, 0.3));
        }
        
        .pipe {
            position: absolute;
            width: 70px;
            background-color: #4CAF50;
            border: 3px solid #2E7D32;
            border-radius: 8px;
            z-index: 5;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
        }
        
        .pipe-top {
            top: 0;
            background: linear-gradient(to bottom, #4CAF50, #81C784);
        }
        
        .pipe-bottom {
            bottom: 0;
            background: linear-gradient(to top, #4CAF50, #81C784);
        }
        
        #score {
            position: absolute;
            top: 20px;
            right: 20px;
            font-size: 28px;
            color: white;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
            z-index: 20;
            font-weight: bold;
        }
        
        #game-over {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 30;
            color: white;
            backdrop-filter: blur(3px);
        }
        
        #game-over h1 {
            font-size: 36px;
            margin-bottom: 20px;
            color: #FFD700;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        #restart-btn {
            padding: 12px 25px;
            font-size: 18px;
            background: linear-gradient(to bottom, #FF9800, #F57C00);
            border: none;
            border-radius: 30px;
            cursor: pointer;
            color: white;
            font-weight: bold;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            transition: transform 0.2s, box-shadow 0.2s;
        }
        
        #restart-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
        }
        
        #restart-btn:active {
            transform: translateY(1px);
        }
        
        #clouds {
            position: absolute;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1;
        }
        
        .cloud {
            position: absolute;
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 60"><path d="M20,30 Q30,10 40,30 Q50,0 60,30 Q70,10 80,30 Q90,20 90,40 Q80,50 60,50 Q50,60 40,50 Q20,50 10,40 Q10,20 20,30" fill="white" opacity="0.8"/></svg>');
            background-size: contain;
            background-repeat: no-repeat;
            opacity: 0.9;
        }
        
        #celebration {
            position: absolute;
            width: 100%;
            height: 100%;
            pointer-events: none;
            display: none;
            z-index: 25;
        }
        
        .confetti {
            position: absolute;
            width: 12px;
            height: 12px;
            background-color: #FFD700;
            border-radius: 50%;
            animation: fall 3s linear forwards;
        }
        
        @keyframes fall {
            to {
                transform: translateY(600px) rotate(360deg);
                opacity: 0;
            }
        }
        
        #ground {
            position: absolute;
            bottom: 0;
            width: 100%;
            height: 40px;
            background: linear-gradient(to bottom, #8D6E63, #5D4037);
            border-top: 3px solid #4E342E;
            z-index: 8;
        }
        
        #message {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(255, 255, 255, 0.9);
            padding: 15px 25px;
            border-radius: 10px;
            font-size: 22px;
            display: none;
            z-index: 15;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
            text-align: center;
            animation: pulse 1.5s infinite;
        }
        
        @keyframes pulse {
            0% { transform: translate(-50%, -50%) scale(1); }
            50% { transform: translate(-50%, -50%) scale(1.05); }
            100% { transform: translate(-50%, -50%) scale(1); }
        }
        
        #ad-container {
            width: 360px;
            height: 90px;
            margin: 15px 0;
            background: rgba(255, 255, 255, 0.8);
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 14px;
            color: #555;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        
        #sound-control {
            position: absolute;
            top: 20px;
            left: 20px;
            z-index: 40;
            background: rgba(255, 255, 255, 0.8);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            font-size: 20px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
            transition: transform 0.2s;
        }
        
        #sound-control:hover {
            transform: scale(1.1);
        }
        
        #monetization-notice {
            position: absolute;
            bottom: 10px;
            left: 10px;
            font-size: 10px;
            color: rgba(255, 255, 255, 0.7);
            z-index: 5;
        }
        
        #mobile-controls {
            display: none;
            width: 360px;
            justify-content: center;
            margin-top: 15px;
        }
        
        #jump-btn {
            padding: 15px 40px;
            background: linear-gradient(to bottom, #FF9800, #F57C00);
            border: none;
            border-radius: 30px;
            color: white;
            font-size: 18px;
            font-weight: bold;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            cursor: pointer;
        }
        
        @media (max-width: 500px) {
            #mobile-controls {
                display: flex;
            }
        }
    </style>
</head>
<body>
    <div id="header">
        <h1>Fluffy Sky Adventure</h1>
    </div>
    
    <div id="game-container">
        <div id="sound-control">🔊</div>
        <div id="clouds"></div>
        <div id="bird"></div>
        <div id="ground"></div>
        <div id="score">0</div>
        <div id="message">Tap to Start!</div>
        <div id="celebration"></div>
        <div id="game-over">
            <h1>Game Over!</h1>
            <p id="final-score">Score: 0</p>
            <button id="restart-btn">Play Again</button>
        </div>
        <div id="monetization-notice">This game contains ads</div>
    </div>
    
    <div id="ad-container">
        Ad Space (Supports the game!)
    </div>
    
    <div id="mobile-controls">
        <button id="jump-btn">FLAP!</button>
    </div>
    
    <!-- Audio Elements -->
    <audio id="flap-sound" src="https://assets.mixkit.co/sfx/preview/mixkit-bird-whistling-2290.mp3" preload="auto"></audio>
    <audio id="score-sound" src="https://assets.mixkit.co/sfx/preview/mixkit-achievement-bell-600.mp3" preload="auto"></audio>
    <audio id="gameover-sound" src="https://assets.mixkit.co/sfx/preview/mixkit-retro-arcade-lose-2027.mp3" preload="auto"></audio>
    <audio id="background-music" loop src="https://assets.mixkit.co/music/preview/mixkit-happy-times-1580.mp3" preload="auto"></audio>
    <audio id="celebration-sound" src="https://assets.mixkit.co/sfx/preview/mixkit-winning-chimes-2015.mp3" preload="auto"></audio>

    <script>
        // ========== GAME SETUP ========== //
        const gameContainer = document.getElementById('game-container');
        const bird = document.getElementById('bird');
        const scoreElement = document.getElementById('score');
        const gameOverScreen = document.getElementById('game-over');
        const finalScoreElement = document.getElementById('final-score');
        const restartBtn = document.getElementById('restart-btn');
        const messageElement = document.getElementById('message');
        const celebrationElement = document.getElementById('celebration');
        const cloudsContainer = document.getElementById('clouds');
        const ground = document.getElementById('ground');
        const soundControl = document.getElementById('sound-control');
        const adContainer = document.getElementById('ad-container');
        const jumpBtn = document.getElementById('jump-btn');
        
        // Audio elements
        const flapSound = document.getElementById('flap-sound');
        const scoreSound = document.getElementById('score-sound');
        const gameoverSound = document.getElementById('gameover-sound');
        const backgroundMusic = document.getElementById('background-music');
        const celebrationSound = document.getElementById('celebration-sound');
        
        // Game settings (easier gameplay)
        let score = 0;
        let gameRunning = false;
        let birdY = 300;
        let birdVelocity = 0;
        const gravity = 0.4; // Reduced gravity for easier control
        let pipes = [];
        let pipeTimer = 0;
        const pipeInterval = 1800; // More time between pipes
        let lastTime = 0;
        let gameSpeed = 1.5; // Slower initial speed
        let animationId;
        let cloudTimer = 0;
        const cloudInterval = 3000;
        let soundEnabled = true;
        let adTimer = 0;
        const adInterval = 30000; // Show ad every 30 seconds
        const gapHeight = 220; // Larger gap between pipes
        
        // Game dimensions
        const gameWidth = 360;
        const gameHeight = 600;
        const birdWidth = 50;
        const birdHeight = 50;
        const pipeWidth = 70;
        
        // ========== GAME FUNCTIONS ========== //
        function init() {
            score = 0;
            birdY = gameHeight / 2;
            birdVelocity = 0;
            pipes = [];
            scoreElement.textContent = score;
            
            bird.style.left = '80px';
            bird.style.top = birdY + 'px';
            bird.style.transform = 'rotate(0deg)';
            
            gameOverScreen.style.display = 'none';
            messageElement.style.display = 'block';
            
            // Clear existing pipes
            document.querySelectorAll('.pipe').forEach(pipe => pipe.remove());
            
            // Create initial clouds
            createClouds();
            
            // Reset ad timer
            adTimer = 0;
        }
        
        function createClouds() {
            cloudsContainer.innerHTML = '';
            for (let i = 0; i < 5; i++) {
                addCloud();
            }
        }
        
        function addCloud() {
            const cloud = document.createElement('div');
            cloud.className = 'cloud';
            const size = Math.random() * 120 + 60;
            cloud.style.width = size + 'px';
            cloud.style.height = size / 2 + 'px';
            cloud.style.left = Math.random() * gameWidth + 'px';
            cloud.style.top = Math.random() * (gameHeight / 3) + 'px';
            cloud.style.opacity = Math.random() * 0.6 + 0.3;
            cloud.style.animationDuration = (Math.random() * 30 + 20) + 's';
            cloudsContainer.appendChild(cloud);
        }
        
        function createPipe() {
            const minHeight = 60;
            const maxHeight = gameHeight - gapHeight - minHeight - 40; // 40 is ground height
            const topHeight = Math.floor(Math.random() * (maxHeight - minHeight + 1)) + minHeight;
            
            const topPipe = document.createElement('div');
            topPipe.className = 'pipe pipe-top';
            topPipe.style.left = gameWidth + 'px';
            topPipe.style.top = '0';
            topPipe.style.height = topHeight + 'px';
            gameContainer.appendChild(topPipe);
            
            const bottomPipe = document.createElement('div');
            bottomPipe.className = 'pipe pipe-bottom';
            bottomPipe.style.left = gameWidth + 'px';
            bottomPipe.style.bottom = '40px';
            bottomPipe.style.height = (gameHeight - topHeight - gapHeight - 40) + 'px';
            gameContainer.appendChild(bottomPipe);
            
            pipes.push({
                top: topPipe,
                bottom: bottomPipe,
                x: gameWidth,
                passed: false
            });
        }
        
        function movePipes(deltaTime) {
            for (let i = pipes.length - 1; i >= 0; i--) {
                const pipe = pipes[i];
                pipe.x -= gameSpeed * deltaTime / 16;
                
                pipe.top.style.left = pipe.x + 'px';
                pipe.bottom.style.left = pipe.x + 'px';
                
                // Check if pipe is passed
                if (!pipe.passed && pipe.x + pipeWidth < 80) {
                    pipe.passed = true;
                    increaseScore();
                    if (score % 5 === 0) createCelebration();
                }
                
                // Remove off-screen pipes
                if (pipe.x + pipeWidth < 0) {
                    pipe.top.remove();
                    pipe.bottom.remove();
                    pipes.splice(i, 1);
                }
            }
        }
        
        function increaseScore() {
            score++;
            scoreElement.textContent = score;
            
            if (soundEnabled) {
                scoreSound.currentTime = 0;
                scoreSound.play().catch(e => console.log("Audio play prevented:", e));
            }
            
            // Gradually increase difficulty
            if (score % 5 === 0) {
                gameSpeed += 0.2;
                showMessage(["Great!", "Awesome!", "You're amazing!"][Math.floor(Math.random() * 3)]);
            }
        }
        
        function showMessage(text) {
            messageElement.textContent = text;
            messageElement.style.display = 'block';
            setTimeout(() => {
                if (gameRunning) messageElement.style.display = 'none';
            }, 1000);
        }
        
        function createCelebration() {
            celebrationElement.style.display = 'block';
            celebrationElement.innerHTML = '';
            
            if (soundEnabled) {
                celebrationSound.currentTime = 0;
                celebrationSound.play().catch(e => console.log("Audio play prevented:", e));
            }
            
            for (let i = 0; i < 30; i++) {
                const confetti = document.createElement('div');
                confetti.className = 'confetti';
                confetti.style.left = Math.random() * gameWidth + 'px';
                confetti.style.top = '-20px';
                confetti.style.backgroundColor = `hsl(${Math.random() * 360}, 100%, 50%)`;
                confetti.style.width = `${Math.random() * 10 + 8}px`;
                confetti.style.height = `${Math.random() * 10 + 8}px`;
                confetti.style.borderRadius = Math.random() > 0.5 ? '50%' : '0';
                celebrationElement.appendChild(confetti);
            }
            
            setTimeout(() => {
                celebrationElement.style.display = 'none';
            }, 3000);
        }
        
        function updateBird(deltaTime) {
            birdVelocity += gravity * deltaTime / 16;
            birdY += birdVelocity;
            
            // Rotate bird based on velocity
            let rotation = Math.min(Math.max(birdVelocity * 5, -25), 25);
            bird.style.transform = `rotate(${rotation}deg)`;
            
            bird.style.top = birdY + 'px';
            
            // Check boundaries
            if (birdY < 0) {
                birdY = 0;
                birdVelocity = 0;
            }
            
            if (birdY + birdHeight > gameHeight - 40) {
                endGame();
            }
        }
        
        function checkCollisions() {
            const birdRect = {
                x: 80,
                y: birdY,
                width: birdWidth,
                height: birdHeight
            };
            
            for (const pipe of pipes) {
                const topPipeRect = {
                    x: pipe.x,
                    y: 0,
                    width: pipeWidth,
                    height: parseFloat(pipe.top.style.height)
                };
                
                const bottomPipeRect = {
                    x: pipe.x,
                    y: gameHeight - parseFloat(pipe.bottom.style.height) - 40,
                    width: pipeWidth,
                    height: parseFloat(pipe.bottom.style.height)
                };
                
                if (isColliding(birdRect, topPipeRect) || isColliding(birdRect, bottomPipeRect)) {
                    endGame();
                    break;
                }
            }
        }
        
        function isColliding(rect1, rect2) {
            return (
                rect1.x < rect2.x + rect2.width &&
                rect1.x + rect1.width > rect2.x &&
                rect1.y < rect2.y + rect2.height &&
                rect1.y + rect1.height > rect2.y
            );
        }
        
        function endGame() {
            gameRunning = false;
            cancelAnimationFrame(animationId);
            
            if (soundEnabled) {
                gameoverSound.currentTime = 0;
                gameoverSound.play().catch(e => console.log("Audio play prevented:", e));
                backgroundMusic.pause();
            }
            
            pipes.forEach(pipe => {
                pipe.top.remove();
                pipe.bottom.remove();
            });
            pipes = [];
            
            finalScoreElement.textContent = `Score: ${score}`;
            gameOverScreen.style.display = 'flex';
            
            // Show ad after game over (if score > 3)
            if (score > 3) {
                setTimeout(() => {
                    showInterstitialAd();
                }, 1000);
            }
        }
        
        function showInterstitialAd() {
            // Simulated ad - replace with real ad code
            adContainer.innerHTML = `
                <div style="padding: 10px; text-align: center;">
                    <h3>Thanks for playing!</h3>
                    <p>Support us by watching an ad</p>
                    <button onclick="closeAd()" style="padding: 8px 15px; background: #FF9800; border: none; border-radius: 5px; margin-top: 10px;">Close Ad</button>
                </div>
            `;
            adContainer.style.height = "120px";
            
            // In a real game, you would load an actual ad here
        }
        
        function closeAd() {
            adContainer.innerHTML = "Ad Space (Supports the game!)";
            adContainer.style.height = "90px";
        }
        
        function gameLoop(timestamp) {
            if (!lastTime) lastTime = timestamp;
            const deltaTime = timestamp - lastTime;
            lastTime = timestamp;
            
            if (gameRunning) {
                updateBird(deltaTime);
                movePipes(deltaTime);
                checkCollisions();
                moveClouds(deltaTime);
                
                pipeTimer += deltaTime;
                if (pipeTimer >= pipeInterval) {
                    createPipe();
                    pipeTimer = 0;
                }
                
                cloudTimer += deltaTime;
                if (cloudTimer >= cloudInterval) {
                    addCloud();
                    cloudTimer = 0;
                }
                
                adTimer += deltaTime;
                if (adTimer >= adInterval) {
                    showInterstitialAd();
                    adTimer = 0;
                }
            }
            
            animationId = requestAnimationFrame(gameLoop);
        }
        
        function jump() {
            if (!gameRunning) {
                startGame();
                return;
            }
            
            birdVelocity = -8; // Reduced jump force for easier control
            
            if (soundEnabled) {
                flapSound.currentTime = 0;
                flapSound.play().catch(e => console.log("Audio play prevented:", e));
            }
        }
        
        function startGame() {
            gameRunning = true;
            messageElement.style.display = 'none';
            init();
            lastTime = 0;
            gameSpeed = 1.5;
            
            if (soundEnabled) {
                backgroundMusic.currentTime = 0;
                backgroundMusic.play().catch(e => console.log("Audio play prevented:", e));
            }
        }
        
        // ========== EVENT LISTENERS ========== //
        soundControl.addEventListener('click', () => {
            soundEnabled = !soundEnabled;
            soundControl.textContent = soundEnabled ? '🔊' : '🔇';
            
            if (soundEnabled) {
                backgroundMusic.play().catch(e => console.log("Audio play prevented:", e));
            } else {
                backgroundMusic.pause();
            }
        });
        
        gameContainer.addEventListener('click', jump);
        jumpBtn.addEventListener('click', jump);
        document.addEventListener('keydown', (e) => {
            if (e.code === 'Space') jump();
        });
        
        restartBtn.addEventListener('click', () => {
            startGame();
            animationId = requestAnimationFrame(gameLoop);
        });
        
        // ========== INITIALIZE GAME ========== //
        init();
        animationId = requestAnimationFrame(gameLoop);
    </script>
</body>
</html>
