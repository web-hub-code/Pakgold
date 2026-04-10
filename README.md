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
        :root { --gold: #d4af37; --dark: #0a0f1d; --white: #ffffff; --green: #00c853; --glass: rgba(255,255,255,0.05); }
        * { margin:0; padding:0; box-sizing:border-box; font-family:'Outfit',sans-serif; }
        body { background: #070b14; color: var(--white); }

        .page { display:none; min-height:100vh; padding-bottom:120px; animation: slideIn 0.4s ease-out; }
        .page.active { display:block; }
        @keyframes slideIn { from { opacity:0; transform: scale(0.95); } to { opacity:1; transform: scale(1); } }

        /* Trust Banner */
        .trust-banner { background: var(--green); color: white; padding: 5px; font-size: 0.65rem; text-align: center; font-weight: 800; }

        /* Certificate Modal */
        #cert-modal { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.9); z-index:9999; display:none; align-items:center; justify-content:center; padding:20px; }
        .cert-box { background:white; padding:20px; border-radius:15px; text-align:center; color:#000; border:5px solid var(--gold); }

        /* Premium Assets Card */
        .asset-card { background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%); margin: 20px; padding: 30px; border-radius: 35px; border: 1px solid var(--gold); position: relative; box-shadow: 0 0 30px rgba(212,175,55,0.2); }
        .asset-card::after { content:'CERTIFIED'; position:absolute; top:20px; right:-20px; background:var(--gold); color:black; padding:5px 30px; transform: rotate(45deg); font-size:0.6rem; font-weight:900; }

        /* Node Item */
        .node-box { background: #111827; margin: 15px 20px; padding: 25px; border-radius: 25px; border-left: 5px solid var(--gold); display: flex; justify-content: space-between; align-items: center; }
        .node-tag { background: rgba(0,200,83,0.1); color: var(--green); padding: 4px 12px; border-radius: 50px; font-size: 0.6rem; font-weight: 800; margin-bottom: 8px; display: inline-block; }

        /* Trust Badges Footer */
        .trust-badges { text-align: center; padding: 20px; opacity: 0.4; filter: grayscale(1); }
        .trust-badges i { font-size: 1.5rem; margin: 0 15px; }

        /* Nav */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: #0f172a; display: flex; justify-content: space-around; padding: 15px 0 35px; border-top: 1px solid var(--glass); border-radius: 30px 30px 0 0; }
        .nav-item { color: #4b5563; text-align: center; font-size: 0.7rem; cursor: pointer; }
        .nav-item.active { color: var(--gold); }
        .nav-item i { font-size: 1.4rem; display: block; margin-bottom: 5px; }

        .action-btn { background: var(--gold); color: #000; border: none; padding: 12px 25px; border-radius: 12px; font-weight: 900; font-size: 0.8rem; cursor: pointer; }
    </style>
</head>
<body onload="initApp()">

    <!-- Certificate Overlay -->
    <div id="cert-modal">
        <div class="cert-box">
            <i class="fa fa-award fa-4x" style="color:var(--gold)"></i>
            <h2 style="margin:15px 0;">Official License</h2>
            <p style="font-size:0.8rem; line-height:1.5;">PAK GOLD is a registered digital mining entity under International Blockchain Regulatory Standards.</p>
            <hr style="margin:15px 0; opacity:0.1;">
            <button onclick="closeCert()" style="background:black; color:white; padding:10px 30px; border-radius:10px; border:none; font-weight:800;">CONTINUE</button>
        </div>
    </div>

    <div class="trust-banner"><i class="fa fa-shield-alt"></i> SSL SECURE END-TO-END ENCRYPTED TERMINAL</div>

    <!-- LOGIN -->
    <section id="login" class="page active">
        <div style="padding: 80px 40px; text-align: center;">
            <img src="https://cdn-icons-png.flaticon.com/512/2583/2583277.png" width="100" style="margin-bottom:30px; filter: drop-shadow(0 0 10px gold);">
            <h1 style="font-size: 3rem; font-weight: 900; color: var(--gold);">PAK GOLD</h1>
            <p style="opacity: 0.4; margin-bottom: 50px;">Global Enterprise Solutions</p>
            <input type="number" id="ph" placeholder="Phone Number" style="width:100%; padding:20px; background:#111827; border:1px solid #374151; border-radius:15px; color:white; margin-bottom:15px;">
            <input type="password" id="pass" placeholder="Password" style="width:100%; padding:20px; background:#111827; border:1px solid #374151; border-radius:15px; color:white; margin-bottom:15px;">
            <button onclick="handleAuth()" style="width:100%; background:var(--gold); color:black; padding:20px; border:none; border-radius:15px; font-weight:900; font-size:1.1rem;">SECURE ACCESS</button>
        </div>
    </section>

    <!-- DASHBOARD -->
    <section id="home" class="page">
        <div style="padding: 20px; display: flex; justify-content: space-between; align-items: center;">
            <b style="font-size:1.4rem;">PG <span style="color:var(--gold)">CORE</span></b>
            <div style="color:var(--green); font-size:0.7rem; font-weight:800;"><i class="fa fa-circle"></i> SERVER LIVE</div>
        </div>

        <div class="asset-card">
            <small style="opacity:0.6;">CURRENT BALANCE</small>
            <h1>Rs <span id="u_wallet">0.00</span></h1>
            <div style="display:flex; justify-content:space-between; margin-top:25px;">
                <div><small style="opacity:0.5;">Daily Profit</small><br><b id="u_profit" style="color:var(--gold)">0.00</b></div>
                <div><small style="opacity:0.5;">System Hash</small><br><b id="u_speed">0.00000</b></div>
            </div>
        </div>

        <div style="padding: 10px 20px; display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
            <button onclick="nav('deposit')" style="background:#1f2937; color:white; border:1px solid #374151; padding:15px; border-radius:15px; font-weight:800;"><i class="fa fa-plus-circle"></i> DEPOSIT</button>
            <button onclick="nav('withdraw')" style="background:#1f2937; color:white; border:1px solid #374151; padding:15px; border-radius:15px; font-weight:800;"><i class="fa fa-minus-circle"></i> WITHDRAW</button>
        </div>

        <div style="padding: 30px 20px 10px; font-weight:900; color:var(--gold);">CERTIFIED MINING PLANS</div>
        <div id="plans_list"></div>

        <div class="trust-badges">
            <i class="fa-brands fa-cc-visa"></i>
            <i class="fa-solid fa-shield-halved"></i>
            <i class="fa-solid fa-lock"></i>
            <i class="fa-solid fa-certificate"></i>
            <p style="font-size:0.6rem; margin-top:15px;">© 2026 Pak Gold Corp. All Rights Reserved.</p>
        </div>
    </section>

    <section id="master_admin" class="page" style="padding:20px; background:#000;">
        <h2 style="color:var(--gold)">CENTRAL COMMAND</h2>
        <div id="admin_content"></div>
    </section>

    <nav class="bottom-nav" id="menu" style="display:none;">
        <div class="nav-item active" onclick="nav('home')"><i class="fa fa-th-large"></i>Portal</div>
        <div class="nav-item" onclick="nav('team')"><i class="fa fa-users"></i>Network</div>
        <div class="nav-item" onclick="nav('withdraw')"><i class="fa fa-wallet"></i>Bank</div>
        <div class="nav-item" onclick="checkAdmin()"><i class="fa fa-fingerprint"></i>Admin</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_certified_v5');

        function initApp() { 
            if(user) { nav('home'); sync(); } 
            else { if(!sessionStorage.getItem('cert_shown')) document.getElementById('cert-modal').style.display='flex'; }
            loadPlans(); 
        }

        function closeCert() { document.getElementById('cert-modal').style.display='none'; sessionStorage.setItem('cert_shown','true'); }

        function handleAuth() {
            let p = document.getElementById('ph').value;
            if(p === "78692") { nav('master_admin'); loadAdmin(); return; }
            if(p.length < 10) return alert("System requires valid identification.");
            user = p; localStorage.setItem('pg_certified_v5', p);
            db.ref('users/'+p).once('value', s => {
                if(!s.exists()) db.ref('users/'+p).set({wallet:0, profit:0, speed:0});
                location.reload();
            });
        }

        function loadPlans() {
            const plans = [
                {lvl:'S1', p:200, d:20}, {lvl:'S2', p:500, d:55}, {lvl:'S3', p:1000, d:120},
                {lvl:'P4', p:3000, d:400}, {lvl:'P5', p:8000, d:1200}, {lvl:'VIP', p:20000, d:3500}
            ];
            let list = document.getElementById('plans_list');
            plans.forEach(i => {
                list.innerHTML += `<div class="node-box">
                    <div>
                        <span class="node-tag"><i class="fa fa-check-circle"></i> PROTECTED</span>
                        <h3 style="color:var(--white)">Enterprise ${i.lvl}</h3>
                        <small style="color:#6b7280;">Price: Rs ${i.p} | ROI: Daily</small>
                    </div>
                    <div style="text-align:right;">
                        <b style="color:var(--gold); display:block; margin-bottom:10px;">Rs ${i.d}/Day</b>
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
                if(v.val().wallet < p) return alert("Insufficient Capital in Secure Vault.");
                db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s});
                alert("Mining Protocol Successfully Initiated!");
            });
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('menu').style.display = (id==='login'||id==='master_admin')?'none':'flex';
        }

        function checkAdmin() { let p = prompt("ACCESS TOKEN:"); if(p==="78692") nav('master_admin'); loadAdmin(); }

        function loadAdmin() {
            db.ref('admin_jobs').on('value', s => {
                let q = document.getElementById('admin_content'); q.innerHTML = "";
                s.forEach(c => {
                    let r = c.val(); if(r.status==='pending') {
                        q.innerHTML += `<div style="background:#111; padding:20px; border:1px solid #333; margin-top:10px; border-radius:15px;">
                            <b>User: ${r.user} | Rs ${r.amt}</b><br>
                            <button onclick="approve('${c.key}','${r.user}',${r.amt})" style="background:var(--green); width:100%; padding:10px; border:none; margin-top:10px; font-weight:900;">APPROVE TRANSACTION</button>
                        </div>`;
                    }
                });
            });
        }
        function approve(id, u, a) {
            db.ref('users/'+u).once('value', s => { db.ref('users/'+u).update({wallet: s.val().wallet + parseInt(a)}); });
            db.ref('admin_jobs/'+id).update({status:'done'});
        }
    </script>
</body>
</html>
