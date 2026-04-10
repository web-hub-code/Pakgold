<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Elite Mining Infrastructure</title>
    <!-- Core Tools -->
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;500;700;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>

    <style>
        :root {
            --primary: #c5a059; --bg: #f8fafc; --card: #ffffff; --text: #1e293b; --secondary: #0f172a;
            --success: #10b981; --error: #ef4444; --shadow: 0 10px 30px rgba(0,0,0,0.05);
        }

        [data-theme='dark'] {
            --bg: #0f172a; --card: #1e293b; --text: #f8fafc; --secondary: #c5a059;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; transition: 0.3s; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Animations & Pages */
        .page { display: none; min-height: 100vh; padding-bottom: 110px; animation: slideUp 0.5s ease; }
        .page.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        /* UI Elements */
        .header { background: var(--card); padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; box-shadow: var(--shadow); }
        .hero { background: var(--primary); color: white; margin: 15px; padding: 25px; border-radius: 30px; position: relative; overflow: hidden; box-shadow: 0 15px 35px rgba(197, 160, 89, 0.3); }
        .card { background: var(--card); margin: 15px; padding: 20px; border-radius: 20px; box-shadow: var(--shadow); border: 1px solid rgba(0,0,0,0.02); }
        
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 15px; }
        .node-card { background: var(--card); padding: 20px; border-radius: 20px; text-align: center; border: 1px solid transparent; }
        .node-card:active { border-color: var(--primary); transform: scale(0.95); }

        /* Inputs & Buttons */
        input, select, textarea { width: 100%; padding: 15px; margin-bottom: 12px; border-radius: 12px; border: 1px solid #e2e8f0; background: var(--bg); color: var(--text); font-weight: 600; outline: none; }
        .btn-gold { background: var(--secondary); color: white; border: none; padding: 18px; border-radius: 15px; width: 100%; font-weight: 900; cursor: pointer; }
        
        /* Fake Notifications */
        #fake-notif { position: fixed; bottom: 100px; left: 20px; right: 20px; background: var(--secondary); color: white; padding: 12px; border-radius: 15px; font-size: 0.8rem; z-index: 9999; display: none; text-align: center; }

        /* Nav Bar */
        .nav { position: fixed; bottom: 0; width: 100%; background: var(--card); display: flex; justify-content: space-around; padding: 10px 0 25px; border-top: 1px solid rgba(0,0,0,0.05); }
        .nav-item { text-align: center; font-size: 0.7rem; color: #94a3b8; flex: 1; }
        .nav-item.active { color: var(--primary); }
        .nav-item i { font-size: 1.4rem; margin-bottom: 4px; display: block; }
    </style>
</head>
<body onload="init()">

    <div id="fake-notif" class="animate__animated animate__fadeInUp"></div>

    <!-- Login Page -->
    <section id="login" class="page active">
        <div style="padding: 60px 30px; text-align: center;">
            <h1 style="font-size: 3rem; font-weight: 900; color: var(--primary);">PAK<span>GOLD</span></h1>
            <p style="opacity: 0.7; margin-bottom: 40px;">Certified Digital Mining Platform</p>
            <input type="number" id="ph" placeholder="Phone Number">
            <input type="password" id="pass" placeholder="Password">
            <input type="text" id="ref_id" placeholder="Referral ID (Optional)">
            <input type="password" id="adm_key" placeholder="Admin PIN">
            <button class="btn-gold" onclick="handleAuth()">SECURE LOGIN</button>
        </div>
    </section>

    <!-- Dashboard -->
    <section id="home" class="page">
        <div class="header">
            <div style="font-weight:900;">PAK<span>GOLD</span></div>
            <button onclick="toggleTheme()" style="background:none; border:none; color:var(--text); font-size:1.2rem;"><i class="fa-solid fa-circle-half-stroke"></i></button>
        </div>
        <div class="hero">
            <small style="opacity:0.8;">CURRENT ASSETS</small>
            <div style="font-size:2.5rem; font-weight:900;">Rs <span id="u_wallet">0.00</span></div>
            <div style="display:flex; justify-content:space-between; margin-top:20px; background:rgba(255,255,255,0.1); padding:10px; border-radius:15px;">
                <div><small>Mining</small><br><b id="u_profit">0.000</b></div>
                <div><small>Speed</small><br><b id="u_speed">0.0/s</b></div>
            </div>
        </div>
        <div class="grid" id="node_grid"></div>
    </section>

    <!-- Team Page -->
    <section id="team" class="page">
        <div class="card">
            <h3>Referral Network</h3>
            <p>Your ID: <b id="my_code" style="color:var(--primary);">---</b></p>
            <button class="btn-gold" style="margin-top:10px;" onclick="copyLink()">COPY INVITE LINK</button>
        </div>
        <div class="card">
            <h4>Direct Team Chart</h4>
            <div id="team_container" style="margin-top:15px;"></div>
        </div>
    </section>

    <!-- Deposit & Withdraw History -->
    <section id="history" class="page">
        <div style="padding:15px;">
            <h3>Transaction Vault</h3>
            <div id="hist_list"></div>
        </div>
    </section>

    <!-- Menu & Promo -->
    <section id="menu" class="page">
        <div class="card">
            <h3>Promo Code</h3>
            <input type="text" id="promo_code" placeholder="Enter Daily Code">
            <button class="btn-gold" style="background:var(--primary)" onclick="applyPromo()">REDEEM CODE</button>
        </div>
        <div class="card">
            <h4>Support & Legal</h4>
            <a href="https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3" class="btn-gold" style="display:block; text-align:center; text-decoration:none; margin-bottom:10px; background:#25d366;"><i class="fab fa-whatsapp"></i> Official Group</a>
            <textarea id="issue" placeholder="Explain your issue..."></textarea>
            <button class="btn-gold" onclick="sendMail()">EMAIL SUPPORT</button>
            <div style="margin-top:20px; font-size:0.8rem; opacity:0.7;">
                <p><b>Privacy Policy:</b> Your data is protected by AES-256 encryption.</p>
                <p><b>Company:</b> PakGold Infrastructure Ltd. Registered 2026.</p>
            </div>
        </div>
    </section>

    <!-- Admin Super Control -->
    <section id="admin" class="page" style="background:var(--secondary); color:white;">
        <div style="padding:20px;">
            <h2>GOD-MODE TERMINAL</h2>
            <div id="adm_requests"></div>
            <hr style="margin:20px 0; opacity:0.2;">
            <h4>User Manual Override</h4>
            <input type="number" id="adm_target" placeholder="User Phone">
            <input type="number" id="adm_bal" placeholder="Add Balance (e.g. 500 or -500)">
            <button class="btn-gold" onclick="admAdjust()">SYNC SYSTEM</button>
        </div>
    </section>

    <!-- Nav Bar -->
    <nav class="nav" id="main_nav" style="display:none;">
        <div class="nav-item active" onclick="nav('home')"><i class="fa-solid fa-microchip"></i>Mining</div>
        <div class="nav-item" onclick="nav('team')"><i class="fa-solid fa-users-rays"></i>Team</div>
        <div class="nav-item" onclick="nav('history')"><i class="fa-solid fa-clock-rotate-left"></i>History</div>
        <div class="nav-item" onclick="nav('menu')"><i class="fa-solid fa-sliders"></i>Menu</div>
    </nav>

    <script>
        // --- Firebase Setup ---
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
            runNotifs();
        }

        function toggleTheme() {
            const b = document.body;
            b.setAttribute('data-theme', b.getAttribute('data-theme') === 'dark' ? 'light' : 'dark');
        }

        function handleAuth() {
            let p = document.getElementById('ph').value;
            let ak = document.getElementById('adm_key').value;
            let ref = document.getElementById('ref_id').value;

            if(ak === "pakgold786") { nav('admin'); loadAdmin(); return; }
            if(p.length < 10) return alert("Sweetie, check number!");

            currentU = p;
            localStorage.setItem('pg_session', p);
            db.ref('users/'+p).once('value', s => {
                if(!s.exists()) {
                    db.ref('users/'+p).set({wallet:0, profit:0, speed:0, uid:'PG'+Math.floor(1000+Math.random()*9000), inviter:ref||'System'});
                    if(ref) db.ref('users/'+ref+'/team').push(p);
                }
                location.reload();
            });
        }

        function sync() {
            db.ref('users/'+currentU).on('value', s => {
                let d = s.val();
                document.getElementById('u_wallet').innerText = d.wallet.toLocaleString();
                document.getElementById('u_profit').innerText = parseFloat(d.profit).toFixed(4);
                document.getElementById('u_speed').innerText = d.speed.toFixed(3);
                document.getElementById('my_code').innerText = currentU;
            });
            // Mining Logic
            setInterval(() => {
                db.ref('users/'+currentU).once('value', s => {
                    let d = s.val();
                    if(d.speed > 0) db.ref('users/'+currentU).update({profit: parseFloat(d.profit) + d.speed});
                });
            }, 1000);
            // Team Chart
            db.ref('users/'+currentU+'/team').on('value', s => {
                let tc = document.getElementById('team_container'); tc.innerHTML = "";
                s.forEach(c => { tc.innerHTML += `<div class="card" style="margin:5px 0;">Member: ${c.val()} <span style="float:right; color:var(--success);">ACTIVE</span></div>`; });
            });
        }

        function loadNodes() {
            let g = document.getElementById('node_grid');
            for(let i=1; i<=20; i++) {
                let p = 200 * i;
                g.innerHTML += `<div class="node-card card" onclick="buy(${p}, ${i*0.0002})">
                    <i class="fa-solid fa-server" style="color:var(--primary); font-size:1.5rem;"></i><br>
                    <b>Level ${i}</b><br><small>Rs ${p}</small></div>`;
            }
        }

        function buy(c, s) {
            db.ref('users/'+currentU).once('value', v => {
                if(v.val().wallet < c) return alert("Sweetie, recharge needed!");
                db.ref('users/'+currentU).update({wallet: v.val().wallet - c, speed: v.val().speed + s});
                alert("Node Activated! 🚀");
            });
        }

        function applyPromo() {
            let c = document.getElementById('promo_code').value;
            db.ref('admin_requests').push({user:currentU, type:'PROMO', code:c, status:'pending'});
            alert("Promo sent for approval sweetie!");
        }

        function sendMail() {
            let m = document.getElementById('issue').value;
            window.location.href = `mailto:webhub262@gmail.com?subject=Help&body=${m}`;
        }

        function runNotifs() {
            const msgs = ["User ***321 withdrew Rs 5,000", "User ***990 activated Level 15", "User ***112 earned Rs 12,000"];
            setInterval(() => {
                const n = document.getElementById('fake-notif');
                n.innerText = msgs[Math.floor(Math.random()*msgs.length)];
                n.style.display = 'block';
                setTimeout(() => n.style.display = 'none', 3000);
            }, 10000);
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('main_nav').style.display = (id==='login'||id==='admin')?'none':'flex';
        }

        function loadAdmin() {
            db.ref('admin_requests').on('value', s => {
                let q = document.getElementById('adm_requests'); q.innerHTML = "";
                s.forEach(c => {
                    let r = c.val();
                    if(r.status === 'pending') {
                        q.innerHTML += `<div class="card" style="color:black;">${r.user} | ${r.type} | ${r.code || r.amt}
                        <button onclick="db.ref('admin_requests/${c.key}').update({status:'approved'})">Approve</button></div>`;
                    }
                });
            });
        }

        function admAdjust() {
            let t = document.getElementById('adm_target').value;
            let v = document.getElementById('adm_bal').value;
            db.ref('users/'+t).once('value', s => {
                if(s.exists()) {
                    db.ref('users/'+t).update({wallet: s.val().wallet + parseInt(v)});
                    alert("System Synced!");
                }
            });
        }
    </script>
</body>
</html>
