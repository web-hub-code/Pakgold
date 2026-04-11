<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | WebHub Elite</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { 
            --gold: #c5a059; --bg: #f8fafc; --card: #ffffff; 
            --text: #1e293b; --sub: #64748b; --success: #10b981; --error: #ef4444;
        }

        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Premium Animations */
        @keyframes slideIn { from { opacity:0; transform: translateY(30px); } to { opacity:1; transform: translateY(0); } }
        .page { display:none; animation: slideIn 0.5s ease-out; padding: 20px 15px 120px; }
        .page.active { display:block; }

        /* Modern UI Components */
        .glass-card { 
            background: var(--card); border-radius: 30px; padding: 25px; 
            border: 1px solid rgba(197, 160, 89, 0.15); box-shadow: 0 15px 35px rgba(0,0,0,0.04); 
            margin-bottom: 20px; transition: 0.3s;
        }

        .wallet-banner { 
            background: linear-gradient(135deg, #1e293b, #334155); color: white;
            padding: 35px; border-radius: 35px; box-shadow: 0 20px 40px rgba(0,0,0,0.15);
            text-align: center;
        }

        .elite-btn { 
            background: linear-gradient(135deg, #c5a059, #b08d48); color: white; border: none; 
            padding: 18px; border-radius: 20px; font-weight: 800; width: 100%; cursor: pointer; 
            box-shadow: 0 10px 20px rgba(197, 160, 89, 0.3); transition: 0.3s;
        }
        .elite-btn:active { transform: scale(0.96); }

        .input-box { 
            width: 100%; padding: 18px; border-radius: 20px; border: 2px solid #edf2f7; 
            outline: none; margin-bottom: 15px; font-size: 1rem;
        }

        /* Status Pills */
        .pill { padding: 5px 12px; border-radius: 10px; font-size: 0.7rem; font-weight: 800; }
        .pill-Approved { background: #dcfce7; color: #166534; }
        .pill-Pending { background: #fef9c3; color: #854d0e; }
        .pill-Rejected { background: #fee2e2; color: #991b1b; }

        /* Sidebar & Floating Nav */
        #side-menu {
            position: fixed; top:0; left: -100%; width: 85%; height: 100%; 
            background: white; z-index: 9999; transition: 0.5s;
            padding: 40px 25px; box-shadow: 20px 0 50px rgba(0,0,0,0.1);
        }
        #side-menu.open { left: 0; }
        
        .nav-bar { 
            position: fixed; bottom: 20px; left: 20px; right: 20px;
            background: rgba(255,255,255,0.95); backdrop-filter: blur(20px);
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
        <div onclick="nav('history-pg'); toggleMenu()" style="padding:15px; border-bottom:1px solid #eee;"><i class="fa fa-receipt"></i> Account History</div>
        <div onclick="nav('policy-pg'); toggleMenu()" style="padding:15px; border-bottom:1px solid #eee;"><i class="fa fa-shield-alt"></i> Privacy Policy</div>
        <div onclick="nav('ref-pg'); toggleMenu()" style="padding:15px; border-bottom:1px solid #eee;"><i class="fa fa-users"></i> Referral Center</div>
        <div onclick="logout()" style="padding:15px; color:var(--error); margin-top:20px;"><i class="fa fa-power-off"></i> Logout</div>
    </div>

    <section id="auth-pg" class="page active" style="text-align:center; padding-top:10vh;">
        <i class="fa fa-gem" style="font-size:4rem; color:var(--gold); margin-bottom:20px;"></i>
        <h1 style="font-weight:900; font-size:2.5rem;">PAK GOLD</h1>
        <div style="padding:40px 10px;">
            <input type="number" id="ph" class="input-box" placeholder="Phone Number">
            <input type="password" id="ps" class="input-box" placeholder="Security PIN">
            <div id="reg-box" style="display:none;">
                <input type="text" id="ref" class="input-box" placeholder="Referral Code (Optional)">
            </div>
            <button class="elite-btn" onclick="handleAuth()">Access System</button>
            <p onclick="toggleAuth()" style="margin-top:20px; font-weight:700; color:var(--sub);" id="a-txt">New Partner? Sign Up</p>
        </div>
    </section>

    <section id="dash-pg" class="page">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:20px;">
            <i class="fa fa-bars-staggered" style="font-size:1.5rem; color:var(--gold)" onclick="toggleMenu()"></i>
            <b id="top-ph">...</b>
        </div>

        <div class="wallet-banner">
            <small style="opacity:0.7; letter-spacing:1px;">AVAILABLE BALANCE</small>
            <h1 style="font-size:3rem; margin:5px 0;">Rs <span id="u-wallet">0.00</span></h1>
            <div style="display:flex; justify-content:space-between; padding:15px; background:rgba(255,255,255,0.1); border-radius:15px; margin-top:10px;">
                <span>Mining: <b id="u-profit">0.00</b></span>
                <span>Rate: <b id="u-speed">0/h</b></span>
            </div>
        </div>

        <div style="display:grid; grid-template-columns:repeat(4, 1fr); gap:10px; margin-top:20px;">
            <div class="glass-card" style="margin:0; text-align:center; padding:15px;" onclick="claimBonus()"><i class="fa fa-gift"></i><br><small>Bonus</small></div>
            <div class="glass-card" style="margin:0; text-align:center; padding:15px;" onclick="nav('depo-pg')"><i class="fa fa-plus-circle"></i><br><small>Depo</small></div>
            <div class="glass-card" style="margin:0; text-align:center; padding:15px;" onclick="nav('with-pg')"><i class="fa fa-wallet"></i><br><small>With</small></div>
            <div class="glass-card" style="margin:0; text-align:center; padding:15px;" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')"><i class="fab fa-whatsapp"></i><br><small>Group</small></div>
        </div>

        <h3 style="margin:30px 0 15px;">Elite Nodes</h3>
        <div id="plans"></div>
    </section>

    <section id="depo-pg" class="page">
        <h2>Secure Deposit</h2>
        <div class="glass-card">
            <p style="font-size:0.8rem; margin-bottom:15px;">Official: 03705519562 (JazzCash) / 03379827882 (EasyPaisa)</p>
            <select id="d-met" class="input-box"><option>JazzCash</option><option>EasyPaisa</option><option>Bank Transfer</option></select>
            <input type="number" id="d-amt" class="input-box" placeholder="Amount">
            <input type="text" id="d-tid" class="input-box" placeholder="Transaction ID (TID)">
            <button class="elite-btn" onclick="subDepo()">Submit Proof</button>
        </div>
    </section>

    <section id="with-pg" class="page">
        <h2>Withdraw Payout</h2>
        <div class="glass-card">
            <select id="w-met" class="input-box"><option>JazzCash</option><option>EasyPaisa</option><option>SadaPay</option></select>
            <input type="number" id="w-amt" class="input-box" placeholder="Amount">
            <input type="text" id="w-acc" class="input-box" placeholder="Account Number">
            <button class="elite-btn" onclick="subWith()">Request Cashout</button>
        </div>
    </section>

    <section id="admin-pg" class="page">
        <h2 style="color:var(--gold)">Global Admin</h2>
        <div class="glass-card" style="background:#f1f5f9;">
            <h4>Edit Wallet</h4>
            <input type="number" id="adm-ph" class="input-box" placeholder="Phone">
            <input type="number" id="adm-val" class="input-box" placeholder="Balance">
            <button class="elite-btn" onclick="admSet()">Force Update</button>
        </div>
        <div id="adm-list"></div>
    </section>

    <section id="history-pg" class="page">
        <h2>Transaction Ledger</h2>
        <div id="hist-list"></div>
    </section>

    <section id="ref-pg" class="page">
        <h2>Referral Program</h2>
        <div class="glass-card" style="text-align:center;">
            <p>Earn Rs 100 per invite!</p>
            <h1 id="u-code" style="color:var(--gold); margin:20px 0; letter-spacing:5px;">----</h1>
            <button class="elite-btn" onclick="navigator.clipboard.writeText(document.getElementById('u-code').innerText); alert('Copied!');">Copy Code</button>
        </div>
    </section>

    <nav class="nav-bar" id="bot-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-pg')"><i class="fa fa-home"></i>Home</div>
        <div class="nav-item" onclick="nav('history-pg')"><i class="fa fa-receipt"></i>Ledger</div>
        <div class="nav-item" onclick="nav('admin-pg')" id="adm-btn" style="display:none;"><i class="fa fa-user-shield"></i>Admin</div>
        <div class="nav-item" onclick="toggleMenu()"><i class="fa fa-bars"></i>Menu</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_key'); let isReg = false;

        function checkSession() {
            if(user) {
                document.getElementById('bot-nav').style.display = 'flex';
                if(user === "03705519562") { document.getElementById('adm-btn').style.display='block'; nav('admin-pg'); loadAdm(); }
                else { nav('dash-pg'); sync(); loadHist(); }
            }
            renderPlans();
        }

        function toggleAuth() { isReg = !isReg; document.getElementById('reg-box').style.display = isReg ? 'block' : 'none'; document.getElementById('a-txt').innerText = isReg ? "Already a partner? Login" : "New Partner? Sign Up"; }
        
        function handleAuth() {
            const p = document.getElementById('ph').value; const s = document.getElementById('ps').value; const r = document.getElementById('ref').value;
            if(p === "03705519562" && s === "admin786") { localStorage.setItem('pg_key', p); location.reload(); return; }
            db.ref('users/'+p).once('value', d => {
                if(isReg) {
                    if(d.exists()) return alert("Exists!");
                    const myId = "PG"+Math.floor(1000+Math.random()*9000);
                    db.ref('users/'+p).set({phone:p, pass:s, wallet:0, profit:0, speed:0, myRef:myId});
                    if(r) db.ref('users').orderByChild('myRef').equalTo(r).once('value', rs => rs.forEach(c => db.ref('users/'+c.key).update({wallet: c.val().wallet+100})));
                }
                localStorage.setItem('pg_key', p); location.reload();
            });
        }

        function sync() {
            db.ref('users/'+user).on('value', s => {
                const d = s.val();
                document.getElementById('u-wallet').innerText = d.wallet.toFixed(2);
                document.getElementById('u-profit').innerText = d.profit.toFixed(2);
                document.getElementById('u-speed').innerText = (d.speed*3600).toFixed(2)+"/h";
                document.getElementById('top-ph').innerText = d.phone;
                document.getElementById('u-code').innerText = d.myRef;
            });
            setInterval(() => { db.ref('users/'+user).once('value', s => { if(s.val().speed > 0) db.ref('users/'+user).update({profit: s.val().profit + s.val().speed}); }); }, 1000);
        }

        function loadHist() {
            db.ref('requests').on('value', s => {
                let h = ""; s.forEach(c => {
                    if(c.val().user === user) h += `<div class="glass-card"><div style="display:flex; justify-content:space-between;"><b>${c.val().type}</b><span class="pill pill-${c.val().status}">${c.val().status}</span></div><small>Rs ${c.val().raw} via ${c.val().met}</small></div>`;
                });
                document.getElementById('hist-list').innerHTML = h || "<p style='text-align:center;'>No history.</p>";
            });
        }

        function subDepo() { const a = document.getElementById('d-amt').value; const t = document.getElementById('d-tid').value; const m = document.getElementById('d-met').value; db.ref('requests').push({type:'Deposit', user:user, amt:a*0.95, raw:a, tid:t, met:m, status:'Pending'}); alert("Submitted!"); }
        function subWith() { const a = document.getElementById('w-amt').value; const m = document.getElementById('w-met').value; db.ref('users/'+user).once('value', s => { if(s.val().wallet < a) return alert("Low funds!"); db.ref('requests').push({type:'Withdraw', user:user, amt:a*0.95, raw:a, acc:document.getElementById('w-acc').value, met:m, status:'Pending'}); db.ref('users/'+user).update({wallet: s.val().wallet - a}); alert("Requested!"); }); }

        function loadAdm() {
            db.ref('requests').on('value', s => {
                let h = ""; s.forEach(c => {
                    if(c.val().status === 'Pending') h += `<div class="glass-card"><b>${c.val().type}</b> | ${c.val().user}<br>Amt: ${c.val().raw} | Met: ${c.val().met}<br><b>ID/Acc: ${c.val().tid || c.val().acc}</b><br><br><button onclick="upd('${c.key}','Approved','${c.val().user}',${c.val().amt},'${c.val().type}')" style="color:green">Approve</button><button onclick="upd('${c.key}','Rejected','${c.val().user}',${c.val().raw},'${c.val().type}')" style="color:red; margin-left:20px;">Reject</button></div>`;
                });
                document.getElementById('adm-list').innerHTML = h;
            });
        }

        function upd(k, st, u, a, t) {
            if(st === 'Approved' && t === 'Deposit') db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a}));
            if(st === 'Rejected' && t === 'Withdraw') db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a}));
            db.ref('requests/'+k).update({status: st});
        }

        function admSet() { db.ref('users/'+document.getElementById('adm-ph').value).update({wallet: parseFloat(document.getElementById('adm-val').value)}); alert("Done!"); }
        function claimBonus() { db.ref('users/'+user).once('value', s => { const n = Date.now(); if(n - (s.val().lb || 0) < 86400000) return alert("Wait 24h!"); db.ref('users/'+user).update({wallet: s.val().wallet + 20, lb: n}); db.ref('requests').push({type:'Bonus', user:user, raw:20, met:'System', status:'Approved'}); alert("Rs 20 Added!"); }); }
        function renderPlans() { let h = ""; for(let i=1; i<=10; i++){ const c = 500*i; const d = c*0.15; h += `<div class="glass-card" style="display:flex; justify-content:space-between; align-items:center;"><b>Node ${i}</b><button class="elite-btn" style="width:auto; padding:10px;" onclick="buy(${c}, ${d/86400})">Rs ${c}</button></div>`; } document.getElementById('plans').innerHTML = h; }
        function buy(p, s) { db.ref('users/'+user).once('value', v => { if(v.val().wallet < p) return alert("Low funds!"); db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s}); alert("Active!"); }); }
        function nav(id) { document.querySelectorAll('.page').forEach(p => p.classList.remove('active')); document.getElementById(id).classList.add('active'); }
        function toggleMenu() { document.getElementById('side-menu').classList.toggle('open'); }
        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
