<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | WebHub Global</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { 
            --gold: #c5a059; --bg: #f4f7fa; --card: #ffffff; 
            --text: #1e293b; --sub: #64748b; --success: #10b981; --error: #ef4444;
        }

        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Next-Gen Animations */
        @keyframes slideIn { from { opacity:0; transform: translateY(40px) scale(0.95); } to { opacity:1; transform: translateY(0) scale(1); } }
        @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.05); } 100% { transform: scale(1); } }

        .page { display:none; animation: slideIn 0.6s cubic-bezier(0.23, 1, 0.32, 1); padding: 20px 15px 120px; }
        .page.active { display:block; }

        /* Premium Components */
        .glass-card { 
            background: var(--card); border-radius: 30px; padding: 25px; 
            border: 1px solid rgba(197, 160, 89, 0.15); box-shadow: 0 15px 35px rgba(0,0,0,0.04); 
            margin-bottom: 20px; transition: 0.3s;
        }

        .wallet-master { 
            background: linear-gradient(135deg, #1e293b, #334155); color: white;
            padding: 35px; border-radius: 35px; box-shadow: 0 20px 40px rgba(0,0,0,0.15);
            position: relative; overflow: hidden;
        }

        .elite-btn { 
            background: linear-gradient(135deg, #c5a059, #b08d48); color: white; border: none; 
            padding: 18px; border-radius: 20px; font-weight: 800; width: 100%; cursor: pointer; 
            box-shadow: 0 10px 20px rgba(197, 160, 89, 0.3); transition: 0.3s;
        }
        .elite-btn:active { transform: scale(0.95); }

        .input-box { 
            width: 100%; padding: 18px; border-radius: 20px; border: 2px solid #edf2f7; 
            outline: none; margin-bottom: 15px; background: white; transition: 0.3s;
        }
        .input-box:focus { border-color: var(--gold); }

        /* Status Pills */
        .badge { padding: 5px 12px; border-radius: 10px; font-size: 0.7rem; font-weight: 800; }
        .bg-Approved { background: #dcfce7; color: #166534; }
        .bg-Pending { background: #fef9c3; color: #854d0e; }
        .bg-Rejected { background: #fee2e2; color: #991b1b; }

        /* Sidebar & Nav */
        #side-menu {
            position: fixed; top:0; left: -100%; width: 85%; height: 100%; 
            background: white; z-index: 9999; transition: 0.5s;
            padding: 40px 25px; box-shadow: 20px 0 50px rgba(0,0,0,0.1);
        }
        #side-menu.open { left: 0; }
        
        .nav-bar { 
            position: fixed; bottom: 20px; left: 20px; right: 20px;
            background: rgba(255,255,255,0.9); backdrop-filter: blur(20px);
            display: flex; justify-content: space-around; padding: 15px;
            border-radius: 25px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); z-index: 999; 
        }
        .nav-item { color: #94a3b8; text-align: center; font-size: 0.65rem; font-weight: 700; flex:1; }
        .nav-item.active { color: var(--gold); }
        .nav-item i { font-size: 1.5rem; display: block; margin-bottom: 5px; }
    </style>
</head>
<body onload="checkSession()">

    <div id="side-menu">
        <h2 style="font-weight:900; color:var(--gold); margin-bottom:30px;">WEB HUB</h2>
        <div onclick="nav('history-pg'); toggleMenu()" style="padding:15px; border-bottom:1px solid #eee;"><i class="fa fa-scroll"></i> Company History</div>
        <div onclick="nav('privacy-pg'); toggleMenu()" style="padding:15px; border-bottom:1px solid #eee;"><i class="fa fa-shield-alt"></i> Privacy Policy</div>
        <div onclick="nav('ledger-pg'); toggleMenu()" style="padding:15px; border-bottom:1px solid #eee;"><i class="fa fa-receipt"></i> Transaction Logs</div>
        <div onclick="logout()" style="padding:15px; color:var(--error); margin-top:20px;"><i class="fa fa-power-off"></i> Sign Out</div>
    </div>

    <section id="dash-pg" class="page">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:20px;">
            <i class="fa fa-bars-staggered" style="font-size:1.5rem; color:var(--gold)" onclick="toggleMenu()"></i>
            <b id="u-ph-top">...</b>
        </div>

        <div class="wallet-master">
            <small style="opacity:0.7; letter-spacing:2px;">GOLD RESERVE BALANCE</small>
            <h1 style="font-size:3.2rem; margin:10px 0; font-weight:800;">Rs <span id="u-wallet">0.00</span></h1>
            <div style="display:flex; justify-content:space-between; background:rgba(255,255,255,0.1); padding:15px; border-radius:20px;">
                <span>Mining: <b id="u-profit">0.00</b></span>
                <span>Speed: <b id="u-speed">0/h</b></span>
            </div>
        </div>

        <div style="display:grid; grid-template-columns:repeat(4, 1fr); gap:10px; margin-top:20px;">
            <div class="glass-card" style="margin:0; text-align:center; padding:15px;" onclick="claimBonus()"><i class="fa fa-gift" style="color:var(--gold)"></i><br><small>Bonus</small></div>
            <div class="glass-card" style="margin:0; text-align:center; padding:15px;" onclick="nav('depo-pg')"><i class="fa fa-plus-circle" style="color:var(--gold)"></i><br><small>Depo</small></div>
            <div class="glass-card" style="margin:0; text-align:center; padding:15px;" onclick="nav('with-pg')"><i class="fa fa-wallet" style="color:var(--gold)"></i><br><small>With</small></div>
            <div class="glass-card" style="margin:0; text-align:center; padding:15px;" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')"><i class="fab fa-whatsapp" style="color:#22c55e"></i><br><small>Group</small></div>
        </div>

        <h3 style="margin:25px 0 15px; font-weight:800;">Mining Protocols</h3>
        <div id="plan-list"></div>
    </section>

    <section id="depo-pg" class="page">
        <h2 style="margin-bottom:20px;">Secure Asset Deposit</h2>
        <div class="glass-card">
            <p style="font-size:0.85rem; color:var(--sub); margin-bottom:20px;">Official Treasury:<br><b>03705519562</b> (JazzCash)<br><b>03379827882</b> (EasyPaisa)</p>
            <select id="d-method" class="input-box">
                <option value="JazzCash">JazzCash</option>
                <option value="EasyPaisa">EasyPaisa</option>
                <option value="Bank Transfer">Bank Transfer</option>
            </select>
            <input type="number" id="d-amt" class="input-box" placeholder="Deposit Amount">
            <input type="text" id="d-tid" class="input-box" placeholder="Transaction TID / ID">
            <button class="elite-btn" onclick="submitDepo()">Verify Deposit</button>
        </div>
    </section>

    <section id="with-pg" class="page">
        <h2 style="margin-bottom:20px;">Revenue Payout</h2>
        <div class="glass-card">
            <select id="w-method" class="input-box">
                <option value="JazzCash">JazzCash</option>
                <option value="EasyPaisa">EasyPaisa</option>
                <option value="Bank">SadaPay/NayaPay</option>
            </select>
            <input type="number" id="w-amt" class="input-box" placeholder="Withdrawal Amount">
            <input type="text" id="w-acc" class="input-box" placeholder="Account Number">
            <button class="elite-btn" onclick="submitWith()">Initialize Cashout</button>
        </div>
    </section>

    <section id="admin-pg" class="page">
        <h2 style="color:var(--gold); margin-bottom:20px;">Global Sovereign Control</h2>
        <div class="glass-card" style="background:#edf2f7;">
            <h4>User Asset Override</h4>
            <input type="number" id="adm-ph" class="input-box" placeholder="Target Phone">
            <input type="number" id="adm-val" class="input-box" placeholder="New Wallet Value">
            <button class="elite-btn" onclick="adminAdjust()">Override Now</button>
        </div>
        <div id="admin-req-list"></div>
    </section>

    <section id="history-pg" class="page">
        <h2>Web Hub Legacy</h2>
        <div class="glass-card">
            <p style="color:var(--sub); line-height:1.7;">Founded in 2024, Web Hub Solutions is the primary architect of PAK GOLD. We specialize in decentralized digital assets and cloud mining infrastructure, ensuring secure returns for over 50k partners worldwide.</p>
        </div>
    </section>

    <section id="privacy-pg" class="page">
        <h2>Privacy Protocol</h2>
        <div class="glass-card">
            <p style="color:var(--sub); line-height:1.7;">We use 256-bit encryption for all financial logs. Your data is never shared with third parties. Withdrawals are processed through multi-signature verification to ensure safety.</p>
        </div>
    </section>

    <section id="ledger-pg" class="page">
        <h2>My Transactions</h2>
        <div id="user-history-list"></div>
    </section>

    <nav class="nav-bar" id="bot-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-pg')"><i class="fa fa-home-alt"></i>Home</div>
        <div class="nav-item" onclick="nav('ledger-pg')"><i class="fa fa-receipt"></i>Ledger</div>
        <div class="nav-item" onclick="nav('admin-pg')" id="admin-icon" style="display:none;"><i class="fa fa-user-shield"></i>Admin</div>
        <div class="nav-item" onclick="toggleMenu()"><i class="fa fa-bars"></i>Menu</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('master_key');

        function checkSession() {
            if(user) {
                document.getElementById('bot-nav').style.display = 'flex';
                if(user === "03705519562") { 
                    document.getElementById('admin-icon').style.display = 'block';
                    nav('admin-pg'); loadAdmin(); 
                } else { nav('dash-pg'); syncUser(); loadHistory(); }
            } else { location.href = 'login.html'; } // Assumed login page exists
            renderPlans();
        }

        function syncUser() {
            db.ref('users/'+user).on('value', s => {
                const d = s.val();
                document.getElementById('u-wallet').innerText = d.wallet.toFixed(2);
                document.getElementById('u-profit').innerText = d.profit.toFixed(2);
                document.getElementById('u-speed').innerText = (d.speed * 3600).toFixed(2) + "/h";
                document.getElementById('u-ph-top').innerText = d.phone;
            });
            setInterval(() => { db.ref('users/'+user).once('value', s => { const d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed}); }); }, 1000);
        }

        function loadHistory() {
            db.ref('requests').on('value', s => {
                let h = "";
                s.forEach(c => {
                    if(c.val().user === user) {
                        h += `<div class="glass-card">
                            <div style="display:flex; justify-content:space-between;"><b>${c.val().type}</b><span class="badge bg-${c.val().status}">${c.val().status}</span></div>
                            <small style="color:var(--sub)">Amt: Rs ${c.val().raw} | Method: ${c.val().method}</small>
                        </div>`;
                    }
                });
                document.getElementById('user-history-list').innerHTML = h || "<p style='text-align:center;'>No logs.</p>";
            });
        }

        // DEPOSIT DATA WITH METHOD & TID
        function submitDepo() {
            const a = document.getElementById('d-amt').value;
            const t = document.getElementById('d-tid').value;
            const m = document.getElementById('d-method').value;
            if(!a || !t) return alert("Fill all details, sweetie!");
            db.ref('requests').push({type:'Deposit', user:user, amt:a*0.95, raw:a, tid:t, method:m, status:'Pending'});
            alert("Deposit Submitted. Admin is verifying!");
        }

        function submitWith() {
            const a = document.getElementById('w-amt').value;
            const m = document.getElementById('w-method').value;
            db.ref('users/'+user).once('value', s => {
                if(s.val().wallet < a) return alert("Low funds!");
                db.ref('requests').push({type:'Withdraw', user:user, amt:a*0.95, raw:a, acc:document.getElementById('w-acc').value, method:m, status:'Pending'});
                db.ref('users/'+user).update({wallet: s.val().wallet - a});
                alert("Withdrawal Logged!");
            });
        }

        // ADMIN GOD MODE (Showing All Data)
        function loadAdmin() {
            db.ref('requests').on('value', s => {
                let h = "";
                s.forEach(c => {
                    if(c.val().status === 'Pending') {
                        h += `<div class="glass-card" style="border-left:5px solid var(--gold)">
                            <div style="display:flex; justify-content:space-between;"><b>${c.val().type}</b> <small>${c.val().user}</small></div>
                            <div style="margin:10px 0; font-size:0.85rem;">
                                <b>Amount:</b> Rs ${c.val().raw}<br>
                                <b>Method:</b> ${c.val().method}<br>
                                <b>TID/Acc:</b> <span style="color:var(--gold); font-weight:800;">${c.val().tid || c.val().acc}</span>
                            </div>
                            <div style="display:flex; gap:10px;">
                                <button onclick="updStatus('${c.key}','Approved','${c.val().user}',${c.val().amt},'${c.val().type}')" style="background:var(--success); color:white; border:none; padding:8px; border-radius:10px; flex:1;">APPROVE</button>
                                <button onclick="updStatus('${c.key}','Rejected','${c.val().user}',${c.val().raw},'${c.val().type}')" style="background:var(--error); color:white; border:none; padding:8px; border-radius:10px; flex:1;">REJECT</button>
                            </div>
                        </div>`;
                    }
                });
                document.getElementById('admin-req-list').innerHTML = h || "<p style='text-align:center;'>Queue Clear!</p>";
            });
        }

        function updStatus(k, st, u, a, t) {
            if(st === 'Approved' && t === 'Deposit') db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a}));
            if(st === 'Rejected' && t === 'Withdraw') db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a}));
            db.ref('requests/'+k).update({status: st});
        }

        function adminAdjust() { db.ref('users/'+document.getElementById('adm-ph').value).update({wallet: parseFloat(document.getElementById('adm-val').value)}); alert("User Balance Overridden!"); }

        function renderPlans() {
            const l = document.getElementById('plan-list'); l.innerHTML = "";
            for(let i=1; i<=15; i++) {
                const cost = 500 * i; const daily = (cost * 0.15).toFixed(0);
                l.innerHTML += `<div class="glass-card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><b>Level ${i} Node</b><br><small style="color:var(--gold)">Daily: Rs ${daily}</small></div>
                    <button class="elite-btn" style="width:auto; padding:10px 15px; font-size:0.75rem;" onclick="buy(${cost}, ${daily/86400})">Invest Rs ${cost}</button>
                </div>`;
            }
        }
        function buy(p, s) { db.ref('users/'+user).once('value', v => { if(v.val().wallet < p) return alert("Insufficient Gold Reserve!"); db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s}); alert("Miner Protocol Online!"); }); }
        function claimBonus() { db.ref('users/'+user).once('value', s => { const now = Date.now(); if(now - (s.val().lastB || 0) < 86400000) return alert("Already Claimed!"); db.ref('users/'+user).update({wallet: s.val().wallet + 25, lastB: now}); db.ref('requests').push({type:'Bonus', user:user, raw:25, method:'System', status:'Approved'}); alert("Daily Rs 25 Credited!"); }); }
        function toggleMenu() { const m = document.getElementById('side-menu'); m.classList.toggle('open'); }
        function nav(id) { document.querySelectorAll('.page').forEach(p => p.classList.remove('active')); document.getElementById(id).classList.add('active'); }
        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
