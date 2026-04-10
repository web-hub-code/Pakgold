<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Master Neural Terminal</title>
    
    <!-- External Resources -->
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap" rel="stylesheet">

    <style>
        :root { --gold: #b38b2d; --bg: #f8fafc; --text: #0f172a; --green: #10b981; --white: #ffffff; }
        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* --- GLOBAL ANIMATIONS --- */
        .page { display:none; animation: slideUp 0.5s ease; min-height: 100vh; padding-bottom: 120px; }
        .page.active { display:block; }
        @keyframes slideUp { from { opacity:0; transform: translateY(20px); } to { opacity:1; transform: translateY(0); } }

        /* --- TRUST CERTIFICATE MODAL --- */
        #cert-modal { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(15, 23, 42, 0.8); backdrop-filter: blur(8px); z-index:10000; display:none; align-items:center; justify-content:center; padding:25px; }
        .cert-card { background: var(--white); padding: 40px 30px; border-radius: 40px; text-align: center; border: 3px solid var(--gold); box-shadow: 0 30px 60px rgba(0,0,0,0.2); }

        /* --- LIVE TOAST (TRUST FEATURE) --- */
        #live-toast { position: fixed; bottom: 100px; left: 20px; right: 20px; background: var(--white); padding: 15px; border-radius: 20px; border-left: 5px solid var(--gold); display: flex; align-items: center; gap: 15px; box-shadow: 0 15px 30px rgba(0,0,0,0.1); transform: translateY(200px); transition: 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275); z-index: 9999; }
        #live-toast.show { transform: translateY(0); }

        /* --- HEADER & CARDS --- */
        .header { padding: 20px 25px; display: flex; justify-content: space-between; align-items: center; background: var(--white); border-bottom: 1px solid #f1f5f9; position: sticky; top: 0; z-index: 100; }
        .main-card { background: var(--white); margin: 20px; padding: 35px; border-radius: 40px; border: 1px solid #f1f5f9; box-shadow: 0 20px 40px rgba(0,0,0,0.04); position: relative; overflow: hidden; }
        .verified-badge { position: absolute; top: 20px; right: 20px; background: #ecfdf5; color: var(--green); padding: 5px 12px; border-radius: 50px; font-size: 0.6rem; font-weight: 800; display: flex; align-items: center; gap: 4px; }

        /* --- DYNAMIC NUMBERS --- */
        .balance-val { font-size: 3.2rem; font-weight: 900; letter-spacing: -2px; color: var(--text); margin: 10px 0; }
        .profit-stream { color: var(--green); font-weight: 800; font-size: 0.9rem; }

        /* --- BUTTON GRID --- */
        .grid-menu { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; padding: 0 20px; }
        .grid-item { background: var(--white); padding: 20px 5px; border-radius: 25px; text-align: center; border: 1px solid #f1f5f9; box-shadow: 0 4px 6px rgba(0,0,0,0.01); }
        .grid-item i { font-size: 1.5rem; color: var(--gold); margin-bottom: 8px; display: block; }
        .grid-item span { font-size: 0.6rem; font-weight: 700; color: #64748b; text-transform: uppercase; }

        /* --- NODE LIST --- */
        .node-card { background: var(--white); margin: 15px 20px; padding: 25px; border-radius: 30px; border: 1px solid #f1f5f9; display: flex; justify-content: space-between; align-items: center; }
        .buy-btn { background: var(--text); color: var(--white); border: none; padding: 12px 22px; border-radius: 15px; font-weight: 800; font-size: 0.75rem; box-shadow: 0 10px 20px rgba(0,0,0,0.1); }

        /* --- FLOATING NAV --- */
        .nav-bar { position: fixed; bottom: 20px; left: 20px; right: 20px; background: rgba(255,255,255,0.9); backdrop-filter: blur(20px); height: 75px; border-radius: 30px; display: flex; justify-content: space-around; align-items: center; border: 1px solid #f1f5f9; box-shadow: 0 20px 50px rgba(0,0,0,0.1); z-index: 1000; }
        .nav-link { color: #94a3b8; text-align: center; font-size: 0.65rem; font-weight: 800; }
        .nav-link.active { color: var(--gold); }
        .nav-link i { font-size: 1.5rem; display: block; margin-bottom: 2px; }

        /* --- LOGIN PAGE --- */
        #login-pg { background: var(--white); text-align: center; padding: 100px 40px; }
        .input-box { width: 100%; padding: 20px; background: #f1f5f9; border: none; border-radius: 20px; margin-bottom: 15px; font-size: 1rem; font-weight: 600; }
        .primary-btn { width: 100%; background: var(--gold); color: white; padding: 20px; border: none; border-radius: 20px; font-weight: 800; font-size: 1.1rem; box-shadow: 0 15px 30px rgba(179,139,45,0.3); }
    </style>
</head>
<body onload="initApp()">

    <!-- 1. Trusted Certificate Modal -->
    <div id="cert-modal">
        <div class="cert-card">
            <i class="fa fa-award fa-4x" style="color:var(--gold)"></i>
            <h2 style="margin:20px 0; font-weight:900; letter-spacing:-1px;">Verified Platform</h2>
            <p style="color:#64748b; font-size:0.85rem; line-height:1.6;">Welcome to PAK GOLD Official. Your assets are secured by the 2026 Neural Encryption Protocol.</p>
            <button onclick="closeCert()" class="primary-btn" style="margin-top:30px;">I AGREE & ENTER</button>
        </div>
    </div>

    <!-- 2. Live Social Proof Toast -->
    <div id="live-toast">
        <div style="background:#f1f5f9; width:45px; height:45px; border-radius:15px; display:flex; align-items:center; justify-content:center; color:var(--gold);"><i class="fa fa-wallet"></i></div>
        <div>
            <div style="font-size:0.8rem; font-weight:800;" id="toast-user">User 0321***</div>
            <div style="font-size:0.7rem; color:#64748b;" id="toast-msg">Withdrew Rs 4,500</div>
        </div>
    </div>

    <!-- 3. Login Section -->
    <section id="login-pg" class="page active">
        <div style="font-size: 4rem; color: var(--gold); margin-bottom: 20px;"><i class="fa fa-gem"></i></div>
        <h1 style="font-weight:900; font-size:2.5rem; letter-spacing:-2px;">PAK <span style="color:var(--gold)">GOLD</span></h1>
        <p style="color:#94a3b8; font-size:0.75rem; margin-bottom:50px; letter-spacing:2px; font-weight:800;">OFFICIAL MINING NETWORK</p>
        
        <input type="number" id="ph" class="input-box" placeholder="Phone Identity">
        <input type="password" id="pass" class="input-box" placeholder="Access Password">
        <button onclick="handleAuth()" class="primary-btn">INITIALIZE SESSION</button>
    </section>

    <!-- 4. Dashboard Section -->
    <section id="home-pg" class="page">
        <div class="header">
            <b style="font-size:1.2rem; font-weight:900;">PG <span style="color:var(--gold)">CORE</span></b>
            <div style="background:#f1f5f9; padding:8px 15px; border-radius:50px; font-size:0.6rem; font-weight:800;">
                <i class="fa fa-circle" style="color:var(--green); font-size:0.5rem;"></i> SYSTEM STABLE
            </div>
        </div>

        <div class="main-card">
            <div class="verified-badge"><i class="fa fa-check-double"></i> VERIFIED NODE</div>
            <small style="color:#94a3b8; font-weight:800; letter-spacing:1px;">TOTAL ASSET VALUE</small>
            <div class="balance-val">Rs <span id="u_wallet">0.00</span></div>
            <div style="display:flex; justify-content:space-between; margin-top:30px;">
                <div><small style="color:#94a3b8; font-size:0.65rem;">Live Profit</small><br><b id="u_profit" class="profit-stream">+0.00</b></div>
                <div><small style="color:#94a3b8; font-size:0.65rem;">Hash Rate</small><br><b id="u_speed" style="font-weight:800;">0.00000</b></div>
            </div>
        </div>

        <div class="grid-menu">
            <div class="grid-item" onclick="nav('deposit')"><i class="fa fa-bolt"></i><span>Fund</span></div>
            <div class="grid-item" onclick="nav('withdraw')"><i class="fa fa-paper-plane"></i><span>Payout</span></div>
            <div class="grid-item" onclick="nav('team')"><i class="fa fa-link"></i><span>Network</span></div>
            <div class="grid-item" onclick="alert('Support is analyzing your account...')"><i class="fa fa-headset"></i><span>Support</span></div>
        </div>

        <div style="padding: 30px 25px 10px; font-weight:900; font-size:1.2rem; color:var(--text);">Certified Nodes</div>
        <div id="plans_container"></div>
    </section>

    <!-- 5. Floating Navigation -->
    <nav class="nav-bar" id="main_menu" style="display:none;">
        <div class="nav-link active" onclick="nav('home-pg')"><i class="fa fa-home-alt"></i>Hub</div>
        <div class="nav-link" onclick="nav('team')"><i class="fa fa-users-rays"></i>Team</div>
        <div class="nav-link" onclick="nav('withdraw')"><i class="fa fa-safe-box"></i>Vault</div>
        <div class="nav-link" onclick="checkAdmin()"><i class="fa fa-user-shield"></i>Secure</div>
    </nav>

    <script>
        // --- FIREBASE SETUP ---
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        
        let user = localStorage.getItem('pg_master_v10');

        function initApp() {
            if(user) { 
                nav('home-pg'); syncData(); 
            } else {
                if(!sessionStorage.getItem('intro_done')) document.getElementById('cert-modal').style.display='flex';
            }
            loadPlans();
            startSocialProof();
        }

        function closeCert() { document.getElementById('cert-modal').style.display='none'; sessionStorage.setItem('intro_done','true'); }

        function handleAuth() {
            let p = document.getElementById('ph').value;
            let s = document.getElementById('pass').value;
            if(p.length < 10 || s.length < 4) return alert("Invalid Identity Details");
            user = p; localStorage.setItem('pg_master_v10', p);
            db.ref('users/'+p).once('value', snap => {
                if(!snap.exists()) db.ref('users/'+p).set({wallet:0, profit:0, speed:0, pass:s});
                location.reload();
            });
        }

        function syncData() {
            db.ref('users/'+user).on('value', s => {
                let d = s.val();
                document.getElementById('u_wallet').innerText = d.wallet.toLocaleString();
                document.getElementById('u_profit').innerText = d.profit.toFixed(2);
                document.getElementById('u_speed').innerText = d.speed.toFixed(5);
            });
            // Real-time Mining Logic
            setInterval(() => {
                db.ref('users/'+user).once('value', s => {
                    let d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed});
                });
            }, 1000);
        }

        function loadPlans() {
            const plans = [
                {n:'Node Alpha', p:300, d:30}, {n:'Node Beta', p:1000, d:120},
                {n:'Node Gamma', p:5000, d:750}, {n:'Master VIP', p:25000, d:4500}
            ];
            let cont = document.getElementById('plans_container');
            plans.forEach(i => {
                cont.innerHTML += `<div class="node-card">
                    <div>
                        <h4 style="font-weight:900;">${i.n}</h4>
                        <small style="color:#94a3b8;">Setup Fee: Rs ${i.p}</small><br>
                        <b style="color:var(--green); font-size:0.8rem;">+Rs ${i.d}/Day</b>
                    </div>
                    <button class="buy-btn" onclick="activateNode(${i.p}, ${i.d/86400})">ACTIVATE</button>
                </div>`;
            });
        }

        function activateNode(p, s) {
            db.ref('users/'+user).once('value', v => {
                if(v.val().wallet < p) return alert("Insufficient Credits in Vault");
                db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s});
                alert("Neural Node Activated! 🚀");
            });
        }

        function startSocialProof() {
            const names = ["0301***", "0345***", "0322***", "0315***", "0300***"];
            const amounts = ["2,000", "15,000", "800", "5,400", "1,200"];
            setInterval(() => {
                const toast = document.getElementById('live-toast');
                document.getElementById('toast-user').innerText = "User " + names[Math.floor(Math.random()*names.length)];
                document.getElementById('toast-msg').innerText = "Withdrew Rs " + amounts[Math.floor(Math.random()*amounts.length)];
                toast.classList.add('show');
                setTimeout(() => toast.classList.remove('show'), 5000);
            }, 15000);
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('main_menu').style.display = (id==='login-pg')?'none':'flex';
        }

        function checkAdmin() { let p = prompt("Enter Master Secure Key:"); if(p==="78692") alert("Admin Privileges Granted."); }
    </script>
</body>
</html>
