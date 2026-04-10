<!DOCTYPE html>
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
            --gold: #d4af37; 
            --gold-light: #f4e1a1;
            --bg: #f8fafc; 
            --card: #ffffff; 
            --text: #1e293b; 
            --sub: #64748b; 
            --success: #10b981; 
            --error: #ef4444; 
        }

        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Premium Animations */
        @keyframes goldPulse { 0% { box-shadow: 0 0 0 0 rgba(212, 175, 55, 0.4); } 70% { box-shadow: 0 0 0 10px rgba(212, 175, 55, 0); } 100% { box-shadow: 0 0 0 0 rgba(212, 175, 55, 0); } }
        @keyframes slideIn { from { opacity:0; transform: translateY(30px); } to { opacity:1; transform: translateY(0); } }

        .page { display:none; animation: slideIn 0.5s cubic-bezier(0.4, 0, 0.2, 1); padding-bottom: 120px; }
        .page.active { display:block; }

        /* Glass Cards */
        .glass-card { 
            background: var(--card); 
            margin: 15px; 
            padding: 20px; 
            border-radius: 24px; 
            border: 1px solid rgba(212, 175, 55, 0.1); 
            box-shadow: 0 10px 25px rgba(0,0,0,0.03);
            transition: 0.3s;
        }
        .glass-card:hover { transform: translateY(-5px); border-color: var(--gold); }

        /* Modern Inputs */
        .input-grp { margin-bottom: 15px; padding: 0 15px; }
        .input-grp input, .input-grp select { 
            width: 100%; padding: 16px; border-radius: 16px; 
            border: 2px solid #e2e8f0; background: white; 
            font-size: 1rem; outline: none; transition: 0.3s;
        }
        .input-grp input:focus { border-color: var(--gold); box-shadow: 0 0 0 4px rgba(212, 175, 55, 0.1); }

        /* Professional Buttons */
        .gold-btn { 
            background: linear-gradient(135deg, #d4af37, #b8860b); 
            color: white; border: none; padding: 18px; 
            border-radius: 18px; font-weight: 800; cursor: pointer; 
            width: 100%; transition: 0.3s; text-transform: uppercase; letter-spacing: 1px;
            box-shadow: 0 4px 15px rgba(184, 134, 11, 0.3);
        }
        .gold-btn:active { transform: scale(0.96); }
        .gold-btn.outline { background: white; color: var(--gold); border: 2px solid var(--gold); }

        /* Dashboard Wallet */
        .wallet-header { 
            background: linear-gradient(135deg, #1e293b, #0f172a); 
            margin: 15px; padding: 30px; border-radius: 30px; color: white;
            position: relative; overflow: hidden;
            animation: goldPulse 3s infinite;
        }

        /* Bottom Nav */
        .nav-bar { 
            position: fixed; bottom: 0; width: 100%; 
            background: rgba(255,255,255,0.9); backdrop-filter: blur(15px); 
            display: flex; justify-content: space-around; padding: 15px 0 35px; 
            border-top: 1px solid #e2e8f0; z-index: 999; 
        }
        .nav-item { color: var(--sub); text-align: center; font-size: 0.7rem; font-weight: 600; }
        .nav-item.active { color: var(--gold); }
        .nav-item i { font-size: 1.5rem; display: block; margin-bottom: 5px; }

        /* VIP Badge */
        .vip-badge { 
            background: linear-gradient(45deg, #d4af37, #f4e1a1); 
            color: #000; padding: 4px 10px; border-radius: 8px; 
            font-size: 0.6rem; font-weight: 900;
        }
    </style>
</head>
<body onload="checkSession()">

    <!-- AUTHENTICATION -->
    <section id="auth-pg" class="page active" style="padding-top: 80px; text-align: center;">
        <img src="https://cdn-icons-png.flaticon.com/512/2583/2583344.png" width="80" style="margin-bottom: 20px;">
        <h1 style="font-weight: 900; font-size: 2.2rem; letter-spacing: -1px;">PAK <span style="color:var(--gold)">GOLD</span></h1>
        <p style="color:var(--sub); margin-bottom: 40px;">Enterprise Asset Management v4.0</p>
        
        <div class="input-grp"><input type="number" id="auth-ph" placeholder="Phone Number"></div>
        <div class="input-grp"><input type="password" id="auth-ps" placeholder="Security Password"></div>
        <div id="signup-fields" style="display:none;">
            <div class="input-grp"><input type="text" id="auth-ref" placeholder="Invitation Code"></div>
        </div>
        
        <div style="padding: 0 15px;">
            <button class="gold-btn" onclick="handleAuth()">Access Dashboard</button>
            <p onclick="toggleAuth()" style="margin-top: 25px; font-weight: 700; color: var(--sub);" id="t-btn">New Partner? Create Account</p>
        </div>
    </section>

    <!-- DASHBOARD -->
    <section id="dash-pg" class="page">
        <div style="padding: 20px; display: flex; justify-content: space-between; align-items: center;">
            <div><b style="font-size: 1.2rem;">Hello, <span id="u-ph-top" style="color:var(--gold)">...</span></b><br><small id="u-id-top" class="vip-badge">ID: LOADING</small></div>
            <i class="fa fa-circle-user" style="font-size: 2rem; color: var(--gold)"></i>
        </div>

        <div class="wallet-header">
            <small style="opacity: 0.7; font-weight: 600; letter-spacing: 1px;">NET PORTFOLIO VALUE</small>
            <h1 style="font-size: 3rem; margin: 10px 0;">Rs <span id="u-wallet">0.00</span></h1>
            <div style="display: flex; gap: 20px; margin-top: 15px; border-top: 1px solid rgba(255,255,255,0.1); padding-top: 15px;">
                <div><small style="opacity: 0.6;">Today's Mining</small><br><b style="color:var(--gold-light)" id="u-profit">0.00</b></div>
                <div><small style="opacity: 0.6;">Referral Team</small><br><b id="u-team">0 Members</b></div>
            </div>
        </div>

        <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; padding: 0 15px;">
            <div class="glass-card" onclick="nav('depo-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-plus-circle" style="color:var(--gold)"></i><br><small style="font-weight:700; font-size:0.6rem;">Deposit</small></div>
            <div class="glass-card" onclick="nav('with-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-wallet"></i><br><small style="font-weight:700; font-size:0.6rem;">Withdraw</small></div>
            <div class="glass-card" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')" style="margin:0; text-align:center; padding:15px;"><i class="fab fa-whatsapp" style="color:#25D366"></i><br><small style="font-weight:700; font-size:0.6rem;">Support</small></div>
            <div class="glass-card" onclick="nav('promo-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-ticket" style="color:#f59e0b"></i><br><small style="font-weight:700; font-size:0.6rem;">Promo</small></div>
        </div>

        <h3 style="padding: 25px 20px 10px; font-weight: 800;">VIP Elite Nodes <span class="vip-badge">LIMITED</span></h3>
        <div id="vip-list" style="display: flex; overflow-x: auto; gap: 15px; padding: 0 15px; scrollbar-width: none;"></div>

        <h3 style="padding: 25px 20px 10px; font-weight: 800;">Regular Mining Nodes</h3>
        <div id="reg-list" style="padding: 0 5px;"></div>
    </section>

    <!-- PROMO CODE PAGE -->
    <section id="promo-pg" class="page">
        <h2 style="padding:20px;">Redeem Code</h2>
        <div class="glass-card">
            <p style="color:var(--sub); margin-bottom:15px; font-size:0.8rem;">Enter the code shared in our WhatsApp group to claim rewards.</p>
            <div class="input-grp" style="padding:0;"><input type="text" id="p-code" placeholder="Enter Promo Code"></div>
            <button class="gold-btn" onclick="submitPromo()">Claim Reward</button>
        </div>
    </section>

    <!-- DEPOSIT & WITHDRAW (SIMPLIFIED FOR PREVIEW) -->
    <section id="depo-pg" class="page">
        <h2 style="padding:20px;">Secure Deposit</h2>
        <div class="glass-card">
            <p style="font-size:0.7rem; color:var(--sub); margin-bottom:15px;">Official Accounts:<br><b>03705519562 (JazzCash/SadaPay)</b><br><b>03379827882 (EasyPaisa)</b></p>
            <div class="input-grp" style="padding:0;"><input type="number" id="d-amt" placeholder="Amount (5% Tax Applied)"></div>
            <div class="input-grp" style="padding:0;"><input type="text" id="d-tid" placeholder="Enter TID (Proof)"></div>
            <button class="gold-btn" onclick="handleDeposit()">Submit for Approval</button>
        </div>
    </section>

    <section id="with-pg" class="page">
        <h2 style="padding:20px;">Request Payout</h2>
        <div class="glass-card">
            <div class="input-grp" style="padding:0;"><input type="number" id="w-amt" placeholder="Amount"></div>
            <div class="input-grp" style="padding:0;"><input type="text" id="w-acc" placeholder="Account Number"></div>
            <div class="input-grp" style="padding:0;"><select id="w-met"><option>JazzCash</option><option>EasyPaisa</option><option>SadaPay</option><option>Bank Transfer</option><option>Binance</option></select></div>
            <button class="gold-btn" onclick="handleWithdraw()">Process Payout</button>
        </div>
    </section>

    <!-- ADMIN -->
    <section id="admin-pg" class="page">
        <h2 style="padding:20px;">Control Tower</h2>
        <div id="admin-list" style="padding: 0 10px;"></div>
    </section>

    <!-- NAVIGATION -->
    <nav class="nav-bar" id="bot-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-pg')"><i class="fa fa-home-alt"></i>Home</div>
        <div class="nav-item" onclick="nav('depo-pg')"><i class="fa fa-arrow-down-long"></i>Deposit</div>
        <div class="nav-item" onclick="nav('with-pg')"><i class="fa fa-arrow-up-long"></i>Withdraw</div>
        <div class="nav-item" onclick="logout()"><i class="fa fa-power-off"></i>Exit</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_v4_user');
        let isSignup = false;

        function checkSession() {
            if(user) {
                document.getElementById('bot-nav').style.display = 'flex';
                if(user === "03705519562") { nav('admin-pg'); loadAdmin(); }
                else { nav('dash-pg'); syncUser(); }
            }
            renderPlans();
        }

        function toggleAuth() {
            isSignup = !isSignup;
            document.getElementById('signup-fields').style.display = isSignup ? 'block' : 'none';
            document.getElementById('t-btn').innerText = isSignup ? "Already a Partner? Login" : "New Partner? Create Account";
        }

        function handleAuth() {
            const ph = document.getElementById('auth-ph').value;
            const ps = document.getElementById('auth-ps').value;
            const ref = document.getElementById('auth-ref').value;

            if(ph === "03705519562" && ps === "admin786") {
                localStorage.setItem('pg_v4_user', ph); location.reload(); return;
            }

            db.ref('users/'+ph).once('value', s => {
                if(isSignup) {
                    if(s.exists()) return alert("Already registered!");
                    const myID = "PG" + Math.floor(10000 + Math.random()*90000);
                    db.ref('users/'+ph).set({phone:ph, pass:ps, wallet:0, profit:0, speed:0, myRef:myID, refBy:ref||'none'});
                    if(ref) {
                        db.ref('users').orderByChild('myRef').equalTo(ref).once('value', r => {
                            r.forEach(c => db.ref('users/'+c.key).update({wallet: c.val().wallet + 100}));
                        });
                    }
                    alert("Welcome to the Elite!");
                }
                localStorage.setItem('pg_v4_user', ph); location.reload();
            });
        }

        function syncUser() {
            db.ref('users/'+user).on('value', s => {
                const d = s.val();
                document.getElementById('u-wallet').innerText = d.wallet.toFixed(2);
                document.getElementById('u-profit').innerText = d.profit.toFixed(2);
                document.getElementById('u-ph-top').innerText = d.phone;
                document.getElementById('u-id-top').innerText = "ID: " + d.myRef;
            });
            setInterval(() => {
                db.ref('users/'+user).once('value', s => {
                    const d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed});
                });
            }, 1000);
        }

        function renderPlans() {
            const reg = document.getElementById('reg-list');
            const vip = document.getElementById('vip-list');
            
            // 15 Regular Plans (Starting from 200)
            for(let i=1; i<=15; i++) {
                const cost = 200 * i;
                const earn = (cost * 0.1).toFixed(0);
                reg.innerHTML += `<div class="glass-card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><i class="fa fa-microchip" style="color:var(--gold)"></i> <b>Node V${i}</b><br><small>Earn Rs ${earn}/day</small></div>
                    <button class="gold-btn" style="width:auto; padding:8px 15px; font-size:0.7rem;" onclick="buy(${cost}, ${earn/86400})">Rs ${cost}</button>
                </div>`;
            }

            // 5 VIP Plans
            for(let i=1; i<=5; i++) {
                const cost = 10000 * i;
                const earn = (cost * 0.2).toFixed(0);
                vip.innerHTML += `<div class="glass-card" style="min-width:200px; text-align:center;">
                    <i class="fa fa-crown" style="color:var(--gold); font-size:2rem;"></i><br><b>VIP ELITE ${i}</b><br>
                    <b style="color:var(--success)">+Rs ${earn}/Day</b><br><br>
                    <button class="gold-btn" onclick="buy(${cost}, ${earn/86400})">Invest ${cost}</button>
                </div>`;
            }
        }

        function buy(p, s) {
            db.ref('users/'+user).once('value', v => {
                if(v.val().wallet < p) return alert("Insufficient Capital!");
                db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s});
                alert("Node Activation Successful! 🚀");
            });
        }

        function submitPromo() {
            const code = document.getElementById('p-code').value;
            db.ref('promo_requests').push({user: user, code: code, status: 'Pending'});
            alert("Promo request sent to Admin! Wait for bonus.");
        }

        function handleDeposit() {
            const amt = document.getElementById('d-amt').value;
            const tid = document.getElementById('d-tid').value;
            const final = amt * 0.95; // 5% Tax
            db.ref('requests').push({type:'Deposit', user:user, amt:final, raw:amt, tid:tid, status:'Pending'});
            alert("TID Submitted! Admin will verify.");
        }

        function handleWithdraw() {
            const amt = document.getElementById('w-amt').value;
            const acc = document.getElementById('w-acc').value;
            const met = document.getElementById('w-met').value;
            db.ref('users/'+user).once('value', s => {
                if(s.val().wallet < amt) return alert("Low Assets!");
                const final = amt * 0.95;
                db.ref('requests').push({type:'Withdraw', user:user, amt:final, raw:amt, acc:acc, method:met, status:'Pending'});
                db.ref('users/'+user).update({wallet: s.val().wallet - amt});
                alert("Withdrawal Requested! 5% Tax Deducted.");
            });
        }

        function loadAdmin() {
            db.ref('requests').on('value', s => {
                let h = "";
                s.forEach(c => {
                    if(c.val().status === 'Pending') {
                        h += `<div class="glass-card">
                            <b>${c.val().type}</b> | User: ${c.val().user}<br>
                            Amt: ${c.val().raw} (To Add: ${c.val().amt})<br>
                            Info: ${c.val().tid || c.val().acc}<br><br>
                            <button class="gold-btn" style="background:var(--success)" onclick="approve('${c.key}','${c.val().user}',${c.val().amt},'${c.val().type}')">Approve</button>
                            <button class="gold-btn" style="background:var(--error); margin-top:5px;" onclick="reject('${c.key}','${c.val().user}',${c.val().raw},'${c.val().type}')">Reject</button>
                        </div>`;
                    }
                });
                document.getElementById('admin-list').innerHTML = h || "<p style='text-align:center'>No Pending Tasks</p>";
            });
        }

        function approve(k, u, a, t) {
            if(t==='Deposit') db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a}));
            db.ref('requests/'+k).update({status:'Approved'});
        }

        function reject(k, u, a, t) {
            if(t==='Withdraw') db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: s.val().wallet + a}));
            db.ref('requests/'+k).update({status:'Rejected'});
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        }

        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
