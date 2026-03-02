
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arcade Hub - XO Ultimate</title>
    
    <style>
        *{margin:0;padding:0;box-sizing:border-box}
        :root{--neon-blue:#00f3ff;--neon-pink:#ff00ff;--neon-green:#00ff88;--neon-yellow:#ffff00;--dark-bg:#0a0a1a}
        body{font-family:system-ui,-apple-system,sans-serif;background:var(--dark-bg);color:#fff;min-height:100vh}
        .bg-animation{position:fixed;inset:0;z-index:-1;background:radial-gradient(circle at 20% 80%,rgba(0,243,255,.15) 0%,transparent 50%),radial-gradient(circle at 80% 20%,rgba(255,0,255,.15) 0%,transparent 50%)}
        
        .header-container{display:flex;justify-content:space-between;align-items:center;padding:1rem;max-width:1200px;margin:0 auto}
        .settings-btn{width:40px;height:40px;border-radius:50%;background:rgba(255,255,255,.1);border:1px solid rgba(255,255,255,.2);color:#fff;font-size:1.2rem;cursor:pointer;transition:all .3s;display:flex;align-items:center;justify-content:center}
        .settings-btn:hover{background:var(--neon-blue)}
        
        header{text-align:center;padding:1rem;flex:1}
        .logo{font-size:clamp(1.5rem,6vw,3rem);font-weight:900;text-transform:uppercase;background:linear-gradient(135deg,var(--neon-blue),var(--neon-pink));-webkit-background-clip:text;-webkit-text-fill-color:transparent}
        .subtitle{opacity:.7;text-transform:uppercase;letter-spacing:.3em;font-size:.7rem}
        
        .games-showcase{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:1rem;max-width:1200px;margin:0 auto;padding:1rem}
        .game-tile{height:180px;border-radius:15px;cursor:pointer;transition:all .3s;border:1px solid rgba(255,255,255,.1);background:linear-gradient(135deg,#1a1a2e,#16213e);display:flex;flex-direction:column;align-items:center;justify-content:center}
        .game-tile:hover{transform:translateY(-5px);border-color:var(--neon-blue)}
        .game-icon-wrapper{width:60px;height:60px;border-radius:15px;display:flex;align-items:center;justify-content:center;font-size:2rem;margin-bottom:.5rem;background:rgba(255,255,255,.05)}
        .game-title{font-size:1rem;font-weight:700;text-transform:uppercase}
        
        .modal{position:fixed;inset:0;background:rgba(10,10,26,.98);z-index:1000;display:none;flex-direction:column}
        .modal.active{display:flex}
        .modal-header{display:flex;justify-content:space-between;align-items:center;padding:1rem;border-bottom:1px solid rgba(255,255,255,.1)}
        .modal-title{font-size:1.3rem;color:var(--neon-blue)}
        .close-btn{width:35px;height:35px;border-radius:50%;background:rgba(255,255,255,.1);border:1px solid rgba(255,255,255,.2);color:#fff;cursor:pointer;font-size:1.2rem}
        .close-btn:hover{background:#ff4444}
        
        .modal-content{flex:1;display:flex;flex-direction:column;align-items:center;padding:1rem;overflow-y:auto}
        .game-board-container{width:100%;max-width:500px;background:rgba(255,255,255,.05);border-radius:15px;padding:1.5rem;border:1px solid rgba(255,255,255,.1)}
        
        /* Pre-Game Menu Styles */
        .pre-game-menu{width:100%;max-width:500px;text-align:center}
        .menu-section{background:rgba(255,255,255,.05);border-radius:15px;padding:1.5rem;margin-bottom:1rem;border:1px solid rgba(255,255,255,.1)}
        .menu-title{font-size:1.2rem;color:var(--neon-blue);margin-bottom:1rem;text-transform:uppercase;letter-spacing:.1em}
        .mode-options{display:flex;gap:1rem;justify-content:center;margin-bottom:1rem}
        .mode-option{flex:1;padding:1rem;border:2px solid rgba(255,255,255,.2);background:rgba(0,0,0,.3);border-radius:10px;cursor:pointer;transition:all .3s}
        .mode-option:hover{border-color:var(--neon-blue)}
        .mode-option.active{border-color:var(--neon-green);background:rgba(0,255,136,.1)}
        .mode-option .icon{font-size:2rem;margin-bottom:.5rem}
        .mode-option .label{font-weight:700;font-size:.9rem}
        
        .difficulty-options{display:flex;gap:.5rem;justify-content:center}
        .diff-option{flex:1;padding:.8rem;border:1px solid rgba(255,255,255,.2);background:rgba(0,0,0,.3);border-radius:8px;cursor:pointer;transition:all .3s;font-weight:600}
        .diff-option:hover{border-color:var(--neon-pink)}
        .diff-option.active{border-color:var(--neon-pink);background:rgba(255,0,255,.1);color:var(--neon-pink)}
        
        .size-options{display:flex;gap:.5rem;justify-content:center;margin-top:1rem}
        .size-option{padding:.5rem 1rem;border:1px solid rgba(255,255,255,.2);background:rgba(0,0,0,.3);border-radius:8px;cursor:pointer;transition:all .3s;font-weight:600}
        .size-option:hover{border-color:var(--neon-blue)}
        .size-option.active{border-color:var(--neon-blue);background:rgba(0,243,255,.1);color:var(--neon-blue)}
        
        .start-game-btn{padding:1rem 2rem;background:linear-gradient(135deg,var(--neon-blue),var(--neon-pink));border:none;color:#fff;border-radius:25px;font-weight:700;cursor:pointer;width:100%;font-size:1.1rem;margin-top:1rem;transition:all .3s}
        .start-game-btn:hover{transform:scale(1.02);box-shadow:0 0 30px rgba(0,243,255,.3)}
        .start-game-btn:disabled{opacity:.5;cursor:not-allowed;transform:none}
        
        .back-btn{padding:.8rem 1.5rem;background:rgba(255,255,255,.1);border:1px solid rgba(255,255,255,.2);color:#fff;border-radius:25px;font-weight:600;cursor:pointer;width:100%;margin-top:.5rem;transition:all .3s}
        .back-btn:hover{background:rgba(255,255,255,.2)}
        
        /* XO Styles */
        .score-board{display:flex;justify-content:space-around;margin-bottom:1rem;padding:.5rem;background:rgba(0,0,0,.3);border-radius:10px}
        .score-item{text-align:center}
        .score-value{font-size:1.2rem;font-weight:700;color:var(--neon-blue)}
        .score-label{font-size:.6rem;opacity:.7;text-transform:uppercase}
        
        .streak-display{text-align:center;margin-bottom:1rem;font-size:1.1rem;color:var(--neon-yellow);font-weight:700;min-height:1.5rem}
        
        .coin-display{position:fixed;top:1rem;right:1rem;background:rgba(0,0,0,.8);padding:.5rem 1rem;border-radius:20px;border:2px solid gold;display:flex;align-items:center;gap:.5rem;z-index:100;font-weight:700;color:gold;font-size:1rem}
        .coin-display::before{content:"🪙"}
        
        .xo-status{text-align:center;margin-bottom:1rem;font-size:1.1rem;color:var(--neon-yellow);min-height:1.5rem;font-weight:600;transition:all .3s}
        .xo-status.ai-turn{color:var(--neon-pink)}
        .xo-status.player-turn{color:var(--neon-green)}
        .xo-status.player2-turn{color:var(--neon-blue)}
        
        .xo-board{display:grid;gap:8px;margin:1rem auto;max-width:100%;aspect-ratio:1}
        .xo-board.size-3{grid-template-columns:repeat(3,1fr)}
        .xo-board.size-4{grid-template-columns:repeat(4,1fr)}
        .xo-board.size-5{grid-template-columns:repeat(5,1fr)}
        .xo-board.size-6{grid-template-columns:repeat(6,1fr)}
        
        .xo-cell{background:rgba(255,255,255,.1);border:2px solid rgba(255,255,255,.2);border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:2rem;cursor:pointer;transition:all .3s;aspect-ratio:1}
        .xo-cell:hover:not(.taken):not(.disabled){background:rgba(0,243,255,.2);border-color:var(--neon-blue);transform:scale(1.05)}
        .xo-cell.taken{cursor:not-allowed}
        .xo-cell.disabled{cursor:not-allowed;opacity:.7}
        .xo-cell.x{color:var(--neon-pink);text-shadow:0 0 20px var(--neon-pink);animation:placePiece 0.3s ease}
        .xo-cell.o{color:var(--neon-blue);text-shadow:0 0 20px var(--neon-blue);animation:placePiece 0.3s ease}
        @keyframes placePiece{0%{transform:scale(0)}100%{transform:scale(1)}}
        .xo-cell.win{background:rgba(0,255,136,.3)!important;border-color:var(--neon-green)!important;box-shadow:0 0 30px var(--neon-green);animation:winPulse 1s ease infinite}
        @keyframes winPulse{0%,100%{opacity:1}50%{opacity:.8}}
        
        .thinking-indicator{display:flex;justify-content:center;gap:5px;margin-top:.5rem;opacity:0;transition:opacity .3s}
        .thinking-indicator.active{opacity:1}
        .thinking-dot{width:8px;height:8px;background:var(--neon-pink);border-radius:50%;animation:thinking 1.4s infinite}
        .thinking-dot:nth-child(2){animation-delay:.2s}
        .thinking-dot:nth-child(3){animation-delay:.4s}
        @keyframes thinking{0%,60%,100%{transform:translateY(0)}30%{transform:translateY(-10px)}}
        
        .game-btn{padding:.8rem 1.5rem;background:linear-gradient(135deg,var(--neon-blue),var(--neon-pink));border:none;color:#fff;border-radius:25px;font-weight:700;cursor:pointer;width:100%;margin-top:1rem;transition:all .3s}
        .game-btn:hover{transform:scale(1.02)}
        .game-btn.restart-btn{animation:restartPulse 2s infinite}
        @keyframes restartPulse{0%,100%{box-shadow:0 0 0 0 rgba(0,243,255,.4)}50%{box-shadow:0 0 20px 10px rgba(0,243,255,0)}}
        
        .win-overlay{position:fixed;inset:0;background:rgba(0,0,0,.9);z-index:2000;display:none;align-items:center;justify-content:center;flex-direction:column}
        .win-overlay.active{display:flex}
        .win-text{font-size:3rem;font-weight:900;color:var(--neon-green);margin-bottom:1rem;text-align:center}
        .win-btn{padding:1rem 2rem;background:linear-gradient(135deg,var(--neon-blue),var(--neon-pink));border:none;color:#fff;border-radius:30px;font-size:1rem;cursor:pointer;margin:.5rem}
        
        /* Confetti */
        .confetti{position:fixed;width:10px;height:10px;position:absolute;animation:confetti-fall 3s linear forwards}
        @keyframes confetti-fall{0%{transform:translateY(-100px) rotate(0deg)}100%{transform:translateY(100vh) rotate(720deg)}}
        
        /* Settings Styles */
        .settings-section{width:100%;max-width:500px;margin-bottom:1.5rem;background:rgba(255,255,255,.05);border-radius:15px;padding:1rem;border:1px solid rgba(255,255,255,.1)}
        .settings-title{font-size:1.1rem;color:var(--neon-blue);margin-bottom:1rem;text-align:center;text-transform:uppercase;letter-spacing:.1em}
        .setting-item{display:flex;justify-content:space-between;align-items:center;padding:.8rem;background:rgba(0,0,0,.3);border-radius:10px;margin-bottom:.5rem}
        .setting-item span{font-size:.9rem}
        .toggle-switch{width:50px;height:25px;background:rgba(255,255,255,.2);border-radius:25px;position:relative;cursor:pointer;transition:all .3s}
        .toggle-switch.active{background:var(--neon-green)}
        .toggle-switch::after{content:"";position:absolute;width:20px;height:20px;background:#fff;border-radius:50%;top:2.5px;left:2.5px;transition:all .3s}
        .toggle-switch.active::after{left:27.5px}
        
        .theme-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:1rem}
        .theme-card{padding:1rem;border-radius:10px;border:1px solid rgba(255,255,255,.2);cursor:pointer;transition:all .3s;text-align:center;background:rgba(0,0,0,.3)}
        .theme-card:hover{border-color:var(--neon-blue);transform:scale(1.05)}
        .theme-card.active{border-color:var(--neon-green);box-shadow:0 0 20px rgba(0,255,136,.3)}
        .theme-preview{width:100%;height:60px;border-radius:5px;margin-bottom:.5rem}
        
        .stats-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:1rem}
        .stats-grid .score-item{background:rgba(0,0,0,.3);padding:.5rem;border-radius:10px}
        
        @media(max-width:480px){
            .xo-cell{font-size:1.5rem}
            .mode-options{flex-direction:column}
            .difficulty-options{flex-wrap:wrap}
            .coin-display{top:.5rem;right:.5rem;font-size:.8rem;padding:.3rem .6rem}
        }
    </style>
</head>
<body>
    <div class="bg-animation"></div>
    <div class="coin-display" id="coin-display">0</div>
    
    <div class="header-container">
        <div style="width:40px"></div>
        <header>
            <h1 class="logo">ARCADE HUB</h1>
            <p class="subtitle">Select Your Game</p>
        </header>
        <button class="settings-btn" onclick="openSettings()">⚙️</button>
    </div>

    <main class="games-showcase">
        <article class="game-tile" onclick="openXOMenu()">
            <div class="game-icon-wrapper">⭕</div>
            <h2 class="game-title">XO Game</h2>
        </article>
        <article class="game-tile" onclick="openGame('color')">
            <div class="game-icon-wrapper">🎨</div>
            <h2 class="game-title">Color Tap</h2>
        </article>
        <article class="game-tile" onclick="openGame('memory')">
            <div class="game-icon-wrapper">🧠</div>
            <h2 class="game-title">Brain Match</h2>
        </article>
        <article class="game-tile" onclick="openGame('speed')">
            <div class="game-icon-wrapper">⚡</div>
            <h2 class="game-title">Speed Click</h2>
        </article>
    </main>

    <div class="win-overlay" id="win-overlay">
        <div class="win-text" id="win-text">YOU WIN!</div>
        <div id="win-stats" style="margin-bottom:1rem;text-align:center;opacity:.8"></div>
        <button class="win-btn" onclick="closeWin()">Play Again</button>
    </div>

    <!-- XO Pre-Game Menu Modal -->
    <div class="modal" id="xo-menu-modal">
        <div class="modal-header">
            <h2 class="modal-title">⭕ XO Game Setup</h2>
            <button class="close-btn" onclick="closeXOMenu()">✕</button>
        </div>
        <div class="modal-content">
            <div class="pre-game-menu" id="xo-menu-content">
                <!-- Menu content injected by JS -->
            </div>
        </div>
    </div>

    <!-- XO Game Modal -->
    <div class="modal" id="game-modal">
        <div class="modal-header">
            <h2 class="modal-title" id="current-game">GAME</h2>
            <button class="close-btn" onclick="closeGame()">✕</button>
        </div>
        <div class="modal-content">
            <div class="game-board-container" id="game-board"></div>
        </div>
    </div>

    <div class="modal" id="settings-modal">
        <div class="modal-header">
            <h2 class="modal-title">⚙️ Settings</h2>
            <button class="close-btn" onclick="closeSettings()">✕</button>
        </div>
        <div class="modal-content">
            <div class="settings-section">
                <div class="settings-title">🔊 Sound & Haptics</div>
                <div class="setting-item">
                    <span>Sound Effects</span>
                    <div class="toggle-switch active" id="sound-toggle" onclick="toggleSound()"></div>
                </div>
                <div class="setting-item">
                    <span>Vibration (Mobile)</span>
                    <div class="toggle-switch active" id="vibrate-toggle" onclick="toggleVibrate()"></div>
                </div>
            </div>
            
            <div class="settings-section">
                <div class="settings-title">🎨 Themes</div>
                <div class="theme-grid" id="theme-grid"></div>
            </div>
            
            <div class="settings-section">
                <div class="settings-title">📊 Statistics</div>
                <div class="stats-grid score-board" id="global-stats">
                    <div class="score-item"><div class="score-value" id="stat-total">0</div><div class="score-label">Total Games</div></div>
                    <div class="score-item"><div class="score-value" id="stat-wins">0</div><div class="score-label">Total Wins</div></div>
                    <div class="score-item"><div class="score-value" id="stat-streak">0</div><div class="score-label">Best Streak</div></div>
                    <div class="score-item"><div class="score-value" id="stat-ai-rate">0%</div><div class="score-label">AI Win Rate</div></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Game State
        let currentGame = null, soundEnabled = true, vibrateEnabled = true, currentDifficulty = 'medium';
        let xoBoardSize = 3, gameIntervals = [], isProcessing = false;
        let gameMode = 'pvp';
        let currentStreak = 0;
        let matchStartTime = 0;
        let tempSettings = {mode: null, difficulty: 'medium', size: 3};
        
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        let audioCtx = null;
        
        // Data Management
        const Data = {
            get: (key, def = 0) => parseInt(localStorage.getItem(`xo_${key}`) || def),
            set: (key, val) => localStorage.setItem(`xo_${key}`, val),
            getJSON: (key, def = {}) => JSON.parse(localStorage.getItem(`xo_${key}`) || JSON.stringify(def)),
            setJSON: (key, val) => localStorage.setItem(`xo_${key}`, JSON.stringify(val))
        };
        
        const themes = {
            neon: {name: 'Neon', colors: ['#00f3ff', '#ff00ff', '#00ff88']},
            dark: {name: 'Dark', colors: ['#4a4a6a', '#6a4a6a', '#4a6a4a']},
            classic: {name: 'Classic', colors: ['#0066cc', '#cc0000', '#00aa00']},
            retro: {name: 'Retro', colors: ['#ff6600', '#cc00cc', '#00ff00']}
        };
        
        function initData() {
            if (!localStorage.getItem('xo_coins')) Data.set('coins', 0);
            if (!localStorage.getItem('xo_totalGames')) Data.set('totalGames', 0);
            if (!localStorage.getItem('xo_totalWins')) Data.set('totalWins', 0);
            if (!localStorage.getItem('xo_bestStreak')) Data.set('bestStreak', 0);
            if (!localStorage.getItem('xo_aiWins')) Data.set('aiWins', 0);
            if (!localStorage.getItem('xo_aiGames')) Data.set('aiGames', 0);
            if (!localStorage.getItem('xo_longestTime')) Data.set('longestTime', 0);
            
            updateCoinDisplay();
        }
        
        function updateCoinDisplay() {
            document.getElementById('coin-display').textContent = Data.get('coins');
        }
        
        function addCoins(amount) {
            const newTotal = Data.get('coins') + amount;
            Data.set('coins', newTotal);
            updateCoinDisplay();
            
            const coinEl = document.getElementById('coin-display');
            coinEl.style.transform = 'scale(1.3)';
            coinEl.style.color = '#00ff88';
            setTimeout(() => {
                coinEl.style.transform = 'scale(1)';
                coinEl.style.color = 'gold';
            }, 300);
        }
        
        function initAudio() {
            if (!audioCtx) audioCtx = new AudioContext();
        }
        
        function vibrate(pattern = 50) {
            if (vibrateEnabled && navigator.vibrate) navigator.vibrate(pattern);
        }
        
        function playSound(type) {
            if (!soundEnabled || !audioCtx) return;
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            const now = audioCtx.currentTime;
            
            if (type === 'click') {
                osc.frequency.setValueAtTime(800, now);
                osc.frequency.exponentialRampToValueAtTime(400, now + 0.1);
                gain.gain.setValueAtTime(0.3, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.1);
                osc.start(now);
                osc.stop(now + 0.1);
                vibrate(30);
            } else if (type === 'win') {
                [523.25, 659.25, 783.99, 1046.50].forEach((f, i) => {
                    const o = audioCtx.createOscillator(), g = audioCtx.createGain();
                    o.connect(g); g.connect(audioCtx.destination);
                    o.frequency.value = f;
                    g.gain.setValueAtTime(0.2, now + i * 0.1);
                    g.gain.linearRampToValueAtTime(0, now + i * 0.1 + 0.3);
                    o.start(now + i * 0.1);
                    o.stop(now + i * 0.1 + 0.3);
                });
                vibrate([50, 50, 50, 50, 50]);
            } else if (type === 'draw') {
                osc.frequency.setValueAtTime(400, now);
                osc.frequency.linearRampToValueAtTime(300, now + 0.3);
                gain.gain.setValueAtTime(0.2, now);
                gain.gain.linearRampToValueAtTime(0, now + 0.3);
                osc.start(now);
                osc.stop(now + 0.3);
                vibrate(100);
            }
        }
        
        function createConfetti() {
            const colors = ['#ff0000', '#00ff00', '#0000ff', '#ffff00', '#ff00ff', '#00ffff'];
            for (let i = 0; i < 50; i++) {
                const confetti = document.createElement('div');
                confetti.className = 'confetti';
                confetti.style.left = Math.random() * 100 + 'vw';
                confetti.style.background = colors[Math.floor(Math.random() * colors.length)];
                confetti.style.animationDuration = (Math.random() * 2 + 2) + 's';
                confetti.style.borderRadius = Math.random() > 0.5 ? '50%' : '0';
                document.body.appendChild(confetti);
                setTimeout(() => confetti.remove(), 4000);
            }
        }
        
        // ==========================================
        // XO PRE-GAME MENU
        // ==========================================
        function openXOMenu() {
            initAudio();
            tempSettings = {mode: null, difficulty: 'medium', size: 3};
            renderXOMenu();
            document.getElementById('xo-menu-modal').classList.add('active');
        }
        
        function closeXOMenu() {
            document.getElementById('xo-menu-modal').classList.remove('active');
        }
        
        function renderXOMenu() {
            const container = document.getElementById('xo-menu-content');
            const isModeSelected = tempSettings.mode !== null;
            const isPVE = tempSettings.mode === 'pve';
            
            container.innerHTML = `
                <div class="menu-section">
                    <div class="menu-title">Select Mode</div>
                    <div class="mode-options">
                        <div class="mode-option ${tempSettings.mode === 'pvp' ? 'active' : ''}" onclick="selectMode('pvp')">
                            <div class="icon">👥</div>
                            <div class="label">Player vs Player</div>
                        </div>
                        <div class="mode-option ${tempSettings.mode === 'pve' ? 'active' : ''}" onclick="selectMode('pve')">
                            <div class="icon">🤖</div>
                            <div class="label">Player vs Bot</div>
                        </div>
                    </div>
                </div>
                
                ${isModeSelected ? `
                <div class="menu-section" style="animation:slideIn 0.3s ease">
                    <div class="menu-title">Board Size</div>
                    <div class="size-options">
                        <div class="size-option ${tempSettings.size === 3 ? 'active' : ''}" onclick="selectSize(3)">3×3</div>
                        <div class="size-option ${tempSettings.size === 4 ? 'active' : ''}" onclick="selectSize(4)">4×4</div>
                        <div class="size-option ${tempSettings.size === 5 ? 'active' : ''}" onclick="selectSize(5)">5×5</div>
                        <div class="size-option ${tempSettings.size === 6 ? 'active' : ''}" onclick="selectSize(6)">6×6</div>
                    </div>
                </div>
                ` : ''}
                
                ${isPVE ? `
                <div class="menu-section" style="animation:slideIn 0.3s ease">
                    <div class="menu-title">Select Difficulty</div>
                    <div class="difficulty-options">
                        <div class="diff-option ${tempSettings.difficulty === 'easy' ? 'active' : ''}" onclick="selectDifficulty('easy')">Easy</div>
                        <div class="diff-option ${tempSettings.difficulty === 'medium' ? 'active' : ''}" onclick="selectDifficulty('medium')">Medium</div>
                        <div class="diff-option ${tempSettings.difficulty === 'hard' ? 'active' : ''}" onclick="selectDifficulty('hard')">Hard</div>
                    </div>
                </div>
                ` : ''}
                
                <button class="start-game-btn" onclick="startXOGame()" ${!isModeSelected ? 'disabled' : ''}>
                    ${isModeSelected ? '▶ Start Game' : 'Select Mode to Start'}
                </button>
                
                <button class="back-btn" onclick="closeXOMenu()">← Back to Games</button>
            `;
        }
        
        function selectMode(mode) {
            playSound('click');
            tempSettings.mode = mode;
            renderXOMenu();
        }
        
        function selectSize(size) {
            playSound('click');
            tempSettings.size = size;
            renderXOMenu();
        }
        
        function selectDifficulty(diff) {
            playSound('click');
            tempSettings.difficulty = diff;
            renderXOMenu();
        }
        
        function startXOGame() {
            if (!tempSettings.mode) return;
            
            playSound('click');
            gameMode = tempSettings.mode;
            xoBoardSize = tempSettings.size;
            currentDifficulty = tempSettings.difficulty;
            currentStreak = 0;
            
            closeXOMenu();
            
            setTimeout(() => {
                document.getElementById('current-game').textContent = '⭕ XO Game';
                document.getElementById('game-modal').classList.add('active');
                initXOGame();
            }, 300);
        }
        
        // ==========================================
        // XO GAME LOGIC
        // ==========================================
        function initXOGame() {
            matchStartTime = Date.now();
            const container = document.getElementById('game-board');
            const wins = Data.get('wins');
            const losses = Data.get('losses');
            const draws = Data.get('draws');
            
            container.innerHTML = `
                <div class="score-board">
                    <div class="score-item"><div class="score-value">${wins}</div><div class="score-label">Wins</div></div>
                    <div class="score-item"><div class="score-value">${draws}</div><div class="score-label">Draws</div></div>
                    <div class="score-item"><div class="score-value">${losses}</div><div class="score-label">Losses</div></div>
                </div>
                <div class="streak-display" id="streak-display" style="${currentStreak > 1 ? '' : 'opacity:0'}">Win Streak: ${currentStreak}</div>
                <div class="xo-status player-turn" id="xo-status">${gameMode === 'pvp' ? 'Player 1' : 'You'} Turn (X)</div>
                <div class="thinking-indicator" id="thinking">
                    <div class="thinking-dot"></div>
                    <div class="thinking-dot"></div>
                    <div class="thinking-dot"></div>
                </div>
                <div class="xo-board size-${xoBoardSize}" id="xo-board"></div>
                <button class="game-btn restart-btn" onclick="restartGame()">🔄 Restart Game</button>
            `;
            
            window.restartGame = () => {
                playSound('click');
                const btn = document.querySelector('.restart-btn');
                btn.style.animation = 'none';
                btn.offsetHeight;
                btn.style.animation = 'restartPulse 2s infinite';
                initXOGame();
            };
            
            const board = document.getElementById('xo-board');
            const totalCells = xoBoardSize * xoBoardSize;
            let gameBoard = Array(totalCells).fill('');
            let gameActive = true;
            let moveCount = 0;
            let currentPlayer = 'X';
            let aiThinking = false;
            let player1Name = gameMode === 'pvp' ? 'Player 1' : 'You';
            let player2Name = gameMode === 'pvp' ? 'Player 2' : 'Computer';
            
            const winLength = xoBoardSize === 3 ? 3 : xoBoardSize === 4 ? 4 : 5;
            
            function render() {
                board.innerHTML = '';
                gameBoard.forEach((cell, index) => {
                    const div = document.createElement('div');
                    div.className = `xo-cell ${cell.toLowerCase()} ${cell ? 'taken' : ''} ${(aiThinking && gameMode === 'pve') ? 'disabled' : ''}`;
                    div.textContent = cell;
                    div.onclick = () => handleMove(index);
                    board.appendChild(div);
                });
            }
            
            function handleMove(index) {
                if (!gameActive) return;
                if (gameBoard[index]) return;
                if (isProcessing) return;
                if (gameMode === 'pve' && currentPlayer !== 'X') return;
                if (aiThinking) return;
                
                isProcessing = true;
                playSound('click');
                
                gameBoard[index] = currentPlayer;
                moveCount++;
                render();
                
                const winResult = checkWinner(gameBoard, xoBoardSize, winLength);
                if (winResult) {
                    endGame(winResult, currentPlayer);
                    isProcessing = false;
                    return;
                }
                
                if (moveCount === totalCells) {
                    endGame(null, null, true);
                    isProcessing = false;
                    return;
                }
                
                currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
                updateStatus();
                isProcessing = false;
                
                if (gameMode === 'pve' && currentPlayer === 'O') {
                    aiThinking = true;
                    updateStatus();
                    render();
                    const thinkTime = currentDifficulty === 'hard' ? 1500 : currentDifficulty === 'medium' ? 1000 : 600;
                    setTimeout(() => aiMove(), thinkTime);
                }
            }
            
            function aiMove() {
                if (!gameActive) return;
                
                const move = calculateAIMove(gameBoard, xoBoardSize, winLength, totalCells);
                if (move !== null && gameBoard[move] === '') {
                    gameBoard[move] = 'O';
                    moveCount++;
                    playSound('click');
                    render();
                    
                    const winResult = checkWinner(gameBoard, xoBoardSize, winLength);
                    if (winResult) {
                        endGame(winResult, 'O');
                        return;
                    }
                    
                    if (moveCount === totalCells) {
                        endGame(null, null, true);
                        return;
                    }
                    
                    currentPlayer = 'X';
                    aiThinking = false;
                    updateStatus();
                    render();
                }
            }
            
            function updateStatus() {
                const status = document.getElementById('xo-status');
                const thinking = document.getElementById('thinking');
                const streakEl = document.getElementById('streak-display');
                
                if (aiThinking) {
                    status.textContent = 'Computer Thinking...';
                    status.className = 'xo-status ai-turn';
                    thinking.classList.add('active');
                } else {
                    const playerName = currentPlayer === 'X' ? player1Name : player2Name;
                    status.textContent = `${playerName} Turn (${currentPlayer})`;
                    status.className = `xo-status ${currentPlayer === 'X' ? 'player-turn' : (gameMode === 'pvp' ? 'player2-turn' : 'ai-turn')}`;
                    thinking.classList.remove('active');
                }
                
                if (currentStreak > 1) {
                    streakEl.textContent = `Win Streak: ${currentStreak}`;
                    streakEl.style.opacity = '1';
                } else {
                    streakEl.style.opacity = '0';
                }
            }
            
            function endGame(winLine, winner, isDraw = false) {
                gameActive = false;
                const matchTime = Math.floor((Date.now() - matchStartTime) / 1000);
                
                const currentLongest = Data.get('longestTime');
                if (matchTime > currentLongest) Data.set('longestTime', matchTime);
                
                Data.set('totalGames', Data.get('totalGames') + 1);
                
                if (isDraw) {
                    playSound('draw');
                    addCoins(3);
                    Data.set('draws', Data.get('draws') + 1);
                    document.getElementById('xo-status').textContent = "It's a Draw! +3 Coins";
                    currentStreak = 0;
                    setTimeout(() => showWin("DRAW!", "3 coins earned"), 500);
                } else {
                    highlightWin(winLine);
                    const isPlayerWin = winner === 'X';
                    
                    if (isPlayerWin) {
                        playSound('win');
                        createConfetti();
                        addCoins(10);
                        currentStreak++;
                        
                        const currentBest = Data.get('bestStreak');
                        if (currentStreak > currentBest) Data.set('bestStreak', currentStreak);
                        
                        Data.set('wins', Data.get('wins') + 1);
                        Data.set('totalWins', Data.get('totalWins') + 1);
                        
                        const streakBonus = currentStreak > 1 ? ` 🔥 Streak: ${currentStreak}` : '';
                        setTimeout(() => showWin("YOU WIN!", `+10 coins${streakBonus}`), 500);
                    } else {
                        playSound('draw');
                        Data.set('losses', Data.get('losses') + 1);
                        currentStreak = 0;
                        
                        if (gameMode === 'pve') {
                            Data.set('aiWins', Data.get('aiWins') + 1);
                        }
                        
                        document.getElementById('xo-status').textContent = `${player2Name} Wins!`;
                        document.getElementById('xo-status').style.color = '#ff4444';
                        setTimeout(() => showWin(`${player2Name} WINS!`, "Better luck next time!"), 500);
                    }
                }
                
                if (gameMode === 'pve') {
                    Data.set('aiGames', Data.get('aiGames') + 1);
                }
                
                updateCoinDisplay();
            }
            
            function highlightWin(cells) {
                const cellElements = document.querySelectorAll('.xo-cell');
                cells.forEach(idx => cellElements[idx].classList.add('win'));
            }
            
            render();
        }

        // AI Logic
        function calculateAIMove(board, size, winLen, totalCells) {
            if (currentDifficulty === 'hard' && size === 3) {
                return getBestMoveMinimax(board);
            } else if (currentDifficulty === 'medium') {
                return Math.random() > 0.4 ? getSmartMove(board, size, winLen, totalCells) : getRandomMove(board, totalCells);
            } else {
                return Math.random() > 0.7 ? getSmartMove(board, size, winLen, totalCells) : getRandomMove(board, totalCells);
            }
        }
        
        function getRandomMove(board, totalCells) {
            const empty = [];
            for (let i = 0; i < totalCells; i++) if (board[i] === '') empty.push(i);
            return empty.length > 0 ? empty[Math.floor(Math.random() * empty.length)] : null;
        }
        
        function getSmartMove(board, size, winLen, totalCells) {
            for (let i = 0; i < totalCells; i++) {
                if (board[i] === '') {
                    board[i] = 'O';
                    const wins = checkWinFor(board, size, winLen, 'O');
                    board[i] = '';
                    if (wins) return i;
                }
            }
            
            for (let i = 0; i < totalCells; i++) {
                if (board[i] === '') {
                    board[i] = 'X';
                    const wins = checkWinFor(board, size, winLen, 'X');
                    board[i] = '';
                    if (wins) return i;
                }
            }
            
            if (size === 3 && board[4] === '') return 4;
            
            const corners = [0, size-1, totalCells-size, totalCells-1];
            const emptyCorners = corners.filter(i => board[i] === '');
            if (emptyCorners.length > 0) return emptyCorners[Math.floor(Math.random() * emptyCorners.length)];
            
            return getRandomMove(board, totalCells);
        }
        
        function getBestMoveMinimax(board) {
            let bestScore = -Infinity;
            let move = 0;
            for (let i = 0; i < 9; i++) {
                if (board[i] === '') {
                    board[i] = 'O';
                    let score = minimax(board, 0, false);
                    board[i] = '';
                    if (score > bestScore) {
                        bestScore = score;
                        move = i;
                    }
                }
            }
            return move;
        }
        
        function minimax(board, depth, isMaximizing) {
            if (checkWinFor(board, 3, 3, 'O')) return 10 - depth;
            if (checkWinFor(board, 3, 3, 'X')) return depth - 10;
            if (!board.includes('')) return 0;
            
            if (isMaximizing) {
                let bestScore = -Infinity;
                for (let i = 0; i < 9; i++) {
                    if (board[i] === '') {
                        board[i] = 'O';
                        let score = minimax(board, depth + 1, false);
                        board[i] = '';
                        bestScore = Math.max(score, bestScore);
                    }
                }
                return bestScore;
            } else {
                let bestScore = Infinity;
                for (let i = 0; i < 9; i++) {
                    if (board[i] === '') {
                        board[i] = 'X';
                        let score = minimax(board, depth + 1, true);
                        board[i] = '';
                        bestScore = Math.min(score, bestScore);
                    }
                }
                return bestScore;
            }
        }
        
        function checkWinFor(board, size, winLen, player) {
            for (let row = 0; row < size; row++) {
                for (let col = 0; col <= size - winLen; col++) {
                    let count = 0;
                    for (let i = 0; i < winLen; i++) {
                        if (board[row * size + col + i] === player) count++;
                    }
                    if (count === winLen) return true;
                }
            }
            
            for (let col = 0; col < size; col++) {
                for (let row = 0; row <= size - winLen; row++) {
                    let count = 0;
                    for (let i = 0; i < winLen; i++) {
                        if (board[(row + i) * size + col] === player) count++;
                    }
                    if (count === winLen) return true;
                }
            }
            
            for (let row = 0; row <= size - winLen; row++) {
                for (let col = 0; col <= size - winLen; col++) {
                    let count1 = 0, count2 = 0;
                    for (let i = 0; i < winLen; i++) {
                        if (board[(row + i) * size + col + i] === player) count1++;
                        if (board[(row + i) * size + col + winLen - 1 - i] === player) count2++;
                    }
                    if (count1 === winLen || count2 === winLen) return true;
                }
            }
            return false;
        }
        
        function checkWinner(board, size, winLen) {
            if (checkWinFor(board, size, winLen, 'X')) return findWinningLine(board, size, winLen, 'X');
            if (checkWinFor(board, size, winLen, 'O')) return findWinningLine(board, size, winLen, 'O');
            return null;
        }
        
        function findWinningLine(board, size, winLen, player) {
            for (let row = 0; row < size; row++) {
                for (let col = 0; col <= size - winLen; col++) {
                    let line = [];
                    for (let i = 0; i < winLen; i++) {
                        const idx = row * size + col + i;
                        if (board[idx] === player) line.push(idx);
                    }
                    if (line.length === winLen) return line;
                }
            }
            for (let col = 0; col < size; col++) {
                for (let row = 0; row <= size - winLen; row++) {
                    let line = [];
                    for (let i = 0; i < winLen; i++) {
                        const idx = (row + i) * size + col;
                        if (board[idx] === player) line.push(idx);
                    }
                    if (line.length === winLen) return line;
                }
            }
            for (let row = 0; row <= size - winLen; row++) {
                for (let col = 0; col <= size - winLen; col++) {
                    let line1 = [], line2 = [];
                    for (let i = 0; i < winLen; i++) {
                        const idx1 = (row + i) * size + col + i;
                        const idx2 = (row + i) * size + col + winLen - 1 - i;
                        if (board[idx1] === player) line1.push(idx1);
                        if (board[idx2] === player) line2.push(idx2);
                    }
                    if (line1.length === winLen) return line1;
                    if (line2.length === winLen) return line2;
                }
            }
            return null;
        }

        // Other games...
        function initColorGame() {
            const container = document.getElementById('game-board');
            container.innerHTML = `
                <div class="color-score" id="color-score">Score: 0</div>
                <div class="color-timer" id="color-timer">Time: 30</div>
                <div class="color-display" id="color-display" style="width:100%;aspect-ratio:1;border-radius:15px;margin-bottom:1rem"></div>
                <div class="color-options" id="color-options" style="display:grid;grid-template-columns:repeat(2,1fr);gap:10px"></div>
                <button class="game-btn" onclick="initColorGame()">Restart</button>
            `;
            
            const colors = ['#ff4444', '#44ff44', '#4444ff', '#ffff44'];
            let score = 0, timeLeft = 30, currentTarget = 0, gameActive = true;
            
            function nextRound() {
                if (!gameActive) return;
                currentTarget = Math.floor(Math.random() * colors.length);
                const display = document.getElementById('color-display');
                display.style.background = colors[currentTarget];
                display.textContent = ['RED','GREEN','BLUE','YELLOW'][currentTarget];
                
                const options = document.getElementById('color-options');
                options.innerHTML = colors.map(c => 
                    `<div style="aspect-ratio:1;border-radius:10px;background:${c};cursor:pointer;border:3px solid rgba(255,255,255,.3)" onclick="checkColor('${c}')"></div>`
                ).join('');
            }
            
            window.checkColor = (hex) => {
                if (!gameActive) return;
                if (hex === colors[currentTarget]) {
                    playSound('click');
                    score += 10;
                    document.getElementById('color-score').textContent = `Score: ${score}`;
                    nextRound();
                } else {
                    timeLeft -= 3;
                    document.getElementById('color-timer').textContent = `Time: ${timeLeft}`;
                }
            };
            
            const timer = setInterval(() => {
                timeLeft--;
                document.getElementById('color-timer').textContent = `Time: ${timeLeft}`;
                if (timeLeft <= 0) {
                    gameActive = false;
                    clearInterval(timer);
                    document.getElementById('color-display').innerHTML = '<div style="text-align:center;padding:2rem">Game Over!</div>';
                }
            }, 1000);
            
            gameIntervals.push(timer);
            nextRound();
        }

        function initMemoryGame() {
            const emojis = ['🎮','🎯','🎲','🎸','🎨','🎭','🎪','🎰'];
            const cards = [...emojis, ...emojis].sort(() => Math.random() - 0.5);
            const container = document.getElementById('game-board');
            container.innerHTML = `<div style="display:grid;grid-template-columns:repeat(4,1fr);gap:8px;margin:1rem 0" id="memory-grid"></div><button class="game-btn" onclick="initMemoryGame()">New Game</button>`;
            
            const grid = document.getElementById('memory-grid');
            let flipped = [], matched = 0, canFlip = true;
            
            cards.forEach(emoji => {
                const card = document.createElement('div');
                card.style = 'aspect-ratio:1;background:linear-gradient(135deg,var(--neon-blue),var(--neon-pink));border-radius:10px;cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:1.5rem;transition:all .3s';
                card.innerHTML = '<span style="opacity:0">?</span>';
                
                card.onclick = () => {
                    if (!canFlip || card.flipped || card.matched) return;
                    playSound('click');
                    card.style.background = 'rgba(255,255,255,.1)';
                    card.innerHTML = emoji;
                    card.flipped = true;
                    flipped.push(card);
                    
                    if (flipped.length === 2) {
                        canFlip = false;
                        const [c1, c2] = flipped;
                        if (c1.innerHTML === c2.innerHTML) {
                            playSound('win');
                            setTimeout(() => {
                                c1.style.background = 'var(--neon-green)';
                                c2.style.background = 'var(--neon-green)';
                                c1.style.opacity = '0.6';
                                c2.style.opacity = '0.6';
                                c1.matched = c2.matched = true;
                                matched++;
                                flipped = [];
                                canFlip = true;
                                if (matched === 8) showWin('YOU WON!');
                            }, 500);
                        } else {
                            setTimeout(() => {
                                c1.style.background = 'linear-gradient(135deg,var(--neon-blue),var(--neon-pink))';
                                c2.style.background = 'linear-gradient(135deg,var(--neon-blue),var(--neon-pink))';
                                c1.innerHTML = c2.innerHTML = '<span style="opacity:0">?</span>';
                                c1.flipped = c2.flipped = false;
                                flipped = [];
                                canFlip = true;
                            }, 1000);
                        }
                    }
                };
                grid.appendChild(card);
            });
        }

        function initSpeedGame() {
            const container = document.getElementById('game-board');
            container.innerHTML = `
                <div style="text-align:center;margin-bottom:1rem">
                    <div style="font-size:1.5rem;color:var(--neon-blue)" id="click-count">0 clicks</div>
                </div>
                <div id="click-area" style="width:100%;aspect-ratio:1;background:radial-gradient(circle,rgba(255,68,68,.3) 0%,rgba(255,68,68,.1) 100%);border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer;transition:all .1s;border:4px solid rgba(255,68,68,.5)">
                    <div style="font-size:1.2rem;font-weight:700">WAIT...</div>
                </div>
                <button class="game-btn" onclick="initSpeedGame()">Restart</button>
            `;
            
            const area = document.getElementById('click-area');
            const text = area.querySelector('div');
            let clicks = 0, startTime = null, isGreen = false, timeoutId = null;
            
            function nextRound() {
                isGreen = false;
                area.style.background = 'radial-gradient(circle,rgba(255,68,68,.3) 0%,rgba(255,68,68,.1) 100%)';
                area.style.borderColor = 'rgba(255,68,68,.5)';
                text.textContent = "WAIT...";
                
                timeoutId = setTimeout(() => {
                    isGreen = true;
                    area.style.background = 'radial-gradient(circle,rgba(0,255,136,.3) 0%,rgba(0,255,136,.1) 100%)';
                    area.style.borderColor = 'var(--neon-green)';
                    text.textContent = "CLICK NOW!";
                    startTime = Date.now();
                }, 1000 + Math.random() * 2000);
            }
            
            area.onclick = () => {
                if (!isGreen) {
                    clearTimeout(timeoutId);
                    text.textContent = "TOO EARLY!";
                    setTimeout(nextRound, 1000);
                    return;
                }
                
                const time = ((Date.now() - startTime) / 1000).toFixed(3);
                clicks++;
                document.getElementById('click-count').textContent = clicks + ' clicks - Last: ' + time + 's';
                playSound('click');
                setTimeout(nextRound, 500);
            };
            
            nextRound();
            gameIntervals.push(timeoutId);
        }

        // Settings Functions
        function openSettings() {
            document.getElementById('settings-modal').classList.add('active');
            renderThemes();
            updateGlobalStats();
            
            document.getElementById('sound-toggle').classList.toggle('active', soundEnabled);
            document.getElementById('vibrate-toggle').classList.toggle('active', vibrateEnabled);
        }
        
        function closeSettings() {
            document.getElementById('settings-modal').classList.remove('active');
        }
        
        function toggleSound() {
            soundEnabled = !soundEnabled;
            document.getElementById('sound-toggle').classList.toggle('active', soundEnabled);
            if (soundEnabled) initAudio();
        }
        
        function toggleVibrate() {
            vibrateEnabled = !vibrateEnabled;
            document.getElementById('vibrate-toggle').classList.toggle('active', vibrateEnabled);
        }
        
        function renderThemes() {
            const grid = document.getElementById('theme-grid');
            grid.innerHTML = '';
            
            Object.entries(themes).forEach(([key, theme]) => {
                const card = document.createElement('div');
                card.className = `theme-card ${currentTheme === key ? 'active' : ''}`;
                card.onclick = () => selectTheme(key);
                
                card.innerHTML = `
                    <div class="theme-preview" style="background:linear-gradient(135deg,${theme.colors[0]},${theme.colors[1]})"></div>
                    <div style="font-weight:700">${theme.name}</div>
                `;
                grid.appendChild(card);
            });
        }
        
        function selectTheme(key) {
            currentTheme = key;
            document.body.style.setProperty('--neon-blue', themes[key].colors[0]);
            document.body.style.setProperty('--neon-pink', themes[key].colors[1]);
            document.body.style.setProperty('--neon-green', themes[key].colors[2]);
            renderThemes();
        }
        
        function updateGlobalStats() {
            document.getElementById('stat-total').textContent = Data.get('totalGames');
            document.getElementById('stat-wins').textContent = Data.get('totalWins');
            document.getElementById('stat-streak').textContent = Data.get('bestStreak');
            
            const aiGames = Data.get('aiGames');
            const aiWins = Data.get('aiWins');
            const aiRate = aiGames > 0 ? Math.round((aiWins / aiGames) * 100) : 0;
            document.getElementById('stat-ai-rate').textContent = aiRate + '%';
        }

        function openGame(game) {
            if (game === 'xo') {
                openXOMenu();
                return;
            }
            
            initAudio();
            gameIntervals.forEach(clearInterval);
            gameIntervals = [];
            currentGame = game;
            currentStreak = 0;
            
            const names = {color: '🎨 Color Tap', memory: '🧠 Brain Match', speed: '⚡ Speed Click'};
            document.getElementById('current-game').textContent = names[game];
            document.getElementById('game-modal').classList.add('active');
            
            switch(game) {
                case 'color': initColorGame(); break;
                case 'memory': initMemoryGame(); break;
                case 'speed': initSpeedGame(); break;
            }
        }

        function closeGame() {
            gameIntervals.forEach(clearInterval);
            document.getElementById('game-modal').classList.remove('active');
            currentStreak = 0;
        }

        function showWin(text, subtext = '') {
            playSound('win');
            document.getElementById('win-text').textContent = text;
            document.getElementById('win-stats').textContent = subtext;
            document.getElementById('win-overlay').classList.add('active');
            if (text.includes('WIN')) createConfetti();
        }

        function closeWin() {
            document.getElementById('win-overlay').classList.remove('active');
            if (currentGame === 'xo') initXOGame();
        }
        
        // Initialize
        initData();
    </script>
</body>
</html>
