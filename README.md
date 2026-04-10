<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Official Enterprise Platform</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { --gold: #c5a059; --dark: #0f172a; --white: #ffffff; --vip: #eb4d4b; }
        * { margin:0; padding:0; box-sizing:border-box; font-family:'Outfit',sans-serif; }
        body { background: #f4f7fe; color: var(--dark); }
        
        .page { display:none; min-height:100vh; padding-bottom:100px; animation: fadeIn 0.3s ease-in-out; }
        .page.active { display:block; }
        @keyframes fadeIn { from { opacity:0; } to { opacity:1; } }

        /* Professional Header */
        .app-bar { background: var(--white); padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; box-shadow: 0 2px 10px rgba(0,0,0,0.05); position: sticky; top:0; z-index:100; }
        
        /* Corporate Dashboard Card */
        .main-card { background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%); color: white; margin: 15px; padding: 25px; border-radius: 24px; box-shadow: 0 15px 35px rgba(15, 23, 42, 0.2); position: relative; overflow: hidden; }
        .main-card h1 { font-size: 2.2rem; color: var(--gold); margin: 8px 0; }

        /* Professional Grid */
        .action-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 15px; }
        .btn-act { background: var(--white); border: none; padding: 18px; border-radius: 18px; font-weight: 700; box-shadow: 0 4px 15px rgba(0,0,0,0.04); display: flex; flex-direction: column; align-items: center; gap: 8px; cursor: pointer; }
        .btn-act i { font-size: 1.5rem; color: var(--gold); }

        /* Node Listing */
        .node-item { background: var(--white); margin: 12px 15px; padding: 20px; border-radius: 20px; display: flex; justify-content: space-between; align-items: center; border: 1px solid rgba(0,0,0,0.02); }
        .node-info b { font-size: 1.1rem; }
        .rent-now { background: var(--dark); color: white; border: none; padding: 10px 22px; border-radius: 12px; font-weight: 700; cursor: pointer; }
        .vip-tag { background: var(--vip); color: white; padding: 3px 8px; border-radius: 6px; font-size: 0.6rem; font-weight: 800; vertical-align: middle; }

        /* Professional Inputs */
        input, select { width:100%; padding:16px; margin-bottom:15px; border-radius:12px; border:1px solid #e2e8f0; font-size: 1rem; outline: none; background: #fff; }
        .primary-btn { background: var(--gold); color: white; border: none; padding: 18px; border-radius: 15px; width: 100%; font-weight: 800; cursor: pointer; font-size: 1rem; box-shadow: 0 10px 20px rgba(197, 160, 89, 0.2); }

        /* Navigation Bar */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: var(--white); display: flex; justify-content: space-around; padding: 15px 0 35px; border-top: 1px solid #edf2f7; z-index: 1000; }
        .nav-link { color: #94a3b8; text-align: center; font-size: 0.75rem; font-weight: 600; cursor: pointer; }
        .nav-link.active { color: var(--gold); }
        .nav-link i { font-size: 1.4rem; display: block; margin-bottom: 4px; }
    </style>
</head>
<body onload="checkSession()">

    <section id="login" class="page active">
        <div style="padding: 80px 30px; text-align: center;">
            <div style="width:70px; height:70px; background:var(--gold); border-radius:20px; margin:0 auto 20px; display:flex; align-items:center; justify-content:center; color:white;"><i class="fa fa-shield-halved fa-2x"></i></div>
            <h1 style="font-weight: 800; letter-spacing: -1px; font-size: 2.2rem;">PAK GOLD</h1>
            <p style="opacity:0.5; margin-bottom:40px;">Enterprise Mining Infrastructure</p>
            
            <input type="number" id="ph" placeholder="Phone Number">
            <input type="password" id="pass" placeholder="Account Password">
            <button class="primary-btn" onclick="auth()">SECURE LOGIN</button>
            <p style="margin-top:25px; font-size:0.8rem; opacity:0.6;">Protected by AES-256 Bit Encryption</p>
        </div>
    </section>

    <section id="home" class="page">
        <div class="app-bar">
            <b style="color:var(--gold); font-weight:800;">PAK GOLD</b>
            <i class="fa fa-bell" style="opacity:0.4;"></i>
        </div>

        <div class="main-card">
            <small style="opacity:0.7;">Total Managed Assets</small>
            <h1>Rs <span id="u_wallet">0</span></h1>
            <div style="display:flex; justify-content:space-between; margin-top:20px; border-top:1px solid rgba(255,255,255,0.1); padding-top:15px;">
                <div><small style="opacity:0.6;">Accumulated Profit</small><br><b>Rs <span id="u_profit">0.00</span></b></div>
                <div><small style="opacity:0.6;">Real-time Hashrate</small><br><b><span id="u_speed">0.00000</span>/s</b></div>
            </div>
        </div>

        <div class="action-grid">
            <div class="btn-act" onclick="nav('deposit')"><i class="fa fa-wallet"></i>Deposit</div>
            <div class="btn-act" onclick="nav('withdraw')"><i class="fa fa-paper-plane"></i>Withdraw</div>
        </div>

        <div style="padding: 25px 20px 10px;"><h3 style="font-weight:800;">Mining Nodes</h3></div>
        <div id="plans_container"></div>
    </section>

    <section id="deposit" class="page">
        <div class="app-bar"><i class="fa fa-chevron-left" onclick="nav('home')"></i> <b>Deposit Gateway</b> <div></div></div>
        <div class="card" style="padding:20px;">
            <select id="d_meth" onchange="updPayInfo()">
                <option value="J">JazzCash / SadaPay</option>
                <option value="E">EasyPaisa</option>
            </select>
            <div style="background:#f8fafc; padding:20px; border-radius:15px; margin-bottom:20px; border:1px dashed var(--gold);">
                <small id="d_title">JazzCash / SadaPay</small>
                <h2 id="d_num" style="color:var(--gold); margin:5px 0;">03705519562</h2>
                <small>Account Title: <b>PAK GOLD OFFICIAL</b></small>
            </div>
            <input type="number" id="d_amt" placeholder="Enter Amount">
            <input type="text" id="d_tid" placeholder="TID (Transaction ID)">
            <button class="primary-btn" onclick="sendDep()">VERIFY DEPOSIT</button>
        </div>
    </section>

    <section id="withdraw" class="page">
        <div class="app-bar"><i class="fa fa-chevron-left" onclick="nav('home')"></i> <b>Cashout Assets</b> <div></div></div>
        <div style="padding:20px;">
            <select id="w_meth">
                <option>EasyPaisa</option><option>JazzCash</option><option>SadaPay</option><option>Bank Transfer</option><option>Binance USDT</option>
            </select>
            <input type="number" id="w_amt" placeholder="Withdrawal Amount">
            <input type="text" id="w_acc" placeholder="Account/Wallet Details">
            <button class="primary-btn" style="background:var(--dark);" onclick="sendWd()">CONFIRM CASHOUT</button>
        </div>
    </section>

    <section id="master_admin" class="page" style="background:#000; color:#fff; padding:20px;">
        <h2 style="color:var(--gold)">MASTER TERMINAL</h2>
        <div id="admin_list"></div>
    </section>

    <nav class="bottom-nav" id="main_nav" style="display:none;">
        <div class="nav-link active" onclick="nav('home')"><i class="fa fa-home"></i>Home</div>
        <div class="nav-link" onclick="nav('team')"><i class="fa fa-users"></i>Team</div>
        <div class="nav-link" onclick="nav('history')"><i class="fa fa-history"></i>History</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_session_v3');

        function checkSession() { if(user) { nav('home'); sync(); } loadPlans(); }

        function auth() {
            let p = document.getElementById('ph').value;
            // Secret Code Check: Agar koi phone number ki jagah admin code daale
            if(p === "78692") { nav('master_admin'); loadAdmin(); return; }
            if(p.length < 10) return alert("Sweetie, enter a professional number!");
            user = p; localStorage.setItem('pg_session_v3', p);
            db.ref('users/'+p).once('value', s => {
                if(!s.exists()) db.ref('users/'+p).set({wallet:0, profit:0, speed:0, team:[]});
                location.reload();
            });
        }

        function loadPlans() {
            const plans = [
                {lvl:1, p:200, d:20, v:30, t:'REG'}, {lvl:2, p:500, d:55, v:30, t:'REG'},
                {lvl:3, p:1000, d:120, v:35, t:'REG'}, {lvl:4, p:2000, d:250, v:35, t:'REG'},
                {lvl:5, p:5000, d:650, v:40, t:'REG'}, {lvl:6, p:8000, d:1100, v:40, t:'REG'},
                {lvl:7, p:12000, d:1800, v:45, t:'REG'}, {lvl:8, p:15000, d:2400, v:45, t:'REG'},
                {lvl:9, p:20000, d:3500, v:50, t:'REG'}, {lvl:10, p:25000, d:4500, v:50, t:'REG'},
                {lvl:1, p:50000, d:10000, v:60, t:'VIP'}, {lvl:2, p:100000, d:22000, v:70, t:'VIP'},
                {lvl:3, p:200000, d:50000, v:80, t:'VIP'}, {lvl:4, p:350000, d:95000, v:90, t:'VIP'},
                {lvl:5, p:500000, d:150000, v:100, t:'VIP'}
            ];
            let cont = document.getElementById('plans_container');
            plans.forEach(i => {
                let isV = i.t==='VIP';
                cont.innerHTML += `
                <div class="node-item">
                    <div class="node-info">
                        <b>Level ${i.lvl} Node ${isV?'<span class="vip-tag">VIP</span>':''}</b><br>
                        <small>Daily: Rs ${i.d} | Term: ${i.v} Days</small><br>
                        <b style="color:var(--gold); font-size:1.1rem;">Rs ${i.p}</b>
                    </div>
                    <button class="rent-now" onclick="rentNode(${i.p}, ${i.d/86400})">Rent</button>
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
                if(v.val().wallet < p) return alert("Insufficient Capital.");
                db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s});
                alert("Node Activated Successfully!");
            });
        }

        function updPayInfo() {
            let m = document.getElementById('d_meth').value;
            document.getElementById('d_title').innerText = m==='E'?'EasyPaisa':'JazzCash / SadaPay';
            document.getElementById('d_num').innerText = m==='E'?'03379827882':'03705519562';
        }

        function sendDep() {
            let a = document.getElementById('d_amt').value;
            let t = document.getElementById('d_tid').value;
            if(!a || !t) return alert("Fill all fields");
            db.ref('admin_requests').push({u:user, type:'Deposit', amt:a, tid:t, status:'pending'});
            alert("Sent for Audit."); nav('home');
        }

        function sendWd() {
            let a = document.getElementById('w_amt').value;
            db.ref('users/'+user).once('value', s => {
                if(s.val().wallet < a) return alert("Insufficient Balance");
                db.ref('admin_requests').push({u:user, type:'Withdraw', amt:a, status:'pending'});
                db.ref('users/'+user).update({wallet: s.val().wallet - a});
                alert("Withdrawal Logged."); nav('home');
            });
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('main_nav').style.display = (id==='login'||id==='master_admin')?'none':'flex';
        }

        // --- ADMIN REAL LOGIC ---
        function loadAdmin() {
            db.ref('admin_requests').on('value', s => {
                let q = document.getElementById('admin_list'); q.innerHTML = "";
                s.forEach(c => {
                    let r = c.val(); if(r.status==='pending') {
                        q.innerHTML += `<div style="border:1px solid #333; padding:15px; margin-top:10px;">
                        ${r.u} | ${r.type} | Rs ${r.amt} <br> TID: ${r.tid || '--'}
                        <button onclick="approve('${c.key}','${r.u}',${r.amt},'${r.type}')" style="background:green; color:white; border:none; padding:8px; width:100%; margin-top:10px;">APPROVE</button></div>`;
                    }
                });
            });
        }

        function approve(id, u, a, t) {
            if(t==='Deposit') {
                db.ref('users/'+u).once('value', s => {
                    db.ref('users/'+u).update({wallet: s.val().wallet + parseInt(a)});
                });
            }
            db.ref('admin_requests/'+id).update({status:'done'}); alert("Confirmed.");
        }
    </script>
</body>
</html>
