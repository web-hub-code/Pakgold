<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Elite Investment Platform</title>
    <!-- Firebase -->
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <!-- Icons & Fonts -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>

    <style>
        :root {
            --primary: #c5a059;
            --secondary: #0f172a;
            --accent: #f59e0b;
            --bg: #f8fafc;
            --card: #ffffff;
            --success: #10b981;
            --error: #ef4444;
            --wa-green: #25d366;
            --shadow: 0 20px 40px rgba(0,0,0,0.06);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: #1e293b; overflow-x: hidden; }

        /* Page Transitions */
        .page { display: none; min-height: 100vh; padding-bottom: 110px; animation: fadeInUp 0.6s cubic-bezier(0.2, 0.8, 0.2, 1); }
        .page.active { display: block; }
        @keyframes fadeInUp { from { opacity: 0; transform: translateY(40px); } to { opacity: 1; transform: translateY(0); } }

        /* Header */
        .header { background: #fff; padding: 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #f1f5f9; position: sticky; top: 0; z-index: 1000; box-shadow: 0 2px 10px rgba(0,0,0,0.02); }
        .logo { font-weight: 900; font-size: 1.5rem; color: var(--secondary); letter-spacing: -1px; }
        .logo span { color: var(--primary); }

        /* Hero Balance Card */
        .hero { background: var(--card); margin: 20px; padding: 30px; border-radius: 35px; border: 1px solid #f1f5f9; box-shadow: var(--shadow); position: relative; overflow: hidden; }
        .hero::before { content: ''; position: absolute; top: -30px; right: -30px; width: 120px; height: 120px; background: var(--primary); opacity: 0.05; border-radius: 50%; }
        .hero-label { font-size: 0.75rem; font-weight: 700; color: #94a3b8; text-transform: uppercase; letter-spacing: 1px; }
        .hero-balance { font-size: 3rem; font-weight: 900; color: var(--secondary); margin: 8px 0; }
        .hero-stats { display: flex; justify-content: space-between; margin-top: 25px; padding-top: 20px; border-top: 1px solid #f1f5f9; }
        .hero-stats b { display: block; font-size: 1.1rem; color: var(--secondary); }

        /* Modern Grid & Cards */
        .section-title { padding: 0 25px; margin: 30px 0 15px; font-weight: 900; font-size: 1.2rem; display: flex; justify-content: space-between; align-items: center; }
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; padding: 0 20px; }
        .plan-card { background: #fff; padding: 25px 15px; border-radius: 25px; border: 1px solid #f1f5f9; text-align: center; transition: 0.4s; position: relative; }
        .plan-card:hover { border-color: var(--primary); transform: translateY(-5px); box-shadow: 0 10px 20px rgba(197, 160, 89, 0.1); }
        .plan-card.vip { grid-column: span 2; background: var(--secondary); color: #fff; display: flex; justify-content: space-between; align-items: center; padding: 25px 30px; text-align: left; }
        .plan-card.vip h4 { color: var(--primary); font-size: 1.4rem; }

        /* Buttons & Forms */
        .btn-wa { background: var(--wa-green); color: #fff; padding: 18px; border-radius: 20px; text-decoration: none; display: flex; align-items: center; justify-content: center; gap: 10px; font-weight: 800; margin: 20px; box-shadow: 0 10px 25px rgba(37, 211, 102, 0.25); }
        .input-group { background: #fff; margin: 20px; padding: 25px; border-radius: 30px; border: 1px solid #f1f5f9; box-shadow: var(--shadow); }
        input, select { width: 100%; padding: 18px; margin-bottom: 15px; border: 1px solid #e2e8f0; border-radius: 15px; font-size: 1rem; outline: none; background: #f8fafc; transition: 0.3s; }
        input:focus { border-color: var(--primary); background: #fff; }
        .btn-gold { background: var(--secondary); color: #fff; border: none; padding: 20px; width: 100%; border-radius: 18px; font-weight: 900; cursor: pointer; font-size: 1.1rem; box-shadow: 0 10px 20px rgba(0,0,0,0.1); }
        .btn-gold:active { transform: scale(0.96); }

        /* Bottom Navigation */
        .nav { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.95); backdrop-filter: blur(20px); display: flex; justify-content: space-around; padding: 15px 0 25px; border-top: 1px solid #f1f5f9; z-index: 2000; }
        .nav-item { text-align: center; color: #94a3b8; font-size: 0.75rem; font-weight: 700; flex: 1; }
        .nav-item.active { color: var(--primary); }
        .nav-item i { font-size: 1.5rem; margin-bottom: 5px; display: block; }

        /* History Items */
        .log-card { background: #fff; padding: 20px; border-radius: 20px; border: 1px solid #f1f5f9; margin-bottom: 15px; display: flex; justify-content: space-between; align-items: center; }
        .status { font-size: 0.7rem; padding: 5px 12px; border-radius: 50px; font-weight: 800; text-transform: uppercase; }
        .pending { background: #fff7ed; color: #c2410c; }
        .approved { background: #ecfdf5; color: #047857; }

        /* Admin Terminal */
        .admin-item { background: #fff; padding: 20px; border-radius: 20px; margin-bottom: 15px; border-left: 5px solid var(--primary); }
    </style>
</head>
<body onload="init()">

    <!-- Login/Signup -->
    <section id="login" class="page active">
        <div style="height:100vh; display:flex; flex-direction:column; justify-content:center; padding:40px;">
            <div style="text-align:center; margin-bottom:60px;">
                <h1 class="logo" style="font-size:3rem;">PAK<span>GOLD</span></h1>
                <p style="color:#64748b; font-weight:600;">Secure Digital Mining Infrastructure</p>
            </div>
            <input type="number" id="ph" placeholder="Mobile Number">
            <input type="text" id="ref_id" placeholder="Referral ID (Optional)">
            <input type="password" id="adm_key" placeholder="Admin Secret Pin">
            <button class="btn-gold" onclick="handleAuth()">Initialize Dashboard</button>
            <a href="https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3" class="btn-wa" style="margin: 20px 0 0;"><i class="fab fa-whatsapp"></i> Public Group</a>
        </div>
    </section>

    <!-- Dashboard -->
    <section id="home" class="page">
        <div class="header">
            <div class="logo">PAK<span>GOLD</span></div>
            <div id="disp_id" style="font-weight:900; font-size:0.8rem; background:#f1f5f9; padding:8px 15px; border-radius:50px; color:var(--primary);">ID: ---</div>
        </div>

        <div class="hero">
            <span class="hero-label">Live Cloud Revenue</span>
            <div class="hero-balance" id="disp_profit">0.0000</div>
            <div class="hero-stats">
                <div><small class="hero-label">Available</small><b id="disp_wallet">Rs 0</b></div>
                <div style="text-align:right;"><small class="hero-label">Hash Rate</small><b id="disp_speed" style="color:var(--success);">0.0000/s</b></div>
            </div>
        </div>

        <a href="https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3" class="btn-wa"><i class="fab fa-whatsapp"></i> Join Official Community</a>

        <div class="section-title">Standard Nodes <span>15 Servers</span></div>
        <div class="grid" id="std_grid"></div>

        <div class="section-title">VIP Cloud Infrastructure <span>Enterprise</span></div>
        <div class="grid" id="vip_grid" style="padding-bottom:30px;"></div>
    </section>

    <!-- History -->
    <section id="history" class="page">
        <div style="padding:25px;">
            <h2 style="font-weight:900; margin-bottom:25px;">Account Activity</h2>
            <div id="hist_container">
                <p style="text-align:center; color:#94a3b8;">Scanning transactions...</p>
            </div>
        </div>
    </section>

    <!-- Team -->
    <section id="team" class="page">
        <div style="padding:25px;">
            <h2 style="font-weight:900; margin-bottom:25px;">My Network</h2>
            <div class="input-group" style="text-align:center; border: 2px dashed var(--primary);">
                <p style="font-weight:700; color:#64748b; margin-bottom:10px;">Referral ID</p>
                <h3 id="my_ref" style="font-size:2rem; color:var(--secondary); letter-spacing:3px; margin-bottom:20px;">---</h3>
                <button class="btn-gold" onclick="copyRef()">Copy Invite Link</button>
            </div>
            <h3 style="font-weight:900; margin:30px 0 15px;">Active Downlines</h3>
            <div id="team_container"></div>
        </div>
    </section>

    <!-- Deposit -->
    <section id="deposit" class="page">
        <div class="input-group">
            <h2 style="font-weight:900; margin-bottom:20px;">Deposit Capital</h2>
            <div style="background:#fffbeb; border-left:5px solid var(--accent); padding:20px; border-radius:15px; margin-bottom:25px;">
                <p style="font-size:0.9rem; font-weight:600;">JazzCash/SADA: 03705519562</p>
                <p style="font-size:0.9rem; font-weight:600;">EasyPaisa: 03379827882</p>
                <small style="color:#d97706;">*Send amount and enter TID below</small>
            </div>
            <input type="number" id="dep_amt" placeholder="Amount (PKR)">
            <input type="text" id="dep_tid" placeholder="Transaction ID (TID)">
            <button class="btn-gold" onclick="sendDeposit()">Verify Assets</button>
        </div>
    </section>

    <!-- Withdraw -->
    <section id="withdraw" class="page">
        <div class="input-group">
            <h2 style="font-weight:900; margin-bottom:20px;">Withdrawal</h2>
            <select id="wit_met">
                <option value="JazzCash">JazzCash</option>
                <option value="EasyPaisa">EasyPaisa</option>
                <option value="Bank Transfer">Bank Transfer</option>
                <option value="Binance (USDT)">Binance (USDT)</option>
            </select>
            <input type="text" id="wit_acc" placeholder="Account Number / Wallet">
            <input type="text" id="wit_nam" placeholder="Account Title">
            <input type="number" id="wit_val" placeholder="Amount (Min 500)">
            <button class="btn-gold" onclick="sendWithdraw()" style="background:var(--primary);">Process Payout</button>
        </div>
    </section>

    <!-- Admin Master Section -->
    <section id="admin" class="page" style="background:#f1f5f9; padding:20px;">
        <h2 style="font-weight:900; margin-bottom:25px;">Admin Master Control</h2>
        <div id="adm_queue"></div>
    </section>

    <!-- Nav Bar -->
    <nav class="nav" id="main_nav" style="display:none;">
        <div class="nav-item active" onclick="nav('home')"><i class="fa-solid fa-house-chimney"></i>Home</div>
        <div class="nav-item" onclick="nav('deposit')"><i class="fa-solid fa-circle-plus"></i>Deposit</div>
        <div class="nav-item" onclick="nav('history')"><i class="fa-solid fa-clock-rotate-left"></i>History</div>
        <div class="nav-item" onclick="nav('team')"><i class="fa-solid fa-users-viewfinder"></i>Team</div>
        <div class="nav-item" onclick="nav('withdraw')"><i class="fa-solid fa-money-bill-transfer"></i>Payout</div>
    </nav>

    <script>
        // --- Firebase Config ---
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
        let currentU = localStorage.getItem('pg_user');

        function init() {
            if(currentU) { nav('home'); syncData(); }
            loadPlanCards();
        }

        function handleAuth() {
            let p = document.getElementById('ph').value;
            let r = document.getElementById('ref_id').value;
            let a = document.getElementById('adm_key').value;
            if(a === "pakgold786") return nav('admin'), loadAdminTerminal();
            if(p.length < 10) return alert("System Error: Use valid mobile number.");
            
            currentU = p;
            localStorage.setItem('pg_user', p);
            db.ref('users/'+p).once('value', s => {
                if(!s.exists()) {
                    db.ref('users/'+p).set({
                        wallet:0, profit:0, speed:0, 
                        uid:'PG-'+Math.floor(1000+Math.random()*9000),
                        inviter: r || 'System'
                    });
                    if(r) db.ref('users/'+r+'/team').push(p);
                }
                location.reload();
            });
        }

        function syncData() {
            db.ref('users/'+currentU).on('value', s => {
                let d = s.val();
                document.getElementById('disp_wallet').innerText = "Rs "+d.wallet;
                document.getElementById('disp_profit').innerText = parseFloat(d.profit).toFixed(4);
                document.getElementById('disp_id').innerText = "ID: "+d.uid;
                document.getElementById('disp_speed').innerText = d.speed.toFixed(4)+"/s";
                document.getElementById('my_ref').innerText = currentU;
            });
            // Live Mining Heartbeat
            setInterval(() => {
                db.ref('users/'+currentU).once('value', s => {
                    let d = s.val();
                    if(d.speed > 0) db.ref('users/'+currentU).update({profit:(parseFloat(d.profit)+d.speed).toFixed(4)});
                });
            }, 1000);
            loadUserLogs();
            loadTeam();
        }

        function loadPlanCards() {
            let std = document.getElementById('std_grid');
            for(let i=1; i<=15; i++) {
                std.innerHTML += `<div class="plan-card" onclick="purchaseNode(${i*750}, ${0.0004*i})">
                    <i class="fa-solid fa-server" style="color:var(--primary); font-size:1.8rem; margin-bottom:15px;"></i>
                    <b style="font-size:1rem;">Node Lvl ${i}</b><br>
                    <small style="color:var(--success); font-weight:800;">Rs ${i*750}</small>
                </div>`;
            }
            let vip = document.getElementById('vip_grid');
            let v_list = [
                {n:'Titanium', p:50000, s:0.1}, {n:'Apex', p:100000, s:0.25}, 
                {n:'Zenith', p:250000, s:0.75}, {n:'Omega', p:500000, s:1.8}, {n:'Quantum', p:1000000, s:4.5}
            ];
            v_list.forEach(v => {
                vip.innerHTML += `<div class="plan-card vip" onclick="purchaseNode(${v.p}, ${v.s})">
                    <div><h4>${v.n} Server</h4><p>Enterprise Grade Node</p></div>
                    <div style="text-align:right;"><b>Rs ${v.p}</b><br><small>${v.s}/s</small></div>
                </div>`;
            });
        }

        function purchaseNode(cost, speed) {
            db.ref('users/'+currentU).once('value', s => {
                if(s.val().wallet < cost) return alert("Investment Required: Low Balance.");
                db.ref('users/'+currentU).update({wallet: s.val().wallet - cost, speed: speed});
                alert("Protocol Activated: Server Deployed!");
            });
        }

        function sendDeposit() {
            let a = document.getElementById('dep_amt').value;
            let t = document.getElementById('dep_tid').value;
            if(!a || !t) return alert("Data mismatch: Enter amount and TID.");
            db.ref('master_logs').push({user:currentU, amt:a, tid:t, status:'pending', type:'Deposit'});
            alert("Sent: Verification in progress.");
        }

        function sendWithdraw() {
            let a = document.getElementById('wit_val').value;
            if(a < 500) return alert("Threshold: Min Rs 500.");
            db.ref('users/'+currentU).once('value', s => {
                if(s.val().wallet < a) return alert("Error: Insufficient Funds.");
                db.ref('master_logs').push({user:currentU, amt:a, method:document.getElementById('wit_met').value, status:'pending', type:'Withdrawal'});
                db.ref('users/'+currentU).update({wallet: s.val().wallet - a});
                alert("Request: Payout queued.");
            });
        }

        function loadUserLogs() {
            db.ref('master_logs').on('value', s => {
                let c = document.getElementById('hist_container'); c.innerHTML = "";
                s.forEach(child => {
                    let r = child.val();
                    if(r.user === currentU) {
                        c.innerHTML += `<div class="log-card">
                            <div><b>${r.type}</b><br><small>Value: ${r.amt || r.tid}</small></div>
                            <span class="status ${r.status}">${r.status}</span>
                        </div>`;
                    }
                });
            });
        }

        function loadTeam() {
            db.ref('users/'+currentU+'/team').on('value', s => {
                let c = document.getElementById('team_container'); c.innerHTML = "";
                s.forEach(child => {
                    c.innerHTML += `<div class="log-card"><span>Member: ${child.val()}</span><span style="color:var(--success); font-weight:800;">ACTIVE</span></div>`;
                });
            });
        }

        function loadAdminTerminal() {
            db.ref('master_logs').on('value', s => {
                let q = document.getElementById('adm_queue'); q.innerHTML = "";
                s.forEach(child => {
                    let r = child.val();
                    if(r.status === 'pending') {
                        q.innerHTML += `<div class="admin-item">
                            <b>${r.type}</b> | User: ${r.user}<br>
                            Detail: ${r.amt || r.tid} | ${r.method || ''}<br><br>
                            <button onclick="approve('${child.key}','${r.user}',${r.amt},'${r.type}')" style="background:var(--success); color:white; border:none; padding:10px 20px; border-radius:10px; font-weight:700;">APPROVE</button>
                            <button onclick="reject('${child.key}')" style="background:var(--error); color:white; border:none; padding:10px 20px; border-radius:10px; font-weight:700; margin-left:10px;">REJECT</button>
                        </div>`;
                    }
                });
            });
        }

        function approve(key, ph, amt, type) {
            if(type === 'Deposit') {
                db.ref('users/'+ph).once('value', s => {
                    let d = s.val();
                    db.ref('users/'+ph).update({wallet: d.wallet + parseInt(amt)});
                    // Referral Kickback (10%)
                    if(d.inviter && d.inviter !== 'System') {
                        db.ref('users/'+d.inviter).once('value', inv => {
                            db.ref('users/'+d.inviter).update({wallet: inv.val().wallet + (amt * 0.1)});
                        });
                    }
                });
                            }
            // Log status update
            db.ref('master_logs/'+key).update({status:'approved'});
            alert("Action Confirmed, sweetie!");
        }

        function reject(key) { 
            if(confirm("Are you sure you want to delete this request?")) {
                db.ref('master_logs/'+key).remove(); 
            }
        }

        // --- Navigation System ---
        function nav(id) {
            // Hide all pages
            document.querySelectorAll('.page').forEach(p => {
                p.classList.remove('active');
                p.style.display = 'none';
            });
            
            // Show selected page
            const targetPage = document.getElementById(id);
            targetPage.classList.add('active');
            targetPage.style.display = 'block';

            // Update Nav Icons highlight
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            
            const navMap = {
                'home': 0,
                'deposit': 1,
                'history': 2,
                'team': 3,
                'withdraw': 4
            };

            if(navMap[id] !== undefined) {
                document.querySelectorAll('.nav-item')[navMap[id]].classList.add('active');
            }

            // Show/Hide bottom nav based on page
            const mainNav = document.getElementById('main_nav');
            if(id === 'login' || id === 'admin') {
                mainNav.style.display = 'none';
            } else {
                mainNav.style.display = 'flex';
            }
            
            // Scroll to top on page change
            window.scrollTo(0,0);
        }

        // --- Utility Functions ---
        function copyRef() {
            // Generates a professional link
            const dummy = document.createElement('input');
            const text = window.location.origin + window.location.pathname + "?ref=" + currentU;
            
            document.body.appendChild(dummy);
            dummy.value = text;
            dummy.select();
            document.execCommand('copy');
            document.body.removeChild(dummy);
            
            alert("Invite link copied to clipboard, sweetie! Share it with your team.");
        }

        // Check for referral in URL on load
        window.addEventListener('DOMContentLoaded', () => {
            const urlParams = new URLSearchParams(window.location.search);
            const ref = urlParams.get('ref');
            if(ref && !currentU) {
                document.getElementById('ref_id').value = ref;
            }
        });

    </script>
</body>
</html>
