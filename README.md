<!DOCTYPE html>
<html lang="no">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minus-Kuben: Master Edition</title>
    <style>
        :root {
            --primary: #6366f1;
            --success: #22c55e;
            --error: #ef4444;
            --bg: #0f172a;
            --wood: #3e2723;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: var(--bg);
            background-image: radial-gradient(circle at top right, #1e1b4b, #0f172a);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            overflow: hidden;
            perspective: 1000px;
        }

        .game-card {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            border: 4px solid var(--wood);
            padding: 2rem;
            border-radius: 24px;
            text-align: center;
            width: 100%;
            max-width: 450px;
            position: relative;
            box-shadow: 0 20px 50px rgba(0,0,0,0.5);
        }

        /* Skjermstyring */
        #setup-screen { display: block; }
        #game-screen { display: none; }

        h1 { margin-bottom: 10px; font-size: 1.8rem; background: linear-gradient(to right, #818cf8, #c084fc); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }

        .time-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin: 20px 0;
        }

        .btn {
            border: none;
            color: white;
            padding: 12px;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
            transition: 0.2s;
        }

        .time-btn { background: #1e293b; border: 1px solid var(--primary); }
        .time-btn:hover { background: var(--primary); }
        
        .special-btn { grid-column: span 3; font-size: 1.1rem; padding: 15px; }
        .infinite-btn { background: var(--primary); }
        .custom-btn { background: #334155; }

        /* Spill-elementer */
        .timer-display { font-family: 'Courier New', monospace; font-size: 2.2rem; color: #f87171; text-shadow: 0 0 10px rgba(248, 113, 113, 0.3); }
        .stats-bar { display: flex; justify-content: space-between; color: #94a3b8; font-size: 0.9rem; margin-top: 10px; }

        .scene {
            width: 260px;
            height: 130px;
            margin: 30px auto;
            perspective: 600px;
        }

        .cube {
            width: 100%;
            height: 100%;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .cube-face {
            position: absolute;
            width: 260px;
            height: 130px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3.2rem;
            font-weight: 900;
            background: #e2e8f0;
            color: #1e293b;
            border: 8px solid var(--wood);
            border-radius: 4px;
            backface-visibility: hidden;
        }

        .face-0 { transform: rotateX(0deg) translateZ(65px); }
        .face-1 { transform: rotateX(-90deg) translateZ(65px); }
        .face-2 { transform: rotateX(-180deg) translateZ(65px); }
        .face-3 { transform: rotateX(-270deg) translateZ(65px); }

        .minus-sign { color: var(--primary); margin: 0 10px; }

        .game-controls { display: flex; gap: 10px; margin-top: 10px; }
        input {
            background: #1e293b;
            border: 2px solid #334155;
            border-radius: 12px;
            color: white;
            font-size: 2rem;
            padding: 10px;
            width: 100%;
            text-align: center;
            outline: none;
        }
        .stop-btn { background: var(--error); width: 80px; font-size: 0.8rem; }

        .feedback { height: 25px; margin-top: 10px; font-weight: bold; }
    </style>
</head>
<body>

<div class="game-card">
    <div id="setup-screen">
        <h1>Minus-Kuben 3D</h1>
        <p style="color:#94a3b8; font-size: 0.9rem;">Velg modus for å starte</p>
        
        <div class="time-grid">
            <button class="btn time-btn" onclick="initGame(10)">10s</button>
            <button class="btn time-btn" onclick="initGame(15)">15s</button>
            <button class="btn time-btn" onclick="initGame(20)">20s</button>
            <button class="btn time-btn" onclick="initGame(30)">30s</button>
            <button class="btn time-btn" onclick="initGame(45)">45s</button>
            <button class="btn time-btn" onclick="initGame(60)">1m</button>
            
            <button class="btn special-btn infinite-btn" onclick="initGame('infinite')">♾️ EVIG MODUS (Tell opp)</button>
            <button class="btn special-btn custom-btn" onclick="startCustom()">Egendefinert tid</button>
        </div>
        <div id="hs-info" style="font-size: 0.8rem; color: var(--primary);"></div>
    </div>

    <div id="game-screen">
        <div class="timer-display" id="timer">0.0</div>
        <div class="stats-bar">
            <span id="counter">Poeng: 0</span>
            <span id="mode-label">Modus: --</span>
        </div>

        <div class="scene">
            <div class="cube" id="cube">
                <div class="cube-face face-0" id="f0"></div>
                <div class="cube-face face-1" id="f1"></div>
                <div class="cube-face face-2" id="f2"></div>
                <div class="cube-face face-3" id="f3"></div>
            </div>
        </div>

        <div class="game-controls">
            <input type="number" id="ans" placeholder="?" autocomplete="off">
            <button class="btn stop-btn" onclick="endGame()">STOPP</button>
        </div>
        <div class="feedback" id="msg"></div>
    </div>
</div>

<script>
    const ranges = [[1,3], [2,5], [3,7], [4,9], [5,12], [7,15], [12,20]];
    let currentRangeIdx = 0;
    let solvedCount = 0;
    let mode = ''; // 'countdown' eller 'infinite'
    let timeValue = 0;
    let startTime = null;
    let timerInterval = null;
    let rotationCount = 0;
    let currentTask = [0, 0];

    // Highscore logikk
    const savedHS = localStorage.getItem('kubeHS') || 0;
    if(savedHS > 0) document.getElementById('hs-info').innerText = `Din Highscore: ${savedHS} poeng`;

    function startCustom() {
        let val = prompt("Antall sekunder?");
        if (val && !isNaN(val)) initGame(parseInt(val));
    }

    function initGame(val) {
        solvedCount = 0;
        rotationCount = 0;
        currentRangeIdx = 0;
        
        if (val === 'infinite') {
            mode = 'infinite';
            timeValue = 0;
            document.getElementById('mode-label').innerText = "Modus: Evig";
        } else {
            mode = 'countdown';
            timeValue = val;
            document.getElementById('mode-label').innerText = `Mål: ${val}s`;
        }

        document.getElementById('setup-screen').style.display = 'none';
        document.getElementById('game-screen').style.display = 'block';
        
        currentTask = generateTask();
        updateFace(0, currentTask);
        
        startTime = Date.now();
        runTimer();
        document.getElementById('ans').focus();
    }

    function runTimer() {
        timerInterval = setInterval(() => {
            const now = Date.now();
            const diff = (now - startTime) / 1000;

            if (mode === 'infinite') {
                document.getElementById('timer').innerText = diff.toFixed(1);
            } else {
                let remaining = (timeValue - diff).toFixed(1);
                document.getElementById('timer').innerText = remaining;
                if (remaining <= 0) endGame();
            }
        }, 100);
    }

    function generateTask() {
        const r = ranges[currentRangeIdx];
        let n1 = Math.floor(Math.random() * (r[1] - r[0] + 1)) + r[0];
        let n2 = Math.floor(Math.random() * (r[1] - r[0] + 1)) + r[0];
        const t1 = Math.max(n1, n2);
        const t2 = Math.min(n1, n2);
        return (t1 === t2) ? [t1 + 1, t2] : [t1, t2];
    }

    function updateFace(faceIdx, task) {
        document.getElementById(`f${faceIdx}`).innerHTML = `${task[0]}<span class="minus-sign">−</span>${task[1]}`;
    }

    function check() {
        const input = document.getElementById('ans');
        const val = parseInt(input.value);
        if (isNaN(val)) return;

        if (val === (currentTask[0] - currentTask[1])) {
            solvedCount++;
            document.getElementById('counter').innerText = `Poeng: ${solvedCount}`;
            if (currentRangeIdx < ranges.length - 1) currentRangeIdx++;
            rotate(1);
        } else {
            if (currentRangeIdx > 0) currentRangeIdx--;
            rotate(-1);
            const msg = document.getElementById('msg');
            msg.innerText = "FEIL! ↩️";
            msg.style.color = "var(--error)";
            setTimeout(() => msg.innerText = "", 600);
        }
    }

    function rotate(dir) {
        rotationCount += dir;
        currentTask = generateTask();
        let faceIdx = ((rotationCount % 4) + 4) % 4;
        updateFace(faceIdx, currentTask);
        document.getElementById('cube').style.transform = `rotateX(${rotationCount * 90}deg)`;
        document.getElementById('ans').value = '';
    }

    function endGame() {
        clearInterval(timerInterval);
        const finalTime = ((Date.now() - startTime) / 1000).toFixed(2);
        
        // Lagre Highscore hvis det var en tidsbegrenset runde
        if (mode === 'countdown' && solvedCount > savedHS) {
            localStorage.setItem('kubeHS', solvedCount);
        }

        document.querySelector('.game-card').innerHTML = `
            <h1 style="color:var(--primary)">Runde Ferdig!</h1>
            <div style="margin: 20px 0; font-size: 1.2rem;">
                <p>Poeng: <b>${solvedCount}</b></p>
                <p>Tid brukt: <b>${finalTime}s</b></p>
                <p>Snittfart: <b>${(finalTime/solvedCount || 0).toFixed(2)}s/stk</b></p>
            </div>
            <button onclick="location.reload()" style="background:var(--success); color:white; width:100%; padding:15px; border:none; border-radius:12px; font-weight:bold; cursor:pointer;">HOVEDMENY</button>
        `;
    }

    document.getElementById('ans').addEventListener('keypress', (e) => {
        if (e.key === 'Enter') check();
    });
</script>

</body>
</html>
