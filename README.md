<!DOCTYPE html>
<html lang="no">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minus-Kuben: Master Pro</title>
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
            user-select: none;
            -webkit-user-select: none;
        }

        .game-card {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            border: 4px solid var(--wood);
            padding: 2.5rem 1.5rem;
            border-radius: 24px;
            text-align: center;
            width: 340px;
            min-height: 620px;
            position: relative;
            box-shadow: 0 20px 50px rgba(0,0,0,0.5);
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        #setup-screen { display: flex; flex-direction: column; }
        #game-screen { display: none; }

        h1 { 
            margin-bottom: 25px; 
            font-size: 1.8rem; 
            background: linear-gradient(to right, #818cf8, #c084fc); 
            -webkit-background-clip: text; 
            -webkit-text-fill-color: transparent;
            font-weight: 900;
        }

        .time-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }

        .btn-wrapper {
            position: relative;
            display: flex;
            flex-direction: column;
            grid-column: span 2;
        }

        .btn-rec-label {
            position: absolute;
            top: -8px;
            left: 12px;
            font-size: 0.6rem;
            color: var(--success);
            font-weight: bold;
            background: #1e293b;
            padding: 0 6px;
            border-radius: 4px;
            z-index: 2;
            border: 1px solid var(--success);
        }

        .btn {
            border: none;
            color: white;
            padding: 14px;
            border-radius: 12px;
            cursor: pointer;
            font-weight: bold;
            transition: 0.2s;
            font-size: 1rem;
        }

        .time-btn { background: #1e293b; border: 1px solid var(--primary); }
        .time-btn:hover { background: var(--primary); transform: translateY(-2px); }
        
        .btn-30s { border: 2px solid var(--success); }
        .btn-wide { grid-column: span 2; }
        .btn-dark { background: #334155; border: 1px solid #475569; }

        /* Hjelpetekst for fullskjerm */
        .fullscreen-tip {
            margin-top: 30px;
            font-size: 0.75rem;
            color: #64748b;
            font-style: italic;
        }
        .kb-key {
            background: #334155;
            padding: 2px 5px;
            border-radius: 4px;
            color: #cbd5e1;
            font-family: monospace;
            border-bottom: 2px solid #1e293b;
        }

        .timer-display { 
            font-family: 'Courier New', monospace; 
            font-size: 3rem; 
            color: #f87171; 
            text-shadow: 0 0 15px rgba(248, 113, 113, 0.4);
            margin-bottom: 10px;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
            margin-bottom: 15px;
            font-size: 0.8rem;
        }

        .stat-box { background: rgba(0,0,0,0.3); padding: 8px; border-radius: 8px; color: #94a3b8; }

        .scene {
            width: 240px;
            height: 120px;
            margin: 20px auto;
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
            width: 240px;
            height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3rem;
            font-weight: 900;
            background: #f8fafc;
            color: #1e293b;
            border: 6px solid var(--wood);
            border-radius: 4px;
            backface-visibility: hidden;
        }

        .face-0 { transform: rotateX(0deg) translateZ(60px); }
        .face-1 { transform: rotateX(-90deg) translateZ(60px); }
        .face-2 { transform: rotateX(-180deg) translateZ(60px); }
        .face-3 { transform: rotateX(-270deg) translateZ(60px); }

        .minus-sign { color: var(--primary); margin: 0 10px; }

        input {
            background: #0f172a;
            border: 2px solid #334155;
            border-radius: 12px;
            color: white;
            font-size: 2.5rem;
            padding: 10px;
            width: 100%;
            box-sizing: border-box;
            text-align: center;
            outline: none;
            margin-top: 10px;
            user-select: auto;
            -webkit-user-select: auto;
        }
        
        .stop-btn { background: var(--error); width: 100%; margin-top: 15px; padding: 12px; }
        .feedback { height: 25px; margin-top: 5px; font-weight: bold; }
    </style>
</head>
<body>

<div class="game-card">
    <div id="setup-screen">
        <h1>MINUS-KUBEN</h1>
        <div class="time-grid">
            <button class="btn time-btn" onclick="initGame(10)">10s</button>
            <button class="btn time-btn" onclick="initGame(15)">15s</button>
            <button class="btn time-btn" onclick="initGame(20)">20s</button>
            <button class="btn time-btn" onclick="initGame(45)">45s</button>
            
            <div class="btn-wrapper">
                <span class="btn-rec-label">Anbefalt</span>
                <button class="btn time-btn btn-30s" onclick="initGame(30)">30 sekunder</button>
            </div>
            
            <button class="btn time-btn" onclick="initGame(60)">1 minutt</button>
            <button class="btn time-btn btn-dark" onclick="startCustom()">⚙️ Custom</button>
            <button class="btn time-btn btn-wide" style="background: var(--primary);" onclick="initGame('infinite')">♾️ EVIG MODUS</button>
        </div>

        <div class="fullscreen-tip">
            Hold <span class="kb-key">fn</span> + <span class="kb-key">F11</span> for fullskjerm
        </div>

        <div id="hs-info" style="font-size: 0.8rem; color: var(--primary); margin-top: 20px; font-weight: bold;"></div>
    </div>

    <div id="game-screen">
        <div class="timer-display" id="timer">0.0</div>
        <div class="stats-grid">
            <div class="stat-box">Poeng: <span id="score">0</span></div>
            <div class="stat-box">RPM: <span id="rpm">0</span></div>
            <div class="stat-box">✅: <span id="correct-count">0</span></div>
            <div class="stat-box">❌: <span id="wrong-count">0</span></div>
        </div>

        <div class="scene">
            <div class="cube" id="cube">
                <div class="cube-face face-0" id="f0"></div>
                <div class="cube-face face-1" id="f1"></div>
                <div class="cube-face face-2" id="f2"></div>
                <div class="cube-face face-3" id="f3"></div>
            </div>
        </div>

        <input type="number" id="ans" placeholder="?" autocomplete="off">
        <button class="btn stop-btn" onclick="endGame()">AVSLUTT</button>
        <div class="feedback" id="msg"></div>
    </div>
</div>

<script>
    const ranges = [[1,3], [2,5], [3,7], [4,9], [5,12], [7,15], [12,20]];
    let currentRangeIdx = 0, score = 0, corrects = 0, wrongs = 0;
    let timeValue = 0, startTime = null, timerInterval = null, rotationCount = 0, currentTask = [0, 0];
    let mode = 'countdown';

    const savedHS = localStorage.getItem('kubeHS') || 0;
    if(savedHS > 0) document.getElementById('hs-info').innerText = `BESTE POENGSUM: ${savedHS}`;

    function startCustom() {
        let input = prompt("Velg tid mellom 5 sekunder og 10 minutter (sekunder):");
        let val = parseInt(input);
        if (!isNaN(val)) {
            if (val >= 5 && val <= 600) initGame(val);
            else alert("Vennligst velg mellom 5 og 600.");
        }
    }

    function initGame(val) {
        score = 0; corrects = 0; wrongs = 0; rotationCount = 0; currentRangeIdx = 0;
        if (val === 'infinite') { mode = 'infinite'; timeValue = 0; }
        else { mode = 'countdown'; timeValue = val; }
        document.getElementById('setup-screen').style.display = 'none';
        document.getElementById('game-screen').style.display = 'block';
        currentTask = generateTask();
        updateFace(0, currentTask);
        startTime = Date.now();
        runTimer();
        setTimeout(() => document.getElementById('ans').focus(), 50);
    }

    function runTimer() {
        timerInterval = setInterval(() => {
            const diff = (Date.now() - startTime) / 1000;
            if (diff > 0) document.getElementById('rpm').innerText = (corrects / (diff / 60)).toFixed(1);
            if (mode === 'infinite') {
                document.getElementById('timer').innerText = diff.toFixed(1);
            } else {
                let remaining = (timeValue - diff).toFixed(1);
                document.getElementById('timer').innerText = Math.max(0, remaining);
                if (remaining <= 0) endGame();
            }
        }, 100);
    }

    function generateTask() {
        const r = ranges[currentRangeIdx];
        let n1 = Math.floor(Math.random() * (r[1] - r[0] + 1)) + r[0];
        let n2 = Math.floor(Math.random() * (r[1] - r[0] + 1)) + r[0];
        return [Math.max(n1, n2), Math.min(n1, n2)];
    }

    function updateFace(faceIdx, task) {
        document.getElementById(`f${faceIdx}`).innerHTML = `${task[0]}<span class="minus-sign">−</span>${task[1]}`;
    }

    function check() {
        const input = document.getElementById('ans');
        const userAns = parseInt(input.value);
        if (isNaN(userAns)) return;
        const msg = document.getElementById('msg');
        const correctAns = currentTask[0] - currentTask[1];

        if (userAns === correctAns) {
            score += 1;
            corrects++;
            if (currentRangeIdx < ranges.length - 1) currentRangeIdx++;
            rotate(1);
            msg.innerText = "RIKTIG! +1"; msg.style.color = "var(--success)";
        } else {
            const diff = Math.abs(userAns - correctAns);
            score = Math.max(0, score - diff);
            wrongs++;
            if (currentRangeIdx > 0) currentRangeIdx--;
            rotate(-1);
            msg.innerText = `FEIL! -${diff}`; msg.style.color = "var(--error)";
        }
        document.getElementById('score').innerText = score;
        document.getElementById('correct-count').innerText = corrects;
        document.getElementById('wrong-count').innerText = wrongs;
        setTimeout(() => msg.innerText = "", 600);
    }

    function rotate(dir) {
        rotationCount += dir;
        currentTask = generateTask();
        updateFace(((rotationCount % 4) + 4) % 4, currentTask);
        document.getElementById('cube').style.transform = `rotateX(${rotationCount * 90}deg)`;
        document.getElementById('ans').value = '';
    }

    function endGame() {
        clearInterval(timerInterval);
        if (mode === 'countdown' && score > savedHS) localStorage.setItem('kubeHS', score);
        document.querySelector('.game-card').innerHTML = `
            <h1 style="color:var(--primary)">RESULTAT</h1>
            <div style="margin: 15px 0; font-size: 1.1rem; text-align: left; background: rgba(0,0,0,0.2); padding: 20px; border-radius: 12px; border: 1px solid rgba(255,255,255,0.1);">
                <p>🏆 Poeng: <b>${score}</b></p>
                <p>✅ Riktige: <b>${corrects}</b></p>
                <p>❌ Feil: <b>${wrongs}</b></p>
                <p>⚡ RPM: <b>${document.getElementById('rpm').innerText}</b></p>
            </div>
            <button onclick="location.reload()" style="background:var(--success); color:white; width:100%; padding:15px; border:none; border-radius:12px; font-weight:bold; cursor:pointer; font-size: 1.1rem;">TIL MENY</button>
        `;
    }

    document.getElementById('ans').addEventListener('keypress', (e) => { if (e.key === 'Enter') check(); });
</script>
</body>
</html>
