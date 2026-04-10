<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Elite Mining</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #f1f5f9; --white: #ffffff; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        
        body { background: var(--bg); color: #1e293b; -webkit-tap-highlight-color: transparent; }

        /* --- Section Logic (Zero Bug) --- */
        .page { display: none; min-height: 100vh; animation: fadeIn 0.3s ease-in-out; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* --- Modern Header --- */
        .top-bar { background: white; padding: 40px 20px 20px; text-align: center; border-radius: 0 0 30px 30px; box-shadow: 0 4px 20px rgba(0,0,0,0.05); }
        .balance-card { background: linear-gradient(135deg, #1e293b, #334155); color: white; padding: 25px; border-radius: 25px; margin: 15px 0; position: relative; overflow: hidden; }
        .balance-card::after { content: ''; position: absolute; top: -50%; right: -50%; width: 150px; height: 150px; background: rgba(212, 175, 55, 0.2); border-radius: 50%; }

        /* --- Icon Grid Plans --- */
        .grid-container { display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; padding: 20px; }
        .icon-card { background: white; padding: 20px; border-radius: 20px; text-align: center; border: 1px solid #e2e8f0; transition: 0.3s; position: relative; }
        .icon-card:active { transform: scale(0.95); }
        .plan-icon { font-size: 2.5rem; margin-bottom: 10px; display: block; }
        .price-tag { background: #fff9e6; color: var(--gold-dark); font-weight: 800; font-size: 0.8rem; padding: 4px 10px; border-radius: 50px; }

        /* --- Forms --- */
        .form-box { background: white; margin: 20px; padding: 25px; border-radius: 25px; box-shadow: 0 10px 30px rgba(0,0,0,0.03); }
        input { width: 100%; padding: 15px; margin: 10px 0; border-radius: 12px; border: 1px solid #e2e8f0; outline: none; font-size: 1rem; }
        .btn-gold { background: var(--gold); color: white; border: none; padding: 15px; width: 100%; border-radius: 12px; font-weight: 800; cursor: pointer; box-shadow: 0 5px 15px rgba(212, 175, 55, 0.3); }

        /* --- Navigation --- */
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; display: flex; justify-content: space-around; padding: 15px; border-top: 1px solid #eee; border-radius: 25px 25px 0 0; z-index: 1000; }
        .nav-link { text-align: center; font-size: 0.7rem; color: #94a3b8; text-decoration: none; cursor: pointer; }
        .nav-link.active { color: var(--gold-dark); font-weight: 800; }

        /* --- VIP Section --- */
        .vip-card { grid-column: span 2; background: linear-gradient(135deg, #d4af37, #b8860b); color: white; }
        .vip-card .price-tag { background: rgba(255,255,255,0.2); color: white; }
    </style>
</head>
<body onload="checkLogin()">

    <!-- 1. LOGIN -->
    <section id="login" class="page active">
        <div style="padding: 100px 30px; text-align: center;">
            <h1 style="font-size: 3rem; color: var(--gold-dark);">⚜️</h1>
            <h2 style="margin-bottom: 30px;">Welcome Back, Sweetie</h2>
            <div class="form-box">
                <input type="text" id="user-ph" placeholder="Phone Number">
                <input type="password" id="user-ps" placeholder="Password">
                <button class="btn-gold" onclick="doAuth()">LOGIN</button>
            </div>
        </div>
    </section>

    <!-- 2. HOMEPAGE -->
    <section id="home" class="page">
        <div class="top-bar">
            <span style="font-size: 0.8rem; font-weight: 600; opacity: 0.5;">CURRENT MINING</span>
            <div class="balance-card">
                <h1 id="main-bal" style="font-size: 2.2rem;">Rs 0.0000</h1>
                <p style="font-size: 0.7rem; opacity: 0.8;">24/7 Auto-Earning Active ✅</p>
            </div>
        </div>

        <div class="grid-container" id="plans-grid">
            <!-- Plans will be generated here -->
        </div>
    </section>

    <!-- 3. DEPOSIT -->
    <section id="deposit" class="page">
        <div class="top-bar"><h2>Deposit Funds</h2></div>
        <div class="form-box">
            <p style="font-size: 0.8rem; margin-bottom: 15px;">Send Payment to: <br><b>EasyPaisa: 0311-1234567</b></p>
            <input type="number" placeholder="Amount (Min 500)">
            <input type="text" placeholder="Transaction ID (TID)">
            <button class="btn-gold" onclick="alert('Sent for verification, sweetie! 💋')">SUBMIT DEPOSIT</button>
        </div>
    </section>

    <!-- 4. WITHDRAW -->
    <section id="withdraw" class="page">
        <div class="top-bar"><h2>Withdraw Profits</h2></div>
        <div class="form-box">
            <p>Your Balance: <b id="w-bal">0.00</b></p>
            <input type="number" placeholder="Withdraw Amount">
            <input type="text" placeholder="Account Number">
            <button class="btn-gold" style="background:#1e293b" onclick="alert('Processing... Check history later.')">REQUEST WITHDRAW</button>
        </div>
    </section>

    <!-- 5. ABOUT -->
    <section id="about" class="page">
        <div class="top-bar"><h2>Company Info</h2></div>
        <div class="form-box">
            <h4>About PakGold ⚜️</h4>
            <p style="font-size: 0.8rem; line-height: 1.6; margin-top:10px;">We are a FBR-registered digital assets firm. Your investment is secured by gold reserves. Minimum withdrawal: Rs 600.</p>
            <hr style="margin:20px 0; opacity:0.1;">
            <h4>Privacy Policy</h4>
            <p style="font-size: 0.75rem; opacity:0.7;">We use 256-bit encryption to protect your transaction data.</p>
        </div>
    </section>

    <!-- FOOTER NAV -->
    <nav class="nav-bar" id="bottom-nav" style="display: none;">
        <div class="nav-link active" onclick="navTo('home')">🏠<br>Home</div>
        <div class="nav-link" onclick="navTo('deposit')">💰<br>Deposit</div>
        <div class="nav-link" onclick="navTo('withdraw')">💸<br>Withdraw</div>
        <div class="nav-link" onclick="navTo('about')">📜<br>Profile</div>
    </nav>

    <script>
        // 15 Regular + 5 VIP
        const allPlans = [
            {n: "Starter", p: 500, i: "🌱"}, {n: "Silver", p: 1000, i: "🥈"},
            {n: "Gold", p: 2000, i: "🥇"}, {n: "Diamond", p: 4000, i: "💎"},
            {n: "Pro", p: 6000, i: "🚀"}, {n: "Elite", p: 8000, i: "👑"},
            {n: "Master", p: 10000, i: "🎯"}, {n: "Expert", p: 15000, i: "⚡"},
            {n: "Veteran", p: 20000, i: "🛡️"}, {n: "Titan", p: 25000, i: "🧱"},
            {n: "Ultimate", p: 35000, i: "🌌"}, {n: "Legend", p: 50000, i: "🏆"},
            {n: "Commander", p: 75000, i: "👨‍✈️"}, {n: "Global", p: 100000, i: "🌍"},
            {n: "Imperial", p: 150000, i: "🏰"},
            // VIPs
            {n: "VIP-1", p: 200000, i: "🔥", v: true}, {n: "VIP-2", p: 400000, i: "🌟", v: true},
            {n: "VIP-3", p: 600000, i: "🎇", v: true}, {n: "VIP-4", p: 800000, i: "💫", v: true},
            {n: "VIP-MAX", p: 1000000, i: "💰", v: true}
        ];

        function checkLogin() {
            if(localStorage.getItem('loggedIn') === 'true') {
                navTo('home');
                runEngine();
            }
            renderGrid();
        }

        function doAuth() {
            let ph = document.getElementById('user-ph').value;
            if(ph.length < 10) return alert("Sweetie, enter valid number! 💋");
            localStorage.setItem('loggedIn', 'true');
            localStorage.setItem('balance', 0.0);
            location.reload();
        }

        function navTo(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('bottom-nav').style.display = (id === 'login') ? 'none' : 'flex';
            
            // UI Active State
            document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));
            if(id === 'withdraw') document.getElementById('w-bal').innerText = localStorage.getItem('balance');
        }

        function renderGrid() {
            let grid = document.getElementById('plans-grid');
            allPlans.forEach(p => {
                grid.innerHTML += `
                    <div class="icon-card ${p.v ? 'vip-card' : ''}" onclick="alert('Deposit to activate!')">
                        <span class="plan-icon">${p.i}</span>
                        <h4 style="font-size:0.9rem;">${p.n}</h4>
                        <span class="price-tag">Rs ${p.p.toLocaleString()}</span>
                    </div>
                `;
            });
        }

        function runEngine() {
            setInterval(() => {
                let current = parseFloat(localStorage.getItem('balance')) + 0.0003;
                localStorage.setItem('balance', current.toFixed(4));
                if(document.getElementById('main-bal')) {
                    document.getElementById('main-bal').innerText = "Rs " + current.toFixed(4);
                }
            }, 1000);
        }
    </script>
</body>
</html>
