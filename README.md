<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Premium Light Terminal</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;500;700;900&display=swap" rel="stylesheet">

    <style>
        :root { --main-bg: #f8fafc; --card-bg: #ffffff; --gold: #b38b2d; --text: #1e293b; --accent: #0ea5e9; }
        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--main-bg); color: var(--text); overflow-x: hidden; }

        /* Smooth Transition */
        .page { display:none; animation: fadeIn 0.5s ease-in-out; }
        .page.active { display:block; }
        @keyframes fadeIn { from { opacity:0; transform: translateY(10px); } to { opacity:1; transform: translateY(0); } }

        /* Premium Light Header */
        .header { background: rgba(255,255,255,0.8); backdrop-filter: blur(10px); padding: 15px 25px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; box-shadow: 0 4px 20px rgba(0,0,0,0.03); }

        /* Soft Floating Card */
        .wallet-card { background: white; margin: 20px; padding: 30px; border-radius: 35px; box-shadow: 0 25px 50px -12px rgba(0,0,0,0.08); border: 1px solid rgba(255,255,255,0.5); position: relative; overflow: hidden; }
        .wallet-card::after { content: ''; position: absolute; top: -10%; right: -10%; width: 100px; height: 100px; background: rgba(179, 139, 45, 0.1); border-radius: 50%; filter: blur(30px); }
        .balance-label { font-size: 0.7rem; font-weight: 700; color: #64748b; letter-spacing: 2px; text-transform: uppercase; }
        .balance-amt { font-size: 3rem; font-weight: 900; color: var(--text); margin: 10px 0; letter-spacing: -1px; }

        /* Modern Quick Actions */
        .action-row { display: grid; grid-template-columns: repeat(4, 1fr); gap: 15px; padding: 0 20px; }
        .action-btn { background: white; padding: 15px 5px; border-radius: 20px; text-align: center; box-shadow: 0 10px 15px rgba(0,0,0,0.02); border: 1px solid #f1f5f9; transition: 0.3s; }
        .action-btn:active { transform: scale(0.9); }
        .action-btn i { font-size: 1.4rem; color: var(--gold); margin-bottom: 8px; display: block; }
        .action-btn span { font-size: 0.6rem; font-weight: 700; color: #64748b; }

        /* Node Item Light */
        .node-card { background: white; margin: 15px 20px; padding: 20px; border-radius: 25px; display: flex; justify-content: space-between; align-items: center; border: 1px solid #f1f5f9; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.02); }
        .node-info h4 { font-weight: 700; color: var(--text); }
        .node-info small { color: #94a3b8; font-size: 0.7rem; }
        .rent-now { background: var(--text); color: white; border: none; padding: 10px 20px; border-radius: 12px; font-weight: 700; font-size: 0.75rem; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.1); }

        /* Floating Light Nav */
        .bottom-nav { position: fixed; bottom: 20px; left: 20px; right: 20px; background: rgba(255,255,255,0.9); backdrop-filter: blur(20px); height: 70px; border-radius: 25px; display: flex; justify-content: space-around; align-items: center; box-shadow: 0 20px 40px rgba(0,0,0,0.1); border: 1px solid #f1f5f9; }
        .nav-link { text-align: center; color: #94a3b8; font-size: 0.65rem; font-weight: 700; }
        .nav-link.active { color: var(--gold); }
        .nav-link i { font-size: 1.3rem; display: block; margin-bottom: 3px; }

        /* Auth Screen Light */
        #login { background: white; text-align: center; padding: 60px 40px; }
        .auth-input { width: 100%; padding: 18px; background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 15px; margin-bottom: 15px; font-size: 1rem; }
        .auth-btn { width: 100%; background: var(--gold); color: white; padding: 18px; border: none; border-radius: 15px; font-weight: 700; font-size: 1.1rem; box-shadow: 0 10px 25px rgba(179,139,45,0.3); }
    </style>
</head>
<body onload="initApp()">

    <!-- LOGIN SCREEN -->
    <section id="login" class="page active">
        <div style="margin-top: 40px;">
            <div style="width:80px; height:80px; background:var(--gold); border-radius:24px; margin:0 auto 30px; display:flex; align-items:center; justify-content:center; box-shadow:0 20px 40px rgba(179,139,45,0.2);">
                <i class="fa fa-crown fa-2x" style="color:white;"></i>
            </div>
            <h1 style="font-weight:900; font-size:2.5rem; color:var(--text); letter-spacing:-1px;">PAK <span style="color:var(--gold);">GOLD</span></h1>
            <p style="color:#94a3b8; font-size:0.8rem; margin-bottom:40px;">Official Mining Protocol v2.0</p>
            
            <input type="number" id="ph" class="auth-input" placeholder="Phone Number">
            <input type="password" id="pass" class="auth-input" placeholder="Security Password">
            <button onclick="handleAuth()" class="auth-btn">SECURE LOGIN</button>
        </div>
    </section>

    <!-- DASHBOARD SCREEN -->
    <section id="home" class="page">
        <div class="header">
            <b style="font-size:1.1rem; font-weight:900;">PAK <span style="color:var(--gold)">GOLD</span></b>
            <div style="display:flex; align-items:center; gap:8px;">
                <div style="width:8px; height:8px; background:#22c55e; border-radius:50%;"></div>
                <span style="font-size:0.65rem; font-weight:700; color:#64748b;">LIVE</span>
            </div>
        </div>

        <div class="wallet-card">
            <div class="balance-label">Current Assets</div>
            <div class="balance-amt">Rs <span id="u_wallet">0.00</span></div>
            <div style="display:flex; gap:20px; margin-top:10px;">
                <div><small style="color:#94a3b8; font-size:0.6rem;">Daily Profit</small><br><b id="u_profit" style="color:#22c55e;">+0.00</b></div>
                <div><small style="color:#94a3b8; font-size:0.6rem;">Mining Power</small><br><b id="u_speed">0.00000</b></div>
            </div>
        </div>

        <div class="action-row">
            <div class="action-btn" onclick="nav('deposit')"><i class="fa fa-plus-circle"></i><span>Load</span></div>
            <div class="action-btn" onclick="nav('withdraw')"><i class="fa fa-arrow-down-long"></i><span>Out</span></div>
            <div class="action-btn" onclick="nav('team')"><i class="fa fa-users"></i><span>Team</span></div>
            <div class="action-btn" onclick="alert('Support is online!')"><i class="fa fa-headset"></i><span>Help</span></div>
        </div>

        <div style="padding: 30px 25px 10px; font-weight: 800; font-size: 1.1rem; color: var(--text);">Mining Node Marketplace</div>
        <div id="plans_list"></div>
    </section>

    <!-- BOTTOM NAV -->
    <nav class="bottom-nav" id="main_menu" style="display:none;">
        <div class="nav-link active" onclick="nav('home')"><i class="fa-solid fa-house"></i>Home</div>
        <div class="nav-link" onclick="nav('team')"><i class="fa-solid fa-network-wired"></i>Network</div>
        <div class="nav-link" onclick="nav('withdraw')"><i class="fa-solid fa-wallet"></i>Assets</div>
        <div class="nav-link" onclick="checkAdmin()"><i class="fa-solid fa-lock"></i>Admin</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_light_v8');

        function initApp() { if(user) { nav('home'); sync(); } loadLightPlans(); }

        function handleAuth() {
            let p = document.getElementById('ph').value;
            if(p === "78692") { nav('master_admin'); return; }
            if(p.length < 10) return;
            user = p; localStorage.setItem('pg_light_v8', p);
            db.ref('users/'+p).once('value', s => {
                if(!s.exists()) db.ref('users/'+p).set({wallet:0, profit:0, speed:0});
                location.reload();
            });
        }

        function loadLightPlans() {
            const plans = [
                {n:'Tier 1', p:200, d:20}, {n:'Tier 2', p:500, d:55}, {n:'Tier 3', p:1000, d:120},
                {n:'Pro Max', p:5000, d:700}, {n:'Enterprise', p:20000, d:3500}
            ];
            let list = document.getElementById('plans_list');
            plans.forEach(i => {
                list.innerHTML += `<div class="node-card">
                    <div class="node-info">
                        <h4>${i.n} Engine</h4>
                        <small>Requirement: Rs ${i.p}</small><br>
                        <b style="color:#22c55e; font-size:0.8rem;">+Rs ${i.d}/day</b>
                    </div>
                    <button class="rent-now" onclick="buyNode(${i.p}, ${i.d/86400})">ACTIVATE</button>
                </div>`;
            });
        }

        function sync() {
            db.ref('users/'+user).on('value', s => {
                let d = s.val();
                document.getElementById('u_wallet').innerText = d.wallet.toLocaleString();
                document.getElementById('u_profit').innerText = d.profit.toFixed(2);
                document.getElementById('u_speed').innerText = d.speed.toFixed(5);
            });
            setInterval(() => {
                db.ref('users/'+user).once('value', s => {
                    let d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed});
                });
            }, 1000);
        }

        function buyNode(p, s) {
            db.ref('users/'+user).once('value', v => {
                if(v.val().wallet < p) return alert("Low balance in your account.");
                db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s});
                alert("Node activated successfully!");
            });
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('main_menu').style.display = (id==='login')?'none':'flex';
        }

        function checkAdmin() { let p = prompt("Enter Master Key:"); if(p==="78692") alert("Authorized!"); }
    </script>
</body>
</html>
