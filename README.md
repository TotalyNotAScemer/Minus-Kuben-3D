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

        ::-webkit-scrollbar {
            display: none;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: var(--bg);
            background-image: radial-gradient(circle at top right, #1e1b4b, #0f172a);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            scrollbar-width: none; 
            -ms-overflow-style: none; 
            overflow: hidden;
            perspective: 1000px;
            user-select: none;
            -webkit-user-select: none;
            position: relative;
        }

        .game-card {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            border: 4px solid var(--wood);
            padding: 3rem 2.5rem 3rem 2.5rem;
            border-radius: 28px;
            text-align: center;
            width: 480px;
            min-height: 680px;
            position: relative;
            box-shadow: 0 25px 60px rgba(0,0,0,0.6);
            display: flex;
            flex-direction: column;
            justify-content: center;
            transition: transform 0.3s ease;
            z-index: 2;
        }

        @media (max-height: 850px) {
            .game-card { transform: scale(0.85); }
        }

        @media (max-height: 720px) {
            .game-card { transform: scale(0.70); }
        }

        /* Skaper-tag: 80% usynlig (opacity 0.2) */
        .creator-tag {
            position: fixed;
            bottom: 20px;
            left: 20px;
            font-size: 0.9rem;
            color: #818cf8;
            font-weight: 900;
            letter-spacing: 1.5px;
            text-transform: uppercase;
            opacity: 0.2; 
            z-index: 10;
        }

        #setup-screen { display: flex; flex-direction: column; }
        #game-screen { display: none; }

        h1 { 
            margin-bottom: 30px; 
            font-size: 2.8rem;
            background: linear-gradient(to right, #818cf8, #c084fc); 
            -webkit-background-clip: text; 
            -webkit-text-fill-color: transparent;
            font-weight: 900;
        }

        .time-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }

        .btn-wrapper {
            position: relative;
            display: flex;
            flex-direction: column;
            grid-column: span 2;
            margin-top: 10px;
        }

        .btn-rec-label {
            position: absolute;
            top: -12px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 0.85rem;
            color: var(--success);
            font-weight: 800;
            background: #1e293b;
            padding: 3px 15px;
            border-radius: 6px;
            z-index: 2;
            border: 1.5px solid var(--success);
            white-space: nowrap;
        }

        .btn {
            border: none;
            color: white;
            padding: 20px;
            border-radius: 16px;
            cursor: pointer;
            font-weight: 900;
            transition: 0.2s;
            font-size: 1.3rem;
        }

        .time-btn { background: #1e293b; border: 2px solid var(--primary); }
        .time-btn:hover { background: var(--primary); transform: translateY(-3px); }
        
        .btn-30s { border: 3px solid var(--success); padding: 24px 20px; }
        .btn-wide { grid-column: span 2; }
        .btn-dark { background: #334155; border: 2px solid #475569; }

        .fullscreen-tip {
            margin-top: 35px;
            font-size: 1rem;
            color: #94a3b8;
            font-style: italic;
        }

        .kb-key {
            background: #475569;
            padding: 3px 8px;
            border-radius: 6px;
            color: white;
            font-family: monospace;
            border-bottom: 3px solid #1e293b;
        }

        .timer-display { 
            font-family: 'Courier New', monospace; 
            font-size: 4.5rem; 
            color: #f87171; 
            text-shadow: 0 0 20px rgba(248, 113, 113, 0.5);
            margin-bottom: 15px;
            font-weight: 900;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
            margin-bottom: 25px;
            font-size: 1.1rem;
        }

        .stat-box { background: rgba(0,0,0,0.4); padding: 12px; border-radius: 10px; color: #cbd5e1; font-weight: 700; }

        .scene {
            width: 320px;
            height: 160px;
            margin: 30px auto;
            perspective: 800px;
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
            width: 320px;
            height: 160px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 4.5rem;
            font-weight: 900;
            background: #ffffff;
            color: #0f172a;
            border: 8px solid var(--wood);
            border-radius: 8px;
            backface-visibility: hidden;
        }

        .face-0 { transform: rotateX(0deg) translateZ(80px); }
        .face-1 { transform: rotateX(-90deg) translateZ(80px); }
        .face-2 { transform: rotateX(-180deg) translateZ(80px); }
        .face-3 { transform: rotateX(-270deg) translateZ(80px); }

        .minus-sign { color: var(--primary); margin: 0 15px; }

        input {
            background: #0f172a;
            border: 3px solid #475569;
            border-radius: 16px;
            color: white;
            font-size: 4rem;
            padding: 15px;
            width: 100%;
            box-sizing: border-box;
            text-align: center;
            outline: none;
            margin-top: 20px;
        }
        
        .stop-btn { background: var(--error); width: 100%; margin-top: 25px; padding: 18px; font-size: 1.4rem; }
        .feedback { height: 35px; margin-top: 15px; font-weight: 900; font-size: 1.4rem; }
    </style>
</head>
<body>

<div class="creator-tag">Skaper: Kristian N.W</div>

<div class="game-card">
    <div id="setup-screen">
        <h1>MINUS-KUBEN</h1>
        <div class="time-grid">
            <button class="btn time-btn" onclick="initGame(10)">10s</button>
            <button class="btn time-btn" onclick="initGame(15)">15s</button>
            <button class="btn time-btn" onclick="initGame(20)">20s</button>
            <button class="btn time-btn" onclick="initGame(45)">45s</button>
            
            <div class="btn-wrapper">
                <span class="btn-rec-label">ANBEFALT FOR SKOLE</span>
                <button class="btn time-btn btn-30s" onclick="initGame(30)">30 SEKUNDER</button>
            </div>
            
            <button class="btn time-btn" onclick="initGame(60)">1 MINUTT</button>
            <button class="btn time-btn btn-dark" onclick="startCustom()">⚙️ CUSTOM</button>
            <button class="btn time-btn btn-wide" style="background: var(--primary);" onclick="initGame('infinite')">♾️ EVIG MODUS</button>
        </div>

        <div class="fullscreen-tip">
            Hold <span class="kb-key">fn</span> + <span class="kb-key">F11</span> for fullskjerm
        </div>

        <div id="hs-info" style="font-size: 1.1rem; color: var(--primary); margin-top: 25px; font-weight: 900; letter-spacing: 1px;"></div>
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
        let input = prompt("Velg tid (sekunder):");
        let val = parseInt(input);
        if (!isNaN(val) && val >= 5 && val <= 600) initGame(val);
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
            <div style="margin: 20px 0; font-size: 1.5rem; text-align: left; background: rgba(0,0,0,0.3); padding: 30px; border-radius: 16px; border: 2px solid rgba(255,255,255,0.1);">
                <p>🏆 POENG: <b style="color:var(--primary);">${score}</b></p>
                <p>✅ RIKTIGE: <b>${corrects}</b></p>
                <p>❌ FEIL: <b>${wrongs}</b></p>
                <p>⚡ RPM: <b>${document.getElementById('rpm').innerText}</b></p>
            </div>
            <button onclick="location.reload()" style="background:var(--success); color:white; width:100%; padding:22px; border:none; border-radius:16px; font-weight:900; cursor:pointer; font-size: 1.5rem; margin-top:20px;">TIL MENY</button>
        `;
    }

    document.getElementById('ans').addEventListener('keypress', (e) => { if (e.key === 'Enter') check(); });
</script>
</body>
</html>
