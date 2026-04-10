<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Official Certified Platform</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { --gold: #b38b2d; --bg: #f3f4f6; --white: #ffffff; --green: #10b981; --text: #111827; }
        * { margin:0; padding:0; box-sizing:border-box; font-family:'Outfit',sans-serif; }
        body { background: var(--bg); color: var(--text); }

        .page { display:none; min-height:100vh; padding-bottom:120px; animation: slideIn 0.4s ease-out; }
        .page.active { display:block; }
        @keyframes slideIn { from { opacity:0; transform: translateY(10px); } to { opacity:1; transform: translateY(0); } }

        /* Trust Banner */
        .trust-banner { background: #1e293b; color: white; padding: 6px; font-size: 0.65rem; text-align: center; font-weight: 600; letter-spacing: 1px; }

        /* Certificate Modal */
        #cert-modal { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.7); backdrop-filter: blur(5px); z-index:9999; display:none; align-items:center; justify-content:center; padding:20px; }
        .cert-box { background:white; padding:30px; border-radius:30px; text-align:center; color:var(--text); border:4px solid var(--gold); box-shadow: 0 25px 50px -12px rgba(0,0,0,0.25); }

        /* Verified Card */
        .asset-card { background: white; margin: 20px; padding: 30px; border-radius: 35px; border: 1px solid rgba(0,0,0,0.05); position: relative; box-shadow: 0 10px 30px rgba(0,0,0,0.05); }
        .verified-tag { position: absolute; top: 20px; right: 20px; background: #dcfce7; color: #166534; padding: 4px 12px; border-radius: 50px; font-size: 0.6rem; font-weight: 800; display: flex; align-items: center; gap: 5px; }

        /* Node Item */
        .node-box { background: white; margin: 15px 20px; padding: 25px; border-radius: 25px; border-left: 6px solid var(--gold); display: flex; justify-content: space-between; align-items: center; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.02); }
        .node-tag { color: var(--green); font-size: 0.6rem; font-weight: 800; text-transform: uppercase; margin-bottom: 5px; display: block; }

        /* Action Buttons */
        .action-btn { background: var(--text); color: white; border: none; padding: 12px 22px; border-radius: 15px; font-weight: 700; font-size: 0.75rem; cursor: pointer; }

        /* Bottom Nav */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: white; display: flex; justify-content: space-around; padding: 15px 0 30px; border-top: 1px solid #e5e7eb; }
        .nav-item { color: #9ca3af; text-align: center; font-size: 0.65rem; cursor: pointer; font-weight: 600; }
        .nav-item.active { color: var(--gold); }
        .nav-item i { font-size: 1.3rem; display: block; margin-bottom: 5px; }
    </style>
</head>
<body onload="initApp()">

    <!-- Certificate Overlay -->
    <div id="cert-modal">
        <div class="cert-box">
            <i class="fa fa-shield-check fa-4x" style="color:var(--gold)"></i>
            <h2 style="margin:20px 0; font-weight:800;">Secure License</h2>
            <p style="font-size:0.85rem; color:#64748b; line-height:1.6;">PAK GOLD is a verified asset management platform. Your data and investments are protected by 256-bit encryption.</p>
            <button onclick="closeCert()" style="background:var(--text); color:white; padding:12px 40px; border-radius:15px; border:none; font-weight:800; margin-top:25px; width:100%;">I AGREE & CONTINUE</button>
        </div>
    </div>

    <div class="trust-banner"><i class="fa fa-lock"></i> SSL ENCRYPTED SECURE CONNECTION</div>

    <!-- DASHBOARD -->
    <section id="home" class="page active">
        <div style="padding: 20px; display: flex; justify-content: space-between; align-items: center;">
            <b style="font-size:1.3rem;">PG <span style="color:var(--gold)">OFFICIAL</span></b>
            <div style="background:white; padding:5px 12px; border-radius:50px; font-size:0.6rem; font-weight:800; border:1px solid #e5e7eb;">
                <i class="fa fa-server" style="color:var(--green)"></i> ONLINE
            </div>
        </div>

        <div class="asset-card">
            <div class="verified-tag"><i class="fa fa-check-circle"></i> VERIFIED</div>
            <small style="color:#64748b; font-weight:600; letter-spacing:1px;">AVAILABLE BALANCE</small>
            <h1 style="font-size:2.8rem; font-weight:900; margin:10px 0;">Rs <span id="u_wallet">0.00</span></h1>
            <div style="display:flex; justify-content:space-between; margin-top:25px; padding-top:15px; border-top:1px solid #f3f4f6;">
                <div><small style="color:#9ca3af;">Daily Yield</small><br><b id="u_profit" style="color:var(--green)">0.00</b></div>
                <div><small style="color:#9ca3af;">Hash Speed</small><br><b id="u_speed">0.00000</b></div>
            </div>
        </div>

        <div style="padding: 0 20px; display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
            <button onclick="nav('deposit')" style="background:white; color:var(--text); border:1px solid #e5e7eb; padding:18px; border-radius:20px; font-weight:800; font-size:0.8rem;"><i class="fa fa-arrow-up" style="color:var(--gold)"></i> DEPOSIT</button>
            <button onclick="nav('withdraw')" style="background:white; color:var(--text); border:1px solid #e5e7eb; padding:18px; border-radius:20px; font-weight:800; font-size:0.8rem;"><i class="fa fa-arrow-down" style="color:var(--gold)"></i> WITHDRAW</button>
        </div>

        <div style="padding: 30px 20px 10px; font-weight:800; font-size:1rem; color: #4b5563;">CERTIFIED MINING PLANS</div>
        <div id="plans_list"></div>

        <div style="text-align: center; padding: 30px; opacity: 0.5;">
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5e/Visa_Inc._logo.svg/2560px-Visa_Inc._logo.svg.png" width="40" style="margin: 0 10px;">
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2a/Mastercard-logo.svg/1280px-Mastercard-logo.svg.png" width="30" style="margin: 0 10px;">
            <p style="font-size:0.6rem; margin-top:10px; font-weight:700;">SECURED BY GLOBAL COMPLIANCE</p>
        </div>
    </section>

    <nav class="bottom-nav" id="menu">
        <div class="nav-item active" onclick="nav('home')"><i class="fa fa-grid-2"></i>Dashboard</div>
        <div class="nav-item" onclick="nav('team')"><i class="fa fa-users"></i>Network</div>
        <div class="nav-item" onclick="nav('withdraw')"><i class="fa fa-wallet"></i>Vault</div>
        <div class="nav-item" onclick="checkAdmin()"><i class="fa fa-shield-lock"></i>Secure</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_trusted_light_v9');

        function initApp() { 
            if(user) { nav('home'); sync(); } 
            else { if(!sessionStorage.getItem('cert_shown')) document.getElementById('cert-modal').style.display='flex'; }
            loadPlans(); 
        }

        function closeCert() { document.getElementById('cert-modal').style.display='none'; sessionStorage.setItem('cert_shown','true'); }

        function handleAuth() {
            // Your login logic here
        }

        function loadPlans() {
            const plans = [
                {lvl:'Safe-Start', p:200, d:20}, {lvl:'Standard', p:500, d:55}, {lvl:'Premium', p:1000, d:120},
                {lvl:'Gold Elite', p:5000, d:650}, {lvl:'VIP Global', p:15000, d:2500}
            ];
            let list = document.getElementById('plans_list');
            plans.forEach(i => {
                list.innerHTML += `<div class="node-box">
                    <div>
                        <span class="node-tag"><i class="fa fa-shield"></i> Verified Plan</span>
                        <h4 style="font-weight:800;">${i.lvl}</h4>
                        <small style="color:#9ca3af;">Cost: Rs ${i.p}</small>
                    </div>
                    <div style="text-align:right;">
                        <b style="color:var(--green); display:block; margin-bottom:10px;">Rs ${i.d}/Day</b>
                        <button class="action-btn" onclick="rentNode(${i.p}, ${i.d/86400})">ACTIVATE</button>
                    </div>
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

        function rentNode(p, s) {
            db.ref('users/'+user).once('value', v => {
                if(v.val().wallet < p) return alert("System requires more capital to link this node.");
                db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s});
                alert("Protocol Linked Successfully! 🚀");
            });
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        }
    </script>
</body>
</html>
