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
            --text-main: #ffffff;
            --text-dim: #cbd5e1;
        }

        ::-webkit-scrollbar { display: none; }

        body {
            font-family: 'Inter', -apple-system, sans-serif;
            background: var(--bg);
            background-image: radial-gradient(circle at top right, #1e1b4b, #0f172a);
            color: var(--text-main);
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            overflow: hidden;
            perspective: 1000px;
            user-select: none;
            -webkit-user-select: none;
        }

        .game-card {
            background: rgba(15, 23, 42, 0.85); /* Mørkere bakgrunn for bedre kontrast */
            backdrop-filter: blur(16px);
            border: 4px solid var(--wood);
            padding: 3rem 2.5rem;
            border-radius: 28px;
            text-align: center;
            width: 480px;
            min-height: 680px;
            position: relative;
            box-shadow: 0 25px 60px rgba(0,0,0,0.7);
            display: flex;
            flex-direction: column;
            justify-content: center;
            transition: transform 0.3s ease;
        }

        @media (max-height: 850px) { .game-card { transform: scale(0.85); } }
        @media (max-height: 720px) { .game-card { transform: scale(0.70); } }

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
        }

        h1 { 
            margin-bottom: 30px; 
            font-size: 3rem;
            background: linear-gradient(to right, #a5b4fc, #e879f9); /* Lysere gradient for klarhet */
            -webkit-background-clip: text; 
            -webkit-text-fill-color: transparent;
            font-weight: 900;
            letter-spacing: -1px;
        }

        .btn {
            border: none;
            color: white;
            padding: 20px;
            border-radius: 16px;
            cursor: pointer;
            font-weight: 900;
            font-size: 1.4rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            transition: 0.2s;
        }

        .time-btn { background: #1e293b; border: 2px solid var(--primary); }
        .time-btn:hover { background: var(--primary); transform: translateY(-3px); box-shadow: 0 5px 15px rgba(99, 102, 241, 0.4); }
        
        .btn-30s { border: 3px solid var(--success); padding: 24px 20px; }
        .btn-dark { background: #334155; border: 2px solid #64748b; }

        .timer-display { 
            font-family: 'Monaco', 'Courier New', monospace; 
            font-size: 5rem; 
            color: #ff4d4d; /* Klarere rød */
            text-shadow: 0 0 25px rgba(255, 77, 77, 0.3);
            margin-bottom: 20px;
            font-weight: 900;
        }

        .stat-box { 
            background: rgba(0,0,0,0.5); 
            padding: 14px; 
            border-radius: 12px; 
            color: #f1f5f9; /* Hvitere tekst på stats */
            font-weight: 800;
            font-size: 1.1rem;
            border: 1px solid rgba(255,255,255,0.05);
        }

        .cube-face {
            position: absolute;
            width: 320px;
            height: 160px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 5rem; /* Enda større tall */
            font-weight: 900;
            background: #ffffff; /* Maksimal kontrast */
            color: #0f172a; /* Dyp mørk farge for tallene */
            border: 8px solid var(--wood);
            border-radius: 12px;
            backface-visibility: hidden;
            letter-spacing: -2px;
        }

        .minus-sign { color: var(--primary); margin: 0 10px; font-weight: 400; }

        input {
            background: #020617; /* Svart bakgrunn for input */
            border: 3px solid #475569;
            border-radius: 18px;
            color: white;
            font-size: 4.5rem;
            padding: 15px;
            width: 100%;
            text-align: center;
            outline: none;
            margin-top: 25px;
            font-weight: 900;
            transition: border-color 0.2s;
        }

        input:focus { border-color: #f8fafc; }
        input::placeholder { color: rgba(255,255,255,0.2); }
        
        .stop-btn { background: var(--error); margin-top: 25px; }
        .feedback { height: 40px; margin-top: 15px; font-weight: 900; font-size: 1.6rem; text-transform: uppercase; }
    </style>
</head>
<body>

<div class="creator-tag">Skaper: Kristian N.W</div>

<div class="game-card">
    <div id="setup-screen">
        <h1>MINUS-KUBEN</h1>
        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
            <button class="btn time-btn" onclick="initGame(10)">10s</button>
            <button class="btn time-btn" onclick="initGame(15)">15s</button>
            <button class="btn time-btn" onclick="initGame(20)">20s</button>
            <button class="btn time-btn" onclick="initGame(45)">45s</button>
            
            <div style="grid-column: span 2; position: relative; margin-top: 10px;">
                <span style="position: absolute; top: -12px; left: 50%; transform: translateX(-50%); font-size: 0.9rem; color: var(--success); font-weight: 900; background: #0f172a; padding: 2px 15px; border-radius: 6px; border: 2px solid var(--success); z-index: 2;">KLASSE-MODUS</span>
                <button class="btn time-btn btn-30s" style="width:100%" onclick="initGame(30)">30 SEKUNDER</button>
            </div>
            
            <button class="btn time-btn" onclick="initGame(60)">1 MIN</button>
            <button class="btn time-btn btn-dark" onclick="startCustom()">⚙️ NY</button>
            <button class="btn time-btn" style="grid-column: span 2; background: var(--primary);" onclick="initGame('infinite')">♾️ EVIG MODUS</button>
        </div>
        <div id="hs-info" style="font-size: 1.2rem; color: #a5b4fc; margin-top: 30px; font-weight: 900;"></div>
    </div>

    <div id="game-screen">
        <div class="timer-display" id="timer">0.0</div>
        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 25px;">
            <div class="stat-box">POENG: <span id="score">0</span></div>
            <div class="stat-box">RPM: <span id="rpm">0</span></div>
            <div class="stat-box" style="color:var(--success)">✅ <span id="correct-count">0</span></div>
            <div class="stat-box" style="color:var(--error)">❌ <span id="wrong-count">0</span></div>
        </div>

        <div style="width: 320px; height: 160px; margin: 30px auto; perspective: 800px;">
            <div class="cube" id="cube" style="width:100%; height:100%; position:relative; transform-style:preserve-3d; transition:transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);">
                <div class="cube-face" style="transform: rotateX(0deg) translateZ(80px);" id="f0"></div>
                <div class="cube-face" style="transform: rotateX(-90deg) translateZ(80px);" id="f1"></div>
                <div class="cube-face" style="transform: rotateX(-180deg) translateZ(80px);" id="f2"></div>
                <div class="cube-face" style="transform: rotateX(-270deg) translateZ(80px);" id="f3"></div>
            </div>
        </div>

        <input type="number" id="ans" placeholder="?" autocomplete="off">
        <button class="btn stop-btn" onclick="endGame()">AVSLUTT</button>
        <div class="feedback" id="msg"></div>
    </div>
</div>

<script>
    const ranges = [[1,4], [2,6], [3,8], [5,10], [7,13], [10,16], [14,22]];
    let currentRangeIdx = 0, score = 0, corrects = 0, wrongs = 0;
    let timeValue = 0, startTime = null, timerInterval = null, rotationCount = 0, currentTask = [0, 0];
    let mode = 'countdown';

    const savedHS = localStorage.getItem('kubeHS') || 0;
    if(savedHS > 0) document.getElementById('hs-info').innerText = `BESTE: ${savedHS} POENG`;

    function startCustom() {
        let val = parseInt(prompt("Sekunder:"));
        if (!isNaN(val) && val >= 5) initGame(val);
    }

    function initGame(val) {
        score = 0; corrects = 0; wrongs = 0; rotationCount = 0; currentRangeIdx = 0;
        mode = val === 'infinite' ? 'infinite' : 'countdown';
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
            document.getElementById('rpm').innerText = (corrects / (Math.max(1, diff) / 60)).toFixed(0);
            if (mode === 'infinite') {
                document.getElementById('timer').innerText = diff.toFixed(1);
            } else {
                let rem = (timeValue - diff).toFixed(1);
                document.getElementById('timer').innerText = Math.max(0, rem);
                if (rem <= 0) endGame();
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
            score++; corrects++;
            if (currentRangeIdx < ranges.length - 1) currentRangeIdx++;
            rotate(1);
            msg.innerText = "RIKTIG!"; msg.style.color = "var(--success)";
        } else {
            score = Math.max(0, score - 1); wrongs++;
            if (currentRangeIdx > 0) currentRangeIdx--;
            rotate(-1);
            msg.innerText = "FEIL!"; msg.style.color = "var(--error)";
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
            <h1>SLUTT!</h1>
            <div style="margin: 20px 0; font-size: 1.6rem; text-align: left; background: rgba(0,0,0,0.4); padding: 30px; border-radius: 16px;">
                <p>🏆 POENG: <b style="color:var(--primary);">${score}</b></p>
                <p>⚡ RPM: <b>${document.getElementById('rpm').innerText}</b></p>
                <p>✅ RIKTIGE: <b>${corrects}</b></p>
            </div>
            <button onclick="location.reload()" class="btn" style="background:var(--success); width:100%;">PRØV IGJEN</button>
        `;
    }

    document.getElementById('ans').addEventListener('keypress', (e) => { if (e.key === 'Enter') check(); });
</script>
</body>
</html>
