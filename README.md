<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Neon Empire</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Syncopate:wght@700&family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { 
            --neon-gold: #ffcc00; 
            --neon-cyan: #00f3ff;
            --bg: #05070a; 
            --card: rgba(255, 255, 255, 0.03); 
            --text: #ffffff; 
        }

        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Neon Animations */
        @keyframes pulse-gold { 0% { box-shadow: 0 0 5px var(--neon-gold); } 50% { box-shadow: 0 0 20px var(--neon-gold); } 100% { box-shadow: 0 0 5px var(--neon-gold); } }
        @keyframes slideUp { from { opacity:0; transform: translateY(40px); } to { opacity:1; transform: translateY(0); } }

        .page { display:none; animation: slideUp 0.5s ease-out; padding-bottom: 120px; }
        .page.active { display:block; }

        /* Neon Sidebar */
        #side-menu {
            position: fixed; top:0; left: -100%; width: 80%; height: 100%; 
            background: #0a0e14; z-index: 9999; transition: 0.5s;
            border-right: 1px solid var(--neon-gold); padding: 40px 25px;
        }
        #side-menu.open { left: 0; }
        .menu-overlay { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.7); display:none; z-index:9998; backdrop-filter: blur(8px); }

        /* Glass Neon Cards */
        .neon-card { 
            background: var(--card); margin: 15px; padding: 25px; border-radius: 25px; 
            border: 1px solid rgba(255, 255, 255, 0.1); backdrop-filter: blur(15px);
            transition: 0.3s;
        }
        .neon-card:hover { border-color: var(--neon-gold); transform: scale(1.02); }

        .wallet-neon { 
            background: linear-gradient(135deg, #1a1a1a, #000); 
            margin: 15px; padding: 35px; border-radius: 30px; border: 1px solid var(--neon-gold);
            text-align: center; animation: pulse-gold 3s infinite;
        }

        /* Buttons & Inputs */
        .neon-btn { 
            background: none; color: var(--neon-gold); border: 2px solid var(--neon-gold); 
            padding: 18px; border-radius: 20px; font-weight: 800; width: 100%; cursor: pointer; 
            transition: 0.3s; text-transform: uppercase; letter-spacing: 2px;
        }
        .neon-btn:active { background: var(--neon-gold); color: black; box-shadow: 0 0 30px var(--neon-gold); }

        .input-neon { 
            width: 100%; padding: 18px; border-radius: 18px; border: 1px solid rgba(255,255,255,0.1); 
            background: rgba(255,255,255,0.05); color: white; outline: none; margin-bottom: 15px;
        }
        .input-neon:focus { border-color: var(--neon-cyan); box-shadow: 0 0 10px var(--neon-cyan); }

        /* History Status Colors */
        .status-Approved { color: #00ff88; font-weight: 800; }
        .status-Pending { color: #ffbb00; font-weight: 800; }
        .status-Rejected { color: #ff4444; font-weight: 800; }

        /* Nav Bar */
        .nav-bar { 
            position: fixed; bottom: 0; width: 100%; background: rgba(0,0,0,0.8); 
            backdrop-filter: blur(20px); display: flex; justify-content: space-around; 
            padding: 15px 0 35px; border-top: 1px solid rgba(255,255,255,0.05); z-index: 999; 
        }
        .nav-item { color: rgba(255,255,255,0.4); text-align: center; font-size: 0.65rem; font-weight: 700; }
        .nav-item.active { color: var(--neon-gold); text-shadow: 0 0 10px var(--neon-gold); }
        .nav-item i { font-size: 1.6rem; display: block; margin-bottom: 5px; }
    </style>
</head>
<body onload="checkSession()">

    <div class="menu-overlay" onclick="toggleMenu()"></div>
    <div id="side-menu">
        <h2 style="font-family:'Syncopate'; color:var(--neon-gold); margin-bottom:40px;">PAK GOLD</h2>
        <div onclick="nav('history-pg'); toggleMenu()" style="padding:15px; border-bottom:1px solid rgba(255,255,255,0.05)"><i class="fa fa-history"></i> My Ledger</div>
        <div onclick="nav('ref-pg'); toggleMenu()" style="padding:15px; border-bottom:1px solid rgba(255,255,255,0.05)"><i class="fa fa-users"></i> Referral System</div>
        <div onclick="window.location.href='mailto:webhub262@gmail.com'" style="padding:15px; border-bottom:1px solid rgba(255,255,255,0.05)"><i class="fa fa-envelope"></i> Contact Support</div>
        <div onclick="logout()" style="padding:15px; color:#ff4444;"><i class="fa fa-power-off"></i> Logout</div>
    </div>

    <section id="dash-pg" class="page">
        <div style="padding: 20px; display: flex; justify-content: space-between;">
            <i class="fa fa-bars-staggered" style="font-size: 1.5rem; color: var(--neon-gold)" onclick="toggleMenu()"></i>
            <div id="u-ph-top" style="font-weight: 800; color:var(--neon-gold)">...</div>
        </div>

        <div class="wallet-neon">
            <small style="letter-spacing: 3px; opacity: 0.6;">CURRENT ASSETS</small>
            <h1 style="font-size: 3rem; margin: 10px 0;">Rs <span id="u-wallet">0.00</span></h1>
            <div style="display:flex; justify-content:center; gap:20px; margin-top:15px; font-size:0.8rem;">
                <span>Speed: <b id="u-speed" style="color:var(--neon-cyan)">0/h</b></span>
                <span>Profit: <b id="u-profit" style="color:var(--neon-gold)">0.00</b></span>
            </div>
        </div>

        <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; padding: 0 15px;">
            <div class="neon-card" onclick="claimBonus()" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-gift" style="color:var(--neon-cyan)"></i><br><small>Bonus</small></div>
            <div class="neon-card" onclick="nav('depo-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-plus"></i><br><small>Depo</small></div>
            <div class="neon-card" onclick="nav('with-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-wallet"></i><br><small>Cash</small></div>
            <div class="neon-card" onclick="nav('history-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-list"></i><br><small>Log</small></div>
        </div>

        <h3 style="padding: 30px 20px 10px; font-family:'Syncopate'; font-size:0.9rem;">Neon Mining Nodes</h3>
        <div id="plan-list"></div>
    </section>

    <section id="history-pg" class="page">
        <h2 style="padding:20px; color:var(--neon-gold)">Transaction Ledger</h2>
        <div id="user-history-list" style="padding:0 5px;"></div>
    </section>

    <section id="ref-pg" class="page">
        <h2 style="padding:20px; color:var(--neon-gold)">Affiliate Network</h2>
        <div class="neon-card" style="text-align:center;">
            <p style="margin-bottom:15px; opacity:0.7;">Share your code and get Rs 100 per referral!</p>
            <div id="u-ref-code" style="font-size: 2rem; font-weight: 900; letter-spacing: 5px; color:var(--neon-cyan); margin-bottom:20px;">----</div>
            <button class="neon-btn" onclick="copyRef()">Copy Invite Link</button>
        </div>
    </section>

    <section id="depo-pg" class="page">
        <h2 style="padding:20px; color:var(--neon-gold)">Add Assets</h2>
        <div class="neon-card">
            <p style="font-size:0.8rem; margin-bottom:15px;">JazzCash: 03705519562<br>EasyPaisa: 03379827882</p>
            <input type="number" id="d-amt" class="input-neon" placeholder="Amount (Min 200)">
            <input type="text" id="d-tid" class="input-neon" placeholder="Transaction TID">
            <button class="neon-btn" onclick="submitDepo()">Submit Proof</button>
        </div>
    </section>

    <section id="with-pg" class="page">
        <h2 style="padding:20px; color:var(--neon-gold)">Withdraw Profits</h2>
        <div class="neon-card">
            <input type="number" id="w-amt" class="input-neon" placeholder="Amount">
            <input type="text" id="w-acc" class="input-neon" placeholder="Account Number">
            <button class="neon-btn" onclick="submitWith()">Request Payout</button>
        </div>
    </section>

    <section id="admin-pg" class="page">
        <h2 style="padding:20px;">Master Console</h2>
        <div id="admin-req-list"></div>
    </section>

    <nav class="nav-bar" id="bot-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-pg')"><i class="fa fa-bolt"></i>Home</div>
        <div class="nav-item" onclick="nav('history-pg')"><i class="fa fa-scroll"></i>History</div>
        <div class="nav-item" onclick="nav('ref-pg')"><i class="fa fa-users"></i>Refer</div>
        <div class="nav-item" onclick="toggleMenu()"><i class="fa fa-bars"></i>Menu</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('neon_user');

        function checkSession() {
            if(user) {
                document.getElementById('bot-nav').style.display = 'flex';
                if(user === "03705519562") { nav('admin-pg'); loadAdmin(); }
                else { nav('dash-pg'); syncUser(); loadUserHistory(); }
            } else { location.href = 'login.html'; } // Separate login page expected
            renderPlans();
        }

        function syncUser() {
            db.ref('users/'+user).on('value', s => {
                const d = s.val();
                document.getElementById('u-wallet').innerText = d.wallet.toFixed(2);
                document.getElementById('u-profit').innerText = d.profit.toFixed(2);
                document.getElementById('u-speed').innerText = (d.speed * 3600).toFixed(2) + "/h";
                document.getElementById('u-ph-top').innerText = d.phone;
                document.getElementById('u-ref-code').innerText = d.myRef;
            });
            setInterval(() => { db.ref('users/'+user).once('value', s => { const d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed}); }); }, 1000);
        }

        function loadUserHistory() {
            db.ref('requests').on('value', s => {
                let h = "";
                s.forEach(c => {
                    if(c.val().user === user) {
                        h += `<div class="neon-card" style="margin-bottom:10px; padding:15px;">
                            <div style="display:flex; justify-content:space-between;">
                                <b>${c.val().type}</b>
                                <span class="status-${c.val().status}">${c.val().status}</span>
                            </div>
                            <small style="opacity:0.5;">Amt: Rs ${c.val().raw} | ID: ${c.val().tid || '---'}</small>
                        </div>`;
                    }
                });
                document.getElementById('user-history-list').innerHTML = h || "<p style='text-align:center; opacity:0.5;'>No history found.</p>";
            });
        }

        function claimBonus() {
            db.ref('users/'+user).once('value', s => {
                const now = Date.now();
                const last = s.val().lastBonus || 0;
                if(now - last < 86400000) return alert("Wait 24h, sweetie!");
                db.ref('users/'+user).update({wallet: s.val().wallet + 20, lastBonus: now});
                db.ref('requests').push({type:'Daily Bonus', user:user, raw:20, status:'Approved', date:now});
                alert("Rs 20 Added to Wallet!");
            });
        }

        function submitDepo() {
            const a = document.getElementById('d-amt').value;
            const t = document.getElementById('d-tid').value;
            db.ref('requests').push({type:'Deposit', user:user, amt:a*0.95, raw:a, tid:t, status:'Pending'});
            alert("Deposit Logged!");
        }

        function submitWith() {
            const a = document.getElementById('w-amt').value;
            db.ref('users/'+user).once('value', s => {
                if(s.val().wallet < a) return alert("Low Funds!");
                db.ref('requests').push({type:'Withdraw', user:user, amt:a*0.95, raw:a, acc:document.getElementById('w-acc').value, status:'Pending'});
                db.ref('users/'+user).update({wallet: s.val().wallet - a});
                alert("Withdrawal Logged!");
            });
        }

        function loadAdmin() {
            db.ref('requests').on('value', s => {
                let h = "";
                s.forEach(c => {
                    if(c.val().status === 'Pending') {
                        h += `<div class="neon-card">
                            <b>${c.val().type}</b> | ${c.val().user}<br>
                            Value: ${c.val().raw}<br><br>
                            <button onclick="updateStatus('${c.key}','Approved','${c.val().user}',${c.val().amt},'${c.val().type}')" style="color:#00ff88; background:none; border:1px solid #00ff88; padding:8px; border-radius:10px;">Approve</button>
                            <button onclick="updateStatus('${c.key}','Rejected','${c.val().user}',${c.val().raw},'${c.val().type}')" style="color:#ff4444; background:none; border:1px solid #ff4444; padding:8px; border-radius:10px; margin-left:10px;">Reject</button>
                        </div>`;
                    }
                });
                document.getElementById('admin-req-list').innerHTML = h;
            });
        }

        function updateStatus(k, stat, u, a, t) {
            if(stat === 'Approved' && t === 'Deposit') {
                db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a}));
            }
            if(stat === 'Rejected' && t === 'Withdraw') {
                db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a}));
            }
            db.ref('requests/'+k).update({status: stat});
        }

        function renderPlans() {
            const l = document.getElementById('plan-list'); l.innerHTML = "";
            for(let i=1; i<=20; i++) {
                const cost = 200 * i; const daily = (cost * 0.12).toFixed(0);
                l.innerHTML += `<div class="neon-card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><b>Node V${i}</b><br><small style="color:var(--neon-cyan)">+Rs ${daily}/day</small></div>
                    <button class="neon-btn" style="width:auto; padding:10px 15px; font-size:0.7rem;" onclick="buy(${cost}, ${daily/86400})">Invest</button>
                </div>`;
            }
        }

        function buy(p, s) { db.ref('users/'+user).once('value', v => { if(v.val().wallet < p) return alert("Balance Low!"); db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s}); alert("Miner Active!"); }); }
        function toggleMenu() { const m = document.getElementById('side-menu'); const o = document.querySelector('.menu-overlay'); m.classList.toggle('open'); o.style.display = m.classList.contains('open') ? 'block' : 'none'; }
        function nav(id) { document.querySelectorAll('.page').forEach(p => p.classList.remove('active')); document.getElementById(id).classList.add('active'); }
        function logout() { localStorage.clear(); location.reload(); }
        function copyRef() { const code = document.getElementById('u-ref-code').innerText; navigator.clipboard.writeText(window.location.origin + "?ref=" + code); alert("Link Copied!"); }
    </script>
</body>
</html>
