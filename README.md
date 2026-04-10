<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Global Corporate</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { 
            --gold: #d4af37; 
            --gold-glow: rgba(212, 175, 55, 0.4);
            --bg: #fdfdfd; 
            --card: #ffffff; 
            --text: #0f172a; 
            --sub: #64748b;
        }

        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Plus Jakarta Sans', sans-serif; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Animations */
        @keyframes fadeIn { from { opacity: 0; transform: scale(0.95); } to { opacity: 1; transform: scale(1); } }
        .page { display:none; animation: fadeIn 0.4s ease-out; padding-bottom: 100px; }
        .page.active { display:block; }

        /* Sidebar Menu */
        #side-menu {
            position: fixed; top:0; left: -100%; width: 80%; height: 100%; 
            background: white; z-index: 9999; transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            box-shadow: 20px 0 50px rgba(0,0,0,0.1); padding: 40px 20px;
        }
        #side-menu.open { left: 0; }
        .menu-overlay { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.4); display:none; z-index:9998; backdrop-filter: blur(4px); }
        .menu-item { padding: 15px; border-radius: 12px; margin-bottom: 10px; font-weight: 700; color: var(--text); display: flex; align-items: center; gap: 15px; }
        .menu-item i { color: var(--gold); font-size: 1.2rem; }

        /* UI Elements */
        .glass-card { 
            background: white; margin: 15px; padding: 22px; border-radius: 28px; 
            border: 1px solid rgba(0,0,0,0.04); box-shadow: 0 12px 30px rgba(0,0,0,0.03); 
        }
        .gold-btn { 
            background: linear-gradient(135deg, #d4af37, #b8860b); color: white; 
            border: none; padding: 18px; border-radius: 20px; font-weight: 800; width: 100%;
            cursor: pointer; box-shadow: 0 8px 20px var(--gold-glow); transition: 0.3s;
        }
        .gold-btn:active { transform: scale(0.95); }

        /* FAQ Accordion */
        .faq-item { border-bottom: 1px solid #f1f5f9; padding: 15px 0; }
        .faq-q { font-weight: 700; font-size: 0.9rem; display: flex; justify-content: space-between; cursor: pointer; }
        .faq-a { font-size: 0.8rem; color: var(--sub); margin-top: 10px; display: none; }

        /* Premium Nav */
        .nav-bar { 
            position: fixed; bottom: 0; width: 100%; background: white; 
            display: flex; justify-content: space-around; padding: 15px 0 30px; 
            border-top: 1px solid #f1f5f9; z-index: 999; 
        }
        .nav-item { color: #94a3b8; text-align: center; font-size: 0.65rem; font-weight: 700; transition: 0.3s; }
        .nav-item.active { color: var(--gold); }
        .nav-item i { font-size: 1.6rem; display: block; margin-bottom: 5px; }
    </style>
</head>
<body onload="checkSession()">

    <div class="menu-overlay" onclick="toggleMenu()"></div>
    <div id="side-menu">
        <h2 style="margin-bottom: 30px; letter-spacing: -1px;">PAK <span style="color:var(--gold)">GOLD</span></h2>
        <div class="menu-item" onclick="nav('history-pg'); toggleMenu()"><i class="fa fa-book"></i> Company History</div>
        <div class="menu-item" onclick="nav('privacy-pg'); toggleMenu()"><i class="fa fa-shield-halved"></i> Privacy Policy</div>
        <div class="menu-item" onclick="nav('faq-pg'); toggleMenu()"><i class="fa fa-circle-question"></i> FAQs</div>
        <div class="menu-item" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')"><i class="fab fa-whatsapp"></i> Support Group</div>
        <hr style="opacity: 0.1; margin: 20px 0;">
        <div class="menu-item" onclick="logout()" style="color: #ef4444;"><i class="fa fa-power-off"></i> Logout</div>
    </div>

    <!-- DASHBOARD -->
    <section id="dash-pg" class="page">
        <div style="padding: 25px; display: flex; justify-content: space-between; align-items: center;">
            <i class="fa fa-bars-staggered" style="font-size: 1.5rem; color: var(--gold)" onclick="toggleMenu()"></i>
            <div style="text-align: right;"><b id="u-ph-top" style="font-size: 0.9rem;">...</b><br><small id="u-id-top" style="color:var(--gold); font-weight: 800; font-size: 0.7rem;"></small></div>
        </div>

        <div class="glass-card" style="background: #0f172a; color: white;">
            <small style="opacity: 0.5; letter-spacing: 2px; font-weight: 700;">TOTAL ASSETS</small>
            <h1 style="font-size: 2.8rem; margin: 5px 0; font-weight: 800;">Rs <span id="u-wallet">0.00</span></h1>
            <div style="display: flex; justify-content: space-between; margin-top: 20px; background: rgba(255,255,255,0.05); padding: 15px; border-radius: 18px;">
                <div><small style="opacity: 0.6;">Mining Speed</small><br><b style="color:var(--gold)" id="u-speed">0.00</b></div>
                <div><small style="opacity: 0.6;">Commission</small><br><b id="u-profit">0.00</b></div>
            </div>
        </div>

        <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; padding: 0 15px;">
            <div class="glass-card" onclick="nav('depo-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-circle-plus" style="color:var(--gold); font-size:1.2rem;"></i><br><small style="font-weight:700; font-size:0.6rem;">Deposit</small></div>
            <div class="glass-card" onclick="nav('with-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-circle-arrow-up" style="font-size:1.2rem;"></i><br><small style="font-weight:700; font-size:0.6rem;">Withdraw</small></div>
            <div class="glass-card" onclick="nav('team-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-users" style="font-size:1.2rem;"></i><br><small style="font-weight:700; font-size:0.6rem;">My Team</small></div>
            <div class="glass-card" onclick="nav('promo-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-gift" style="color:#f59e0b; font-size:1.2rem;"></i><br><small style="font-weight:700; font-size:0.6rem;">Promo</small></div>
        </div>

        <h3 style="padding: 25px 20px 10px;">Mining Operations</h3>
        <div id="plan-container"></div>
    </section>

    <!-- COMPANY HISTORY -->
    <section id="history-pg" class="page">
        <h2 style="padding:20px;">Our Journey</h2>
        <div class="glass-card">
            <h4 style="color:var(--gold); margin-bottom: 10px;">Founded in 2024</h4>
            <p style="font-size:0.85rem; color:var(--sub); line-height:1.6;">PAK GOLD was established to bridge the gap between traditional investment and digital asset mining. With over 50,000 active partners, we are the leading cloud mining platform in South Asia.</p>
        </div>
    </section>

    <!-- PRIVACY POLICY -->
    <section id="privacy-pg" class="page">
        <h2 style="padding:20px;">Data Protection</h2>
        <div class="glass-card">
            <p style="font-size:0.8rem; color:var(--sub); line-height:1.8;">1. All transactions are encrypted via 256-bit SSL.<br>2. User IDs are kept anonymous in public logs.<br>3. 5% tax on withdrawals is used for liquidity pool maintenance.<br>4. Multiple accounts from one IP are subject to verification.</p>
        </div>
    </section>

    <!-- FAQ SECTION -->
    <section id="faq-pg" class="page">
        <h2 style="padding:20px;">Common Inquiries</h2>
        <div class="glass-card">
            <div class="faq-item">
                <div class="faq-q" onclick="toggleFaq(this)">How much is minimum deposit? <i class="fa fa-plus"></i></div>
                <div class="faq-a">Minimum deposit is Rs 200 PKR. Tax of 5% applies.</div>
            </div>
            <div class="faq-item">
                <div class="faq-q" onclick="toggleFaq(this)">When will I get withdrawal? <i class="fa fa-plus"></i></div>
                <div class="faq-a">Withdrawals are processed within 1 to 24 hours after approval.</div>
            </div>
        </div>
    </section>

    <!-- ADMIN POWER CONSOLE -->
    <section id="admin-pg" class="page">
        <h2 style="padding:20px;">Super Admin Control</h2>
        <div class="glass-card">
            <h4>Manual Wallet Adjust</h4>
            <input type="number" id="adm-u-ph" class="glass-card" style="width:100%; margin:10px 0; border:1px solid #ddd;" placeholder="User Phone">
            <input type="number" id="adm-val" class="glass-card" style="width:100%; margin:10px 0; border:1px solid #ddd;" placeholder="Amount (+/-)">
            <button class="gold-btn" onclick="adminAdjust()">Adjust Balance</button>
        </div>
        <div id="admin-req-list"></div>
    </section>

    <!-- NAVIGATION BAR -->
    <nav class="nav-bar" id="bot-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-pg')"><i class="fa fa-home"></i>Home</div>
        <div class="nav-item" onclick="nav('depo-pg')"><i class="fa fa-wallet"></i>Assets</div>
        <div class="nav-item" onclick="nav('faq-pg')"><i class="fa fa-circle-question"></i>Help</div>
        <div class="nav-item" onclick="toggleMenu()"><i class="fa fa-ellipsis"></i>More</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_v5_user');

        function checkSession() {
            if(user) {
                document.getElementById('bot-nav').style.display = 'flex';
                if(user === "03705519562") { nav('admin-pg'); loadAdmin(); }
                else { nav('dash-pg'); syncUser(); }
            } else { location.href = "login.html"; } // Assuming login logic is separate
            renderPlans();
        }

        function toggleMenu() { 
            const m = document.getElementById('side-menu');
            const o = document.querySelector('.menu-overlay');
            m.classList.toggle('open'); 
            o.style.display = m.classList.contains('open') ? 'block' : 'none';
        }

        function toggleFaq(el) {
            const ans = el.nextElementSibling;
            ans.style.display = (ans.style.display === 'block') ? 'none' : 'block';
        }

        function syncUser() {
            db.ref('users/'+user).on('value', s => {
                const d = s.val();
                document.getElementById('u-wallet').innerText = d.wallet.toFixed(2);
                document.getElementById('u-profit').innerText = d.profit.toFixed(2);
                document.getElementById('u-speed').innerText = (d.speed * 3600).toFixed(2) + "/hr";
                document.getElementById('u-id-top').innerText = "ID: " + d.myRef;
                document.getElementById('u-ph-top').innerText = d.phone;
            });
            setInterval(() => {
                db.ref('users/'+user).once('value', s => {
                    const d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed});
                });
            }, 1000);
        }

        function renderPlans() {
            let cont = document.getElementById('plan-container');
            for(let i=1; i<=20; i++) {
                let cost = 200 * i;
                let daily = (cost * 0.12).toFixed(0);
                cont.innerHTML += `<div class="glass-card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><b style="color:var(--gold)">Level ${i} Miner</b><br><small>Daily: Rs ${daily}</small></div>
                    <button class="gold-btn" style="width:auto; padding:10px 20px; font-size:0.8rem;" onclick="buy(${cost}, ${daily/86400})">Rs ${cost}</button>
                </div>`;
            }
        }

        function adminAdjust() {
            const ph = document.getElementById('adm-u-ph').value;
            const val = parseFloat(document.getElementById('adm-val').value);
            db.ref('users/'+ph).once('value', s => {
                if(!s.exists()) return alert("User not found!");
                db.ref('users/'+ph).update({wallet: s.val().wallet + val});
                alert("Balance Adjusted Successfully, sweetie!");
            });
        }

        function loadAdmin() {
            db.ref('requests').on('value', s => {
                let h = "";
                s.forEach(c => {
                    if(c.val().status === 'Pending') {
                        h += `<div class="glass-card">
                            <b>${c.val().type}</b> | User: ${c.val().user}<br>
                            Amt: ${c.val().raw} | ID: ${c.val().tid || c.val().acc}<br><br>
                            <button class="gold-btn" style="background:#10b981; width:45%;" onclick="approve('${c.key}','${c.val().user}',${c.val().amt},'${c.val().type}')">OK</button>
                            <button class="gold-btn" style="background:#ef4444; width:45%;" onclick="reject('${c.key}','${c.val().user}',${c.val().raw},'${c.val().type}')">NO</button>
                        </div>`;
                    }
                });
                document.getElementById('admin-req-list').innerHTML = h;
            });
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
        }

        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
