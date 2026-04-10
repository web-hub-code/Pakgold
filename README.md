<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Trusted Mining Network</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #f8fafc; --text: #1e293b; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* --- Sections Control --- */
        .section { display: none !important; width: 100%; min-height: 100vh; }
        .section.active { display: block !important; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* --- Trust Features: Live Ticker --- */
        .ticker-wrap {
            background: #1e293b; color: #fff; padding: 10px 0; font-size: 0.7rem;
            position: fixed; top: 0; width: 100%; z-index: 1000; overflow: hidden;
        }
        .ticker { display: inline-block; white-space: nowrap; animation: marquee 20s linear infinite; }
        @keyframes marquee { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        /* --- Premium Header --- */
        .header {
            background: white; padding: 60px 20px 30px; text-align: center;
            border-bottom: 5px solid var(--gold); border-radius: 0 0 40px 40px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.03);
        }
        .bal-amount { font-size: 3rem; font-weight: 800; color: var(--gold-dark); margin: 5px 0; }

        /* --- Cards & Badges --- */
        .container { padding: 20px 20px 120px; }
        .card { 
            background: white; border-radius: 25px; padding: 25px; margin-bottom: 20px;
            border: 1px solid #edf2f7; position: relative; overflow: hidden;
            box-shadow: 0 5px 15px rgba(0,0,0,0.02);
        }
        .shield-badge {
            background: #e3fcef; color: #00875a; font-size: 0.6rem; 
            padding: 4px 10px; border-radius: 50px; font-weight: 700;
            display: inline-flex; align-items: center; gap: 4px;
        }

        /* --- Buttons --- */
        .btn-gold {
            background: linear-gradient(135deg, var(--gold), var(--gold-dark));
            color: white; border: none; padding: 16px; border-radius: 15px;
            width: 100%; font-weight: 800; cursor: pointer; font-size: 1rem;
            box-shadow: 0 8px 20px rgba(184, 134, 11, 0.2);
        }

        /* --- Floating Support --- */
        .support-btn {
            position: fixed; bottom: 100px; right: 20px; width: 60px; height: 60px;
            background: #25d366; border-radius: 50%; display: flex;
            align-items: center; justify-content: center; color: white;
            font-size: 1.5rem; box-shadow: 0 5px 15px rgba(0,0,0,0.2); z-index: 2000;
        }

        .bottom-nav {
            position: fixed; bottom: 0; width: 100%; background: white;
            display: none; justify-content: space-around; padding: 20px;
            border-top: 1px solid #eee; border-radius: 30px 30px 0 0;
            box-shadow: 0 -10px 30px rgba(0,0,0,0.05); z-index: 1500;
        }
    </style>
</head>
<body onload="checkAuth()">

    <!-- Live Transaction Ticker -->
    <div class="ticker-wrap" id="ticker-bar" style="display:none;">
        <div class="ticker">
            ⚡ LIVE: User_982*** withdrew Rs 4,500 via JazzCash... 🛡️ Deposit of Rs 10,000 Verified for User_221***... ⚡ New Gold Plan Activated by User_776***...
        </div>
    </div>

    <!-- 1. LOGIN -->
    <section id="login-page" class="section active">
        <div style="height:100vh; display:flex; align-items:center; justify-content:center; background:white;">
            <div style="width:85%; max-width:380px; text-align:center;">
                <h1 style="color:var(--gold-dark); font-size:2.8rem; font-weight:800;">⚜️ PakGold</h1>
                <p style="color:#64748b; margin-bottom:30px;">The Most Trusted Mining Platform</p>
                <input type="text" id="phone" style="width:100%; padding:18px; border-radius:15px; border:1px solid #ddd; margin-bottom:15px;" placeholder="Phone Number">
                <input type="password" id="pass" style="width:100%; padding:18px; border-radius:15px; border:1px solid #ddd; margin-bottom:20px;" placeholder="Password">
                <button class="btn-gold" onclick="login()">LOGIN TO SECURE VAULT</button>
            </div>
        </div>
    </section>

    <!-- 2. DASHBOARD -->
    <section id="dash-page" class="section">
        <div class="header">
            <span class="shield-badge">🛡️ FBR VERIFIED SYSTEM</span>
            <p style="margin-top:10px; font-size:0.8rem; opacity:0.7;">Verified Portfolio Balance</p>
            <div class="bal-amount" id="live-bal">Rs 0.0000</div>
            <p id="miner-id" style="font-weight:bold; color:var(--gold-dark);"></p>
        </div>

        <div class="container">
            <h3 style="margin-bottom:20px;">Secured Investment Plans</h3>
            <div id="plan-list">
                <!-- Plans with Trust Badges -->
                <div class="card">
                    <div class="shield-badge">✅ LOW RISK</div>
                    <h2 style="margin:10px 0; color:var(--gold-dark);">Nano Miner</h2>
                    <p style="font-weight:bold; font-size:1.4rem;">Rs 500</p>
                    <p style="font-size:0.8rem; color:#64748b;">Daily: Rs 60 | Period: 30 Days</p>
                    <hr style="margin:15px 0; border:0; border-top:1px solid #eee;">
                    <button class="btn-gold" onclick="alert('Proceeding to Secure Gateway...')">ACTIVATE PLAN</button>
                </div>
                
                <div class="card" style="border: 2px solid var(--gold);">
                    <div class="shield-badge" style="background:#fff4e5; color:#b8860b;">🔥 MOST POPULAR</div>
                    <h2 style="margin:10px 0; color:var(--gold-dark);">Starter Pro</h2>
                    <p style="font-weight:bold; font-size:1.4rem;">Rs 1,000</p>
                    <p style="font-size:0.8rem; color:#64748b;">Daily: Rs 130 | Period: 30 Days</p>
                    <hr style="margin:15px 0; border:0; border-top:1px solid #eee;">
                    <button class="btn-gold" onclick="alert('Proceeding to Secure Gateway...')">ACTIVATE PLAN</button>
                </div>
            </div>
        </div>
    </section>

    <!-- Floating Support -->
    <a href="https://wa.me/YOUR_NUMBER" class="support-btn" id="help-btn" style="display:none;">💬</a>

    <!-- Nav Bar -->
    <nav class="bottom-nav" id="menu">
        <div onclick="showTab('dash-page')">🏠<br><small>Home</small></div>
        <div onclick="logout()">🚪<br><small>Logout</small></div>
    </nav>

    <script>
        function checkAuth() {
            if(localStorage.getItem('logged') === 'true') {
                showTab('dash-page');
                startMining();
            }
        }

        function login() {
            let p = document.getElementById('phone').value;
            if(p.length < 10) return alert("Please enter valid phone, sweetie! 💋");
            localStorage.setItem('logged', 'true');
            localStorage.setItem('user', p);
            localStorage.setItem('bal', 0.0);
            location.reload();
        }

        function showTab(id) {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            
            let isDashboard = (id !== 'login-page');
            document.getElementById('menu').style.display = isDashboard ? 'flex' : 'none';
            document.getElementById('ticker-bar').style.display = isDashboard ? 'block' : 'none';
            document.getElementById('help-btn').style.display = isDashboard ? 'flex' : 'none';
            
            if(isDashboard) document.getElementById('miner-id').innerText = "ID: " + localStorage.getItem('user');
        }

        function startMining() {
            setInterval(() => {
                let b = parseFloat(localStorage.getItem('bal')) + 0.0001;
                localStorage.setItem('bal', b);
                if(document.getElementById('live-bal')) document.getElementById('live-bal').innerText = "Rs " + b.toFixed(4);
            }, 1000);
        }

        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
