<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Global Empire v3.0</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { --neon: #ffcc00; --bg: #f8fafc; --card: #ffffff; --text: #0f172a; --sub: #64748b; --success: #10b981; --error: #ef4444; }
        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        .page { display:none; animation: fadeIn 0.4s ease-out; padding-bottom: 120px; min-height: 100vh; }
        .page.active { display:block; }
        @keyframes fadeIn { from { opacity:0; transform: translateY(10px); } to { opacity:1; transform: translateY(0); } }

        /* UI Elements */
        .glass-card { background: var(--card); margin: 15px; padding: 25px; border-radius: 30px; border: 1px solid rgba(0,0,0,0.03); box-shadow: 0 10px 30px rgba(0,0,0,0.02); }
        .neon-btn { background: var(--text); color: white; border: none; padding: 16px; border-radius: 20px; font-weight: 800; cursor: pointer; transition: 0.3s; width: 100%; }
        .neon-btn:active { background: var(--neon); color: black; transform: scale(0.98); }
        .input-box { width: 100%; padding: 18px; border-radius: 18px; border: 2px solid #f1f5f9; background: #f8fafc; font-size: 1rem; outline: none; margin-bottom: 15px; }

        /* Navigation */
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.9); backdrop-filter: blur(20px); display: flex; justify-content: space-around; padding: 15px 0 30px; border-top: 1px solid #f1f5f9; z-index: 999; }
        .nav-item { color: var(--sub); text-align: center; font-size: 0.65rem; font-weight: 700; }
        .nav-item.active { color: var(--neon); }
        .nav-item i { font-size: 1.5rem; display: block; margin-bottom: 5px; }

        /* Nodes Scroll */
        .scroll-nodes { display: flex; overflow-x: auto; gap: 15px; padding: 10px 20px; scrollbar-width: none; }
        .vip-node { border: 2px solid var(--neon); background: #fffdf2; }
        
        /* History Items */
        .h-card { padding: 15px; border-bottom: 1px solid #f1f5f9; display: flex; justify-content: space-between; align-items: center; font-size: 0.8rem; }
        .status-pend { color: var(--neon); font-weight: 800; }
        .status-done { color: var(--success); font-weight: 800; }
    </style>
</head>
<body onload="checkSession()">

    <!-- AUTH PAGE -->
    <section id="auth-pg" class="page active" style="padding: 60px 30px;">
        <h1 style="font-size: 2.8rem; font-weight: 900; letter-spacing: -2px;">PAK <span style="color:var(--neon)">GOLD</span></h1>
        <p id="auth-sub" style="color:var(--sub); margin-bottom: 40px; font-weight: 600;">Secure Mining Protocol v3.0</p>
        
        <input type="number" id="auth-phone" class="input-box" placeholder="Phone Number">
        <input type="password" id="auth-pass" class="input-box" placeholder="Access Password">
        <div id="signup-extra" style="display:none;"><input type="text" id="auth-ref" class="input-box" placeholder="Referral Link (Optional)"></div>
        
        <button class="neon-btn" onclick="handleAuth()" id="auth-btn">LOGIN SESSION</button>
        <p onclick="toggleAuth()" style="text-align:center; margin-top:25px; font-weight:800; font-size:0.75rem; color:var(--sub);" id="toggle-txt">New Miner? Create Account</p>
    </section>

    <!-- DASHBOARD -->
    <section id="dash-pg" class="page">
        <div style="padding: 25px; display: flex; justify-content: space-between; align-items: center;">
            <b style="font-size:1.2rem;">CORE <span style="color:var(--neon)">GOLD</span></b>
            <i class="fa fa-bell" onclick="alert('No new notifications sweetie!')"></i>
        </div>

        <div class="glass-card" style="background: var(--text); color: white; position: relative; overflow: hidden;">
            <div style="position: absolute; top: -20px; right: -20px; width: 100px; height: 100px; background: var(--neon); border-radius: 50%; opacity: 0.2; filter: blur(30px);"></div>
            <small style="opacity: 0.6; font-weight: 700; letter-spacing: 1px;">AVAILABLE ASSETS</small>
            <h1 style="font-size: 3.2rem; margin: 10px 0; font-weight: 800;">Rs <span id="u-wallet">0.00</span></h1>
            <div style="display: flex; gap: 30px; margin-top: 15px;">
                <div><small style="opacity: 0.6;">Today's Mining</small><br><b style="color:var(--neon); font-size: 1.1rem;" id="u-profit">0.00</b></div>
                <div><small style="opacity: 0.6;">Total Team</small><br><b id="u-team" style="font-size: 1.1rem;">0</b></div>
            </div>
        </div>

        <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; padding: 0 15px;">
            <div class="glass-card" onclick="nav('depo-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-qrcode" style="color:var(--neon)"></i><br><span style="font-size:0.6rem; font-weight:800;">Deposit</span></div>
            <div class="glass-card" onclick="nav('with-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-paper-plane"></i><br><span style="font-size:0.6rem; font-weight:800;">Withdraw</span></div>
            <div class="glass-card" onclick="window.open('https://chat.whatsapp.com/YOUR_LINK_HERE')" style="margin:0; text-align:center; padding:15px;"><i class="fab fa-whatsapp" style="color:#25D366"></i><br><span style="font-size:0.6rem; font-weight:800;">Group</span></div>
            <div class="glass-card" onclick="nav('history-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-list-check"></i><br><span style="font-size:0.6rem; font-weight:800;">History</span></div>
        </div>

        <h3 style="padding: 30px 20px 5px;">VIP Elite Nodes</h3>
        <div class="scroll-nodes" id="vip-list"></div>

        <h3 style="padding: 10px 20px 5px;">Economical Nodes</h3>
        <div id="regular-list"></div>
    </section>

    <!-- DEPOSIT PAGE -->
    <section id="depo-pg" class="page">
        <h2 style="padding:20px;">Secure Deposit</h2>
        <div class="glass-card">
            <p style="font-size:0.8rem; color:var(--sub); margin-bottom:15px;">Transfer to: <b>JazzCash (03XX-XXXXXXX)</b></p>
            <input type="number" id="depo-amt" class="input-box" placeholder="Amount (Min 500)">
            <input type="text" id="depo-tid" class="input-box" placeholder="Transaction ID (TID)">
            <button class="neon-btn" onclick="submitDeposit()">SUBMIT REQUEST</button>
        </div>
    </section>

    <!-- WITHDRAWAL PAGE -->
    <section id="with-pg" class="page">
        <h2 style="padding:20px;">Request Payout</h2>
        <div class="glass-card">
            <input type="number" id="with-amt" class="input-box" placeholder="Amount (Min 500)">
            <input type="text" id="with-wallet" class="input-box" placeholder="EasyPaisa/JazzCash Number">
            <button class="neon-btn" onclick="submitWithdraw()">WITHDRAW NOW</button>
        </div>
    </section>

    <!-- HISTORY PAGE -->
    <section id="history-pg" class="page">
        <h2 style="padding:20px;">Finance Logs</h2>
        <div class="glass-card" id="hist-list">
            <p style="text-align:center; color:var(--sub);">No transactions found.</p>
        </div>
    </section>

    <!-- ADMIN PANEL -->
    <section id="admin-pg" class="page">
        <h2 style="padding:20px;">God Mode Control</h2>
        <div class="glass-card">
            <h4>Update User Wallet</h4>
            <input type="number" id="adm-ph" class="input-box" placeholder="User Phone">
            <input type="number" id="adm-val" class="input-box" placeholder="Amount to Add">
            <button class="neon-btn" onclick="adminAdd()">INJECT BALANCE</button>
        </div>
        <div id="admin-requests" style="padding:20px;"></div>
    </section>

    <!-- INFO PAGE -->
    <section id="info-pg" class="page">
        <h2 style="padding:20px;">Company Profile</h2>
        <div class="glass-card">
            <h4>PAK GOLD OFFICIAL</h4>
            <p style="font-size:0.8rem; color:var(--sub); line-height:1.6;">Email: support@pakgold.com<br>Location: Blue Area, Islamabad<br>License: #2026-PG-99</p>
        </div>
        <div class="glass-card">
            <h4>Privacy Policy</h4>
            <p style="font-size:0.75rem; color:var(--sub);">We use end-to-end encryption. Your funds are protected by decentralized blockchain proof-of-stake.</p>
        </div>
    </section>

    <!-- NAV BAR -->
    <nav class="nav-bar" id="bot-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-pg')"><i class="fa fa-house"></i>Home</div>
        <div class="nav-item" onclick="nav('info-pg')"><i class="fa fa-shield-halved"></i>Policy</div>
        <div class="nav-item" onclick="nav('history-pg')"><i class="fa fa-clock-rotate-left"></i>Logs</div>
        <div class="nav-item" onclick="logout()"><i class="fa fa-right-from-bracket"></i>Exit</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_empire_user');

        function checkSession() {
            if(user) { 
                if(user === "78692") { nav('admin-pg'); loadAdminRequests(); }
                else { nav('dash-pg'); syncUser(); }
                document.getElementById('bot-nav').style.display = 'flex';
            }
            loadAllNodes();
        }

        function handleAuth() {
            const ph = document.getElementById('auth-phone').value;
            const ps = document.getElementById('auth-pass').value;
            if(ph === "78692" && ps === "sweetie123") { localStorage.setItem('pg_empire_user', "78692"); location.reload(); return; }
            db.ref('users/'+ph).once('value', s => {
                if(!s.exists()) db.ref('users/'+ph).set({phone:ph, pass:ps, wallet:0, profit:0, speed:0});
                localStorage.setItem('pg_empire_user', ph); location.reload();
            });
        }

        function syncUser() {
            db.ref('users/'+user).on('value', s => {
                const d = s.val();
                document.getElementById('u-wallet').innerText = d.wallet.toLocaleString();
                document.getElementById('u-profit').innerText = d.profit.toFixed(2);
            });
            setInterval(() => {
                db.ref('users/'+user).once('value', s => {
                    const d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed});
                });
            }, 1000);
            loadHistory();
        }

        function loadAllNodes() {
            const regs = Array.from({length: 15}, (_, i) => ({n:`Reg-Node ${i+1}`, p:(i+1)*200, d:(i+1)*25}));
            const vips = Array.from({length: 5}, (_, i) => ({n:`VIP Elite ${i+1}`, p:(i+1)*5000, d:(i+1)*850}));
            
            let regCont = document.getElementById('regular-list');
            let vipCont = document.getElementById('vip-list');

            regs.forEach(i => {
                regCont.innerHTML += `<div class="glass-card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><b>${i.n}</b><br><small>Earn Rs ${i.d}/Day</small></div>
                    <button class="neon-btn" style="width:auto; padding:8px 15px;" onclick="buyNode(${i.p}, ${i.d/86400})">Rs ${i.p}</button>
                </div>`;
            });

            vips.forEach(i => {
                vipCont.innerHTML += `<div class="glass-card vip-node" style="min-width:200px; text-align:center;">
                    <i class="fa fa-crown" style="color:var(--neon)"></i><br><b>${i.n}</b><br>
                    <b style="color:var(--success)">+Rs ${i.d}/Day</b><br><br>
                    <button class="neon-btn" onclick="buyNode(${i.p}, ${i.d/86400})">Rs ${i.p}</button>
                </div>`;
            });
        }

        function buyNode(p, s) {
            db.ref('users/'+user).once('value', v => {
                if(v.val().wallet < p) return alert("Insufficient Assets!");
                db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s});
                alert("Node Established! 🚀");
            });
        }

        function submitDeposit() {
            const amt = document.getElementById('depo-amt').value;
            const tid = document.getElementById('depo-tid').value;
            db.ref('requests').push({type:'Deposit', user:user, amt:amt, tid:tid, status:'Pending'});
            alert("Request sent to Admin!");
        }

        function submitWithdraw() {
            const amt = document.getElementById('with-amt').value;
            const wall = document.getElementById('with-wallet').value;
            db.ref('users/'+user).once('value', s => {
                if(s.val().wallet < amt) return alert("Low Balance!");
                db.ref('requests').push({type:'Withdraw', user:user, amt:amt, wall:wall, status:'Pending'});
                db.ref('users/'+user).update({wallet: s.val().wallet - amt});
                alert("Withdraw Request sent!");
            });
        }

        function loadHistory() {
            db.ref('requests').on('value', s => {
                let h = document.getElementById('hist-list'); h.innerHTML = "";
                s.forEach(child => {
                    if(child.val().user === user) {
                        h.innerHTML += `<div class="h-card">
                            <div><b>${child.val().type}</b><br><small>Rs ${child.val().amt}</small></div>
                            <span class="${child.val().status==='Pending'?'status-pend':'status-done'}">${child.val().status}</span>
                        </div>`;
                    }
                });
            });
        }

        // ADMIN FUNCTIONS
        function loadAdminRequests() {
            db.ref('requests').on('value', s => {
                let cont = document.getElementById('admin-requests'); cont.innerHTML = "<h3>Pending Requests</h3>";
                s.forEach(child => {
                    if(child.val().status === 'Pending') {
                        cont.innerHTML += `<div class="glass-card">
                            <p><b>${child.val().type}</b> | User: ${child.val().user}</p>
                            <p>Amt: ${child.val().amt} | Info: ${child.val().tid || child.val().wall}</p>
                            <button onclick="approveReq('${child.key}', '${child.val().user}', ${child.val().amt}, '${child.val().type}')" style="background:var(--success); color:white; border:none; padding:10px; border-radius:10px;">Approve</button>
                        </div>`;
                    }
                });
            });
        }

        function approveReq(key, uid, amt, type) {
            if(type === 'Deposit') {
                db.ref('users/'+uid).once('value', s => {
                    db.ref('users/'+uid).update({wallet: s.val().wallet + parseInt(amt)});
                });
            }
            db.ref('requests/'+key).update({status: 'Completed'});
            alert("Approved!");
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        }

        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
