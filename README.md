index.html
<!doctype html>
<html lang="es">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Super Heroes Memory Game</title>
  <style>
        body {
            box-sizing: border-box;
            font-family: 'Comic Sans MS', cursive, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            margin: 0;
            padding: 20px;
            min-height: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
            color: white;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
        }

        .header h1 {
            font-size: 3rem;
            margin: 0;
            background: linear-gradient(45deg, #FFD700, #FFA500);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .stats {
            display: flex;
            gap: 30px;
            margin-bottom: 20px;
            background: rgba(255,255,255,0.1);
            padding: 15px 30px;
            border-radius: 15px;
            backdrop-filter: blur(10px);
        }

        .stat {
            text-align: center;
            color: white;
            font-weight: bold;
        }

        .stat-value {
            font-size: 2rem;
            display: block;
            color: #FFD700;
        }

        .game-container {
            background: rgba(255,255,255,0.1);
            padding: 30px;
            border-radius: 20px;
            backdrop-filter: blur(10px);
            box-shadow: 0 8px 32px rgba(0,0,0,0.3);
        }

        .game-board {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
            max-width: 600px;
            margin: 0 auto;
        }

        .card {
            width: 120px;
            height: 120px;
            background: linear-gradient(145deg, #ffffff, #e6e6e6);
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            position: relative;
            overflow: hidden;
        }

        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0,0,0,0.3);
        }

        .card.flipped {
            background: linear-gradient(145deg, #4facfe, #00f2fe);
            transform: rotateY(180deg);
        }

        .card.matched {
            background: linear-gradient(145deg, #56ab2f, #a8e6cf);
            animation: pulse 0.6s ease-in-out;
        }

        .card-content {
            font-size: 3rem;
            transition: all 0.3s ease;
        }

        .card.flipped .card-content {
            transform: rotateY(180deg);
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        .controls {
            text-align: center;
            margin-top: 30px;
        }

        .btn {
            background: linear-gradient(145deg, #ff6b6b, #ee5a24);
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 1.2rem;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            margin: 0 10px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.3);
        }

        .victory-message {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: linear-gradient(145deg, #FFD700, #FFA500);
            color: #333;
            padding: 40px;
            border-radius: 20px;
            text-align: center;
            box-shadow: 0 10px 40px rgba(0,0,0,0.5);
            display: none;
            z-index: 1000;
        }

        .victory-message h2 {
            margin: 0 0 20px 0;
            font-size: 2.5rem;
        }

        .victory-stats {
            font-size: 1.3rem;
            margin: 20px 0;
        }

        .premium-btn {
            background: linear-gradient(145deg, #FFD700, #FFA500) !important;
            color: #333 !important;
            font-weight: bold;
            animation: glow 2s ease-in-out infinite alternate;
        }

        @keyframes glow {
            from { box-shadow: 0 4px 15px rgba(255, 215, 0, 0.4); }
            to { box-shadow: 0 4px 25px rgba(255, 215, 0, 0.8); }
        }

        .premium-panel {
            background: linear-gradient(145deg, #FFD700, #FFA500);
            color: #333;
            padding: 30px;
            border-radius: 20px;
            margin-top: 20px;
            text-align: center;
        }

        .premium-features {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin: 20px 0;
        }

        .feature {
            background: rgba(255,255,255,0.2);
            padding: 10px;
            border-radius: 10px;
            font-weight: bold;
        }

        .difficulty-selector {
            margin-top: 20px;
        }

        .premium-only {
            position: relative;
        }

        .premium-only:not(.unlocked) {
            opacity: 0.6;
            cursor: not-allowed;
        }

        .premium-only:not(.unlocked):hover {
            transform: none;
        }

        .locked-overlay {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0,0,0,0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 15px;
            color: white;
            font-size: 2rem;
        }

        @media (max-width: 768px) {
            .game-board {
                grid-template-columns: repeat(3, 1fr);
                gap: 10px;
            }
            
            .card {
                width: 90px;
                height: 90px;
            }
            
            .card-content {
                font-size: 2rem;
            }
            
            .header h1 {
                font-size: 2rem;
            }
            
            .stats {
                gap: 15px;
                padding: 10px 20px;
            }

            .premium-features {
                grid-template-columns: 1fr;
            }
        }
    </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="/_sdk/element_sdk.js" type="text/javascript"></script>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
  <header class="header">
   <h1>🦸‍♂️ SUPER HEROES MEMORY 🦸‍♀️</h1>
   <p>¡Encuentra las parejas de superhéroes!</p>
  </header>
  <div class="stats">
   <div class="stat"><span class="stat-value" id="moves">0</span>
    <div>
     Movimientos
    </div>
   </div>
   <div class="stat"><span class="stat-value" id="matches">0</span>
    <div>
     Parejas
    </div>
   </div>
   <div class="stat"><span class="stat-value" id="timer">00:00</span>
    <div>
     Tiempo
    </div>
   </div>
   <div class="stat"><span class="stat-value" id="score">0</span>
    <div>
     Puntos
    </div>
   </div>
  </div>
  <div class="game-container">
   <div class="game-board" id="gameBoard"></div>
   <div class="controls"><button class="btn" onclick="startNewGame()">🔄 Nuevo Juego</button> <button class="btn" onclick="showHint()">💡 Pista</button> <button class="btn premium-btn" onclick="togglePremium()" id="premiumBtn"> 🌟 Desbloquear Premium - $2.99 </button>
   </div><!-- Panel Premium -->
   <div class="premium-panel" id="premiumPanel" style="display: none;">
    <h3>🌟 VERSIÓN PREMIUM DESBLOQUEADA 🌟</h3>
    <div class="premium-features">
     <div class="feature">
      ✅ 12 superhéroes adicionales
     </div>
     <div class="feature">
      ✅ Modo difícil (6x6 cartas)
     </div>
     <div class="feature">
      ✅ Sin límite de pistas
     </div>
     <div class="feature">
      ✅ Temas especiales
     </div>
     <div class="feature">
      ✅ Estadísticas avanzadas
     </div>
    </div>
    <div class="difficulty-selector"><button class="btn" onclick="setDifficulty('easy')">Fácil (4x3)</button> <button class="btn" onclick="setDifficulty('normal')">Normal (4x4)</button> <button class="btn premium-only" onclick="setDifficulty('hard')">Difícil (6x4) 🌟</button> <button class="btn premium-only" onclick="setDifficulty('expert')">Experto (6x6) 🌟</button>
    </div>
   </div>
  </div>
  <div class="victory-message" id="victoryMessage">
   <h2>🎉 ¡FELICITACIONES! 🎉</h2>
   <div class="victory-stats" id="victoryStats"></div><button class="btn" onclick="startNewGame()">Jugar de Nuevo</button>
  </div>
  <script>
        // Superhéroes expandidos
        // Superhéroes básicos (gratis)
        const basicHeroes = [
            '🦸‍♂️', '🦸‍♀️', '🕷️', '🦇', 
            '⚡', '🔥'
        ];

        // Superhéroes premium
        const premiumHeroes = [
            '❄️', '🌟', '💎', '🛡️', '⚔️', '🏹',
            '🌊', '🌪️', '🔮', '👑', '🗲', '🌙'
        ];

        let isPremium = localStorage.getItem('isPremium') === 'true';

        let gameState = {
            cards: [],
            flippedCards: [],
            matchedPairs: 0,
            moves: 0,
            score: 0,
            timer: 0,
            gameStarted: false,
            timerInterval: null,
            difficulty: 'normal',
            hintsUsed: 0
        };

        function initGame() {
            // Seleccionar superhéroes según versión
            let availableHeroes = [...basicHeroes];
            if (isPremium) {
                availableHeroes = [...basicHeroes, ...premiumHeroes];
            }

            // Ajustar cantidad según dificultad
            const difficultySettings = {
                easy: { pairs: 6, cols: 4, rows: 3 },
                normal: { pairs: 8, cols: 4, rows: 4 },
                hard: { pairs: 12, cols: 6, rows: 4 },
                expert: { pairs: 18, cols: 6, rows: 6 }
            };

            const settings = difficultySettings[gameState.difficulty];
            const selectedHeroes = availableHeroes.slice(0, settings.pairs);
            
            // Crear pares de cartas
            const cardPairs = [...selectedHeroes, ...selectedHeroes];
            gameState.cards = shuffleArray(cardPairs);
            
            // Reset game state
            gameState.flippedCards = [];
            gameState.matchedPairs = 0;
            gameState.moves = 0;
            gameState.score = 0;
            gameState.timer = 0;
            gameState.gameStarted = false;
            gameState.hintsUsed = 0;
            
            if (gameState.timerInterval) {
                clearInterval(gameState.timerInterval);
            }
            
            updateStats();
            renderBoard();
            updatePremiumUI();
            document.getElementById('victoryMessage').style.display = 'none';
        }

        function shuffleArray(array) {
            const shuffled = [...array];
            for (let i = shuffled.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
            }
            return shuffled;
        }

        function renderBoard() {
            const board = document.getElementById('gameBoard');
            board.innerHTML = '';
            
            // Configurar grid según dificultad
            const difficultySettings = {
                easy: { cols: 4 },
                normal: { cols: 4 },
                hard: { cols: 6 },
                expert: { cols: 6 }
            };
            
            const cols = difficultySettings[gameState.difficulty].cols;
            board.style.gridTemplateColumns = `repeat(${cols}, 1fr)`;
            
            gameState.cards.forEach((hero, index) => {
                const card = document.createElement('div');
                card.className = 'card';
                
                // Mostrar contenido premium bloqueado
                if (!isPremium && premiumHeroes.includes(hero)) {
                    card.innerHTML = `
                        <div class="card-content">❓</div>
                        <div class="locked-overlay">🔒</div>
                    `;
                } else {
                    card.innerHTML = `<div class="card-content">❓</div>`;
                    card.addEventListener('click', () => flipCard(index));
                }
                
                board.appendChild(card);
            });
        }

        function flipCard(index) {
            if (!gameState.gameStarted) {
                startTimer();
                gameState.gameStarted = true;
            }

            const card = document.querySelectorAll('.card')[index];
            
            // No permitir flip si ya está volteada o emparejada
            if (card.classList.contains('flipped') || 
                card.classList.contains('matched') || 
                gameState.flippedCards.length >= 2) {
                return;
            }

            // Voltear carta
            card.classList.add('flipped');
            card.querySelector('.card-content').textContent = gameState.cards[index];
            gameState.flippedCards.push(index);

            // Verificar si hay 2 cartas volteadas
            if (gameState.flippedCards.length === 2) {
                gameState.moves++;
                updateStats();
                
                setTimeout(() => {
                    checkMatch();
                }, 1000);
            }
        }

        function checkMatch() {
            const [first, second] = gameState.flippedCards;
            const firstCard = document.querySelectorAll('.card')[first];
            const secondCard = document.querySelectorAll('.card')[second];

            if (gameState.cards[first] === gameState.cards[second]) {
                // ¡Pareja encontrada!
                firstCard.classList.add('matched');
                secondCard.classList.add('matched');
                firstCard.classList.remove('flipped');
                secondCard.classList.remove('flipped');
                
                gameState.matchedPairs++;
                gameState.score += 100;
                
                // Bonus por velocidad
                if (gameState.moves <= gameState.matchedPairs * 2) {
                    gameState.score += 50;
                }
                
                updateStats();
                
                // Verificar victoria
                if (gameState.matchedPairs === superheroes.length) {
                    setTimeout(() => {
                        showVictory();
                    }, 500);
                }
            } else {
                // No coinciden
                setTimeout(() => {
                    firstCard.classList.remove('flipped');
                    secondCard.classList.remove('flipped');
                    firstCard.querySelector('.card-content').textContent = '❓';
                    secondCard.querySelector('.card-content').textContent = '❓';
                }, 500);
            }

            gameState.flippedCards = [];
        }

        function startTimer() {
            gameState.timerInterval = setInterval(() => {
                gameState.timer++;
                updateStats();
            }, 1000);
        }

        function updateStats() {
            document.getElementById('moves').textContent = gameState.moves;
            document.getElementById('matches').textContent = gameState.matchedPairs;
            document.getElementById('score').textContent = gameState.score;
            
            const minutes = Math.floor(gameState.timer / 60);
            const seconds = gameState.timer % 60;
            document.getElementById('timer').textContent = 
                `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }

        function showVictory() {
            clearInterval(gameState.timerInterval);
            
            const finalScore = gameState.score - (gameState.timer * 2); // Penalización por tiempo
            const efficiency = Math.round((gameState.matchedPairs * 2 / gameState.moves) * 100);
            
            document.getElementById('victoryStats').innerHTML = `
                <div>⏱️ Tiempo: ${document.getElementById('timer').textContent}</div>
                <div>🎯 Movimientos: ${gameState.moves}</div>
                <div>📊 Eficiencia: ${efficiency}%</div>
                <div>🏆 Puntuación Final: ${Math.max(finalScore, 0)}</div>
            `;
            
            document.getElementById('victoryMessage').style.display = 'block';
        }

        function startNewGame() {
            initGame();
        }

        function showHint() {
            // Límite de pistas para usuarios gratuitos
            if (!isPremium && gameState.hintsUsed >= 3) {
                alert('🌟 ¡Límite de pistas alcanzado! Desbloquea Premium para pistas ilimitadas.');
                return;
            }
            
            if (gameState.moves < 3) return; // Solo después de 3 movimientos
            
            const unmatched = [];
            document.querySelectorAll('.card').forEach((card, index) => {
                if (!card.classList.contains('matched') && !card.querySelector('.locked-overlay')) {
                    unmatched.push(index);
                }
            });
            
            if (unmatched.length >= 2) {
                const randomIndex = unmatched[Math.floor(Math.random() * unmatched.length)];
                const card = document.querySelectorAll('.card')[randomIndex];
                
                card.style.border = '3px solid #FFD700';
                card.style.animation = 'pulse 1s ease-in-out 3';
                
                setTimeout(() => {
                    card.style.border = '';
                    card.style.animation = '';
                }, 3000);
                
                gameState.hintsUsed++;
                gameState.score = Math.max(0, gameState.score - 25); // Penalización por pista
                updateStats();
            }
        }

        function togglePremium() {
            if (isPremium) {
                // Ya es premium, mostrar panel
                const panel = document.getElementById('premiumPanel');
                panel.style.display = panel.style.display === 'none' ? 'block' : 'none';
            } else {
                // Simular compra (en producción sería Stripe/PayPal)
                if (confirm('🌟 ¿Desbloquear Premium por $2.99?\n\n✅ 12 superhéroes adicionales\n✅ Modo difícil y experto\n✅ Pistas ilimitadas\n✅ Temas especiales')) {
                    isPremium = true;
                    localStorage.setItem('isPremium', 'true');
                    updatePremiumUI();
                    initGame();
                    alert('🎉 ¡Premium desbloqueado! Disfruta todas las funciones.');
                }
            }
        }

        function updatePremiumUI() {
            const btn = document.getElementById('premiumBtn');
            const panel = document.getElementById('premiumPanel');
            
            if (isPremium) {
                btn.textContent = '🌟 Premium Activo';
                btn.style.background = 'linear-gradient(145deg, #56ab2f, #a8e6cf)';
                panel.style.display = 'block';
                
                // Habilitar botones premium
                document.querySelectorAll('.premium-only').forEach(btn => {
                    btn.classList.add('unlocked');
                    btn.disabled = false;
                });
            } else {
                btn.textContent = '🌟 Desbloquear Premium - $2.99';
                panel.style.display = 'none';
                
                // Deshabilitar botones premium
                document.querySelectorAll('.premium-only').forEach(btn => {
                    btn.classList.remove('unlocked');
                    btn.disabled = true;
                });
            }
        }

        function setDifficulty(level) {
            if ((level === 'hard' || level === 'expert') && !isPremium) {
                alert('🌟 Esta dificultad requiere Premium. ¡Desbloquéalo por solo $2.99!');
                return;
            }
            
            gameState.difficulty = level;
            initGame();
        }

        // Inicializar juego al cargar
        window.addEventListener('load', () => {
            initGame();
            updatePremiumUI();
        });
    </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'995d4be747f3db36',t:'MTc2MTY4NDUzMi4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
