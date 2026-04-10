<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Neon Flux Premium</title>
    
    <!-- Scripts & Fonts -->
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;500;700;800&display=swap" rel="stylesheet">

    <style>
        :root { --neon-gold: #ffcc00; --soft-bg: #ffffff; --glass: rgba(255, 255, 255, 0.7); --text: #1a1a1a; --shadow: rgba(255, 204, 0, 0.3); }
        
        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--soft-bg); color: var(--text); overflow-x: hidden; }

        /* --- PREMIUM ANIMATIONS --- */
        .page { display:none; animation: fluxIn 0.6s cubic-bezier(0.23, 1, 0.32, 1); padding-bottom: 120px; }
        .page.active { display:block; }
        @keyframes fluxIn { from { opacity:0; transform: scale(0.95); filter: blur(10px); } to { opacity:1; transform: scale(1); filter: blur(0); } }

        /* --- NEON UTILITIES --- */
        .neon-glow { box-shadow: 0 10px 30px var(--shadow); border: 1px solid var(--neon-gold); }
        .ripple { position: relative; overflow: hidden; }

        /* --- MASTER HEADER --- */
        .premium-header { padding: 25px; display: flex; justify-content: space-between; align-items: center; background: var(--glass); backdrop-filter: blur(15px); position: sticky; top: 0; z-index: 1000; border-bottom: 1px solid rgba(0,0,0,0.05); }
        .logo-text { font-weight: 800; font-size: 1.4rem; letter-spacing: -1px; }

        /* --- NEON ASSET CARD --- */
        .neon-card { background: white; margin: 20px; padding: 35px; border-radius: 40px; position: relative; border: 1px solid rgba(0,0,0,0.03); box-shadow: 0 30px 60px rgba(0,0,0,0.05); overflow: hidden; }
        .neon-card::before { content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 5px; background: linear-gradient(90deg, transparent, var(--neon-gold), transparent); animation: moveLine 3s infinite; }
        @keyframes moveLine { 0% { transform: translateX(-100%); } 100% { transform: translateX(100%); } }

        .balance-title { color: #94a3b8; font-size: 0.7rem; font-weight: 800; letter-spacing: 2px; text-transform: uppercase; }
        .balance-main { font-size: 3.5rem; font-weight: 800; margin: 10px 0; color: var(--text); }

        /* --- CLICKABLE ACTION TILES --- */
        .action-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; padding: 0 20px; }
        .tile { background: white; padding: 22px 10px; border-radius: 25px; text-align: center; border: 1px solid #f1f5f9; transition: all 0.3s ease; box-shadow: 0 10px 20px rgba(0,0,0,0.02); }
        .tile:active { transform: scale(0.9); background: #fffdf2; border-color: var(--neon-gold); }
        .tile i { font-size: 1.6rem; color: var(--neon-gold); margin-bottom: 8px; display: block; filter: drop-shadow(0 0 5px var(--shadow)); }
        .tile span { font-size: 0.65rem; font-weight: 800; color: #64748b; }

        /* --- PREMIUM NODE BOX --- */
        .node-box { background: white; margin: 15px 20px; padding: 25px; border-radius: 30px; display: flex; justify-content: space-between; align-items: center; border: 1px solid #f1f5f9; position: relative; transition: 0.3s; }
        .node-box:hover { border-color: var(--neon-gold); box-shadow: 0 15px 30px var(--shadow); }
        .node-status { position: absolute; top: 15px; left: 25px; font-size: 0.55rem; font-weight: 900; color: #10b981; display: flex; align-items: center; gap: 5px; }
        .pulse-dot { width: 6px; height: 6px; background: #10b981; border-radius: 50%; animation: pulse 1.5s infinite; }
        @keyframes pulse { 0% { opacity: 1; transform: scale(1); } 100% { opacity: 0; transform: scale(3); } }

        .btn-neon { background: var(--text); color: white; border: none; padding: 14px 25px; border-radius: 18px; font-weight: 800; font-size: 0.75rem; transition: 0.3s; box-shadow: 0 10px 20px rgba(0,0,0,0.1); }
        .btn-neon:active { background: var(--neon-gold); color: black; }

        /* --- FLOATING MODERN NAV --- */
        .neon-nav { position: fixed; bottom: 25px; left: 20px; right: 20px; background: rgba(255,255,255,0.85); backdrop-filter: blur(25px); height: 75px; border-radius: 30px; display: flex; justify-content: space-around; align-items: center; border: 1px solid rgba(0,0,0,0.05); box-shadow: 0 20px 40px rgba(0,0,0,0.1); z-index: 2000; }
        .nav-item { text-align: center; color: #94a3b8; font-size: 0.65rem; font-weight: 800; transition: 0.3s; }
        .nav-item.active { color: var(--neon-gold); transform: translateY(-5px); }
        .nav-item i { font-size: 1.6rem; display: block; margin-bottom: 2px; }

        /* --- LOGIN NEON --- */
        #login-scr { background: white; padding: 80px 40px; text-align: center; min-height: 100vh; }
        .neon-input { width: 100%; padding: 22px; background: #f8fafc; border: 2px solid #f1f5f9; border-radius: 20px; margin-bottom: 15px; font-size: 1.1rem; font-weight: 600; transition: 0.3s; }
        .neon-input:focus { border-color: var(--neon-gold); outline: none; background: white; box-shadow: 0 0 20px var(--shadow); }
        .btn-auth { width: 100%; background: var(--neon-gold); color: black; padding: 22px; border: none; border-radius: 20px; font-weight: 900; font-size: 1.2rem; box-shadow: 0 15px 30px var(--shadow); }
    </style>
</head>
<body onload="initApp()">

    <!-- LOGIN -->
    <section id="login-scr" class="page active">
        <div style="width:90px; height:90px; background:var(--neon-gold); border-radius:30px; margin:0 auto 30px; display:flex; align-items:center; justify-content:center; box-shadow: 0 20px 40px var(--shadow);">
            <i class="fa fa-bolt-lightning fa-3x" style="color:black"></i>
        </div>
        <h1 style="font-size: 2.8rem; font-weight: 900; letter-spacing: -2px;">NEON <span style="color:var(--neon-gold)">CORE</span></h1>
        <p style="color:#94a3b8; font-weight:800; font-size:0.75rem; letter-spacing:3px; margin-bottom:50px;">PREMIUM MINING HUB</p>
        
        <input type="number" id="ph" class="neon-input" placeholder="03XXXXXXXXX">
        <input type="password" id="pass" class="neon-input" placeholder="Secret Access">
        <button onclick="handleAuth()" class="btn-auth">START ENGINE</button>
    </section>

    <!-- HUB (HOME) -->
    <section id="hub-scr" class="page">
        <div class="premium-header">
            <div class="logo-text">PG <span style="color:var(--neon-gold)">ULTRA</span></div>
            <div id="u_id" style="font-size:0.6rem; background:black; color:white; padding:8px 15px; border-radius:50px; font-weight:800;">ID: 03XX...</div>
        </div>

        <div class="neon-card">
            <div class="balance-title">Liquid Capital</div>
            <div class="balance-main">Rs <span id="u_wallet">0.00</span></div>
            <div style="display:flex; justify-content:space-between; margin-top:20px; padding-top:20px; border-top:1px solid #f1f5f9;">
                <div><small style="color:#94a3b8; font-size:0.6rem; font-weight:800;">LIVE YIELD</small><br><b id="u_profit" style="color:#10b981;">+0.00</b></div>
                <div><small style="color:#94a3b8; font-size:0.6rem; font-weight:800;">HASH SPEED</small><br><b id="u_speed" style="color:var(--neon-gold)">0.00000</b></div>
            </div>
        </div>

        <div class="action-grid">
            <div class="tile ripple" onclick="nav('deposit')"><i class="fa fa-plus-square"></i><span>Add</span></div>
            <div class="tile ripple" onclick="nav('withdraw')"><i class="fa fa-arrow-right-from-bracket"></i><span>Out</span></div>
            <div class="tile ripple" onclick="nav('team')"><i class="fa fa-users-viewfinder"></i><span>Network</span></div>
            <div class="tile ripple" onclick="alert('Premium AI Support is ready.')"><i class="fa fa-microchip"></i><span>AI</span></div>
        </div>

        <div style="padding: 35px 25px 10px; font-weight: 900; font-size: 1.3rem; letter-spacing: -1px;">Premium Flux Nodes</div>
        <div id="nodes_list"></div>
    </section>

    <!-- NAVIGATION -->
    <nav class="neon-nav" id="bot-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('hub-scr')"><i class="fa fa-house-chimney-window"></i>Hub</div>
        <div class="nav-item" onclick="nav('team')"><i class="fa fa-diagram-project"></i>Team</div>
        <div class="nav-item" onclick="nav('withdraw')"><i class="fa fa-vault"></i>Vault</div>
        <div class="nav-item" onclick="checkAdmin()"><i class="fa fa-fingerprint"></i>Secure</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_neon_v11');

        function initApp() {
            if(user) { nav('hub-scr'); syncHub(); document.getElementById('u_id').innerText = "ID: " + user.substring(0,5)+"..."; }
            loadPremiumNodes();
        }

        function handleAuth() {
            let p = document.getElementById('ph').value;
            let s = document.getElementById('pass').value;
            if(p.length < 10) return;
            user = p; localStorage.setItem('pg_neon_v11', p);
            db.ref('users/'+p).once('value', snap => {
                if(!snap.exists()) db.ref('users/'+p).set({wallet:0, profit:0, speed:0, pass:s});
                location.reload();
            });
        }

        function syncHub() {
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

        function loadPremiumNodes() {
            const nodes = [
                {n:'Flux-Core 1', p:500, d:50}, {n:'Flux-Core 2', p:2000, d:240},
                {n:'Neon VIP', p:10000, d:1500}, {n:'Titanium', p:50000, d:8500}
            ];
            let list = document.getElementById('nodes_list');
            nodes.forEach(i => {
                list.innerHTML += `<div class="node-box">
                    <div class="node-status"><div class="pulse-dot"></div> LIVE MINING</div>
                    <div style="margin-top:10px;">
                        <h4 style="font-weight:800; color:var(--text);">${i.n}</h4>
                        <small style="color:#94a3b8; font-weight:700;">Price: Rs ${i.p.toLocaleString()}</small>
                    </div>
                    <div style="text-align:right;">
                        <b style="color:#10b981; display:block; margin-bottom:10px;">+Rs ${i.d}/Day</b>
                        <button class="btn-neon" onclick="buyNode(${i.p}, ${i.d/86400})">CONNECT</button>
                    </div>
                </div>`;
            });
        }

        function buyNode(p, s) {
            db.ref('users/'+user).once('value', v => {
                if(v.val().wallet < p) return alert("Insufficient Neural Funds.");
                db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s});
                alert("Protocol Established! Check Hub.");
            });
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('bot-nav').style.display = (id==='login-scr')?'none':'flex';
            
            // Animation for nav icons
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            event.currentTarget.classList.add('active');
        }

        function checkAdmin() { let p = prompt("ACCESS KEY:"); if(p==="78692") alert("Admin Auth Success."); }
    </script>
</body>
</html>
