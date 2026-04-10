<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Official Premium</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { --neon: #ffcc00; --bg: #0b0f19; --card: #161b2c; --text: #ffffff; --sub: #94a3b8; --acc: #22c55e; --err: #ef4444; }
        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Plus Jakarta Sans', sans-serif; }
        body { background: var(--bg); color: var(--text); }

        /* Modern UI */
        .page { display:none; animation: slideUp 0.4s ease; padding-bottom: 100px; }
        .page.active { display:block; }
        @keyframes slideUp { from { opacity:0; transform: translateY(20px); } to { opacity:1; transform: translateY(0); } }

        .glass-card { background: var(--card); margin: 15px; padding: 20px; border-radius: 24px; border: 1px solid rgba(255,255,255,0.05); }
        .input-grp { margin-bottom: 15px; }
        .input-grp input, .input-grp select { width: 100%; padding: 15px; border-radius: 12px; border: 1px solid #2d3748; background: #0b0f19; color: white; outline: none; }
        
        .neon-btn { background: var(--neon); color: #000; border: none; padding: 15px; border-radius: 15px; font-weight: 800; cursor: pointer; width: 100%; box-shadow: 0 4px 15px rgba(255,204,0,0.3); }
        .neon-btn.reject { background: var(--err); color: white; }

        /* Header */
        .top-bar { padding: 20px; display: flex; justify-content: space-between; align-items: center; }
        .user-id-chip { background: rgba(255,255,255,0.1); padding: 5px 12px; border-radius: 20px; font-size: 0.7rem; color: var(--neon); }

        /* Nav */
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(22,27,44,0.95); backdrop-filter: blur(10px); display: flex; justify-content: space-around; padding: 12px 0 25px; border-top: 1px solid rgba(255,255,255,0.05); }
        .nav-item { color: var(--sub); text-align: center; font-size: 0.6rem; }
        .nav-item.active { color: var(--neon); }
        .nav-item i { font-size: 1.4rem; display: block; margin-bottom: 4px; }
    </style>
</head>
<body onload="checkSession()">

    <!-- AUTH SECTION -->
    <section id="auth-pg" class="page active" style="padding: 40px 20px;">
        <h1 style="color:var(--neon); font-size: 2.5rem; font-weight: 800;">PAK GOLD</h1>
        <p style="color:var(--sub); margin-bottom: 30px;">Login to your trusted dashboard</p>
        <div class="input-grp"><input type="number" id="auth-ph" placeholder="Phone Number"></div>
        <div class="input-grp"><input type="password" id="auth-ps" placeholder="Password"></div>
        <div id="signup-box" style="display:none;">
            <div class="input-grp"><input type="text" id="auth-ref" placeholder="Referral Code (Optional)"></div>
        </div>
        <button class="neon-btn" onclick="handleAuth()">ENTER SYSTEM</button>
        <p onclick="toggleAuth()" style="text-align:center; margin-top:20px; font-size:0.8rem;" id="t-txt">New user? Signup</p>
    </section>

    <!-- DASHBOARD -->
    <section id="dash-pg" class="page">
        <div class="top-bar">
            <div><small style="color:var(--sub)">Welcome back,</small><br><b id="u-ph-display">User</b></div>
            <div class="user-id-chip" id="u-id-display">ID: ---</div>
        </div>

        <div class="glass-card" style="background: linear-gradient(135deg, #ffcc00, #f59e0b); color: #000;">
            <p style="font-weight: 700; opacity: 0.8;">TOTAL BALANCE (After Tax)</p>
            <h1 style="font-size: 2.5rem; font-weight: 900;">Rs <span id="u-wallet">0</span></h1>
            <div style="display:flex; justify-content:space-between; margin-top:15px; background:rgba(0,0,0,0.1); padding:10px; border-radius:10px;">
                <span>Profit: <b id="u-profit">0</b></span>
                <span>Team: <b id="u-team">0</b></span>
            </div>
        </div>

        <div style="display:grid; grid-template-columns: repeat(4, 1fr); gap:10px; padding:0 15px;">
            <div class="glass-card" onclick="nav('depo-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-arrow-down" style="color:var(--neon)"></i><br><small>Deposit</small></div>
            <div class="glass-card" onclick="nav('with-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-arrow-up"></i><br><small>Withdraw</small></div>
            <div class="glass-card" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')" style="margin:0; text-align:center; padding:15px;"><i class="fab fa-whatsapp"></i><br><small>Group</small></div>
            <div class="glass-card" onclick="nav('promo-pg')" style="margin:0; text-align:center; padding:15px;"><i class="fa fa-gift"></i><br><small>Promo</small></div>
        </div>

        <h3 style="padding:20px;">Mining Nodes</h3>
        <div id="plan-list"></div>
    </section>

    <!-- DEPOSIT PAGE -->
    <section id="depo-pg" class="page">
        <h2 style="padding:20px;">Modern Deposit</h2>
        <div class="glass-card">
            <p style="font-size:0.7rem; color:var(--sub)">*5% System Tax applicable on all deposits</p><br>
            <div class="input-grp">
                <select id="depo-method">
                    <option value="JazzCash/SadaPay (03705519562)">JazzCash/SadaPay (03705519562)</option>
                    <option value="EasyPaisa (03379827882)">EasyPaisa (03379827882)</option>
                </select>
            </div>
            <div class="input-grp"><input type="number" id="depo-amt" placeholder="Amount (Min 500)"></div>
            <div class="input-grp"><input type="text" id="depo-tid" placeholder="TID Number (Transaction ID)"></div>
            <button class="neon-btn" onclick="submitDepo()">SUBMIT PROOF</button>
        </div>
    </section>

    <!-- WITHDRAW PAGE -->
    <section id="with-pg" class="page">
        <h2 style="padding:20px;">Withdraw Funds</h2>
        <div class="glass-card">
            <div class="input-grp">
                <select id="with-method">
                    <option value="JazzCash">JazzCash</option>
                    <option value="EasyPaisa">EasyPaisa</option>
                    <option value="SadaPay">SadaPay</option>
                    <option value="Binance (USDT)">Binance (USDT)</option>
                    <option value="Bank Transfer">Bank Transfer</option>
                </select>
            </div>
            <div class="input-grp"><input type="number" id="with-amt" placeholder="Amount"></div>
            <div class="input-grp"><input type="text" id="with-acc" placeholder="Account Number"></div>
            <div class="input-grp"><input type="text" id="with-name" placeholder="Account Holder Name"></div>
            <button class="neon-btn" onclick="submitWith()">REQUEST PAYOUT</button>
        </div>
    </section>

    <!-- ADMIN PANEL -->
    <section id="admin-pg" class="page">
        <h2 style="padding:20px;">Master Administration</h2>
        <div id="admin-req-list" style="padding:0 10px;"></div>
    </section>

    <!-- NAV BAR -->
    <nav class="nav-bar" id="bottom-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-pg')"><i class="fa fa-home"></i>Home</div>
        <div class="nav-item" onclick="nav('depo-pg')"><i class="fa fa-plus"></i>Depo</div>
        <div class="nav-item" onclick="nav('with-pg')"><i class="fa fa-wallet"></i>With</div>
        <div class="nav-item" onclick="logout()"><i class="fa fa-sign-out"></i>Exit</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_user');
        let isSignup = false;

        function checkSession() {
            if(user) {
                document.getElementById('bottom-nav').style.display = 'flex';
                if(user === "03705519562") { nav('admin-pg'); loadAdmin(); }
                else { nav('dash-pg'); syncUser(); }
            }
            renderPlans();
        }

        function toggleAuth() {
            isSignup = !isSignup;
            document.getElementById('signup-box').style.display = isSignup ? 'block' : 'none';
            document.getElementById('t-txt').innerText = isSignup ? "Already have account? Login" : "New user? Signup";
        }

        function handleAuth() {
            const ph = document.getElementById('auth-ph').value;
            const ps = document.getElementById('auth-ps').value;
            const ref = document.getElementById('auth-ref').value;

            if(ph === "03705519562" && ps === "admin786") {
                localStorage.setItem('pg_user', ph); location.reload(); return;
            }

            db.ref('users/'+ph).once('value', s => {
                if(isSignup) {
                    if(s.exists()) return alert("User exists");
                    const myID = "PG" + Math.floor(Math.random()*89999 + 10000);
                    db.ref('users/'+ph).set({phone:ph, pass:ps, wallet:0, profit:0, speed:0, myRef:myID, refBy:ref||'none'});
                    // Referral Bonus Logic
                    if(ref) {
                        db.ref('users').orderByChild('myRef').equalTo(ref).once('value', rs => {
                            rs.forEach(child => {
                                db.ref('users/'+child.key).update({wallet: child.val().wallet + 100}); // Rs 100 per referral
                            });
                        });
                    }
                    alert("Account Created!");
                }
                localStorage.setItem('pg_user', ph); location.reload();
            });
        }

        function syncUser() {
            db.ref('users/'+user).on('value', s => {
                const d = s.val();
                document.getElementById('u-wallet').innerText = Math.floor(d.wallet);
                document.getElementById('u-profit').innerText = d.profit.toFixed(2);
                document.getElementById('u-id-display').innerText = "ID: " + d.myRef;
                document.getElementById('u-ph-display').innerText = d.phone;
            });
            setInterval(() => {
                db.ref('users/'+user).once('value', s => {
                    const d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed});
                });
            }, 1000);
        }

        function submitDepo() {
            const amt = parseInt(document.getElementById('depo-amt').value);
            const tid = document.getElementById('depo-tid').value;
            const met = document.getElementById('depo-method').value;
            if(amt < 500) return alert("Min 500");
            const afterTax = amt * 0.95; // 5% Tax
            db.ref('requests').push({type:'Deposit', user:user, amt:afterTax, fullAmt:amt, tid:tid, method:met, status:'Pending'});
            alert("Deposit Proof Sent! Wait for Approval.");
        }

        function submitWith() {
            const amt = parseInt(document.getElementById('with-amt').value);
            const acc = document.getElementById('with-acc').value;
            const met = document.getElementById('with-method').value;
            db.ref('users/'+user).once('value', s => {
                if(s.val().wallet < amt) return alert("Low Balance");
                const afterTax = amt * 0.95;
                db.ref('requests').push({type:'Withdraw', user:user, amt:afterTax, fullAmt:amt, acc:acc, method:met, status:'Pending'});
                db.ref('users/'+user).update({wallet: s.val().wallet - amt});
                alert("Withdrawal Requested! Tax Deducted.");
            });
        }

        function loadAdmin() {
            db.ref('requests').on('value', s => {
                let html = "<h3>Pending Tasks</h3>";
                s.forEach(child => {
                    if(child.val().status === 'Pending') {
                        const d = child.val();
                        html += `<div class="glass-card">
                            <b>${d.type} [${d.method}]</b><hr style="opacity:0.1;margin:10px 0;">
                            User: ${d.user} | Amount: ${d.fullAmt}<br>
                            Detail: ${d.tid || d.acc}<br><br>
                            <button class="neon-btn" onclick="approve('${child.key}','${d.user}',${d.amt},'${d.type}')">APPROVE</button>
                            <button class="neon-btn reject" onclick="reject('${child.key}','${d.user}',${d.fullAmt},'${d.type}')" style="margin-top:5px;">REJECT</button>
                        </div>`;
                    }
                });
                document.getElementById('admin-req-list').innerHTML = html;
            });
        }

        function approve(key, uid, amt, type) {
            if(type === 'Deposit') {
                db.ref('users/'+uid).once('value', s => {
                    db.ref('users/'+uid).update({wallet: s.val().wallet + amt});
                });
            }
            db.ref('requests/'+key).update({status:'Approved'});
        }

        function reject(key, uid, amt, type) {
            if(type === 'Withdraw') { // Return money on reject
                db.ref('users/'+uid).once('value', s => {
                    db.ref('users/'+uid).update({wallet: s.val().wallet + amt});
                });
            }
            db.ref('requests/'+key).update({status:'Rejected'});
        }

        function renderPlans() {
            let list = document.getElementById('plan-list');
            for(let i=1; i<=15; i++) {
                list.innerHTML += `<div class="glass-card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><i class="fa fa-microchip" style="color:var(--neon)"></i> <b>Node V${i}</b><br><small>Rs ${i*50}/day</small></div>
                    <button class="neon-btn" style="width:auto; padding:8px 15px;" onclick="buy(${i*1000}, ${i*50/86400})">Rs ${i*1000}</button>
                </div>`;
            }
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        }

        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
