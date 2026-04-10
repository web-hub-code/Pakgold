<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | The Gold Standard of Mining</title>
    <!-- Firebase & Core Scripts -->
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;500;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>

    <style>
        :root {
            --primary: #FFD700;
            --secondary: #1a1a2e;
            --accent: #f59e0b;
            --bg: #0f172a;
            --card: #1e293b;
            --text: #f8fafc;
            --success: #10b981;
            --error: #ef4444;
            --shadow: 0 15px 35px rgba(0,0,0,0.5);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Space Grotesk', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Premium Animations */
        .page { display: none; min-height: 100vh; padding-bottom: 110px; animation: fadeIn 0.4s ease; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: scale(0.98); } to { opacity: 1; transform: scale(1); } }

        /* Floating Header */
        .header { background: rgba(30, 41, 59, 0.8); backdrop-filter: blur(15px); padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; border-bottom: 1px solid rgba(255,215,0,0.1); }
        .logo { font-weight: 700; font-size: 1.5rem; letter-spacing: -1px; }
        .logo span { color: var(--primary); text-shadow: 0 0 10px rgba(255,215,0,0.3); }

        /* Cyberpunk Balance Card */
        .hero { background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%); margin: 20px; padding: 30px; border-radius: 30px; border: 1px solid rgba(255,215,0,0.2); position: relative; overflow: hidden; }
        .hero::after { content: ''; position: absolute; top: -50%; right: -50%; width: 200px; height: 200px; background: var(--primary); filter: blur(100px); opacity: 0.1; }
        .hero-balance { font-size: 2.8rem; font-weight: 700; margin: 10px 0; background: linear-gradient(to right, #fff, #FFD700); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        
        /* Glassmorphism Cards */
        .card { background: var(--card); margin: 15px; padding: 25px; border-radius: 25px; box-shadow: var(--shadow); border: 1px solid rgba(255,255,255,0.05); }
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 15px; }
        
        .node-card { 
            background: #1e293b; padding: 25px 15px; border-radius: 20px; text-align: center; 
            border: 1px solid rgba(255,255,255,0.05); transition: 0.3s; position: relative;
        }
        .node-card i { font-size: 2rem; color: var(--primary); margin-bottom: 10px; }
        .node-card.vip { grid-column: span 2; background: linear-gradient(90deg, #1e293b, #334155); border: 1px solid var(--primary); display: flex; justify-content: space-around; align-items: center; text-align: left; }

        /* Inputs & Buttons */
        input, select { width: 100%; padding: 18px; margin-bottom: 15px; border-radius: 15px; border: 1px solid #334155; background: #0f172a; color: white; font-size: 1rem; outline: none; }
        input:focus { border-color: var(--primary); }
        .btn-gold { background: linear-gradient(90deg, #FFD700, #b8860b); color: #000; width: 100%; padding: 18px; border-radius: 18px; border: none; font-weight: 700; cursor: pointer; transition: 0.3s; font-size: 1rem; }
        .btn-gold:active { transform: scale(0.95); }

        /* History Status Styles */
        .log-item { display: flex; justify-content: space-between; align-items: center; padding: 15px; border-bottom: 1px solid #334155; }
        .status-badge { font-size: 0.7rem; padding: 4px 10px; border-radius: 50px; text-transform: uppercase; font-weight: 700; }
        .pending { background: #4b5563; } .approved { background: var(--success); } .rejected { background: var(--error); }

        /* Navbar */
        .nav { position: fixed; bottom: 0; width: 100%; background: rgba(30,41,59,0.9); backdrop-filter: blur(20px); display: flex; justify-content: space-around; padding: 12px 0 25px; border-top: 1px solid rgba(255,255,255,0.1); z-index: 2000; }
        .nav-item { text-align: center; font-size: 0.75rem; color: #94a3b8; transition: 0.3s; cursor: pointer; }
        .nav-item.active { color: var(--primary); transform: translateY(-5px); }
        .nav-item i { font-size: 1.5rem; margin-bottom: 4px; display: block; }
    </style>
</head>
<body onload="init()">

    <!-- AUTH PAGE -->
    <section id="login" class="page active">
        <div style="padding: 50px 30px; text-align: center;">
            <div class="animate__animated animate__pulse animate__infinite">
                <i class="fa-solid fa-gem" style="font-size: 4rem; color: var(--primary); margin-bottom: 20px;"></i>
            </div>
            <h1 class="logo" style="font-size: 3rem;">PAK<span>GOLD</span></h1>
            <p style="color: #94a3b8; margin-bottom: 40px;">Professional Mining Ecosystem</p>
            
            <input type="number" id="ph" placeholder="Phone Number">
            <input type="password" id="pass" placeholder="Account Password">
            <input type="text" id="ref_id" placeholder="Invite Code (Optional)">
            <input type="password" id="adm_key" placeholder="Admin Secret Key">
            
            <button class="btn-gold" onclick="handleAuth()">INITIALIZE ACCESS</button>
            <p style="margin-top: 20px; font-size: 0.8rem; opacity: 0.6;">Secure 256-bit Encrypted Connection</p>
        </div>
    </section>

    <!-- DASHBOARD -->
    <section id="home" class="page">
        <div class="header">
            <div class="logo">PAK<span>GOLD</span></div>
            <div id="u_id_tag" style="background: rgba(255,215,0,0.1); color: var(--primary); padding: 5px 15px; border-radius: 50px; font-size: 0.8rem; font-weight: 700;">ID: ---</div>
        </div>

        <div class="hero">
            <div style="display: flex; justify-content: space-between; align-items: flex-start;">
                <div>
                    <small style="opacity: 0.6; font-weight: 500;">TOTAL LIQUIDITY</small>
                    <div class="hero-balance">Rs <span id="u_wallet">0.00</span></div>
                </div>
                <i class="fa-solid fa-shield-check" style="color: var(--success); font-size: 1.5rem;"></i>
            </div>
            <div style="display: flex; justify-content: space-between; margin-top: 20px; background: rgba(0,0,0,0.2); padding: 15px; border-radius: 15px;">
                <div><small style="color: var(--primary);">Mining Rate</small><br><b id="u_speed">0.00/s</b></div>
                <div style="text-align: right;"><small style="color: var(--success);">Daily Profit</small><br><b id="u_profit">0.0000</b></div>
            </div>
        </div>

        <div style="padding: 0 20px; margin: 20px 0;">
            <button class="btn-gold" style="background: #25d366; color: white;" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')">
                <i class="fab fa-whatsapp"></i> JOIN OFFICIAL COMMUNITY
            </button>
        </div>

        <h3 style="padding: 0 20px; margin-bottom: 15px;">Standard Nodes <small style="color: var(--primary);">(Start Rs 200)</small></h3>
        <div class="grid" id="node_grid"></div>

        <h3 style="padding: 0 20px; margin: 25px 0 15px;">VIP Enterprise Nodes</h3>
        <div class="grid" id="vip_grid"></div>
    </section>

    <!-- DEPOSIT PAGE -->
    <section id="deposit" class="page">
        <div class="card">
            <h2 style="color: var(--primary); margin-bottom: 10px;">Deposit Capital</h2>
            <p style="font-size: 0.9rem; opacity: 0.8; margin-bottom: 20px;">Choose a gateway and upload proof after payment.</p>
            
            <div style="background: rgba(255,255,255,0.05); padding: 20px; border-radius: 15px; margin-bottom: 20px;">
                <div style="margin-bottom: 10px;"><b>EasyPaisa:</b> <span style="color: var(--primary);">03379827882</span></div>
                <div><b>JazzCash/SADA:</b> <span style="color: var(--primary);">03705519562</span></div>
            </div>

            <input type="number" id="dep_amt" placeholder="Amount (Min Rs 200)">
            <input type="text" id="dep_tid" placeholder="Transaction ID (TID)">
            <p style="font-size: 0.75rem; color: var(--accent); margin-bottom: 15px;"><i class="fa-solid fa-circle-info"></i> Screenshot upload is automatic via TID verification.</p>
            <button class="btn-gold" onclick="requestDeposit()">VERIFY ASSETS</button>
        </div>
    </section>

    <!-- WITHDRAW PAGE -->
    <section id="withdraw" class="page">
        <div class="card">
            <h2 style="color: var(--primary); margin-bottom: 20px;">Payout Request</h2>
            <select id="wit_met">
                <option value="EasyPaisa">EasyPaisa</option>
                <option value="JazzCash">JazzCash</option>
                <option value="Bank Transfer">Bank Transfer</option>
            </select>
            <input type="text" id="wit_acc" placeholder="Account Number">
            <input type="text" id="wit_nam" placeholder="Account Title">
            <input type="number" id="wit_amt" placeholder="Amount (Min Rs 500)">
            <button class="btn-gold" onclick="requestWithdraw()">PROCESS PAYOUT</button>
        </div>
    </section>

    <!-- LOGS / HISTORY PAGE -->
    <section id="history" class="page">
        <div style="padding: 20px;">
            <h2 style="margin-bottom: 20px;">Activity Logs</h2>
            <div id="logs_container" class="card" style="padding: 0;"></div>
        </div>
    </section>

    <!-- ADMIN MASTER PANEL -->
    <section id="admin" class="page" style="background: #000;">
        <div style="padding: 20px;">
            <h1 style="color: var(--primary); margin-bottom: 20px;">MASTER CONTROL</h1>
            <div id="admin_queue"></div>
            
            <div class="card" style="background: #111; margin: 30px 0;">
                <h3>User God-Mode</h3>
                <input type="number" id="adm_target" placeholder="User Phone">
                <input type="number" id="adm_val" placeholder="Add/Remove Balance">
                <button class="btn-gold" onclick="admAdjust()">SYNC DATA</button>
            </div>
        </div>
    </section>

    <!-- NAVIGATION BAR -->
    <nav class="nav" id="main_nav" style="display: none;">
        <div class="nav-item active" onclick="nav('home')"><i class="fa-solid fa-bolt"></i>Mining</div>
        <div class="nav-item" onclick="nav('deposit')"><i class="fa-solid fa-plus-circle"></i>Deposit</div>
        <div class="nav-item" onclick="nav('withdraw')"><i class="fa-solid fa-arrow-up-right-from-square"></i>Payout</div>
        <div class="nav-item" onclick="nav('history')"><i class="fa-solid fa-receipt"></i>History</div>
    </nav>

    <script>
        // --- CORE INITIALIZATION ---
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
        let currentU = localStorage.getItem('pg_session');

        function init() {
            if(currentU) { nav('home'); syncData(); }
            loadPlans();
        }

        // --- AUTHENTICATION ---
        function handleAuth() {
            let p = document.getElementById('ph').value;
            let ak = document.getElementById('adm_key').value;
            let ref = document.getElementById('ref_id').value;

            if(ak === "pakgold786") { nav('admin'); loadAdminQueue(); return; }
            if(p.length < 10) return alert("Sweetie, enter a valid number!");

            currentU = p;
            localStorage.setItem('pg_session', p);
            db.ref('users/'+p).once('value', s => {
                if(!s.exists()) {
                    db.ref('users/'+p).set({
                        wallet: 0, profit: 0, speed: 0, 
                        uid: 'PG-'+Math.floor(Math.random()*9999),
                        inviter: ref || 'System'
                    });
                }
                location.reload();
            });
        }

        // --- DATA SYNC & MINING ---
        function syncData() {
            db.ref('users/'+currentU).on('value', s => {
                let d = s.val();
                document.getElementById('u_wallet').innerText = d.wallet.toLocaleString();
                document.getElementById('u_profit').innerText = parseFloat(d.profit).toFixed(5);
                document.getElementById('u_speed').innerText = d.speed.toFixed(5) + "/s";
                document.getElementById('u_id_tag').innerText = "ID: " + d.uid;
            });

            // Live Mining Heartbeat (Every Second)
            setInterval(() => {
                db.ref('users/'+currentU).once('value', s => {
                    let d = s.val();
                    if(d.speed > 0) db.ref('users/'+currentU).update({profit: parseFloat(d.profit) + d.speed});
                });
            }, 1000);

            // History Sync
            db.ref('master_logs').on('value', s => {
                let container = document.getElementById('logs_container');
                container.innerHTML = "";
                s.forEach(child => {
                    let log = child.val();
                    if(log.user === currentU) {
                        container.innerHTML += `
                            <div class="log-item">
                                <div><b>${log.type}</b><br><small style="opacity:0.5;">${log.tid || 'Payout'}</small></div>
                                <div style="text-align:right;">
                                    <b>Rs ${log.amt}</b><br>
                                    <span class="status-badge ${log.status}">${log.status}</span>
                                </div>
                            </div>`;
                    }
                });
            });
        }

        // --- PLAN GENERATION (15+5) ---
        function loadPlans() {
            const nodes = document.getElementById('node_grid');
            const vips = document.getElementById('vip_grid');
            
            // 15 Standard Plans (Starting 200)
            for(let i=1; i<=15; i++) {
                let price = 200 * i;
                let speed = 0.0001 * i;
                nodes.innerHTML += `
                    <div class="node-card" onclick="purchaseNode(${price}, ${speed})">
                        <i class="fa-solid fa-microchip"></i><br>
                        <b>Node Lvl ${i}</b><br>
                        <small style="color:var(--primary);">Rs ${price}</small>
                    </div>`;
            }

            // 5 VIP Plans
            const vipData = [
                {n: 'TITAN 1', p: 50000, s: 0.05}, {n: 'TITAN 2', p: 100000, s: 0.12},
                {n: 'GALAXY', p: 250000, s: 0.35}, {n: 'ORACLE', p: 500000, s: 0.8},
                {n: 'GOD-LEVEL', p: 1000000, s: 2.0}
            ];
            vipData.forEach(v => {
                vips.innerHTML += `
                    <div class="node-card vip" onclick="purchaseNode(${v.p}, ${v.s})">
                        <i class="fa-solid fa-crown"></i>
                        <div><b>${v.n} Node</b><br><small>Limitless Hashrate</small></div>
                        <b style="color:var(--primary);">Rs ${v.p}</b>
                    </div>`;
            });
        }

        function purchaseNode(cost, speed) {
            db.ref('users/'+currentU).once('value', s => {
                if(s.val().wallet < cost) return alert("Sweetie, low balance! Please deposit first.");
                db.ref('users/'+currentU).update({
                    wallet: s.val().wallet - cost,
                    speed: s.val().speed + speed
                });
                alert("Node Deployed! Mining speed increased.");
            });
        }

        // --- FINANCE FUNCTIONS ---
        function requestDeposit() {
            let a = document.getElementById('dep_amt').value;
            let t = document.getElementById('dep_tid').value;
            if(a < 200 || !t) return alert("Minimum Rs 200 and TID required!");
            db.ref('master_logs').push({
                user: currentU, amt: a, tid: t, type: 'Deposit', status: 'pending'
            });
            alert("Sent! Verification usually takes 10-30 mins.");
        }

        function requestWithdraw() {
            let a = document.getElementById('wit_amt').value;
            if(a < 500) return alert("Minimum payout is Rs 500!");
            db.ref('users/'+currentU).once('value', s => {
                if(s.val().wallet < a) return alert("Insufficient Balance!");
                db.ref('users/'+currentU).update({wallet: s.val().wallet - a});
                db.ref('master_logs').push({
                    user: currentU, amt: a, type: 'Withdrawal', status: 'pending', 
                    method: document.getElementById('wit_met').value,
                    account: document.getElementById('wit_acc').value
                });
                alert("Withdrawal request queued!");
            });
        }

        // --- ADMIN POWERS ---
        function loadAdminQueue() {
            db.ref('master_logs').on('value', s => {
                let q = document.getElementById('admin_queue');
                q.innerHTML = "";
                s.forEach(child => {
                    let r = child.val();
                    if(r.status === 'pending') {
                        q.innerHTML += `
                            <div class="card" style="background: #111;">
                                <b>${r.type} Request</b><br>
                                User: ${r.user} | Rs ${r.amt}<br>
                                Detail: ${r.tid || r.account}<br><br>
                                <button class="btn-gold" style="width: 45%;" onclick="admApprove('${child.key}', '${r.user}', ${r.amt}, '${r.type}')">APPROVE</button>
                                <button class="btn-gold" style="width: 45%; background: var(--error); color: white;" onclick="admReject('${child.key}')">REJECT</button>
                            </div>`;
                    }
                });
            });
        }

        function admApprove(key, user, amt, type) {
            if(type === 'Deposit') {
                db.ref('users/'+user).once('value', s => {
                    let d = s.val();
                    db.ref('users/'+user).update({wallet: d.wallet + parseInt(amt)});
                    // 10% Referral Bonus
                    if(d.inviter && d.inviter !== 'System') {
                        db.ref('users/'+d.inviter).once('value', inv => {
                            if(inv.exists()) db.ref('users/'+d.inviter).update({wallet: inv.val().wallet + (amt*0.1)});
                        });
                    }
                });
            }
            db.ref('master_logs/'+key).update({status: 'approved'});
        }

        function admReject(key) { db.ref('master_logs/'+key).update({status: 'rejected'}); }

        function admAdjust() {
            let target = document.getElementById('adm_target').value;
            let val = document.getElementById('adm_val').value;
            db.ref('users/'+target).once('value', s => {
                if(!s.exists()) return alert("User Not Found!");
                db.ref('users/'+target).update({wallet: s.val().wallet + parseInt(val)});
                alert("God-Mode: Balance Adjusted!");
            });
        }

        // --- NAVIGATION ---
        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('main_nav').style.display = (id === 'login' || id === 'admin') ? 'none' : 'flex';
            window.scrollTo(0,0);
        }
    </script>
</body>
</html>
