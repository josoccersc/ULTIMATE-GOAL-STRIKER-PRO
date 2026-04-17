<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>ULTIMATE GOAL: STRIKER PRO</title>
    <!-- External library for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background-color: #020617;
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            touch-action: none; /* Prevents unwanted scrolling on mobile */
        }
        canvas {
            display: block;
        }
        .ui-overlay {
            pointer-events: none; /* Allows clicks to pass through to the game except on buttons */
        }
        #stamina-container {
            width: 10px;
            height: 120px;
            background: rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 999px;
            overflow: hidden;
            position: relative;
            display: flex;
            flex-direction: column-reverse;
        }
        #stamina-bar {
            width: 100%;
            background: #60a5fa;
            transition: height 0.1s linear;
        }
        .glass-ui {
            background: rgba(15, 23, 42, 0.9);
            backdrop-filter: blur(8px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        .menu-gradient {
            background: radial-gradient(circle at center, #1e293b 0%, #020617 100%);
        }
        .title-glow {
            text-shadow: 0 0 20px rgba(59, 130, 246, 0.5);
        }
    </style>
</head>
<body class="bg-slate-900">

    <!-- Main Home Screen -->
    <div id="home-screen" class="absolute inset-0 z-[60] flex flex-col items-center justify-center menu-gradient p-6 text-center">
        <div class="mb-12">
            <h1 class="text-6xl md:text-8xl font-black text-white italic tracking-tighter title-glow uppercase">
                ULTIMATE GOAL<br>
                <span class="text-blue-500">STRIKER PRO</span>
            </h1>
            <p class="text-blue-300 font-bold tracking-[0.3em] mt-4 uppercase text-sm">Precision Soccer Simulation</p>
        </div>

        <div class="flex flex-col space-y-3 w-full max-w-sm pointer-events-auto">
            <button onclick="startMode('tournament')" class="px-8 py-5 bg-blue-600 hover:bg-blue-500 text-white font-black rounded-2xl transition-all transform hover:scale-105 shadow-[0_0_20px_rgba(37,99,235,0.4)]">
                <span class="text-xl italic uppercase">World Tournament</span>
            </button>
            <button onclick="startMode('friendly')" class="px-8 py-4 bg-emerald-600 hover:bg-emerald-500 text-white font-black rounded-2xl transition-all transform hover:scale-105">
                <span class="text-lg italic uppercase">Friendly Match</span>
            </button>
            <button onclick="startMode('practice')" class="px-8 py-4 bg-slate-800 hover:bg-slate-700 text-white font-black rounded-2xl transition-all">
                <span class="text-lg italic uppercase">Practice Mode</span>
            </button>
            
            <div class="flex space-x-2 mt-4">
                <button onclick="toggleInstructions()" class="flex-1 py-2 bg-slate-800/50 text-slate-400 font-bold hover:text-white transition-all text-xs uppercase rounded-lg border border-white/5">
                    Controls
                </button>
            </div>
        </div>

        <!-- Instructions Overlay -->
        <div id="instructions" class="hidden absolute inset-0 z-[70] bg-black/95 flex items-center justify-center p-6">
            <div class="glass-ui border border-slate-700 p-8 rounded-3xl max-w-md text-left pointer-events-auto">
                <h3 class="text-2xl font-black text-white mb-4 italic uppercase text-center underline decoration-blue-500">How to Play</h3>
                <ul class="text-slate-300 space-y-3 mb-6 font-medium">
                    <li><strong class="text-blue-400">WASD / ARROWS:</strong> Full Movement</li>
                    <li><strong class="text-blue-400">SHIFT:</strong> Sprint (Uses Stamina)</li>
                    <li><strong class="text-blue-400">X:</strong> Slide Tackle (Defending)</li>
                    <li><strong class="text-blue-400">HOLD Z:</strong> Precision Aim (Slo-Mo)</li>
                    <li><strong class="text-blue-400">RELEASE Z:</strong> Shoot</li>
                    <li><strong class="text-emerald-400">P Key:</strong> Reset Ball to Feet (Practice Mode)</li>
                </ul>
                <button onclick="toggleInstructions()" class="w-full py-3 bg-white text-black font-black rounded-xl uppercase">Enter Pitch</button>
            </div>
        </div>
    </div>

    <!-- Setup Menu -->
    <div id="setup-screen" class="hidden absolute inset-0 z-50 flex items-center justify-center bg-black/80 backdrop-blur-md p-4">
        <div class="bg-white rounded-3xl p-8 max-w-4xl w-full text-center shadow-2xl border-b-8 border-blue-500">
            <div id="step-selection" class="hidden">
                <h2 id="selection-title" class="text-3xl font-black text-slate-800 mb-1 uppercase italic">Select Team</h2>
                <div id="team-grid" class="grid grid-cols-3 sm:grid-cols-5 gap-3 mb-8 overflow-y-auto max-h-[50vh] p-2"></div>
                <button onclick="backToHome()" class="text-slate-400 font-bold uppercase text-xs hover:text-slate-600">Back</button>
            </div>
            <div id="step-skill" class="hidden py-8">
                <h2 class="text-4xl font-black text-slate-800 mb-6 italic">DIFFICULTY</h2>
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                    <button onclick="setDifficulty('amateur')" class="p-6 rounded-2xl border-2 border-slate-100 hover:border-green-500 font-bold text-slate-700 group">
                        <div class="text-xl mb-1">AMATEUR</div>
                        <div class="text-[10px] text-slate-400 group-hover:text-green-600 uppercase font-bold">New Player</div>
                    </button>
                    <button onclick="setDifficulty('good')" class="p-6 rounded-2xl border-2 border-slate-100 hover:border-blue-500 font-bold text-slate-700 group">
                        <div class="text-xl mb-1">PRO</div>
                        <div class="text-[10px] text-slate-400 group-hover:text-blue-600 uppercase font-bold">Experienced</div>
                    </button>
                    <button onclick="setDifficulty('hard')" class="p-6 rounded-2xl border-2 border-slate-100 hover:border-red-500 font-bold text-slate-700 group">
                        <div class="text-xl mb-1">LEGEND</div>
                        <div class="text-[10px] text-slate-400 group-hover:text-red-600 uppercase font-bold">Unbeatable</div>
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- UI Overlay -->
    <div id="ui-root" class="ui-overlay absolute inset-0 z-10 hidden">
        <div class="absolute top-6 left-6 flex flex-col items-start space-y-2">
            <div id="scoreboard" class="glass-ui px-5 py-2 rounded-xl border-l-4 border-yellow-500 shadow-xl flex items-center space-x-4">
                <span id="p-flag" class="text-xl"></span>
                <div id="player-score" class="text-2xl font-black text-white">0</div>
                <div class="h-6 w-px bg-white/20"></div>
                <div id="cpu-score" class="text-2xl font-black text-white">0</div>
                <span id="c-flag" class="text-xl"></span>
            </div>
            <div id="game-timer" class="glass-ui px-4 py-1 rounded-lg text-white font-mono font-bold text-sm tracking-tighter">05:00</div>
        </div>

        <div class="absolute left-6 bottom-10 flex flex-col items-center space-y-2">
            <div id="stamina-container"><div id="stamina-bar"></div></div>
            <div class="text-[8px] text-white/60 font-black tracking-widest uppercase">STAMINA</div>
        </div>

        <div class="absolute bottom-6 right-6 flex flex-col items-end space-y-4">
            <div class="glass-ui p-1 rounded-xl shadow-2xl border-2 border-white/10">
                <canvas id="minimapCanvas" width="120" height="168" class="rounded-lg"></canvas>
            </div>
        </div>

        <div id="status-msg" class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 hidden text-center">
            <h1 id="status-text" class="text-7xl font-black text-white italic drop-shadow-[0_10px_10px_rgba(0,0,0,0.5)] uppercase tracking-tighter">GOAL!</h1>
        </div>

        <div id="practice-ui" class="hidden absolute top-1/2 right-6 -translate-y-1/2 glass-ui p-4 rounded-2xl flex flex-col space-y-4">
             <div class="text-center">
                <div class="text-[8px] text-white/50 uppercase font-black">Goals</div>
                <div id="practice-goals" class="text-2xl text-white font-black">0</div>
             </div>
             <button onclick="spawnBallToFeet()" class="bg-blue-600 p-3 rounded-lg text-white font-black text-xs uppercase hover:bg-blue-500 pointer-events-auto">Reset (P)</button>
             <button onclick="backToHome()" class="bg-slate-700 p-3 rounded-lg text-white font-black text-xs uppercase hover:bg-slate-600 pointer-events-auto">Exit</button>
        </div>
    </div>

    <canvas id="gameCanvas"></canvas>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const miniCanvas = document.getElementById('minimapCanvas');
        const miniCtx = miniCanvas.getContext('2d');

        // Game Constants
        const WORLD_W = 1000; 
        const WORLD_H = 1400; 
        const GOAL_W = 200;
        const BOX_18_W = 600;
        const BOX_18_H = 250;
        const BOX_6_W = 300;
        const BOX_6_H = 80;
        const PLAYER_SIZE = 20;

        // State Variables
        let playerTeam, cpuTeam;
        let gameState = 'HOME';
        let gameMode = 'tournament'; 
        let score = { player: 0, cpu: 0 };
        let timeLeft = 300;
        let timerInterval;
        const keys = {};
        const camera = { x: 0, y: 0 };
        let difficulty = 'good';
        
        const SKILL_MODS = {
            'amateur': { speed: 3.5, dribble: 2.8, cpuTackle: 0.01, shoot: 0.01, gkRush: 0.04, gkReact: 0.05 },
            'good': { speed: 4.8, dribble: 4.0, cpuTackle: 0.05, shoot: 0.03, gkRush: 0.08, gkReact: 0.12 },
            'hard': { speed: 6.5, dribble: 5.8, cpuTackle: 0.12, shoot: 0.07, gkRush: 0.15, gkReact: 0.2 }
        };

        const COUNTRIES = [
            { id: 'BRA', color: '#FFDF00', sock: '#009b3a', flag: '🇧🇷' },
            { id: 'ARG', color: '#74ACDF', sock: '#ffffff', flag: '🇦🇷' },
            { id: 'FRA', color: '#002395', sock: '#ffffff', flag: '🇫🇷' },
            { id: 'GER', color: '#ffffff', sock: '#000000', flag: '🇩🇪' },
            { id: 'ENG', color: '#ffffff', sock: '#000033', flag: '🏴󠁧󠁢󠁥󠁮󠁧󠁿' },
            { id: 'ESP', color: '#AA151B', sock: '#AA151B', flag: '🇪🇸' },
            { id: 'POR', color: '#FF0000', sock: '#006600', flag: '🇵🇹' },
            { id: 'ITA', color: '#004d99', sock: '#004d99', flag: '🇮🇹' },
            { id: 'NED', color: '#FF4F00', sock: '#FF4F00', flag: '🇳🇱' },
            { id: 'BEL', color: '#E30613', sock: '#000000', flag: '🇧🇪' }
        ];

        const CLUBS = [
            { id: 'ARS', name: 'Arsenal', color: '#EF0107', sock: '#ffffff', flag: '🔫' },
            { id: 'RMA', name: 'Real Madrid', color: '#ffffff', sock: '#ffffff', flag: '⚪️' },
            { id: 'FCB', name: 'Barcelona', color: '#a50044', sock: '#004d98', flag: '🔵' },
            { id: 'MCI', name: 'Man City', color: '#6cabdd', sock: '#ffffff', flag: '🏙️' },
            { id: 'LIV', name: 'Liverpool', color: '#c8102e', sock: '#c8102e', flag: '🔴' },
            { id: 'BAY', name: 'Bayern', color: '#dc052d', sock: '#ffffff', flag: '🧤' },
            { id: 'PSG', name: 'Paris', color: '#004170', sock: '#da291c', flag: '🗼' },
            { id: 'MUN', name: 'Man Utd', color: '#DA291C', sock: '#000000', flag: '👹' },
            { id: 'CHE', name: 'Chelsea', color: '#034694', sock: '#ffffff', flag: '🦁' },
            { id: 'JUV', name: 'Juventus', color: '#ffffff', sock: '#000000', flag: '🦓' },
            { id: 'INT', name: 'Inter Milan', color: '#0062a4', sock: '#000000', flag: '🐍' },
            { id: 'DOR', name: 'Dortmund', color: '#FDE100', sock: '#000000', flag: '🐝' },
            { id: 'MIL', name: 'AC Milan', color: '#FB090B', sock: '#000000', flag: '👹' },
            { id: 'ATM', name: 'Atalanta', color: '#2463AD', sock: '#000000', flag: '⚔️' },
            { id: 'NAP', name: 'Napoli', color: '#12A0D7', sock: '#ffffff', flag: '🌋' }
        ];

        let isChargingZ = false;
        let chargePower = 0;
        let timeScale = 1.0;
        let playerStamina = 100;
        let isExhausted = false;
        let tackleCooldown = 0;
        let tackleActive = 0;

        const ball = { x: WORLD_W/2, y: WORLD_H/2, vx: 0, vy: 0, z: 0, vz: 0, radius: 9, owner: null };
        const player = { x: WORLD_W/2, y: WORLD_H * 0.8, vx: 0, vy: 0, radius: PLAYER_SIZE, anim: 0, angle: -Math.PI/2, team: 'player', targetAngle: -Math.PI/2 };
        const cpu = { x: WORLD_W/2, y: WORLD_H * 0.2, vx: 0, vy: 0, radius: PLAYER_SIZE, anim: 0, angle: Math.PI/2, team: 'cpu' };
        
        const gkPlayer = { x: WORLD_W/2, y: WORLD_H - 40, vx: 0, vy: 0, radius: PLAYER_SIZE, anim: 0, angle: -Math.PI/2, team: 'player', isGK: true };
        const gkCpu = { x: WORLD_W/2, y: 40, vx: 0, vy: 0, radius: PLAYER_SIZE, anim: 0, angle: Math.PI/2, team: 'cpu', isGK: true };

        // Helper Functions
        function startMode(mode) {
            gameMode = mode;
            if (mode === 'practice') {
                playerTeam = COUNTRIES[0];
                cpuTeam = COUNTRIES[1];
                document.getElementById('home-screen').classList.add('hidden');
                document.getElementById('ui-root').classList.remove('hidden');
                document.getElementById('scoreboard').classList.add('hidden');
                document.getElementById('game-timer').classList.add('hidden');
                document.getElementById('practice-ui').classList.remove('hidden');
                startMatch();
            } else {
                showSelectionStep(mode);
            }
        }

        function showSelectionStep(mode) {
            document.getElementById('home-screen').classList.add('hidden');
            document.getElementById('setup-screen').classList.remove('hidden');
            document.getElementById('step-selection').classList.remove('hidden');
            document.getElementById('step-skill').classList.add('hidden');
            const grid = document.getElementById('team-grid');
            grid.innerHTML = '';
            const list = mode === 'tournament' ? COUNTRIES : CLUBS;
            list.forEach(t => {
                const btn = document.createElement('button');
                btn.className = "flex flex-col items-center p-3 rounded-xl border-2 border-slate-100 hover:border-blue-500 transition-all bg-slate-50";
                btn.innerHTML = `<span class="text-3xl mb-1">${t.flag}</span><span class="text-[9px] font-black">${t.id}</span>`;
                btn.onclick = () => {
                    playerTeam = t;
                    document.getElementById('step-selection').classList.add('hidden');
                    document.getElementById('step-skill').classList.remove('hidden');
                };
                grid.appendChild(btn);
            });
        }

        function setDifficulty(level) {
            difficulty = level;
            const list = gameMode === 'tournament' ? COUNTRIES : CLUBS;
            cpuTeam = list.filter(t => t.id !== playerTeam.id)[Math.floor(Math.random()*(list.length-1))];
            document.getElementById('p-flag').innerText = playerTeam.flag;
            document.getElementById('c-flag').innerText = cpuTeam.flag;
            document.getElementById('setup-screen').classList.add('hidden');
            document.getElementById('ui-root').classList.remove('hidden');
            document.getElementById('scoreboard').classList.remove('hidden');
            document.getElementById('game-timer').classList.remove('hidden');
            document.getElementById('practice-ui').classList.add('hidden');
            startMatch();
        }

        function backToHome() {
            gameState = 'HOME';
            if(timerInterval) clearInterval(timerInterval);
            document.getElementById('home-screen').classList.remove('hidden');
            document.getElementById('setup-screen').classList.add('hidden');
            document.getElementById('ui-root').classList.add('hidden');
        }

        function toggleInstructions() {
            document.getElementById('instructions').classList.toggle('hidden');
        }

        function startMatch() {
            score = { player: 0, cpu: 0 };
            timeLeft = 300;
            resetField();
            gameState = 'PLAYING';
            if(timerInterval) clearInterval(timerInterval);
            timerInterval = setInterval(() => {
                if (gameState === 'PLAYING' && gameMode !== 'practice') {
                    timeLeft--;
                    updateUI();
                    if (timeLeft <= 0) gameState = 'GAMEOVER';
                }
            }, 1000);
            requestAnimationFrame(gameLoop);
        }

        function updateUI() {
            const m = Math.floor(timeLeft / 60);
            const s = timeLeft % 60;
            const timerEl = document.getElementById('game-timer');
            if (timerEl) timerEl.innerText = `${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`;
            
            const pScoreEl = document.getElementById('player-score');
            if (pScoreEl) pScoreEl.innerText = score.player;
            
            const cScoreEl = document.getElementById('cpu-score');
            if (cScoreEl) cScoreEl.innerText = score.cpu;
            
            const pracGoalsEl = document.getElementById('practice-goals');
            if (pracGoalsEl) pracGoalsEl.innerText = score.player;
        }

        function resetField() {
            ball.owner = null; ball.vx = 0; ball.vy = 0; ball.z = 0; ball.vz = 0;
            isChargingZ = false; chargePower = 0; timeScale = 1.0;
            tackleActive = 0; tackleCooldown = 0;
            ball.x = WORLD_W/2; ball.y = WORLD_H/2;
            player.x = WORLD_W/2; player.y = WORLD_H * 0.75;
            player.angle = -Math.PI/2; player.targetAngle = -Math.PI/2;
            cpu.x = WORLD_W/2; cpu.y = WORLD_H * 0.25;
            gkPlayer.x = WORLD_W/2; gkPlayer.y = WORLD_H - 40;
            gkCpu.x = WORLD_W/2; gkCpu.y = 40;
        }

        function spawnBallToFeet() {
            if (gameMode !== 'practice') return;
            ball.owner = player;
            ball.vx = 0; ball.vy = 0; ball.vz = 0; ball.z = 0;
            isChargingZ = false; chargePower = 0; timeScale = 1.0;
        }

        // Input Listeners
        window.addEventListener('keydown', e => {
            const key = e.key.toLowerCase();
            keys[key] = true;
            if (key === 'z' && ball.owner === player && gameState === 'PLAYING') isChargingZ = true;
            if (key === 'x' && tackleCooldown <= 0 && ball.owner !== player && gameState === 'PLAYING') {
                tackleActive = 12; tackleCooldown = 50; 
            }
            if (key === 'p' && gameMode === 'practice') spawnBallToFeet();
        });

        window.addEventListener('keyup', e => {
            keys[e.key.toLowerCase()] = false;
            if (e.key.toLowerCase() === 'z' && isChargingZ) {
                isChargingZ = false; timeScale = 1.0; ball.owner = null;
                const force = 12 + (chargePower * 0.15);
                ball.vx = Math.cos(player.angle) * force;
                ball.vy = Math.sin(player.angle) * force;
                ball.vz = chargePower > 85 ? 6 + (chargePower * 0.05) : 2;
                chargePower = 0;
            }
        });

        // Core Physics and AI
        function update() {
            if (gameState !== 'PLAYING') return;
            
            // Time Slowdown for aiming
            if (isChargingZ) {
                timeScale = 0.1; chargePower = Math.min(100, chargePower + 1.5);
            } else timeScale = 1.0;
            
            const mods = SKILL_MODS[difficulty];
            const ts = timeScale; 
            if (tackleActive > 0) tackleActive -= ts;
            if (tackleCooldown > 0) tackleCooldown -= ts;

            // Stamina System
            const isSprinting = keys['shift'] && !isExhausted;
            if (isSprinting) {
                playerStamina = Math.max(0, playerStamina - 0.6 * ts);
                if (playerStamina <= 0) isExhausted = true;
            } else {
                playerStamina = Math.min(100, playerStamina + 0.2 * ts);
                if (playerStamina > 20) isExhausted = false;
            }
            const staminaBar = document.getElementById('stamina-bar');
            if (staminaBar) staminaBar.style.height = playerStamina + '%';

            // Player Movement
            let moveX = 0, moveY = 0;
            if (keys['arrowleft'] || keys['a']) moveX -= 1;
            if (keys['arrowright'] || keys['d']) moveX += 1;
            if (keys['arrowup'] || keys['w']) moveY -= 1;
            if (keys['arrowdown'] || keys['s']) moveY += 1;

            if (moveX !== 0 || moveY !== 0) {
                player.targetAngle = Math.atan2(moveY, moveX);
                let diff = player.targetAngle - player.angle;
                while (diff < -Math.PI) diff += Math.PI * 2;
                while (diff > Math.PI) diff -= Math.PI * 2;
                player.angle += diff * 0.2 * ts;
                const speed = (isSprinting ? 9 : 5.5) * ts * (tackleActive > 0 ? 1.8 : 1.0);
                player.vx = Math.cos(player.angle) * speed;
                player.vy = Math.sin(player.angle) * speed;
                player.x += player.vx;
                player.y += player.vy;
                player.anim += 0.15 * ts;
            } else { player.vx = 0; player.vy = 0; }

            // CPU AI
            if (gameMode !== 'practice') {
                const dx = ball.x - cpu.x; const dy = ball.y - cpu.y;
                const dist = Math.hypot(dx, dy);
                const spd = ball.owner === cpu ? mods.dribble : mods.speed;
                cpu.angle = Math.atan2(dy, dx);
                cpu.vx = (dx / dist) * spd * ts;
                cpu.vy = (dy / dist) * spd * ts;
                cpu.x += cpu.vx; cpu.y += cpu.vy;
                cpu.anim += 0.15 * ts;
                if (dist < 35 && ball.z < 25) {
                    if (!ball.owner) ball.owner = cpu;
                    else if (ball.owner === player && Math.random() < mods.cpuTackle) ball.owner = cpu;
                }
                if (ball.owner === cpu && cpu.y > WORLD_H * 0.7 && Math.random() < mods.shoot) {
                    ball.owner = null;
                    const shootAngle = Math.atan2(WORLD_H - ball.y, WORLD_W/2 - ball.x);
                    ball.vx = Math.cos(shootAngle) * 18 * ts;
                    ball.vy = Math.sin(shootAngle) * 18 * ts;
                    ball.vz = 4;
                }
            } else { cpu.x = -1000; }

            // Tackle logic
            if (tackleActive > 0 && ball.owner === cpu) {
                if (Math.hypot(player.x - ball.x, player.y - ball.y) < 60) {
                    ball.owner = player; tackleActive = 0;
                }
            }

            // Goalkeeper AI
            [gkPlayer, gkCpu].forEach(gk => {
                if (gameMode === 'practice' && gk === gkPlayer) { gk.x = -1000; return; }
                const isTop = (gk === gkCpu);
                const goalY = isTop ? 40 : WORLD_H - 40;
                const boxBoundaryY = isTop ? BOX_18_H + 40 : WORLD_H - BOX_18_H - 40;
                
                const ballInBoxX = ball.x > WORLD_W/2 - BOX_18_W/2 && ball.x < WORLD_W/2 + BOX_18_W/2;
                const ballInBoxY = isTop ? (ball.y < boxBoundaryY) : (ball.y > boxBoundaryY);
                
                let targetX, targetY;

                if (ballInBoxX && ballInBoxY && ball.z < 50) {
                    targetX = ball.x;
                    targetY = ball.y;
                    const dx = targetX - gk.x;
                    const dy = targetY - gk.y;
                    const dist = Math.hypot(dx, dy);
                    const rushSpeed = 4.5 * mods.gkRush;
                    if (dist > 0) {
                        gk.x += (dx / dist) * rushSpeed * ts;
                        gk.y += (dy / dist) * rushSpeed * ts;
                    }
                    gk.angle = Math.atan2(dy, dx);
                } else {
                    targetX = Math.max(WORLD_W/2 - 80, Math.min(WORLD_W/2 + 80, ball.x));
                    targetY = goalY;
                    gk.x += (targetX - gk.x) * 0.15 * ts;
                    gk.y += (targetY - gk.y) * 0.15 * ts;
                    gk.angle = isTop ? Math.PI/2 : -Math.PI/2;
                }

                if (Math.hypot(ball.x - gk.x, ball.y - gk.y) < 45 && ball.z < 40) {
                    ball.owner = null;
                    ball.vx = (ball.x - gk.x) * 0.5 + (Math.random() - 0.5) * 5;
                    ball.vy = isTop ? 12 : -12;
                    ball.vz = 3;
                }
            });

            // Ball Physics
            if (ball.owner) {
                const dist = 24;
                ball.x = ball.owner.x + Math.cos(ball.owner.angle) * dist;
                ball.y = ball.owner.y + Math.sin(ball.owner.angle) * dist;
                ball.vx = ball.owner.vx; ball.vy = ball.owner.vy;
                ball.z = 0;
            } else {
                ball.x += ball.vx; ball.y += ball.vy;
                ball.z += ball.vz * ts;
                if (ball.z > 0) ball.vz -= 0.25 * ts; else { ball.z = 0; ball.vz = 0; }
                ball.vx *= 0.985; ball.vy *= 0.985;
                if (Math.hypot(player.x - ball.x, player.y - ball.y) < 35 && ball.z < 25 && !isChargingZ) ball.owner = player;
            }

            // Scoring and Boundary logic
            if (ball.y < 0 || ball.y > WORLD_H) {
                if (ball.x > WORLD_W/2 - GOAL_W/2 && ball.x < WORLD_W/2 + GOAL_W/2 && ball.z < 75) {
                    const scoredOnCpu = ball.y < 0;
                    if (scoredOnCpu) score.player++; else score.cpu++;
                    if (gameMode === 'practice') { updateUI(); resetField(); }
                    else handleStatus("GOAL!", scoredOnCpu ? 'cpu-kick' : 'player-kick');
                } else if (gameMode !== 'practice') handleStatus("OUT!", 'center');
                else resetField();
            }
            camera.x = Math.max(0, Math.min(WORLD_W - canvas.width, player.x - canvas.width/2));
            camera.y = Math.max(0, Math.min(WORLD_H - canvas.height, player.y - canvas.height/2));
        }

        // Display Status Messages
        function handleStatus(text, type) {
            gameState = 'STATUS';
            const msg = document.getElementById('status-msg');
            const stext = document.getElementById('status-text');
            if (stext) stext.innerText = text;
            if (msg) msg.classList.remove('hidden');
            setTimeout(() => {
                if (msg) msg.classList.add('hidden');
                resetField();
                gameState = 'PLAYING';
                updateUI();
            }, 1500);
        }

        // Rendering Functions
        function drawPlayer(p, country) {
            ctx.save();
            ctx.translate(p.x, p.y);
            if (p === player) {
                ctx.strokeStyle = 'rgba(96, 165, 250, 0.5)';
                ctx.lineWidth = 2;
                ctx.beginPath(); ctx.arc(0, 0, 30, 0, Math.PI*2); ctx.stroke();
            }
            ctx.rotate(p.angle);
            const walk = Math.sin(p.anim);
            ctx.fillStyle = country.sock;
            ctx.fillRect(-10, 8 + walk * 6, 8, 10);
            ctx.fillRect(2, 8 - walk * 6, 8, 10);
            ctx.fillStyle = p.isGK ? '#facc15' : country.color; 
            ctx.beginPath(); ctx.roundRect(-15, -12, 30, 24, 6); ctx.fill();
            ctx.fillStyle = '#ffdbac'; ctx.beginPath(); ctx.arc(0, 0, 10, 0, Math.PI*2); ctx.fill();
            ctx.fillStyle = '#333'; ctx.fillRect(6, -4, 4, 3); ctx.fillRect(6, 1, 4, 3);
            ctx.restore();
        }

        function draw() {
            ctx.clearRect(0,0,canvas.width,canvas.height);
            ctx.save(); ctx.translate(-camera.x, -camera.y);
            
            // Draw Pitch
            ctx.fillStyle = '#166534'; ctx.fillRect(0, 0, WORLD_W, WORLD_H);
            ctx.strokeStyle = 'rgba(255,255,255,0.2)'; ctx.lineWidth = 3;
            
            // Pitch Lines
            ctx.strokeRect(40, 40, WORLD_W-80, WORLD_H-80);
            ctx.beginPath(); ctx.moveTo(40, WORLD_H/2); ctx.lineTo(WORLD_W-40, WORLD_H/2); ctx.stroke();
            ctx.beginPath(); ctx.arc(WORLD_W/2, WORLD_H/2, 90, 0, Math.PI*2); ctx.stroke();
            ctx.strokeRect(WORLD_W/2 - BOX_18_W/2, 40, BOX_18_W, BOX_18_H);
            ctx.strokeRect(WORLD_W/2 - BOX_18_W/2, WORLD_H - BOX_18_H - 40, BOX_18_W, BOX_18_H);
            ctx.strokeRect(WORLD_W/2 - BOX_6_W/2, 40, BOX_6_W, BOX_6_H);
            ctx.strokeRect(WORLD_W/2 - BOX_6_W/2, WORLD_H - BOX_6_H - 40, BOX_6_W, BOX_6_H);
            
            // Goals
            ctx.fillStyle = 'white';
            ctx.fillRect(WORLD_W/2 - GOAL_W/2, 20, GOAL_W, 20);
            ctx.fillRect(WORLD_W/2 - GOAL_W/2, WORLD_H - 40, GOAL_W, 20);
            
            if (gameState === 'PLAYING' || gameState === 'STATUS') {
                drawPlayer(player, playerTeam);
                if (gameMode !== 'practice') drawPlayer(cpu, cpuTeam);
                if (gameMode !== 'practice') drawPlayer(gkPlayer, playerTeam);
                drawPlayer(gkCpu, cpuTeam);
                
                // Ball Shadow
                ctx.fillStyle = 'rgba(0,0,0,0.2)';
                ctx.beginPath(); ctx.ellipse(ball.x, ball.y + ball.radius/2, ball.radius, ball.radius/2.5, 0, 0, Math.PI*2); ctx.fill();
                
                // Charge Bar
                if (isChargingZ) {
                    const x = player.x - 30; const y = player.y - 65;
                    ctx.fillStyle = 'black'; ctx.fillRect(x, y, 60, 6);
                    ctx.fillStyle = '#60a5fa'; ctx.fillRect(x, y, (chargePower/100)*60, 6);
                }
                
                // Ball
                ctx.fillStyle = 'white'; ctx.beginPath(); ctx.arc(ball.x, ball.y - ball.z, ball.radius, 0, Math.PI*2); ctx.fill();
                ctx.strokeStyle = 'black'; ctx.lineWidth = 1; ctx.stroke();
            }
            ctx.restore();
            
            // Minimap
            if (gameState === 'PLAYING') {
                miniCtx.fillStyle = '#064e3b'; miniCtx.fillRect(0, 0, miniCanvas.width, miniCanvas.height);
                const sx = miniCanvas.width/WORLD_W; const sy = miniCanvas.height/WORLD_H;
                const dot = (x, y, c, r=3) => { miniCtx.fillStyle = c; miniCtx.beginPath(); miniCtx.arc(x*sx, y*sy, r, 0, Math.PI*2); miniCtx.fill(); };
                dot(player.x, player.y, '#60a5fa', 4);
                if (gameMode !== 'practice') dot(cpu.x, cpu.y, '#f87171', 4);
                dot(ball.x, ball.y, 'white', 2);
            }
        }

        // Global Loop and Sizing
        function gameLoop() { update(); draw(); requestAnimationFrame(gameLoop); }
        function resize() { 
            canvas.width = window.innerWidth; 
            canvas.height = window.innerHeight; 
        }
        window.onload = () => { resize(); requestAnimationFrame(gameLoop); };
        window.onresize = resize;
    </script>
</body>
</html>
