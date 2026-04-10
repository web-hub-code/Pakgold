<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Professional Mining Platform</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>

    <style>
        :root {
            --gold: #c5a059; --dark: #0f172a; --light: #f1f5f9; --white: #ffffff;
            --success: #10b981; --shadow: 0 10px 30px rgba(0,0,0,0.08);
        }
        [data-theme='dark'] { --light: #0f172a; --white: #1e293b; --dark: #f8fafc; }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--light); color: var(--dark); overflow-x: hidden; }

        .page { display: none; min-height: 100vh; padding-bottom: 100px; animation: fadeIn 0.4s ease; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* Header & Cards */
        .header { background: var(--white); padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; box-shadow: var(--shadow); }
        .card { background: var(--white); margin: 15px; padding: 20px; border-radius: 20px; box-shadow: var(--shadow); }
        
        /* Stats Dashboard */
        .stat-card { background: linear-gradient(135deg, #c5a059, #9c7d42); color: white; margin: 15px; padding: 25px; border-radius: 25px; position: relative; overflow: hidden; }
        
        /* Action Buttons */
        .action-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 15px; }
        .act-btn { background: var(--white); padding: 15px; border-radius: 15px; text-align: center; font-weight: 600; cursor: pointer; box-shadow: var(--shadow); }
        .act-btn i { color: var(--gold); font-size: 1.5rem; margin-bottom: 5px; }

        /* Inputs & Selection */
        input, select { width: 100%; padding: 15px; margin-bottom: 12px; border-radius: 12px; border: 1px solid #ddd; background: var(--white); color: var(--dark); font-size: 1rem; outline: none; }
        .btn-gold { background: var(--gold); color: white; border: none; padding: 16px; border-radius: 12px; width: 100%; font-weight: 700; cursor: pointer; }
        
        /* Node Cards */
        .node { background: var(--white); margin: 15px; padding: 15px; border-radius: 20px; display: flex; align-items: center; gap: 15px; border-left: 5px solid var(--gold); }
        
        /* Nav */
        .nav { position: fixed; bottom: 0; width: 100%; background: var(--white); display: flex; justify-content: space-around; padding: 12px 0 25px; border-top: 1px solid #eee; }
        .nav-item { color: #94a3b8; text-align: center; font-size: 0.75rem; flex: 1; }
        .nav-item.active { color: var(--gold); }
        .nav-item i { font-size: 1.4rem; display: block; }
    </style>
</head>
<body onload="init()">

    <!-- 1. AUTH PAGE -->
    <section id="login" class="page active">
        <div style="padding: 50px 30px; text-align: center;">
            <h1 style="color:var(--gold); font-size: 2.5rem; font-weight:800;">PAK GOLD</h1>
            <p style="opacity:0.6; margin-bottom:30px;">Professional Mining Ecosystem</p>
            <input type="number" id="ph" placeholder="Phone Number">
            <input type="password" id="pass" placeholder="Password">
            <input type="password" id="adm_key" placeholder="Admin Terminal PIN">
            <button class="btn-gold" onclick="handleAuth()">SECURE LOGIN</button>
        </div>
    </section>

    <!-- 2. HOME DASHBOARD -->
    <section id="home" class="page">
        <div class="header">
            <b style="color:var(--gold)">PAK GOLD PRO</b>
            <i class="fa-solid fa-bell"></i>
        </div>
        <div class="stat-card">
            <small>Current Balance</small>
            <h2 style="font-size: 2.2rem;">Rs <span id="u_wallet">0.00</span></h2>
            <div style="display:flex; justify-content:space-between; margin-top:15px; background:rgba(255,255,255,0.1); padding:10px; border-radius:10px;">
                <span>Profit: <b id="u_profit">0.00</b></span>
                <span>Speed: <b id="u_speed">0.00</b></span>
            </div>
        </div>

        <div class="action-row">
            <div class="act-btn" onclick="nav('deposit')"><i class="fa-solid fa-plus-circle"></i><br>Deposit</div>
            <div class="act-btn" onclick="nav('withdraw')"><i class="fa-solid fa-arrow-up-right-from-square"></i><br>Withdraw</div>
        </div>

        <div style="padding: 20px 15px 5px;"><h3>Premium Nodes</h3></div>
        <div id="node_container"></div>
    </section>

    <!-- 3. DEPOSIT SYSTEM -->
    <section id="deposit" class="page">
        <div class="header"><i class="fa-solid fa-arrow-left" onclick="nav('home')"></i> <b>Deposit Funds</b> <div></div></div>
        <div class="card">
            <label>Select Method:</label>
            <select id="dep_method" onchange="updateDepInfo()">
                <option value="JazzCash">JazzCash / SadaPay</option>
                <option value="EasyPaisa">EasyPaisa</option>
            </select>
            
            <div id="acc_info" style="background:var(--light); padding:15px; border-radius:12px; margin-bottom:15px; border:1px dashed var(--gold);">
                <small id="meth_name">JazzCash / SadaPay</small>
                <h3 id="meth_num">03705519562</h3>
                <small>Account Title: <b>PakGold Official</b></small>
            </div>

            <input type="number" id="dep_amt" placeholder="Enter Amount (Rs)">
            <input type="text" id="dep_tid" placeholder="Transaction ID (TID)">
            <p style="font-size:0.7rem; margin-bottom:5px; color:var(--gold);">Upload Payment Proof (Screenshot):</p>
            <input type="file" id="dep_proof" accept="image/*" style="padding:10px;">
            
            <button class="btn-gold" onclick="submitDep()">CONFIRM DEPOSIT</button>
        </div>
    </section>

    <!-- 4. WITHDRAW SYSTEM -->
    <section id="withdraw" class="page">
        <div class="header"><i class="fa-solid fa-arrow-left" onclick="nav('home')"></i> <b>Withdraw Profits</b> <div></div></div>
        <div class="card">
            <select id="wd_method">
                <option>EasyPaisa</option>
                <option>JazzCash</option>
                <option>SadaPay</option>
                <option>Bank Transfer</option>
                <option>Binance (USDT)</option>
            </select>
            <input type="number" id="wd_amt" placeholder="Amount to Withdraw">
            <input type="text" id="wd_acc" placeholder="Account Number / Wallet Address">
            <input type="text" id="wd_name" placeholder="Account Holder Name">
            <button class="btn-gold" style="background:#0f172a;" onclick="submitWd()">REQUEST WITHDRAWAL</button>
            <p style="font-size:0.7rem; text-align:center; margin-top:10px;">Withdrawals are processed within 24 hours.</p>
        </div>
    </section>

    <!-- 5. ADMIN CONTROL PANEL -->
    <section id="admin" class="page" style="background:#000; color:#0f0; padding:20px;">
        <h2 style="color:var(--gold)">MASTER CONTROL</h2>
        <hr style="border:0.5px solid #333; margin:15px 0;">
        
        <h4>Pending Requests</h4>
        <div id="adm_requests"></div>

        <div style="background:#111; padding:15px; border-radius:15px; margin-top:20px; border:1px solid #333;">
            <p>Manual Balance Update</p>
            <input type="number" id="adm_target" placeholder="User Phone" style="background:#000; border-color:#333; color:#fff;">
            <input type="number" id="adm_val" placeholder="Amount (+/-)" style="background:#000; border-color:#333; color:#fff;">
            <button class="btn-gold" onclick="admSync()">UPDATE DATABASE</button>
        </div>
    </section>

    <!-- NAV BAR -->
    <nav class="nav" id="bot_nav" style="display:none;">
        <div class="nav-item active" onclick="nav('home')"><i class="fa-solid fa-house"></i>Home</div>
        <div class="nav-item" onclick="nav('nodes')"><i class="fa-solid fa-microchip"></i>Nodes</div>
        <div class="nav-item" onclick="nav('team')"><i class="fa-solid fa-users"></i>Team</div>
        <div class="nav-item" onclick="openAdmin()"><i class="fa-solid fa-shield-halved"></i>Admin</div>
    </nav>

    <script>
        // --- Firebase Config ---
        const fbConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com",
            projectId: "dark-web-9",
        };
        firebase.initializeApp(fbConfig);
        const db = firebase.database();
        let user = localStorage.getItem('pg_session');

        function init() {
            if(user) { nav('home'); sync(); }
            loadNodes();
        }

        function handleAuth() {
            let p = document.getElementById('ph').value;
            let a = document.getElementById('adm_key').value;
            if(a === "pakgold786") { nav('admin'); loadAdmin(); return; }
            if(p.length < 10) return alert("Sweetie, enter valid number!");
            user = p; localStorage.setItem('pg_session', p);
            db.ref('users/'+p).once('value', s => {
                if(!s.exists()) db.ref('users/'+p).set({wallet:0, profit:0, speed:0, team:[]});
                location.reload();
            });
        }

        function sync() {
            db.ref('users/'+user).on('value', s => {
                let d = s.val();
                document.getElementById('u_wallet').innerText = d.wallet.toLocaleString();
                document.getElementById('u_profit').innerText = parseFloat(d.profit).toFixed(2);
                document.getElementById('u_speed').innerText = d.speed.toFixed(4);
            });
            // Auto Mining logic
            setInterval(() => {
                db.ref('users/'+user).once('value', s => {
                    let d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed});
                });
            }, 1000);
        }

        function loadNodes() {
            let c = document.getElementById('node_container');
            const nodes = [
                {lvl:1, price:200, daily:20, life:30},
                {lvl:2, price:1000, daily:110, life:30},
                {lvl:3, price:5000, daily:600, life:40}
            ];
            nodes.forEach(n => {
                c.innerHTML += `
                <div class="node">
                    <div style="background:var(--gold); color:white; padding:10px; border-radius:12px;"><i class="fa-solid fa-server"></i></div>
                    <div style="flex:1;">
                        <b>Level ${n.lvl} Mining Node</b><br>
                        <small>Daily: Rs ${n.daily} | Total: ${n.daily*n.life}</small>
                    </div>
                    <button onclick="buyNode(${n.price}, ${n.daily/86400})" style="border:none; background:var(--dark); color:white; padding:8px 15px; border-radius:8px;">Rs ${n.price}</button>
                </div>`;
            });
        }

        function buyNode(p, s) {
            db.ref('users/'+user).once('value', v => {
                if(v.val().wallet < p) return alert("Sweetie, balance low!");
                db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s});
                alert("Node Activated!");
            });
        }

        function updateDepInfo() {
            let m = document.getElementById('dep_method').value;
            document.getElementById('meth_name').innerText = m === 'EasyPaisa' ? 'EasyPaisa Account' : 'JazzCash / SadaPay';
            document.getElementById('meth_num').innerText = m === 'EasyPaisa' ? '03379827882' : '03705519562';
        }

        function submitDep() {
            let a = document.getElementById('dep_amt').value;
            let t = document.getElementById('dep_tid').value;
            if(!a || !t) return alert("Fill all fields!");
            db.ref('admin_req').push({user:user, type:'Deposit', amt:a, tid:t, status:'pending'});
            alert("Request Sent! Wait for approval."); nav('home');
        }

        function submitWd() {
            let a = document.getElementById('wd_amt').value;
            let ac = document.getElementById('wd_acc').value;
            db.ref('users/'+user).once('value', s => {
                if(s.val().wallet < a) return alert("Insufficient Balance!");
                db.ref('admin_req').push({user:user, type:'Withdraw', amt:a, acc:ac, status:'pending'});
                db.ref('users/'+user).update({wallet: s.val().wallet - a});
                alert("Withdrawal Requested!"); nav('home');
            });
        }

        function openAdmin() {
            let k = prompt("Enter Master PIN:");
            if(k === "pakgold786") { nav('admin'); loadAdmin(); }
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('bot_nav').style.display = (id === 'login' || id === 'admin') ? 'none' : 'flex';
        }

        // --- Admin Ops ---
        function loadAdmin() {
            db.ref('admin_req').on('value', s => {
                let q = document.getElementById('adm_requests'); q.innerHTML = "";
                s.forEach(c => {
                    let r = c.val(); if(r.status === 'pending') {
                        q.innerHTML += `
                        <div style="background:#111; padding:10px; margin-top:10px; border:1px solid #444;">
                            ${r.type} | User: ${r.user} | Rs ${r.amt}<br>
                            <small>TID: ${r.tid || 'N/A'}</small><br>
                            <button onclick="approveReq('${c.key}','${r.user}',${r.amt},'${r.type}')" style="color:green">APPROVE</button>
                        </div>`;
                    }
                });
            });
        }

        function approveReq(key, u, a, t) {
            if(t === 'Deposit') {
                db.ref('users/'+u).once('value', s => {
                    db.ref('users/'+u).update({wallet: s.val().wallet + parseInt(a)});
                });
            }
            db.ref('admin_req/'+key).update({status:'approved'});
            alert("Done!");
        }

        function admSync() {
            let t = document.getElementById('adm_target').value;
            let v = document.getElementById('adm_val').value;
            db.ref('users/'+t).once('value', s => {
                db.ref('users/'+t).update({wallet: s.val().wallet + parseInt(v)});
                alert("Database Updated!");
            });
        }
    </script>
</body>
</html>
