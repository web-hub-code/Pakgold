<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Professional Investment Hub</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #f8fafc; --text: #1e293b; --white: #fff; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: var(--text); }

        /* --- UI Logic --- */
        .page { display: none; min-height: 100vh; padding-bottom: 100px; animation: fadeIn 0.4s ease; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* --- Login Theme --- */
        #login-page { background: linear-gradient(135deg, #fff, #fff9e6); display: flex; align-items: center; justify-content: center; }
        .login-box { width: 90%; max-width: 380px; text-align: center; padding: 40px; background: white; border-radius: 35px; box-shadow: 0 20px 50px rgba(0,0,0,0.05); }
        
        /* --- Dashboard Header --- */
        .header { background: white; padding: 50px 20px 30px; border-bottom: 3px solid var(--gold); border-radius: 0 0 35px 35px; text-align: center; }
        .bal-card { background: linear-gradient(135deg, var(--gold), var(--gold-dark)); color: white; padding: 25px; border-radius: 25px; margin-top: 20px; box-shadow: 0 10px 20px rgba(184,134,11,0.2); }

        /* --- Plan Cards & Timer --- */
        .card { background: white; margin: 15px; padding: 20px; border-radius: 20px; box-shadow: 0 5px 15px rgba(0,0,0,0.03); border: 1px solid #eee; position: relative; }
        .timer { font-size: 0.8rem; color: #ef4444; font-weight: 700; margin-top: 10px; }
        .badge { position: absolute; top: 10px; right: 10px; background: var(--gold); color: white; font-size: 0.6rem; padding: 4px 10px; border-radius: 50px; }

        /* --- Forms & Buttons --- */
        .input-group { margin-bottom: 15px; text-align: left; }
        input { width: 100%; padding: 15px; border-radius: 12px; border: 1px solid #ddd; outline: none; margin-top: 5px; }
        .btn-main { background: var(--gold-dark); color: white; border: none; width: 100%; padding: 15px; border-radius: 15px; font-weight: 800; cursor: pointer; margin-top: 10px; }

        /* --- Navigation --- */
        .nav { position: fixed; bottom: 0; width: 100%; background: white; display: flex; justify-content: space-around; padding: 15px; border-top: 1px solid #eee; border-radius: 25px 25px 0 0; box-shadow: 0 -5px 20px rgba(0,0,0,0.05); z-index: 1000; }
        .nav-item { text-align: center; font-size: 0.7rem; color: #94a3b8; cursor: pointer; }
        .nav-item.active { color: var(--gold-dark); font-weight: 800; }
    </style>
</head>
<body onload="init()">

    <!-- LOGIN -->
    <section id="login" class="page active">
        <div class="login-box">
            <h1 style="color:var(--gold-dark)">PakGold ⚜️</h1>
            <p style="margin-bottom:20px; color:#64748b">Secure Member Access</p>
            <input type="text" id="u-ph" placeholder="Phone Number">
            <input type="password" id="u-ps" placeholder="Password">
            <button class="btn-main" onclick="auth()">LOGIN NOW</button>
        </div>
    </section>

    <!-- HOME (DASHBOARD) -->
    <section id="home" class="page">
        <div class="header">
            <span style="font-size:0.8rem; font-weight:600; opacity:0.6">TOTAL BALANCE</span>
            <h1 id="main-bal" style="font-size:2.5rem; color:var(--gold-dark)">Rs 0.00</h1>
            <div style="display:flex; gap:10px; margin-top:15px;">
                <button class="btn-main" style="padding:10px;" onclick="showPage('deposit')">Deposit</button>
                <button class="btn-main" style="padding:10px; background:#1e293b" onclick="showPage('withdraw')">Withdraw</button>
            </div>
        </div>
        
        <div class="card" style="background:#fff9e6">
            <h4>📢 Official Announcement</h4>
            <p style="font-size:0.8rem">Welcome to PakGold. Withdrawal time is 10 AM to 6 PM daily. 100% Secure.</p>
        </div>

        <div id="plans-container"></div>
    </section>

    <!-- DEPOSIT PAGE -->
    <section id="deposit" class="page">
        <div class="header"><h2>Add Funds</h2></div>
        <div class="card">
            <p>Select Method: <b>EasyPaisa / JazzCash</b></p>
            <input type="number" id="dep-amt" placeholder="Enter Amount (Min 500)">
            <p style="margin-top:10px; font-size:0.8rem">Send to: <b>0344-XXXXXXX</b> (Ahmad)</p>
            <input type="text" id="tid" placeholder="Transaction ID (TID)">
            <button class="btn-main" onclick="alert('Request Submitted!')">SUBMIT DEPOSIT</button>
        </div>
    </section>

    <!-- WITHDRAW PAGE -->
    <section id="withdraw" class="page">
        <div class="header"><h2>Withdraw Profits</h2></div>
        <div class="card">
            <p>Current Balance: <b id="w-bal">Rs 0.00</b></p>
            <input type="number" id="w-amt" placeholder="Amount (Min 600)">
            <input type="text" placeholder="Wallet Account Number">
            <button class="btn-main" style="background:#ef4444" onclick="alert('Insufficient Balance, sweetie!')">REQUEST WITHDRAW</button>
        </div>
    </section>

    <!-- ABOUT & POLICY -->
    <section id="about" class="page">
        <div class="header"><h2>About & Policy</h2></div>
        <div class="card">
            <h4>Company Details</h4>
            <p style="font-size:0.8rem">PakGold is a legally registered mining company providing gold-backed digital investments since 2024.</p>
        </div>
        <div class="card">
            <h4>Privacy Policy</h4>
            <p style="font-size:0.8rem">We never share your phone number. Your data is encrypted with SSL 256-bit security protocols.</p>
        </div>
    </section>

    <!-- NAVIGATION -->
    <nav class="nav" id="main-nav" style="display:none">
        <div class="nav-item" onclick="showPage('home')">🏠<br>Home</div>
        <div class="nav-item" onclick="showPage('deposit')">💰<br>Deposit</div>
        <div class="nav-item" onclick="showPage('withdraw')">💸<br>Withdraw</div>
        <div class="nav-item" onclick="showPage('about')">📜<br>About</div>
    </nav>

    <script>
        const plans = [
            {name: "Silver Miner", price: 1000, daily: 130, time: "24:00:00"},
            {name: "Gold Miner", price: 3000, daily: 410, time: "18:22:10"},
            {name: "Diamond Miner", price: 10000, daily: 1450, time: "12:05:45"}
        ];

        function init() {
            if(localStorage.getItem('isAuth')) {
                showPage('home');
                startIncome();
            }
            renderPlans();
        }

        function auth() {
            let ph = document.getElementById('u-ph').value;
            if(ph.length < 10) return alert("Valid number please, sweetie! 💋");
            localStorage.setItem('isAuth', 'true');
            localStorage.setItem('bal', 0.0);
            showPage('home');
            startIncome();
        }

        function showPage(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('main-nav').style.display = (id === 'login') ? 'none' : 'flex';
            if(id === 'withdraw') document.getElementById('w-bal').innerText = "Rs " + localStorage.getItem('bal');
        }

        function renderPlans() {
            let html = '';
            plans.forEach(p => {
                html += `
                <div class="card">
                    <span class="badge">LIMIT REACHED SOON</span>
                    <h3 style="color:var(--gold-dark)">${p.name}</h3>
                    <p>Price: <b>Rs ${p.price}</b> | Daily: <b>Rs ${p.daily}</b></p>
                    <div class="timer">⏳ Ends in: ${p.time}</div>
                    <button class="btn-main" style="padding:10px; font-size:0.8rem" onclick="alert('Deposit First!')">INVEST NOW</button>
                </div>`;
            });
            document.getElementById('plans-container').innerHTML = html;
        }

        function startIncome() {
            setInterval(() => {
                let b = parseFloat(localStorage.getItem('bal')) + 0.0005;
                localStorage.setItem('bal', b.toFixed(4));
                document.getElementById('main-bal').innerText = "Rs " + b.toFixed(4);
            }, 1000);
        }
    </script>
</body>
</html>
