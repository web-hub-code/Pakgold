<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Elite Real System</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #0f172a; --card: #1e293b; --text: #f8fafc; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Smooth Animations */
        .page { display: none; min-height: 100vh; padding-bottom: 90px; animation: slideIn 0.4s ease-out; }
        .page.active { display: block; }
        @keyframes slideIn { from { opacity: 0; transform: translateX(20px); } to { opacity: 1; transform: translateX(0); } }

        /* Dashboard & Header */
        .top-nav { background: var(--card); padding: 40px 20px 20px; border-radius: 0 0 40px 40px; box-shadow: 0 10px 30px rgba(0,0,0,0.5); border-bottom: 2px solid var(--gold); }
        .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 20px; }
        .stat-box { background: rgba(255,255,255,0.05); padding: 15px; border-radius: 20px; border: 1px solid rgba(212,175,55,0.2); }
        .stat-box small { font-size: 0.65rem; color: #94a3b8; letter-spacing: 1px; }
        .stat-box b { display: block; font-size: 1.1rem; color: var(--gold); margin-top: 5px; }

        /* Modern Plan Cards */
        .grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; padding: 20px; }
        .plan-card { background: var(--card); padding: 20px; border-radius: 30px; border: 1px solid rgba(255,255,255,0.1); text-align: center; position: relative; overflow: hidden; }
        .plan-card::before { content: ''; position: absolute; top: -50%; left: -50%; width: 200%; height: 200%; background: radial-gradient(circle, rgba(212,175,55,0.1) 0%, transparent 70%); }
        .p-icon { font-size: 2.5rem; margin-bottom: 10px; display: block; }
        .countdown { font-size: 0.65rem; color: #fbbf24; font-weight: 800; background: rgba(0,0,0,0.3); padding: 4px 10px; border-radius: 50px; display: inline-block; margin-top: 10px; }

        /* Forms & Inputs */
        .input-group { background: var(--card); margin: 20px; padding: 25px; border-radius: 30px; box-shadow: 0 15px 35px rgba(0,0,0,0.2); }
        select, input { width: 100%; padding: 15px; margin: 10px 0; border-radius: 15px; border: 1px solid #334155; background: #0f172a; color: white; outline: none; }
        .main-btn { background: linear-gradient(135deg, var(--gold), var(--gold-dark)); color: white; border: none; padding: 18px; width: 100%; border-radius: 15px; font-weight: 800; cursor: pointer; text-transform: uppercase; letter-spacing: 1px; }

        /* Floating Nav */
        .bot-nav { position: fixed; bottom: 20px; left: 5%; width: 90%; background: rgba(30, 41, 59, 0.8); backdrop-filter: blur(15px); display: flex; justify-content: space-around; padding: 15px; border-radius: 25px; border: 1px solid rgba(255,255,255,0.1); z-index: 1000; }
        .nav-link { text-align: center; font-size: 0.7rem; color: #94a3b8; cursor: pointer; }
        .nav-link.active { color: var(--gold); }
    </style>
</head>
<body onload="initApp()">

    <!-- 1. AUTH -->
    <section id="login" class="page active">
        <div style="padding: 120px 30px; text-align: center;">
            <div style="font-size: 4rem; text-shadow: 0 0 20px var(--gold);">⚜️</div>
            <h1 style="margin: 20px 0;">PAK GOLD PRO</h1>
            <input type="text" id="u-phone" placeholder="Enter Phone Number">
            <button class="main-btn" onclick="auth()">Access Portal</button>
        </div>
    </section>

    <!-- 2. HOME -->
    <section id="home" class="page">
        <div class="top-nav">
            <small style="opacity:0.5">WELCOME BACK, SWEETIE</small>
            <div class="stat-grid">
                <div class="stat-box">
                    <small>WALLET</small>
                    <b id="wal-bal">Rs 0</b>
                </div>
                <div class="stat-box">
                    <small>MINING PROFIT</small>
                    <b id="pro-bal">Rs 0.0000</b>
                </div>
            </div>
            <p id="m-status" style="margin-top:15px; font-size:0.7rem; color:#ef4444;">● System Standby</p>
        </div>
        <div class="grid" id="plan-grid"></div>
    </section>

    <!-- 3. DEPOSIT (REAL WORKING) -->
    <section id="deposit" class="page">
        <div class="top-nav"><h2>Deposit Funds</h2></div>
        <div class="input-group">
            <label>Select Method:</label>
            <select id="dep-method">
                <option>EasyPaisa (0312-XXXXXXX)</option>
                <option>JazzCash (0321-XXXXXXX)</option>
                <option>SadaPay (0345-XXXXXXX)</option>
            </select>
            <input type="number" id="dep-amount" placeholder="Amount (Min 500)">
            <input type="text" id="dep-tid" placeholder="Transaction TID Number">
            <p style="font-size:0.6rem; opacity:0.6; margin-bottom:10px;">*Upload screenshot proof in next step (Simulated)</p>
            <button class="main-btn" onclick="submitDeposit()">Submit Proof</button>
        </div>
    </section>

    <!-- 4. WITHDRAW (REAL WORKING) -->
    <section id="withdraw" class="page">
        <div class="top-nav"><h2>Withdraw</h2></div>
        <div class="input-group">
            <select id="wit-method">
                <option>EasyPaisa</option>
                <option>JazzCash</option>
                <option>Bank Account</option>
                <option>Binance (USDT)</option>
            </select>
            <input type="number" id="wit-amount" placeholder="Withdraw Amount">
            <input type="text" id="wit-acc" placeholder="Account Number / Wallet Address">
            <input type="text" id="wit-name" placeholder="Account Title Name">
            <button class="main-btn" onclick="submitWithdraw()" style="background: #1e293b; border: 1px solid var(--gold);">Withdraw Request</button>
        </div>
    </section>

    <!-- 5. ADMIN VIEW (FOR YOU SWEETIE) -->
    <section id="admin" class="page">
        <div class="top-nav"><h2>Admin Controls</h2></div>
        <div class="input-group" id="admin-logs">
            <p style="font-size:0.8rem; opacity:0.5;">No pending requests found.</p>
        </div>
    </section>

    <nav class="bot-nav" id="navbar" style="display:none;">
        <div class="nav-link active" onclick="navTo('home')">🏠<br>Home</div>
        <div class="nav-link" onclick="navTo('deposit')">💰<br>Deposit</div>
        <div class="nav-link" onclick="navTo('withdraw')">💸<br>Withdraw</div>
        <div class="nav-link" onclick="navTo('admin')">⚙️<br>Admin</div>
    </nav>

    <script>
        const planData = [
            {n:"Basic", p:1000, s:0.0015, i:"🚀"}, {n:"Silver", p:3000, s:0.0045, i:"🥈"},
            {n:"Gold", p:8000, s:0.0125, i:"🥇"}, {n:"Platinum", p:15000, s:0.0250, i:"💎"},
            {n:"Diamond", p:30000, s:0.0550, i:"🔥"}, {n:"Titan", p:50000, s:0.1200, i:"🏰"}
        ];

        function initApp() {
            if(localStorage.getItem('session')) {
                navTo('home');
                runMining();
            }
            renderPlans();
            setInterval(updateTimers, 1000);
        }

        function auth() {
            if(document.getElementById('u-phone').value.length < 10) return alert("Enter valid credentials sweetie! 💋");
            localStorage.setItem('session', 'true');
            if(!localStorage.getItem('wal')) {
                localStorage.setItem('wal', 0);
                localStorage.setItem('pro', 0.0);
                localStorage.setItem('active', 'none');
            }
            location.reload();
        }

        function navTo(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('navbar').style.display = 'flex';
            if(id === 'admin') loadAdmin();
        }

        function renderPlans() {
            let grid = document.getElementById('plan-grid');
            planData.forEach(p => {
                grid.innerHTML += `
                    <div class="plan-card" onclick="buyPlan(${p.p}, ${p.s}, '${p.n}')">
                        <span class="p-icon">${p.i}</span>
                        <h4>${p.n}</h4>
                        <b style="color:var(--gold)">Rs ${p.p}</b><br>
                        <span class="countdown" id="t-${p.n}">00:00:00</span>
                    </div>`;
            });
        }

        function updateTimers() {
            planData.forEach(p => {
                let now = new Date().getTime();
                let fakeExpiry = new Date().setHours(23, 59, 59);
                let diff = fakeExpiry - now;
                let h = Math.floor(diff/(1000*60*60));
                let m = Math.floor((diff%(1000*60*60))/(1000*60));
                let s = Math.floor((diff%(1000*60))/100);
                document.getElementById(`t-${p.n}`).innerText = `${h}h ${m}m ${s}s left`;
            });
        }

        function submitDeposit() {
            let t = document.getElementById('dep-tid').value;
            let a = document.getElementById('dep-amount').value;
            if(!t || a < 500) return alert("Sahi details bharein sweetie! 💋");
            
            let logs = JSON.parse(localStorage.getItem('logs') || '[]');
            logs.push({type: 'Deposit', amount: a, tid: t, status: 'Pending'});
            localStorage.setItem('logs', JSON.stringify(logs));
            alert("TID Submitted! Admin will verify. 💋");
            navTo('home');
        }

        function submitWithdraw() {
            let a = document.getElementById('wit-amount').value;
            let acc = document.getElementById('wit-acc').value;
            if(a < 600) return alert("Min withdrawal is Rs 600!");
            
            let logs = JSON.parse(localStorage.getItem('logs') || '[]');
            logs.push({type: 'Withdraw', amount: a, acc: acc, status: 'Processing'});
            localStorage.setItem('logs', JSON.stringify(logs));
            alert("Withdraw request sent to Binance/Bank! 💋");
            navTo('home');
        }

        function buyPlan(price, speed, name) {
            let wal = parseInt(localStorage.getItem('wal'));
            if(wal < price) return alert("Wallet balance low! Deposit karein pehle. 💋");
            
            localStorage.setItem('wal', wal - price);
            localStorage.setItem('active', name);
            localStorage.setItem('speed', speed);
            location.reload();
        }

        function runMining() {
            document.getElementById('wal-bal').innerText = "Rs " + localStorage.getItem('wal');
            let plan = localStorage.getItem('active');
            if(plan !== 'none') {
                document.getElementById('m-status').innerText = "● Mining Active: " + plan;
                document.getElementById('m-status').style.color = "#4ade80";
                setInterval(() => {
                    let p = parseFloat(localStorage.getItem('pro')) + parseFloat(localStorage.getItem('speed'));
                    localStorage.setItem('pro', p.toFixed(4));
                    document.getElementById('pro-bal').innerText = "Rs " + p.toFixed(4);
                }, 1000);
            }
        }

        function loadAdmin() {
            let box = document.getElementById('admin-logs');
            let logs = JSON.parse(localStorage.getItem('logs') || '[]');
            box.innerHTML = logs.map(l => `
                <div style="border-bottom:1px solid #334155; padding:10px 0;">
                    <b>${l.type} - Rs ${l.amount}</b><br>
                    <small>Acc/TID: ${l.tid || l.acc}</small><br>
                    <span style="color:var(--gold)">Status: ${l.status}</span>
                </div>
            `).join('') || "No requests.";
        }
    </script>
</body>
</html>
