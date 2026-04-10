<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Official Enterprise</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { 
            --gold: #d4af37; --gold-dark: #b8860b; --bg: #f8fafc; 
            --card: #ffffff; --text: #0f172a; --sub: #64748b; 
        }

        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Animations */
        @keyframes slideUp { from { opacity:0; transform: translateY(30px); } to { opacity:1; transform: translateY(0); } }
        .page { display:none; animation: slideUp 0.4s ease-out; padding-bottom: 120px; }
        .page.active { display:block; }

        /* Premium Sidebar */
        #side-menu {
            position: fixed; top:0; left: -100%; width: 75%; height: 100%; 
            background: white; z-index: 9999; transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            box-shadow: 20px 0 50px rgba(0,0,0,0.1); padding: 40px 20px;
        }
        #side-menu.open { left: 0; }
        .menu-overlay { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.4); display:none; z-index:9998; backdrop-filter: blur(4px); }
        .menu-item { padding: 15px; border-radius: 15px; margin-bottom: 8px; font-weight: 700; display: flex; align-items: center; gap: 15px; }
        .menu-item i { color: var(--gold); }

        /* Cards & UI */
        .glass-card { 
            background: var(--card); margin: 15px; padding: 22px; border-radius: 28px; 
            border: 1px solid rgba(212, 175, 55, 0.1); box-shadow: 0 10px 30px rgba(0,0,0,0.03); 
        }
        .wallet-card { 
            background: linear-gradient(135deg, #1e293b, #0f172a); 
            margin: 15px; padding: 30px; border-radius: 30px; color: white;
            box-shadow: 0 15px 35px rgba(212, 175, 55, 0.2);
        }

        /* Buttons */
        .gold-btn { 
            background: linear-gradient(135deg, #d4af37, #b8860b); color: white; border: none; 
            padding: 18px; border-radius: 20px; font-weight: 800; width: 100%; cursor: pointer; 
            transition: 0.3s; text-transform: uppercase; letter-spacing: 1px; box-shadow: 0 8px 20px rgba(212, 175, 55, 0.3);
        }
        .gold-btn:active { transform: scale(0.96); }
        .gold-btn.red { background: #ef4444; box-shadow: none; }
        .gold-btn.green { background: #22c55e; box-shadow: none; }

        .input-style { 
            width: 100%; padding: 16px; border-radius: 18px; border: 2px solid #e2e8f0; 
            outline: none; font-size: 1rem; transition: 0.3s; margin-bottom: 12px;
        }
        .input-style:focus { border-color: var(--gold); }

        /* Navigation Bar */
        .nav-bar { 
            position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.9); 
            backdrop-filter: blur(15px); display: flex; justify-content: space-around; 
            padding: 15px 0 35px; border-top: 1px solid #e2e8f0; z-index: 999; 
        }
        .nav-item { color: #94a3b8; text-align: center; font-size: 0.65rem; font-weight: 700; }
        .nav-item.active { color: var(--gold); }
        .nav-item i { font-size: 1.6rem; display: block; margin-bottom: 5px; }

        /* FAQ */
        .faq-box { margin-bottom: 10px; border-bottom: 1px solid #f1f5f9; padding-bottom: 10px; }
        .faq-q { font-weight: 800; cursor: pointer; display: flex; justify-content: space-between; }
        .faq-a { display: none; color: var(--sub); font-size: 0.85rem; margin-top: 8px; }
    </style>
</head>
<body onload="checkSession()">

    <div class="menu-overlay" onclick="toggleMenu()"></div>
    <div id="side-menu">
        <h2 style="margin-bottom: 30px;">PAK <span style="color:var(--gold)">GOLD</span></h2>
        <div class="menu-item" onclick="nav('history-pg'); toggleMenu()"><i class="fa fa-history"></i> Company History</div>
        <div class="menu-item" onclick="nav('privacy-pg'); toggleMenu()"><i class="fa fa-user-shield"></i> Privacy Policy</div>
        <div class="menu-item" onclick="nav('faq-pg'); toggleMenu()"><i class="fa fa-headset"></i> Support & FAQ</div>
        <div class="menu-item" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')"><i class="fab fa-whatsapp"></i> Official Group</div>
        <hr style="margin: 20px 0; opacity: 0.1;">
        <div class="menu-item" onclick="logout()" style="color: #ef4444;"><i class="fa fa-power-off"></i> Exit System</div>
    </div>

    <section id="auth-pg" class="page active" style="padding-top: 60px; text-align: center;">
        <i class="fa fa-shield-halved" style="font-size: 4rem; color: var(--gold); margin-bottom: 20px;"></i>
        <h1 style="font-weight: 900; font-size: 2.5rem; letter-spacing: -1px;">PAK GOLD</h1>
        <p style="color: var(--sub); margin-bottom: 30px;">Professional Investment Portal</p>
        <div style="padding: 0 20px;">
            <input type="number" id="auth-ph" class="input-style" placeholder="Phone Number">
            <input type="password" id="auth-ps" class="input-style" placeholder="Security PIN">
            <div id="signup-box" style="display:none;">
                <input type="text" id="auth-ref" class="input-style" placeholder="Referral Code (Optional)">
            </div>
            <button class="gold-btn" onclick="handleAuth()">Access Dashboard</button>
            <p onclick="toggleAuth()" style="margin-top: 25px; font-weight: 700; color: var(--sub);" id="t-txt">New Partner? Create Account</p>
        </div>
    </section>

    <section id="dash-pg" class="page">
        <div style="padding: 20px; display: flex; justify-content: space-between; align-items: center;">
            <i class="fa fa-bars-staggered" style="font-size: 1.5rem; color: var(--gold)" onclick="toggleMenu()"></i>
            <div style="text-align: right;"><b id="u-ph-top">...</b><br><small id="u-id-top" style="color:var(--gold); font-weight:800; font-size:0.7rem;"></small></div>
        </div>
        <div class="wallet-card">
            <small style="opacity: 0.6; font-weight: 700; letter-spacing: 1px;">TOTAL ASSETS</small>
            <h1 style="font-size: 2.8rem; margin: 5px 0;">Rs <span id="u-wallet">0.00</span></h1>
            <div style="display: flex; gap: 20px; margin-top: 15px; border-top: 1px solid rgba(255,255,255,0.1); padding-top: 15px;">
                <div><small style="opacity: 0.6;">Profit</small><br><b id="u-profit">0.00</b></div>
                <div><small style="opacity: 0.6;">Mining Speed</small><br><b style="color:var(--gold)" id="u-speed">0.00/hr</b></div>
            </div>
        </div>
        <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; padding: 0 15px;">
            <div class="glass-card" onclick="nav('depo-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-circle-plus" style="color:var(--gold)"></i><br><small style="font-weight:700; font-size:0.6rem;">Deposit</small></div>
            <div class="glass-card" onclick="nav('with-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-wallet"></i><br><small style="font-weight:700; font-size:0.6rem;">Withdraw</small></div>
            <div class="glass-card" onclick="nav('promo-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-ticket" style="color:#f59e0b"></i><br><small style="font-weight:700; font-size:0.6rem;">Promo</small></div>
            <div class="glass-card" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')" style="margin:0; text-align:center; padding:15px;"><i class="fab fa-whatsapp" style="color:#22c55e"></i><br><small style="font-weight:700; font-size:0.6rem;">Group</small></div>
        </div>
        <h3 style="padding: 25px 20px 10px;">Investment Plans</h3>
        <div id="plan-list"></div>
    </section>

    <section id="depo-pg" class="page">
        <h2 style="padding:20px;">Deposit Assets</h2>
        <div class="glass-card">
            <p style="font-size:0.8rem; margin-bottom: 15px;">Accounts:<br><b>03705519562</b> (JazzCash/SadaPay)<br><b>03379827882</b> (EasyPaisa)</p>
            <select id="d-met" class="input-style"><option>JazzCash</option><option>EasyPaisa</option><option>SadaPay</option></select>
            <input type="number" id="d-amt" class="input-style" placeholder="Amount (Min 200)">
            <input type="text" id="d-tid" class="input-style" placeholder="TID Proof (Transaction ID)">
            <button class="gold-btn" onclick="submitDepo()">Submit Proof</button>
            <p style="font-size:0.65rem; color:var(--sub); margin-top:10px; text-align:center;">*5% maintenance tax will be deducted automatically.</p>
        </div>
    </section>

    <section id="with-pg" class="page">
        <h2 style="padding:20px;">Request Payout</h2>
        <div class="glass-card">
            <input type="number" id="w-amt" class="input-style" placeholder="Amount">
            <input type="text" id="w-acc" class="input-style" placeholder="Account Number">
            <select id="w-met" class="input-style"><option>JazzCash</option><option>EasyPaisa</option><option>SadaPay</option><option>Bank</option><option>Binance</option></select>
            <button class="gold-btn" onclick="submitWith()">Process Withdrawal</button>
        </div>
    </section>

    <section id="promo-pg" class="page">
        <h2 style="padding:20px;">Redeem Code</h2>
        <div class="glass-card">
            <input type="text" id="p-code" class="input-style" placeholder="Enter Admin Promo Code">
            <button class="gold-btn" onclick="submitPromo()">Apply Code</button>
        </div>
    </section>

    <section id="faq-pg" class="page">
        <h2 style="padding:20px;">FAQ</h2>
        <div class="glass-card">
            <div class="faq-box"><div class="faq-q" onclick="toggleFaq(this)">Min Deposit? <i class="fa fa-plus"></i></div><div class="faq-a">Minimum Rs 200.</div></div>
            <div class="faq-box"><div class="faq-q" onclick="toggleFaq(this)">Processing Time? <i class="fa fa-plus"></i></div><div class="faq-a">1 to 24 hours.</div></div>
        </div>
    </section>

    <section id="history-pg" class="page">
        <h2 style="padding:20px;">Our History</h2>
        <div class="glass-card"><p style="font-size:0.9rem; color:var(--sub); line-height:1.6;">PAK GOLD is a leading digital mining venture launched in 2024 to provide secure and transparent earning opportunities to investors across Pakistan.</p></div>
    </section>

    <section id="privacy-pg" class="page">
        <h2 style="padding:20px;">Privacy Policy</h2>
        <div class="glass-card"><p style="font-size:0.85rem; color:var(--sub);">Your data is encrypted. We only store your phone number for authentication. We never share your transaction history with third parties.</p></div>
    </section>

    <section id="admin-pg" class="page">
        <h2 style="padding:20px;">Master Control</h2>
        <div class="glass-card">
            <h4>Balance Adjustment</h4>
            <input type="number" id="adm-ph" class="input-style" placeholder="User Phone Number">
            <input type="number" id="adm-val" class="input-style" placeholder="Set Exact Amount">
            <button class="gold-btn" onclick="adminAdjust()">Force Update</button>
        </div>
        <div id="admin-req-list" style="padding: 0 5px;"></div>
    </section>

    <nav class="nav-bar" id="bot-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-pg')"><i class="fa fa-home"></i>Home</div>
        <div class="nav-item" onclick="nav('depo-pg')"><i class="fa fa-circle-plus"></i>Depo</div>
        <div class="nav-item" onclick="nav('with-pg')"><i class="fa fa-wallet"></i>With</div>
        <div class="nav-item" onclick="toggleMenu()"><i class="fa fa-bars"></i>Menu</div>
    </nav>

    <script>
        // CONFIG
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_vfinal_user');
        let isSignup = false;

        function checkSession() {
            if(user) {
                document.getElementById('bot-nav').style.display = 'flex';
                if(user === "03705519562") { nav('admin-pg'); loadAdmin(); }
                else { nav('dash-pg'); syncUser(); }
            }
            renderPlans();
        }

        // AUTH
        function toggleAuth() { isSignup = !isSignup; document.getElementById('signup-box').style.display = isSignup ? 'block' : 'none'; document.getElementById('t-txt').innerText = isSignup ? "Already have account? Login" : "New Partner? Create Account"; }
        
        function handleAuth() {
            const ph = document.getElementById('auth-ph').value;
            const ps = document.getElementById('auth-ps').value;
            const ref = document.getElementById('auth-ref').value;
            if(ph === "03705519562" && ps === "admin786") { localStorage.setItem('pg_vfinal_user', ph); location.reload(); return; }
            db.ref('users/'+ph).once('value', s => {
                if(isSignup) {
                    if(s.exists()) return alert("Exists!");
                    const myID = "PG" + Math.floor(10000 + Math.random()*89999);
                    db.ref('users/'+ph).set({phone:ph, pass:ps, wallet:0, profit:0, speed:0, myRef:myID, refBy:ref||'none'});
                    if(ref) db.ref('users').orderByChild('myRef').equalTo(ref).once('value', rs => rs.forEach(c => db.ref('users/'+c.key).update({wallet: c.val().wallet + 100})));
                    alert("Account Ready!");
                }
                localStorage.setItem('pg_vfinal_user', ph); location.reload();
            });
        }

        // DATA SYNC
        function syncUser() {
            db.ref('users/'+user).on('value', s => {
                const d = s.val();
                document.getElementById('u-wallet').innerText = d.wallet.toFixed(2);
                document.getElementById('u-profit').innerText = d.profit.toFixed(2);
                document.getElementById('u-speed').innerText = (d.speed * 3600).toFixed(2) + "/hr";
                document.getElementById('u-ph-top').innerText = d.phone;
                document.getElementById('u-id-top').innerText = "ID: " + d.myRef;
            });
            setInterval(() => { db.ref('users/'+user).once('value', s => { const d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed}); }); }, 1000);
        }

        // PLANS
        function renderPlans() {
            const l = document.getElementById('plan-list'); l.innerHTML = "";
            for(let i=1; i<=20; i++) {
                const cost = 200 * i; const daily = (cost * 0.12).toFixed(0);
                l.innerHTML += `<div class="glass-card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><b style="color:var(--gold)">Level ${i} Miner</b><br><small>Rs ${daily}/day</small></div>
                    <button class="gold-btn" style="width:auto; padding:10px 15px; font-size:0.7rem;" onclick="buy(${cost}, ${daily/86400})">Rs ${cost}</button>
                </div>`;
            }
        }
        function buy(p, s) { db.ref('users/'+user).once('value', v => { if(v.val().wallet < p) return alert("Low Funds!"); db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s}); alert("Miner Started!"); }); }

        // OPS
        function submitDepo() { const a = document.getElementById('d-amt').value; const t = document.getElementById('d-tid').value; db.ref('requests').push({type:'Deposit', user:user, amt:a*0.95, raw:a, tid:t, status:'Pending'}); alert("Proof Sent!"); }
        function submitWith() { const a = document.getElementById('w-amt').value; db.ref('users/'+user).once('value', s => { if(s.val().wallet < a) return alert("Low Balance!"); db.ref('requests').push({type:'Withdraw', user:user, amt:a*0.95, raw:a, acc:document.getElementById('w-acc').value, method:document.getElementById('w-met').value, status:'Pending'}); db.ref('users/'+user).update({wallet: s.val().wallet - a}); alert("Requested!"); }); }
        function submitPromo() { db.ref('requests').push({type:'Promo', user:user, code:document.getElementById('p-code').value, status:'Pending'}); alert("Submitted!"); }

        // ADMIN
        function loadAdmin() {
            db.ref('requests').on('value', s => {
                let h = "<h3>Pending</h3>";
                s.forEach(c => {
                    if(c.val().status === 'Pending') {
                        h += `<div class="glass-card">
                            <b>${c.val().type}</b> | User: ${c.val().user}<br>Amt: ${c.val().raw} | ID: ${c.val().tid || c.val().acc || c.val().code}<br><br>
                            <button class="gold-btn green" style="width:45%;" onclick="approve('${c.key}','${c.val().user}',${c.val().amt},'${c.val().type}')">Approve</button>
                            <button class="gold-btn red" style="width:45%;" onclick="reject('${c.key}','${c.val().user}',${c.val().raw},'${c.val().type}')">Reject</button>
                        </div>`;
                    }
                });
                document.getElementById('admin-req-list').innerHTML = h;
            });
        }
        function approve(k, u, a, t) { if(t === 'Deposit' || t === 'Promo') { db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a})); } db.ref('requests/'+k).update({status:'Approved'}); }
        function reject(k, u, a, t) { if(t === 'Withdraw') { db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a})); } db.ref('requests/'+k).update({status:'Rejected'}); }
        function adminAdjust() { db.ref('users/'+document.getElementById('adm-ph').value).update({wallet: parseFloat(document.getElementById('adm-val').value)}); alert("Done!"); }

        // UI HELPERS
        function toggleMenu() { const m = document.getElementById('side-menu'); const o = document.querySelector('.menu-overlay'); m.classList.toggle('open'); o.style.display = m.classList.contains('open') ? 'block' : 'none'; }
        function toggleFaq(el) { const a = el.nextElementSibling; a.style.display = (a.style.display === 'block') ? 'none' : 'block'; }
        function nav(id) { document.querySelectorAll('.page').forEach(p => p.classList.remove('active')); document.getElementById(id).classList.add('active'); }
        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
