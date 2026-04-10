<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Official Elite</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { 
            --gold: #c5a059; 
            --gold-light: #f3e9d2;
            --bg: #f8fafc; 
            --card: #ffffff; 
            --text: #1e293b; 
            --sub: #64748b;
            --accent: #2563eb;
        }

        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Premium Animations */
        @keyframes fadeIn { from { opacity:0; transform: translateY(20px); } to { opacity:1; transform: translateY(0); } }
        @keyframes shine { 0% { left: -100%; } 100% { left: 100%; } }

        .page { display:none; animation: fadeIn 0.6s cubic-bezier(0.23, 1, 0.32, 1); padding-bottom: 120px; }
        .page.active { display:block; }

        /* Sidebar Glassmorphism */
        #side-menu {
            position: fixed; top:0; left: -100%; width: 80%; height: 100%; 
            background: white; z-index: 9999; transition: 0.5s cubic-bezier(0.7, 0, 0.3, 1);
            box-shadow: 25px 0 50px rgba(0,0,0,0.05); padding: 40px 25px;
        }
        #side-menu.open { left: 0; }
        .menu-overlay { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.2); display:none; z-index:9998; backdrop-filter: blur(5px); }

        /* Trusted UI Cards */
        .premium-card { 
            background: var(--card); margin: 15px; padding: 25px; border-radius: 32px; 
            border: 1px solid rgba(197, 160, 89, 0.15); box-shadow: 0 10px 30px rgba(0,0,0,0.03); 
            transition: 0.4s;
        }
        .premium-card:active { transform: scale(0.97); }

        /* Wallet Section (Corporate Look) */
        .wallet-banner { 
            background: linear-gradient(135deg, #1e293b, #334155); 
            margin: 15px; padding: 35px; border-radius: 35px; color: white;
            box-shadow: 0 20px 40px rgba(197, 160, 89, 0.2); position: relative; overflow: hidden;
        }
        .wallet-banner::after {
            content: ''; position: absolute; top:0; left:-100%; width: 50%; height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.1), transparent);
            animation: shine 4s infinite;
        }

        /* Modern Buttons */
        .elite-btn { 
            background: linear-gradient(135deg, #c5a059, #b08d48); color: white; border: none; 
            padding: 18px; border-radius: 20px; font-weight: 800; width: 100%; cursor: pointer; 
            transition: 0.3s; text-transform: uppercase; letter-spacing: 1px;
            box-shadow: 0 10px 20px rgba(197, 160, 89, 0.3);
        }
        .elite-btn:active { transform: translateY(2px); box-shadow: none; }

        .input-elite { 
            width: 100%; padding: 18px; border-radius: 20px; border: 2px solid #f1f5f9; 
            background: #ffffff; color: var(--text); outline: none; margin-bottom: 15px; transition: 0.3s;
        }
        .input-elite:focus { border-color: var(--gold); background: #fff; }

        /* Status Pills */
        .pill { padding: 4px 12px; border-radius: 10px; font-size: 0.7rem; font-weight: 800; text-transform: uppercase; }
        .pill-Approved { background: #dcfce7; color: #166534; }
        .pill-Pending { background: #fef9c3; color: #854d0e; }
        .pill-Rejected { background: #fee2e2; color: #991b1b; }

        /* Bottom Nav (Floating Style) */
        .nav-bar { 
            position: fixed; bottom: 15px; left: 15px; right: 15px;
            background: rgba(255,255,255,0.9); backdrop-filter: blur(20px);
            display: flex; justify-content: space-around; 
            padding: 12px 0; border-radius: 25px; border: 1px solid rgba(0,0,0,0.05);
            box-shadow: 0 10px 30px rgba(0,0,0,0.1); z-index: 999; 
        }
        .nav-item { color: #94a3b8; text-align: center; font-size: 0.65rem; font-weight: 700; flex: 1; }
        .nav-item.active { color: var(--gold); }
        .nav-item i { font-size: 1.4rem; display: block; margin-bottom: 3px; }
    </style>
</head>
<body onload="checkSession()">

    <div class="menu-overlay" onclick="toggleMenu()"></div>
    <div id="side-menu">
        <div style="display:flex; align-items:center; gap:12px; margin-bottom:40px;">
            <div style="background:var(--gold); width:45px; height:45px; border-radius:12px; display:grid; place-items:center;">
                <i class="fa fa-gem" style="color:white; font-size:1.5rem;"></i>
            </div>
            <h2 style="font-weight:900; letter-spacing:-1px;">PAK <span style="color:var(--gold)">GOLD</span></h2>
        </div>
        <div onclick="nav('history-pg'); toggleMenu()" style="padding:15px; display:flex; gap:15px; align-items:center;"><i class="fa fa-receipt" style="color:var(--gold)"></i> My Ledger</div>
        <div onclick="nav('ref-pg'); toggleMenu()" style="padding:15px; display:flex; gap:15px; align-items:center;"><i class="fa fa-users" style="color:var(--gold)"></i> Referral Center</div>
        <div onclick="window.location.href='mailto:webhub262@gmail.com'" style="padding:15px; display:flex; gap:15px; align-items:center;"><i class="fa fa-envelope" style="color:var(--gold)"></i> Official Support</div>
        <hr style="margin:20px 0; opacity:0.05;">
        <div onclick="logout()" style="padding:15px; color:#ef4444;"><i class="fa fa-power-off"></i> Logout</div>
    </div>

    <section id="dash-pg" class="page">
        <div style="padding: 25px; display: flex; justify-content: space-between; align-items:center;">
            <i class="fa fa-bars-staggered" style="font-size: 1.5rem; color: var(--gold)" onclick="toggleMenu()"></i>
            <div id="u-ph-top" style="font-weight: 800; font-size:0.9rem;">...</div>
        </div>

        <div class="wallet-banner">
            <div style="display:flex; justify-content:space-between; opacity:0.8;">
                <small style="letter-spacing: 2px; font-weight:700;">TOTAL LIQUID ASSETS</small>
                <i class="fa fa-shield-check"></i>
            </div>
            <h1 style="font-size: 3rem; margin: 10px 0; font-weight:800;">Rs <span id="u-wallet">0.00</span></h1>
            <div style="display:flex; gap:15px; margin-top:15px;">
                <div style="background:rgba(255,255,255,0.1); padding:10px 15px; border-radius:15px; flex:1;">
                    <small style="opacity:0.6; font-size:0.6rem; display:block;">LIVE PROFIT</small>
                    <b id="u-profit" style="color:var(--gold-light)">0.00</b>
                </div>
                <div style="background:rgba(255,255,255,0.1); padding:10px 15px; border-radius:15px; flex:1;">
                    <small style="opacity:0.6; font-size:0.6rem; display:block;">HASH RATE</small>
                    <b id="u-speed">0.00/h</b>
                </div>
            </div>
        </div>

        <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; padding: 0 15px;">
            <div class="premium-card" onclick="claimBonus()" style="margin:0; text-align:center; padding:15px; border-radius:22px;"><i class="fa fa-gift" style="color:var(--accent); font-size:1.4rem;"></i><br><small style="font-weight:800; font-size:0.6rem; display:block; margin-top:5px;">Bonus</small></div>
            <div class="premium-card" onclick="nav('depo-pg')" style="margin:0; text-align:center; padding:15px; border-radius:22px;"><i class="fa fa-circle-plus" style="color:var(--gold); font-size:1.4rem;"></i><br><small style="font-weight:800; font-size:0.6rem; display:block; margin-top:5px;">Depo</small></div>
            <div class="premium-card" onclick="nav('with-pg')" style="margin:0; text-align:center; padding:15px; border-radius:22px;"><i class="fa fa-wallet" style="font-size:1.4rem;"></i><br><small style="font-weight:800; font-size:0.6rem; display:block; margin-top:5px;">Cash</small></div>
            <div class="premium-card" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')" style="margin:0; text-align:center; padding:15px; border-radius:22px;"><i class="fab fa-whatsapp" style="color:#22c55e; font-size:1.4rem;"></i><br><small style="font-weight:800; font-size:0.6rem; display:block; margin-top:5px;">Group</small></div>
        </div>

        <h3 style="padding: 30px 20px 10px; font-weight:800;">Exclusive Investment Nodes</h3>
        <div id="plan-list"></div>
    </section>

    <section id="history-pg" class="page">
        <h2 style="padding:25px 20px 10px;">Account Ledger</h2>
        <div id="user-history-list" style="padding: 0 10px;"></div>
    </section>

    <section id="ref-pg" class="page">
        <h2 style="padding:25px 20px 10px;">Affiliate Program</h2>
        <div class="premium-card" style="text-align:center;">
            <div style="background:var(--gold-light); width:80px; height:80px; border-radius:50%; display:grid; place-items:center; margin:0 auto 20px;">
                <i class="fa fa-users-crown" style="color:var(--gold); font-size:2rem;"></i>
            </div>
            <p style="color:var(--sub); margin-bottom:15px;">Invite friends and earn **Rs 100** instantly upon their first node activation.</p>
            <div id="u-ref-code" style="font-size: 2.2rem; font-weight: 900; color:var(--gold); margin-bottom:25px; letter-spacing:2px;">----</div>
            <button class="elite-btn" onclick="copyRef()">Copy Referral Link</button>
        </div>
    </section>

    <section id="depo-pg" class="page">
        <h2 style="padding:25px 20px 10px;">Secure Deposit</h2>
        <div class="premium-card">
            <p style="font-size:0.9rem; color:var(--sub); margin-bottom:20px; line-height:1.6;">Send funds to the verified treasury account below:<br>
            <b>03705519562</b> (JazzCash)<br>
            <b>03379827882</b> (EasyPaisa)</p>
            <input type="number" id="d-amt" class="input-elite" placeholder="Amount (PKR)">
            <input type="text" id="d-tid" class="input-elite" placeholder="Transaction ID (TID)">
            <button class="elite-btn" onclick="submitDepo()">Submit Assets</button>
        </div>
    </section>

    <section id="with-pg" class="page">
        <h2 style="padding:25px 20px 10px;">Withdraw Revenue</h2>
        <div class="premium-card">
            <input type="number" id="w-amt" class="input-elite" placeholder="Withdrawal Amount">
            <input type="text" id="w-acc" class="input-elite" placeholder="Receiving Account Number">
            <select id="w-met" class="input-elite"><option>JazzCash</option><option>EasyPaisa</option><option>SadaPay</option></select>
            <button class="elite-btn" onclick="submitWith()">Initialize Cashout</button>
        </div>
    </section>

    <section id="admin-pg" class="page">
        <h2 style="padding:25px 20px 10px;">Global Command</h2>
        <div class="premium-card" style="background:#f8fafc;">
            <h4>Asset Override</h4>
            <input type="number" id="adm-ph" class="input-elite" placeholder="Target Phone">
            <input type="number" id="adm-val" class="input-elite" placeholder="New Wallet Balance">
            <button class="elite-btn" onclick="adminAdjust()">Confirm Update</button>
        </div>
        <div id="admin-req-list" style="padding: 10px;"></div>
    </section>

    <nav class="nav-bar" id="bot-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-pg')"><i class="fa fa-house-chimney-window"></i>Home</div>
        <div class="nav-item" onclick="nav('history-pg')"><i class="fa fa-receipt"></i>Ledger</div>
        <div class="nav-item" onclick="nav('ref-pg')"><i class="fa fa-users"></i>Refer</div>
        <div class="nav-item" onclick="toggleMenu()"><i class="fa fa-user-shield"></i>Menu</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_elite_user');

        function checkSession() {
            if(user) {
                document.getElementById('bot-nav').style.display = 'flex';
                if(user === "03705519562") { nav('admin-pg'); loadAdmin(); }
                else { nav('dash-pg'); syncUser(); loadHistory(); }
            } else { location.href = 'index.html'; }
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

        function loadHistory() {
            db.ref('requests').on('value', s => {
                let h = "";
                s.forEach(c => {
                    if(c.val().user === user) {
                        h += `<div class="premium-card" style="margin-bottom:12px; padding:20px;">
                            <div style="display:flex; justify-content:space-between; align-items:center;">
                                <b style="font-size:1rem;">${c.val().type}</b>
                                <span class="pill pill-${c.val().status}">${c.val().status}</span>
                            </div>
                            <div style="margin-top:10px; display:flex; justify-content:space-between; color:var(--sub); font-size:0.8rem;">
                                <span>Rs ${c.val().raw}</span>
                                <span>ID: ${c.val().tid || '---'}</span>
                            </div>
                        </div>`;
                    }
                });
                document.getElementById('user-history-list').innerHTML = h || "<p style='text-align:center; opacity:0.5;'>No transactions yet.</p>";
            });
        }

        function claimBonus() {
            db.ref('users/'+user).once('value', s => {
                const now = Date.now();
                if(now - (s.val().lastBonus || 0) < 86400000) return alert("Bonus already claimed! Return in 24 hours.");
                db.ref('users/'+user).update({wallet: s.val().wallet + 20, lastBonus: now});
                db.ref('requests').push({type:'Daily Reward', user:user, raw:20, status:'Approved', date:now});
                alert("Daily Reward of Rs 20 Credited!");
            });
        }

        function renderPlans() {
            const l = document.getElementById('plan-list'); l.innerHTML = "";
            for(let i=1; i<=20; i++) {
                const cost = 200 * i; const daily = (cost * 0.12).toFixed(0);
                l.innerHTML += `<div class="premium-card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><b style="color:var(--text); font-size:1.1rem;">Miner Node ${i}</b><br><small style="color:var(--gold); font-weight:800;">+Rs ${daily} / Daily</small></div>
                    <button class="elite-btn" style="width:auto; padding:12px 20px; font-size:0.75rem;" onclick="buy(${cost}, ${daily/86400})">Invest Rs ${cost}</button>
                </div>`;
            }
        }

        function buy(p, s) { db.ref('users/'+user).once('value', v => { if(v.val().wallet < p) return alert("Insufficient funds for this node!"); db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s}); alert("Investment Node Activated!"); }); }
        
        function submitDepo() {
            const a = document.getElementById('d-amt').value;
            const t = document.getElementById('d-tid').value;
            if(!a || !t) return alert("Please fill all security fields!");
            db.ref('requests').push({type:'Deposit', user:user, amt:a*0.95, raw:a, tid:t, status:'Pending'});
            alert("Deposit request under verification.");
        }

        function submitWith() {
            const a = document.getElementById('w-amt').value;
            db.ref('users/'+user).once('value', s => {
                if(s.val().wallet < a) return alert("Balance too low for withdrawal!");
                db.ref('requests').push({type:'Withdraw', user:user, amt:a*0.95, raw:a, acc:document.getElementById('w-acc').value, status:'Pending'});
                db.ref('users/'+user).update({wallet: s.val().wallet - a});
                alert("Withdrawal request processed.");
            });
        }

        function loadAdmin() {
            db.ref('requests').on('value', s => {
                let h = "";
                s.forEach(c => {
                    if(c.val().status === 'Pending') {
                        h += `<div class="premium-card">
                            <b>${c.val().type}</b> - ${c.val().user}<br>
                            Amt: ${c.val().raw} | Ref: ${c.val().tid || c.val().acc}<br><br>
                            <div style="display:flex; gap:10px;">
                                <button onclick="updateStatus('${c.key}','Approved','${c.val().user}',${c.val().amt},'${c.val().type}')" style="background:#10b981; color:white; border:none; flex:1; padding:10px; border-radius:12px;">Approve</button>
                                <button onclick="updateStatus('${c.key}','Rejected','${c.val().user}',${c.val().raw},'${c.val().type}')" style="background:#ef4444; color:white; border:none; flex:1; padding:10px; border-radius:12px;">Reject</button>
                            </div>
                        </div>`;
                    }
                });
                document.getElementById('admin-req-list').innerHTML = h || "<p style='text-align:center;'>Queue Clear</p>";
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

        function adminAdjust() { db.ref('users/'+document.getElementById('adm-ph').value).update({wallet: parseFloat(document.getElementById('adm-val').value)}); alert("Account Override Successful."); }
        function toggleMenu() { const m = document.getElementById('side-menu'); const o = document.querySelector('.menu-overlay'); m.classList.toggle('open'); o.style.display = m.classList.contains('open') ? 'block' : 'none'; }
        function nav(id) { document.querySelectorAll('.page').forEach(p => p.classList.remove('active')); document.getElementById(id).classList.add('active'); }
        function logout() { localStorage.clear(); location.reload(); }
        function copyRef() { const code = document.getElementById('u-ref-code').innerText; navigator.clipboard.writeText(window.location.origin + "?ref=" + code); alert("Invitation link copied!"); }
    </script>
</body>
</html>
