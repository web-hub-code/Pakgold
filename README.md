<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Trusted Premium Mining</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #f8fafc; --text: #1e293b; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        
        body { background: var(--bg); color: var(--text); overflow-x: hidden; width: 100%; height: 100vh; }

        /* --- Animations --- */
        @keyframes flowBg { 0% { background-position: 0% 50%; } 50% { background-position: 100% 50%; } 100% { background-position: 0% 50%; } }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes marquee { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        /* --- Section System --- */
        .section { display: none !important; width: 100%; min-height: 100vh; }
        .section.active { display: block !important; animation: fadeIn 0.5s ease-out; }

        /* --- Modern Login Page --- */
        #login-page {
            background: linear-gradient(-45deg, #f1f5f9, #ffffff, #fff9e6, #f1f5f9);
            background-size: 400% 400%;
            animation: flowBg 12s ease infinite;
            display: flex !important; align-items: center; justify-content: center;
        }
        .login-card {
            width: 90%; max-width: 400px; padding: 45px 30px;
            background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(10px);
            border-radius: 40px; border: 1px solid rgba(212, 175, 55, 0.2);
            box-shadow: 0 25px 60px rgba(184, 134, 11, 0.1); text-align: center;
        }
        .logo-ring { width: 80px; height: 80px; background: var(--gold); border-radius: 50%; margin: 0 auto 20px; display: flex; align-items: center; justify-content: center; font-size: 2rem; color: white; box-shadow: 0 10px 20px rgba(212, 175, 55, 0.3); }

        /* --- Trusted Ticker --- */
        .ticker-wrap {
            background: #1e293b; color: #fff; padding: 10px 0; font-size: 0.75rem;
            position: fixed; top: 0; width: 100%; z-index: 2000; overflow: hidden; display: none;
        }
        .ticker { display: inline-block; white-space: nowrap; animation: marquee 25s linear infinite; }

        /* --- Dashboard --- */
        .header { background: white; padding: 60px 20px 30px; text-align: center; border-bottom: 4px solid var(--gold); border-radius: 0 0 40px 40px; box-shadow: 0 10px 30px rgba(0,0,0,0.02); }
        .bal-display { font-size: 3rem; font-weight: 800; color: var(--gold-dark); margin: 5px 0; }
        .shield-badge { background: #e3fcef; color: #00875a; font-size: 0.65rem; padding: 5px 12px; border-radius: 50px; font-weight: 700; display: inline-block; }

        .container { padding: 25px 20px 120px; }
        .plan-card { background: white; border-radius: 25px; padding: 25px; margin-bottom: 20px; border: 1px solid #edf2f7; position: relative; overflow: hidden; }
        .hot-tag { position: absolute; top: 12px; right: -25px; background: #ff4757; color: white; font-size: 0.6rem; padding: 4px 30px; transform: rotate(45deg); font-weight: 800; }

        /* --- Inputs & Buttons --- */
        .input-box { width: 100%; padding: 18px; margin-bottom: 15px; border-radius: 15px; border: 1px solid #e2e8f0; outline: none; transition: 0.3s; font-size: 1rem; }
        .input-box:focus { border-color: var(--gold); box-shadow: 0 0 10px rgba(212, 175, 55, 0.1); }
        .btn-gold { background: linear-gradient(135deg, var(--gold), var(--gold-dark)); color: white; border: none; padding: 18px; border-radius: 18px; width: 100%; font-weight: 800; cursor: pointer; font-size: 1.1rem; box-shadow: 0 8px 20px rgba(184, 134, 11, 0.2); }

        /* --- Nav & Support --- */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: white; display: none; justify-content: space-around; padding: 18px; border-top: 1px solid #f1f5f9; border-radius: 30px 30px 0 0; z-index: 3000; box-shadow: 0 -10px 30px rgba(0,0,0,0.05); }
        .support-float { position: fixed; bottom: 100px; right: 20px; width: 60px; height: 60px; background: #25d366; border-radius: 50%; display: none; align-items: center; justify-content: center; color: white; font-size: 1.8rem; z-index: 3000; box-shadow: 0 10px 20px rgba(0,0,0,0.1); text-decoration: none; }
    </style>
</head>
<body onload="checkStatus()">

    <!-- 1. LIVE TICKER -->
    <div class="ticker-wrap" id="t-bar">
        <div class="ticker">
            🛡️ SSL SECURED GATEWAY ... ⚡ User_991* withdrawn Rs 3,400 ... ✅ New Miner activated by User_772* ... 🛡️ PakGold Official Verified ...
        </div>
    </div>

    <!-- 2. LOGIN PAGE -->
    <section id="login-page" class="section active">
        <div class="login-card">
            <div class="logo-ring">⚜️</div>
            <h1 style="color:var(--gold-dark); font-size:2.5rem; font-weight:800;">PakGold</h1>
            <p style="color:#64748b; margin-bottom:30px;">The Most Trusted Mining Network</p>
            <input type="text" id="phone" class="input-box" placeholder="Phone Number">
            <input type="password" id="pass" class="input-box" placeholder="Password">
            <button class="btn-gold" onclick="doLogin()">SECURE LOGIN</button>
            <p style="margin-top:20px; font-size:0.7rem; color:#94a3b8;">Protected by 256-bit Encryption 🔒</p>
        </div>
    </section>

    <!-- 3. DASHBOARD PAGE -->
    <section id="dash-page" class="section">
        <div class="header">
            <span class="shield-badge">🛡️ FBR VERIFIED SYSTEM</span>
            <div class="bal-display" id="bal-txt">₨ 0.0000</div>
            <p id="u-id" style="font-weight:700; color:var(--gold-dark);"></p>
        </div>

        <div class="container">
            <h3 style="margin-bottom:20px;">Secured Investment Plans</h3>
            <div class="plan-card">
                <div class="hot-tag">VIP</div>
                <h4 style="color:var(--gold-dark)">Starter Miner</h4>
                <h2 style="margin:10px 0;">₨ 1,000</h2>
                <p style="font-size:0.8rem; color:#64748b;">Daily: Rs 130 | Period: 30 Days</p>
                <button class="btn-gold" style="margin-top:15px; padding:12px;" onclick="alert('Proceeding to Secure Gateway...')">INVEST NOW</button>
            </div>
            
            <div class="plan-card">
                <h4 style="color:var(--gold-dark)">Nano Miner</h4>
                <h2 style="margin:10px 0;">₨ 500</h2>
                <p style="font-size:0.8rem; color:#64748b;">Daily: Rs 60 | Period: 30 Days</p>
                <button class="btn-gold" style="margin-top:15px; padding:12px;" onclick="alert('Proceeding to Secure Gateway...')">INVEST NOW</button>
            </div>
        </div>
    </section>

    <!-- FLOATING SUPPORT -->
    <a href="https://wa.me/YOUR_NUMBER" class="support-float" id="wa-icon">💬</a>

    <!-- BOTTOM NAV -->
    <nav class="bottom-nav" id="b-nav">
        <div onclick="switchPage('dash-page')" style="text-align:center;">🏠<br><small>Home</small></div>
        <div onclick="switchPage('dash-page')" style="text-align:center; opacity:0.5;">💰<br><small>Profit</small></div>
        <div onclick="doLogout()" style="text-align:center; opacity:0.5;">🚪<br><small>Logout</small></div>
    </nav>

    <script>
        function checkStatus() {
            if(localStorage.getItem('auth') === 'true') {
                switchPage('dash-page');
                startAutoMining();
            }
        }

        function doLogin() {
            let ph = document.getElementById('phone').value;
            if(ph.length < 10) return alert("Enter valid number, sweetie! 💋");
            
            localStorage.setItem('auth', 'true');
            localStorage.setItem('userPhone', ph);
            localStorage.setItem('userBal', 0.0);
            
            switchPage('dash-page');
            startAutoMining();
        }

        function switchPage(id) {
            // Forcefully hide all sections
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            // Show current
            document.getElementById(id).classList.add('active');
            
            let isDash = (id === 'dash-page');
            document.getElementById('b-nav').style.display = isDash ? 'flex' : 'none';
            document.getElementById('t-bar').style.display = isDash ? 'block' : 'none';
            document.getElementById('wa-icon').style.display = isDash ? 'flex' : 'none';
            
            if(isDash) {
                document.getElementById('u-id').innerText = "ID: " + localStorage.getItem('userPhone');
            }
        }

        function startAutoMining() {
            setInterval(() => {
                let b = parseFloat(localStorage.getItem('userBal')) + 0.0001;
                localStorage.setItem('userBal', b);
                let el = document.getElementById('bal-txt');
                if(el) el.innerText = "₨ " + b.toFixed(4);
            }, 1000);
        }

        function doLogout() {
            localStorage.clear();
            location.reload();
        }
    </script>
</body>
</html>
