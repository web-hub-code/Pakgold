<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | WebHub Official</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { 
            --gold: #c5a059; --bg: #f8fafc; --card: #ffffff; 
            --text: #1e293b; --sub: #64748b; --accent: #2563eb;
        }

        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Smooth Animations */
        @keyframes slideUp { from { opacity:0; transform: translateY(30px); } to { opacity:1; transform: translateY(0); } }
        .page { display:none; animation: slideUp 0.5s cubic-bezier(0.23, 1, 0.32, 1); padding-bottom: 120px; }
        .page.active { display:block; }

        /* Premium Sidebar */
        #side-menu {
            position: fixed; top:0; left: -100%; width: 80%; height: 100%; 
            background: white; z-index: 9999; transition: 0.5s cubic-bezier(0.7, 0, 0.3, 1);
            box-shadow: 25px 0 50px rgba(0,0,0,0.05); padding: 40px 25px;
        }
        #side-menu.open { left: 0; }
        .menu-overlay { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.2); display:none; z-index:9998; backdrop-filter: blur(5px); }

        /* Modern Glass Cards */
        .glass-card { 
            background: var(--card); margin: 15px; padding: 25px; border-radius: 30px; 
            border: 1px solid rgba(197, 160, 89, 0.1); box-shadow: 0 10px 30px rgba(0,0,0,0.03); 
            transition: 0.3s;
        }

        .wallet-card { 
            background: linear-gradient(135deg, #1e293b, #334155); 
            margin: 15px; padding: 35px; border-radius: 35px; color: white;
            box-shadow: 0 20px 40px rgba(197, 160, 89, 0.2);
        }

        /* Buttons & Inputs */
        .elite-btn { 
            background: linear-gradient(135deg, #c5a059, #b08d48); color: white; border: none; 
            padding: 18px; border-radius: 20px; font-weight: 800; width: 100%; cursor: pointer; 
            transition: 0.3s; text-transform: uppercase; letter-spacing: 1px;
        }
        .elite-btn:active { transform: scale(0.96); }

        .input-elite { 
            width: 100%; padding: 18px; border-radius: 20px; border: 2px solid #f1f5f9; 
            outline: none; margin-bottom: 15px; transition: 0.3s; font-size: 1rem;
        }
        .input-elite:focus { border-color: var(--gold); }

        /* History Status */
        .status { padding: 5px 12px; border-radius: 10px; font-size: 0.7rem; font-weight: 800; }
        .status-Approved { background: #dcfce7; color: #166534; }
        .status-Pending { background: #fef9c3; color: #854d0e; }
        .status-Rejected { background: #fee2e2; color: #991b1b; }

        /* Bottom Navigation */
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
        <h2 style="font-weight:900; margin-bottom:30px;">PAK <span style="color:var(--gold)">GOLD</span></h2>
        <div onclick="nav('history-pg'); toggleMenu()" style="padding:15px; border-bottom:1px solid #f1f5f9;"><i class="fa fa-receipt"></i> Transaction History</div>
        <div onclick="nav('ref-pg'); toggleMenu()" style="padding:15px; border-bottom:1px solid #f1f5f9;"><i class="fa fa-users"></i> Refer & Earn</div>
        <div onclick="window.location.href='mailto:webhub262@gmail.com'" style="padding:15px; border-bottom:1px solid #f1f5f9;"><i class="fa fa-envelope"></i> Contact Support</div>
        <div onclick="logout()" style="padding:15px; color:#ef4444;"><i class="fa fa-power-off"></i> Logout</div>
        <p style="position:absolute; bottom:20px; font-size:0.7rem; color:var(--sub);">Powered by WebHub Solutions</p>
    </div>

    <section id="auth-pg" class="page active" style="padding-top: 60px; text-align: center;">
        <i class="fa fa-crown" style="font-size: 4.5rem; color: var(--gold); margin-bottom: 20px;"></i>
        <h1 style="font-weight: 900; font-size: 2.8rem;">PAK GOLD</h1>
        <div style="padding: 40px 25px;">
            <input type="number" id="auth-ph" class="input-elite" placeholder="Phone Number">
            <input type="password" id="auth-ps" class="input-elite" placeholder="Security PIN">
            <div id="signup-box" style="display:none;">
                <input type="text" id="auth-ref" class="input-elite" placeholder="Referral Code (Optional)">
            </div>
            <button class="elite-btn" onclick="handleAuth()">Enter System</button>
            <p onclick="toggleAuth()" style="margin-top: 25px; font-weight: 700; color: var(--sub);" id="t-txt">New Partner? Sign Up</p>
        </div>
    </section>

    <section id="dash-pg" class="page">
        <div style="padding: 20px; display: flex; justify-content: space-between; align-items:center;">
            <i class="fa fa-bars-staggered" style="font-size: 1.5rem; color: var(--gold)" onclick="toggleMenu()"></i>
            <div id="u-ph-top" style="font-weight: 800;">...</div>
        </div>

        <div class="wallet-card">
            <small style="opacity:0.7; letter-spacing:1px;">TOTAL BALANCE</small>
            <h1 style="font-size: 3rem; margin: 5px 0;">Rs <span id="u-wallet">0.00</span></h1>
            <div style="display:flex; justify-content:space-between; margin-top:15px; background:rgba(255,255,255,0.1); padding:10px; border-radius:15px;">
                <span>Profit: <b id="u-profit" style="color:var(--gold)">0.00</b></span>
                <span>Speed: <b id="u-speed">0/h</b></span>
            </div>
        </div>

        <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; padding: 0 15px;">
            <div class="glass-card" onclick="claimBonus()" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-gift" style="color:var(--accent)"></i><br><small>Bonus</small></div>
            <div class="glass-card" onclick="nav('depo-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-circle-plus" style="color:var(--gold)"></i><br><small>Depo</small></div>
            <div class="glass-card" onclick="nav('with-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-wallet"></i><br><small>Cash</small></div>
            <div class="glass-card" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')" style="margin:0; text-align:center; padding:15px;"><i class="fab fa-whatsapp" style="color:#22c55e"></i><br><small>Group</small></div>
        </div>

        <h3 style="padding: 30px 20px 10px;">Investment Plans</h3>
        <div id="plan-list"></div>
    </section>

    <section id="history-pg" class="page">
        <h2 style="padding:20px;">My Transactions</h2>
        <div id="user-history-list" style="padding:0 10px;"></div>
    </section>

    <section id="ref-pg" class="page">
        <h2 style="padding:20px;">Affiliate Network</h2>
        <div class="glass-card" style="text-align:center;">
            <p style="margin-bottom:15px; opacity:0.7;">Refer friends & Earn Rs 100!</p>
            <div id="u-ref-code" style="font-size: 2.5rem; font-weight: 900; color:var(--gold); margin-bottom:20px;">----</div>
            <button class="elite-btn" onclick="copyRef()">Copy Link</button>
        </div>
    </section>

    <section id="depo-pg" class="page">
        <h2 style="padding:20px;">Add Funds</h2>
        <div class="glass-card">
            <p style="margin-bottom:15px; font-size:0.85rem;">JazzCash: 03705519562<br>EasyPaisa: 03379827882</p>
            <input type="number" id="d-amt" class="input-elite" placeholder="Amount">
            <input type="text" id="d-tid" class="input-elite" placeholder="Transaction TID">
            <button class="elite-btn" onclick="submitDepo()">Submit Proof</button>
        </div>
    </section>

    <section id="with-pg" class="page">
        <h2 style="padding:20px;">Withdraw</h2>
        <div class="glass-card">
            <input type="number" id="w-amt" class="input-elite" placeholder="Amount">
            <input type="text" id="w-acc" class="input-elite" placeholder="Account Number">
            <button class="elite-btn" onclick="submitWith()">Initialize Cashout</button>
        </div>
    </section>

    <section id="admin-pg" class="page">
        <h2 style="padding:20px;">Global Admin Console</h2>
        <div class="glass-card" style="background:#f1f5f9;">
            <h4>Edit User Balance</h4>
            <input type="number" id="adm-ph" class="input-elite" placeholder="User Phone">
            <input type="number" id="adm-val" class="input-elite" placeholder="New Balance">
            <button class="elite-btn" onclick="adminAdjust()">Force Update</button>
        </div>
        <div id="admin-req-list" style="padding: 10px;"></div>
    </section>

    <nav class="nav-bar" id="bot-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-pg')"><i class="fa fa-home"></i>Home</div>
        <div class="nav-item" onclick="nav('history-pg')"><i class="fa fa-receipt"></i>Ledger</div>
        <div class="nav-item" onclick="nav('ref-pg')"><i class="fa fa-users"></i>Refer</div>
        <div class="nav-item" onclick="toggleMenu()"><i class="fa fa-user-shield"></i>Menu</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_master_user');
        let isSignup = false;

        function checkSession() {
            if(user) {
                document.getElementById('bot-nav').style.display = 'flex';
                if(user === "03705519562") { nav('admin-pg'); loadAdmin(); }
                else { nav('dash-pg'); syncUser(); loadUserHistory(); }
            }
            renderPlans();
        }

        // AUTH
        function toggleAuth() { isSignup = !isSignup; document.getElementById('signup-box').style.display = isSignup ? 'block' : 'none'; document.getElementById('t-txt').innerText = isSignup ? "Already have account? Login" : "New Partner? Create Account"; }
        
        function handleAuth() {
            const ph = document.getElementById('auth-ph').value;
            const ps = document.getElementById('auth-ps').value;
            const ref = document.getElementById('auth-ref').value;
            if(ph === "03705519562" && ps === "admin786") { localStorage.setItem('pg_master_user', ph); location.reload(); return; }
            db.ref('users/'+ph).once('value', s => {
                if(isSignup) {
                    if(s.exists()) return alert("Account already exists!");
                    const myID = "WH" + Math.floor(1000 + Math.random()*8999);
                    db.ref('users/'+ph).set({phone:ph, pass:ps, wallet:0, profit:0, speed:0, myRef:myID, refBy:ref||'none'});
                    if(ref) db.ref('users').orderByChild('myRef').equalTo(ref).once('value', rs => rs.forEach(c => db.ref('users/'+c.key).update({wallet: c.val().wallet + 100})));
                    alert("Account Created!");
                }
                localStorage.setItem('pg_master_user', ph); location.reload();
            });
        }

        // SYNC
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
                        h += `<div class="glass-card" style="margin-bottom:10px; padding:15px;">
                            <div style="display:flex; justify-content:space-between;">
                                <b>${c.val().type}</b>
                                <span class="status status-${c.val().status}">${c.val().status}</span>
                            </div>
                            <small>Amt: Rs ${c.val().raw} | ID: ${c.val().tid || '---'}</small>
                        </div>`;
                    }
                });
                document.getElementById('user-history-list').innerHTML = h || "<p style='text-align:center;'>No history.</p>";
            });
        }

        function claimBonus() {
            db.ref('users/'+user).once('value', s => {
                const now = Date.now();
                if(now - (s.val().lastBonus || 0) < 86400000) return alert("Already claimed! Wait 24h.");
                db.ref('users/'+user).update({wallet: s.val().wallet + 20, lastBonus: now});
                db.ref('requests').push({type:'Daily Bonus', user:user, raw:20, status:'Approved'});
                alert("Rs 20 Bonus Added!");
            });
        }

        // PLANS
        function renderPlans() {
            const l = document.getElementById('plan-list'); l.innerHTML = "";
            for(let i=1; i<=20; i++) {
                const cost = 200 * i; const daily = (cost * 0.12).toFixed(0);
                l.innerHTML += `<div class="glass-card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><b>Miner Level ${i}</b><br><small style="color:var(--gold)">Rs ${daily}/day</small></div>
                    <button class="elite-btn" style="width:auto; padding:10px 15px; font-size:0.7rem;" onclick="buy(${cost}, ${daily/86400})">Invest Rs ${cost}</button>
                </div>`;
            }
        }
        function buy(p, s) { db.ref('users/'+user).once('value', v => { if(v.val().wallet < p) return alert("Low Funds!"); db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s}); alert("Miner Activated!"); }); }

        // OPS
        function submitDepo() { const a = document.getElementById('d-amt').value; const t = document.getElementById('d-tid').value; db.ref('requests').push({type:'Deposit', user:user, amt:a*0.95, raw:a, tid:t, status:'Pending'}); alert("Submitted!"); }
        function submitWith() { const a = document.getElementById('w-amt').value; db.ref('users/'+user).once('value', s => { if(s.val().wallet < a) return alert("Low Balance!"); db.ref('requests').push({type:'Withdraw', user:user, amt:a*0.95, raw:a, acc:document.getElementById('w-acc').value, status:'Pending'}); db.ref('users/'+user).update({wallet: s.val().wallet - a}); alert("Requested!"); }); }

        // ADMIN
        function loadAdmin() {
            db.ref('requests').on('value', s => {
                let h = "<h3>Pending Tasks</h3>";
                s.forEach(c => {
                    if(c.val().status === 'Pending') {
                        h += `<div class="glass-card">
                            <b>${c.val().type}</b> - ${c.val().user}<br>Amt: ${c.val().raw}<br><br>
                            <button onclick="upd('${c.key}','Approved','${c.val().user}',${c.val().amt},'${c.val().type}')" style="color:green">Approve</button>
                            <button onclick="upd('${c.key}','Rejected','${c.val().user}',${c.val().raw},'${c.val().type}')" style="color:red; margin-left:15px;">Reject</button>
                        </div>`;
                    }
                });
                document.getElementById('admin-req-list').innerHTML = h;
            });
        }
        function upd(k, stat, u, a, t) {
            if(stat === 'Approved' && t === 'Deposit') db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a}));
            if(stat === 'Rejected' && t === 'Withdraw') db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a}));
            db.ref('requests/'+k).update({status: stat});
        }
        function adminAdjust() { db.ref('users/'+document.getElementById('adm-ph').value).update({wallet: parseFloat(document.getElementById('adm-val').value)}); alert("Updated!"); }

        function toggleMenu() { const m = document.getElementById('side-menu'); const o = document.querySelector('.menu-overlay'); m.classList.toggle('open'); o.style.display = m.classList.contains('open') ? 'block' : 'none'; }
        function nav(id) { document.querySelectorAll('.page').forEach(p => p.classList.remove('active')); document.getElementById(id).classList.add('active'); }
        function logout() { localStorage.clear(); location.reload(); }
        function copyRef() { const code = document.getElementById('u-ref-code').innerText; navigator.clipboard.writeText(window.location.origin + "?ref=" + code); alert("Link Copied!"); }
    </script>
</body>
</html>
