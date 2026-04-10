<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Premium Mining</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #ffffff; --card: #f8fafc; --text: #1e293b; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: #f1f5f9; color: var(--text); }

        /* Smooth Transitions */
        .section { display: none; min-height: 100vh; animation: fadeIn 0.4s ease; }
        .active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* Login Page Specific Styling */
        #login-page { background: white; display: flex; align-items: center; justify-content: center; text-align: center; padding: 20px; }
        .login-card { background: white; padding: 40px 30px; border-radius: 30px; box-shadow: 0 20px 50px rgba(0,0,0,0.1); width: 100%; max-width: 400px; }

        /* Dashboard Header */
        .header { background: white; padding: 30px 20px; text-align: center; border-bottom: 3px solid var(--gold); border-radius: 0 0 30px 30px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); }
        .balance-box { font-size: 2.5rem; font-weight: 800; color: var(--gold-dark); margin: 10px 0; }

        /* Content Area */
        .container { padding: 20px; padding-bottom: 100px; }
        .card { background: white; padding: 20px; border-radius: 20px; box-shadow: 0 4px 12px rgba(0,0,0,0.05); margin-bottom: 20px; position: relative; border: 1px solid #edf2f7; }
        .elite-card { border: 2px solid var(--gold); background: #fffdf5; }

        /* Form Elements */
        .input-box { width: 100%; padding: 15px; margin: 10px 0; border-radius: 12px; border: 1px solid #e2e8f0; background: #f8fafc; outline: none; transition: 0.3s; }
        .input-box:focus { border-color: var(--gold); box-shadow: 0 0 0 3px rgba(212, 175, 55, 0.1); }
        .btn-gold { background: linear-gradient(135deg, var(--gold), var(--gold-dark)); color: white; border: none; padding: 15px; border-radius: 15px; width: 100%; font-weight: bold; font-size: 1rem; cursor: pointer; box-shadow: 0 10px 20px rgba(184, 134, 11, 0.2); }

        /* Navigation */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: white; display: flex; justify-content: space-around; padding: 15px; border-top: 1px solid #eee; border-radius: 25px 25px 0 0; box-shadow: 0 -5px 20px rgba(0,0,0,0.05); }
        .nav-item { color: #94a3b8; font-size: 0.75rem; text-align: center; cursor: pointer; }
        .nav-item.active-nav { color: var(--gold-dark); font-weight: bold; }

        /* Secret Admin Area */
        #admin-panel { background: #fff; border: 2px dashed #6366f1; padding: 20px; border-radius: 20px; margin-top: 20px; display: none; }
    </style>
</head>
<body onload="checkAuth()">

    <!-- 1. LOGIN SECTION (Totally Separate) -->
    <section id="login-page" class="section active">
        <div class="login-card">
            <h1 style="color: var(--gold-dark); font-size: 2.5rem;">⚜️ PakGold</h1>
            <p style="color: #64748b; margin-bottom: 30px;">Premium Mining Portal</p>
            <input type="text" id="phone" class="input-box" placeholder="Phone Number">
            <input type="password" id="pass" class="input-box" placeholder="Password">
            <button class="btn-gold" onclick="login()">LOGIN TO VAULT</button>
            <p style="margin-top: 20px; font-size: 0.8rem; color: #94a3b8;">Don't have an account? <span style="color: var(--gold-dark);">Contact Support</span></p>
        </div>
    </section>

    <!-- 2. DASHBOARD SECTION -->
    <section id="dash-page" class="section">
        <div class="header">
            <h4 onclick="adminTrigger()" style="cursor: pointer; color: #94a3b8;">PAK GOLD SOLUTIONS</h4>
            <div class="balance-box" id="live-bal">₨ 0.0000</div>
            <p style="font-size: 0.8rem; color: #64748b;" id="miner-id"></p>
        </div>

        <div class="container">
            <!-- Secret Admin Box -->
            <div id="admin-box" style="display: none; margin-bottom: 20px;">
                <div class="card" style="border-color: #6366f1;">
                    <input type="text" id="adm-user" class="input-box" placeholder="Admin ID">
                    <input type="password" id="adm-key" class="input-box" placeholder="Key">
                    <button class="btn-gold" style="background: #6366f1;" onclick="accessAdmin()">OPEN CONTROL</button>
                </div>
            </div>

            <div id="admin-panel">
                <h3 style="margin-bottom: 15px;">Live Requests</h3>
                <div id="req-list">Loading...</div>
                <button class="btn-gold" style="background: #ef4444; margin-top: 15px;" onclick="location.reload()">LOGOUT ADMIN</button>
            </div>

            <!-- Dashboard Content -->
            <div id="user-content">
                <h3 style="margin-bottom: 15px; font-weight: 600;">Mining Plans</h3>
                <div id="plans-container">
                    <!-- Plans Injected by JS -->
                </div>
            </div>
        </div>
    </section>

    <!-- 3. DEPOSIT SECTION -->
    <section id="deposit-page" class="section">
        <div class="header"><h2>Add Funds</h2></div>
        <div class="container">
            <div class="card">
                <p style="font-size: 0.9rem; margin-bottom: 15px;">Official Payment Method:</p>
                <div style="background: #f1f5f9; padding: 15px; border-radius: 12px; margin-bottom: 20px;">
                    <b>EasyPaisa:</b> 03379827882<br>
                    <b>JazzCash:</b> 03705519562
                </div>
                <input type="number" id="d-amt" class="input-box" placeholder="Amount">
                <input type="text" id="d-tid" class="input-box" placeholder="TID Number">
                <button class="btn-gold" onclick="sendRequest('Deposit')">SUBMIT PROOF</button>
            </div>
        </div>
    </section>

    <!-- 4. WITHDRAW SECTION -->
    <section id="withdraw-page" class="section">
        <div class="header"><h2>Withdraw</h2></div>
        <div class="container">
            <div class="card">
                <input type="number" id="w-amt" class="input-box" placeholder="Amount (Min 500)">
                <input type="text" class="input-box" placeholder="Wallet Number">
                <button class="btn-gold" onclick="sendRequest('Withdrawal')">REQUEST PAYOUT</button>
            </div>
        </div>
    </section>

    <!-- NAV BAR -->
    <nav class="bottom-nav" id="nav-bar" style="display: none;">
        <div class="nav-item" onclick="nav('dash-page')">🏠<br>Home</div>
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

        // --- Plans Definition ---
        const plans = [
            { name: "Starter Gold", price: 1000, daily: 130 },
            { name: "Silver Pro", price: 5000, daily: 750 },
            { name: "Elite Master", price: 20000, daily: 3200, elite: true }
        ];

        window.checkAuth = function() {
            const grid = document.getElementById('plans-container');
            plans.forEach(p => {
                grid.innerHTML += `
                    <div class="card ${p.elite ? 'elite-card' : ''}">
                        <h3 style="color: var(--gold-dark)">${p.name}</h3>
                        <p style="font-weight: bold; margin: 5px 0;">Price: ₨ ${p.price}</p>
                        <p style="font-size: 0.8rem; color: #64748b;">Daily: ₨ ${p.daily} | 30 Days</p>
                        <button class="btn-gold" style="margin-top:10px;" onclick="buyPlan(${p.price}, ${p.daily})">ACTIVATE</button>
                    </div>`;
            });

            if(localStorage.getItem('pg_user')) {
                nav('dash-page');
                startMining();
            }
        }

        window.login = function() {
            const ph = document.getElementById('phone').value;
            if(ph.length < 10) return alert("Valid phone sweetie!");
            localStorage.setItem('pg_user', ph);
            localStorage.setItem('pg_bal', 0.0);
            localStorage.setItem('pg_speed', 0.0001);
            nav('dash-page');
            startMining();
        }

        window.nav = function(id) {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('nav-bar').style.display = (id === 'login-page') ? 'none' : 'flex';
            if(id === 'dash-page') document.getElementById('miner-id').innerText = "ID: " + localStorage.getItem('pg_user');
        }

        function startMining() {
            setInterval(() => {
                let bal = parseFloat(localStorage.getItem('pg_bal'));
                let spd = parseFloat(localStorage.getItem('pg_speed') || 0.0001);
                bal += spd;
                localStorage.setItem('pg_bal', bal);
                if(document.getElementById('live-bal')) {
                    document.getElementById('live-bal').innerText = "₨ " + bal.toFixed(4);
                }
            }, 1000);
        }

        window.sendRequest = function(type) {
            const amt = document.getElementById('d-amt').value || document.getElementById('w-amt').value;
            const tid = document.getElementById('d-tid').value || "W-"+Date.now();
            push(ref(db, 'requests/'), {
                type, amt, tid, user: localStorage.getItem('pg_user'), time: new Date().toLocaleString()
            }).then(() => { alert("Sent to Admin! 💋"); nav('dash-page'); });
        }

        // Admin
        let clicks = 0;
        window.adminTrigger = function() { clicks++; if(clicks >= 5) { document.getElementById('admin-box').style.display = 'block'; clicks = 0; } }
        window.accessAdmin = function() {
            if(document.getElementById('adm-user').value === 'admin786' && document.getElementById('adm-key').value === 'gold_master_786') {
                document.getElementById('admin-panel').style.display = 'block';
                document.getElementById('user-content').style.display = 'none';
                onValue(ref(db, 'requests/'), (snap) => {
                    const list = document.getElementById('req-list');
                    list.innerHTML = "";
                    snap.forEach(c => {
                        const d = c.val();
                        list.innerHTML += `<div style="border-bottom:1px solid #eee; padding:10px; font-size:0.8rem;"><b>${d.type}</b>: ${d.amt} PKR<br>User: ${d.user} | TID: ${d.tid}</div>`;
                    });
                });
            }
        }

        window.logout = function() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
