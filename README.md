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
            --text-ultra-white: #ffffff;
        }

        ::-webkit-scrollbar {
            display: none;
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            background: var(--bg);
            background-image: radial-gradient(circle at top right, #1e1b4b, #0f172a);
            color: var(--text-ultra-white);
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            scrollbar-width: none; 
            overflow: hidden;
            perspective: 1000px;
            user-select: none;
            -webkit-user-select: none;
            -webkit-font-smoothing: antialiased;
            text-rendering: optimizeLegibility;
        }

        .game-card {
            background: rgba(15, 23, 42, 0.85);
            backdrop-filter: blur(16px);
            border: 4px solid var(--wood);
            padding: 2.5rem 1.5rem;
            border-radius: 28px;
            text-align: center;
            width: 380px;
            min-height: 650px;
            position: relative;
            box-shadow: 0 25px 60px rgba(0,0,0,0.6);
            display: flex;
            flex-direction: column;
            justify-content: center;
            z-index: 2;
        }

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

        h1 { 
            margin-bottom: 25px; 
            font-size: 2.4rem;
            background: linear-gradient(to bottom right, #ffffff, #818cf8); 
            -webkit-background-clip: text; 
            -webkit-text-fill-color: transparent;
            font-weight: 900;
            filter: drop-shadow(0 2px 4px rgba(0,0,0,0.3));
        }

        .time-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
        }

        .btn {
            border: none;
            color: white;
            padding: 16px;
            border-radius: 14px;
            cursor: pointer;
            font-weight: 900;
            font-size: 1.2rem;
            text-shadow: 0 1px 2px rgba(0,0,0,0.5);
            transition: all 0.2s ease;
        }

        .time-btn { 
            background: #1e293b; 
            border: 2px solid var(--primary);
            box-shadow: 0 4px 0 #312e81;
        }

        .time-btn:active {
            transform: translateY(2px);
            box-shadow: 0 2px 0 #312e81;
        }

        .fullscreen-instruction {
            margin-top: 20px;
            font-size: 0.95rem;
            color: #94a3b8;
            font-weight: 700;
            letter-spacing: 0.5px;
            text-transform: uppercase;
        }

        .kb-highlight {
            background: #334155;
            padding: 2px 6px;
            border-radius: 5px;
            color: #ffffff;
            border-bottom: 2px solid #1e293b;
        }

        /* Resten av stilen beholdes */
        .btn-rec-label { position: absolute; top: -10px; left: 50%; transform: translateX(-50%); font-size: 0.75rem; color: var(--success); font-weight: 800; background: #0f172a; padding: 2px 12px; border-radius: 6px; z-index: 2; border: 1.5px solid var(--success); white-space: nowrap; }
        .timer-display { font-family: 'JetBrains Mono', monospace; font-size: 4rem; color: #ff4d4d; text-shadow: 0 0 20px rgba(255, 77, 77, 0.4); font-weight: 900; margin-bottom: 10px; }
        .scene { width: 280px; height: 140px; margin: 20px auto; perspective: 800px; }
        .cube { width: 100%; height: 100%; position: relative; transform-style: preserve-3d; transition: transform 0.5s cubic-bezier(0.4, 0, 0.2, 1); }
        .cube-face { position: absolute; width: 280px; height: 140px; display: flex; align-items: center; justify-content: center; font-size: 4.5rem; font-weight: 900; background: #ffffff; color: #000000; border: 6px solid var(--wood); border-radius: 12px; backface-visibility: hidden; }
        .face-0 { transform: rotateX(0deg) translateZ(70px); }
        .face-1 { transform: rotateX(-90deg) translateZ(70px); }
        .face-2 { transform: rotateX(-180deg) translateZ(70px); }
        .face-3 { transform: rotateX(-270deg) translateZ(70px); }
        input { background: #020617; border: 3px solid #475569; border-radius: 16px; color: #ffffff; font-size: 3.5rem; font-weight: 900; padding: 10px; width: 100%; text-align: center; outline: none; margin-top: 15px; }
        .stop-btn { background: var(--error); width: 100%; margin-top: 20px; padding: 15px; box-shadow: 0 4px 0 #991b1b; }
        #hs-info { font-size: 1rem; color: #818cf8; margin-top: 15px; font-weight: 900; }
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
            
            <div style="position: relative; grid-column: span 2; margin-top: 10px;">
                <span class="btn-rec-label">ANBEFALT</span>
                <button class="btn time-btn" style="width:100%; border-color: var(--success);" onclick="initGame(30)">30 SEKUNDER</button>
            </div>
            
            <button class="btn time-btn" onclick="initGame(60)">1 MIN</button>
            <button class="btn time-btn" style="background:#334155;" onclick="startCustom()">⚙️</button>
            <button class="btn time-btn" style="grid-column: span 2; background: var(--primary);" onclick="initGame('infinite')">♾️ EVIG MODUS</button>
        </div>

        <div class="fullscreen-instruction">
            Trykk <span class="kb-highlight">fn</span> + <span class="kb-highlight">f11</span> For Og Skru På/Av FullSkjerm
        </div>

        <div id="hs-info"></div>
    </div>

    <div id="game-screen" style="display:none;">
        <div class="timer-display" id="timer">0.0</div>
        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 15px;">
            <div style="background: rgba(2,6,23,0.6); padding: 10px; border-radius: 10px; font-weight: 800;">Poeng: <span id="score">0</span></div>
            <div style="background: rgba(2,6,23,0.6); padding: 10px; border-radius: 10px; font-weight: 800;">RPM: <span id="rpm">0</span></div>
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
    </div>
</div>

<script>
    const ranges = [[1,3], [2,5], [3,7], [4,9], [5,12], [7,15], [12,20]];
    let currentRangeIdx = 0, score = 0, corrects = 0;
    let timeValue = 0, startTime = null, timerInterval = null, rotationCount = 0, currentTask = [0, 0];
    let mode = 'countdown';

    const savedHS = localStorage.getItem('kubeHS') || 0;
    if(savedHS > 0) document.getElementById('hs-info').innerText = `BEST: ${savedHS}`;

    function startCustom() {
        let input = prompt("Velg tid (sekunder):");
        let val = parseInt(input);
        if (!isNaN(val) && val >= 5) initGame(val);
    }

    function initGame(val) {
        score = 0; corrects = 0; rotationCount = 0; currentRangeIdx = 0;
        mode = (val === 'infinite') ? 'infinite' : 'countdown';
        timeValue = val;
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
            document.getElementById('rpm').innerText = (corrects / (diff / 60) || 0).toFixed(1);
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
        document.getElementById(`f${faceIdx}`).innerHTML = `${task[0]} <span style="color:var(--primary)">−</span> ${task[1]}`;
    }

    function check() {
        const input = document.getElementById('ans');
        const userAns = parseInt(input.value);
        if (isNaN(userAns)) return;
        if (userAns === (currentTask[0] - currentTask[1])) {
            score++; corrects++;
            if (currentRangeIdx < ranges.length - 1) currentRangeIdx++;
            rotate(1);
        } else {
            score = Math.max(0, score - 1);
            if (currentRangeIdx > 0) currentRangeIdx--;
            rotate(-1);
        }
        document.getElementById('score').innerText = score;
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
        location.reload();
    }

    document.getElementById('ans').addEventListener('keypress', (e) => { if (e.key === 'Enter') check(); });
</script>
</body>
</html>
