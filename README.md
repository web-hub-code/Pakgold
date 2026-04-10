<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Enterprise Edition</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { --neon: #ffcc00; --bg: #f8fafc; --card: #ffffff; --text: #0f172a; --sub: #64748b; }
        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Animations */
        .page { display:none; animation: fadeIn 0.4s ease-out; padding-bottom: 100px; min-height: 100vh; }
        .page.active { display:block; }
        @keyframes fadeIn { from { opacity:0; transform: translateY(10px); } to { opacity:1; transform: translateY(0); } }

        /* Premium UI Components */
        .glass-card { background: var(--card); margin: 15px; padding: 25px; border-radius: 30px; border: 1px solid rgba(0,0,0,0.03); box-shadow: 0 10px 30px rgba(0,0,0,0.02); }
        .neon-btn { background: var(--text); color: white; border: none; padding: 15px; border-radius: 18px; font-weight: 800; cursor: pointer; transition: 0.3s; width: 100%; }
        .neon-btn:active { background: var(--neon); color: black; transform: scale(0.98); }
        
        .input-grp { margin-bottom: 15px; }
        .input-grp input { width: 100%; padding: 18px; border-radius: 18px; border: 2px solid #f1f5f9; background: #f8fafc; font-size: 1rem; outline: none; }
        .input-grp input:focus { border-color: var(--neon); }

        /* Navigation */
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.9); backdrop-filter: blur(15px); display: flex; justify-content: space-around; padding: 15px 0 30px; border-top: 1px solid #f1f5f9; z-index: 999; }
        .nav-item { color: var(--sub); text-align: center; font-size: 0.6rem; font-weight: 700; }
        .nav-item.active { color: var(--neon); }
        .nav-item i { font-size: 1.4rem; display: block; margin-bottom: 4px; }

        /* Specifics */
        .badge { background: var(--neon); color: black; padding: 4px 10px; border-radius: 8px; font-size: 0.6rem; font-weight: 900; }
        .history-item { display: flex; justify-content: space-between; padding: 15px; border-bottom: 1px solid #f1f5f9; font-size: 0.8rem; }
    </style>
</head>
<body onload="checkSession()">

    <!-- AUTH PAGE -->
    <section id="auth-page" class="page active" style="padding: 50px 30px;">
        <h1 style="font-size: 2.5rem; font-weight: 800; margin-bottom: 10px;">PAK <span style="color:var(--neon)">GOLD</span></h1>
        <p id="auth-sub" style="color:var(--sub); margin-bottom: 40px;">Sign in to continue mining.</p>
        
        <div class="input-grp"><input type="number" id="auth-phone" placeholder="Phone Number"></div>
        <div class="input-grp"><input type="password" id="auth-pass" placeholder="Password"></div>
        <div id="signup-fields" style="display:none;">
            <div class="input-grp"><input type="text" id="auth-ref" placeholder="Referral Code (Optional)"></div>
        </div>
        
        <button class="neon-btn" id="main-auth-btn" onclick="handleAuth()">LOGIN NOW</button>
        <p onclick="toggleAuth()" style="text-align:center; margin-top:20px; font-weight:700; font-size:0.8rem; color:var(--sub);" id="toggle-txt">Don't have an account? Signup</p>
    </section>

    <!-- DASHBOARD -->
    <section id="dash-page" class="page">
        <div style="padding: 20px; display: flex; justify-content: space-between; align-items: center;">
            <b>CORE <span style="color:var(--neon)">V2</span></b>
            <div class="badge" onclick="nav('profile-page')">PRO USER</div>
        </div>

        <div class="glass-card" style="background: var(--text); color: white;">
            <small style="opacity: 0.6; font-weight: 700;">TOTAL ASSETS</small>
            <h1 style="font-size: 3rem; margin: 10px 0;">Rs <span id="u-wallet">0</span></h1>
            <div style="display: flex; gap: 20px; margin-top: 15px;">
                <div><small style="opacity: 0.6;">Profit</small><br><b style="color:var(--neon)" id="u-profit">0.00</b></div>
                <div><small style="opacity: 0.6;">Team</small><br><b id="u-team-count">0</b></div>
            </div>
        </div>

        <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; padding: 0 15px;">
            <div class="glass-card" onclick="nav('depo-page')" style="padding: 15px; text-align: center; margin: 0;"><i class="fa fa-plus-circle" style="color:var(--neon)"></i><br><span style="font-size:0.6rem; font-weight:700;">Deposit</span></div>
            <div class="glass-card" onclick="nav('with-page')" style="padding: 15px; text-align: center; margin: 0;"><i class="fa fa-wallet"></i><br><span style="font-size:0.6rem; font-weight:700;">Withdraw</span></div>
            <div class="glass-card" onclick="nav('promo-page')" style="padding: 15px; text-align: center; margin: 0;"><i class="fa fa-ticket"></i><br><span style="font-size:0.6rem; font-weight:700;">Promo</span></div>
            <div class="glass-card" onclick="nav('team-page')" style="padding: 15px; text-align: center; margin: 0;"><i class="fa fa-users"></i><br><span style="font-size:0.6rem; font-weight:700;">Team</span></div>
        </div>

        <h3 style="padding: 25px 20px 10px;">Active Mining Nodes</h3>
        <div id="nodes-list"></div>
    </section>

    <!-- ADMIN PANEL -->
    <section id="admin-page" class="page">
        <h2 style="padding: 20px;">Master Administration</h2>
        <div class="glass-card">
            <h4>System Control</h4>
            <p style="font-size:0.7rem; color:var(--sub);">Update user balances manually.</p>
            <div class="input-grp" style="margin-top:15px;"><input type="number" id="adm-target" placeholder="User Phone"></div>
            <div class="input-grp"><input type="number" id="adm-amount" placeholder="Amount to Add"></div>
            <button class="neon-btn" onclick="adminAddFund()">INJECT FUNDS</button>
        </div>
        <div id="all-users-list" style="padding: 20px;"></div>
    </section>

    <!-- PROMO CODE PAGE -->
    <section id="promo-page" class="page">
        <h2 style="padding: 20px;">Redeem Code</h2>
        <div class="glass-card">
            <input type="text" id="promo-input" class="input-box" placeholder="Enter Code (e.g. PAK2026)" style="width:100%; padding:15px; border-radius:10px; border:1px solid #ddd; margin-bottom:15px;">
            <button class="neon-btn" onclick="applyPromo()">REDEEM</button>
        </div>
    </section>

    <!-- TEAM PAGE -->
    <section id="team-page" class="page">
        <h2 style="padding: 20px;">My Network</h2>
        <div class="glass-card">
            <small>Your Referral ID</small>
            <h3 id="u-my-ref">----</h3>
        </div>
        <div id="team-list" style="padding: 20px;"></div>
    </section>

    <!-- COMPANY DETAILS & FAQ -->
    <section id="info-page" class="page">
        <h2 style="padding: 20px;">Company & Policy</h2>
        <div class="glass-card">
            <h4>About PAK GOLD</h4>
            <p style="font-size:0.8rem; color:var(--sub);">We are a verified mining consortium based in Karachi, operating since 2024 with a focus on neural-link gold mining technology.</p>
        </div>
        <div class="glass-card">
            <h4>Privacy Policy</h4>
            <p style="font-size:0.8rem; color:var(--sub);">Your data is encrypted via 256-bit SSL protocol. We never share your contact details with third parties.</p>
        </div>
        <div class="glass-card">
            <h4>FAQ</h4>
            <details style="font-size:0.8rem; padding:10px 0;">
                <summary>Minimum Withdrawal?</summary>
                <p>Minimum withdrawal is Rs 500 via EasyPaisa/JazzCash.</p>
            </details>
        </div>
    </section>

    <!-- NAV BAR -->
    <nav class="nav-bar" id="bottom-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-page')"><i class="fa fa-house"></i>Home</div>
        <div class="nav-item" onclick="nav('team-page')"><i class="fa fa-users-viewfinder"></i>Team</div>
        <div class="nav-item" onclick="nav('info-page')"><i class="fa fa-file-contract"></i>Info</div>
        <div class="nav-item" onclick="nav('dash-page')"><i class="fa fa-user"></i>Me</div>
    </nav>

    <script>
        // FIREBASE SETUP
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();

        let currentUser = localStorage.getItem('pg_user');
        let isSignup = false;

        function checkSession() {
            if(currentUser) { 
                if(currentUser === "78692") nav('admin-page');
                else { nav('dash-page'); syncUserData(); }
                document.getElementById('bottom-nav').style.display = 'flex';
            }
            loadNodes();
        }

        function toggleAuth() {
            isSignup = !isSignup;
            document.getElementById('signup-fields').style.display = isSignup ? 'block' : 'none';
            document.getElementById('main-auth-btn').innerText = isSignup ? 'CREATE ACCOUNT' : 'LOGIN NOW';
            document.getElementById('auth-sub').innerText = isSignup ? 'Join the gold revolution.' : 'Sign in to continue mining.';
            document.getElementById('toggle-txt').innerText = isSignup ? 'Already have an account? Login' : "Don't have an account? Signup";
        }

        function handleAuth() {
            const ph = document.getElementById('auth-phone').value;
            const ps = document.getElementById('auth-pass').value;
            const ref = document.getElementById('auth-ref').value;

            if(ph === "78692" && ps === "admin123") {
                localStorage.setItem('pg_user', "78692");
                location.reload(); return;
            }

            if(ph.length < 10) return alert("Valid phone required");

            db.ref('users/' + ph).once('value', s => {
                if(isSignup) {
                    if(s.exists()) return alert("User already exists");
                    db.ref('users/' + ph).set({
                        phone: ph, pass: ps, wallet: 0, profit: 0, speed: 0, 
                        refBy: ref || "none", myRef: "PG" + Math.floor(1000 + Math.random() * 9000)
                    });
                    alert("Account Created!");
                } else {
                    if(!s.exists() || s.val().pass !== ps) return alert("Wrong details");
                }
                localStorage.setItem('pg_user', ph);
                location.reload();
            });
        }

        function syncUserData() {
            db.ref('users/' + currentUser).on('value', s => {
                const d = s.val();
                document.getElementById('u-wallet').innerText = d.wallet.toLocaleString();
                document.getElementById('u-profit').innerText = d.profit.toFixed(2);
                document.getElementById('u-my-ref').innerText = d.myRef;
            });
            // Passive Income Engine
            setInterval(() => {
                db.ref('users/' + currentUser).once('value', s => {
                    const d = s.val(); if(d.speed > 0) db.ref('users/'+currentUser).update({profit: d.profit + d.speed});
                });
            }, 1000);
        }

        function loadNodes() {
            const nodes = [
                {n:'Starter Node', p:500, d:50}, {n:'Premium Node', p:2000, d:250}, {n:'Ultra Node', p:10000, d:1400}
            ];
            let list = document.getElementById('nodes-list');
            nodes.forEach(i => {
                list.innerHTML += `<div class="glass-card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><b>${i.n}</b><br><small>Earn Rs ${i.d}/day</small></div>
                    <button class="neon-btn" style="width:auto; padding:10px 20px;" onclick="buyNode(${i.p}, ${i.d/86400})">Rs ${i.p}</button>
                </div>`;
            });
        }

        function buyNode(p, s) {
            db.ref('users/'+currentUser).once('value', v => {
                if(v.val().wallet < p) return alert("Low balance");
                db.ref('users/'+currentUser).update({wallet: v.val().wallet - p, speed: v.val().speed + s});
                alert("Node Started!");
            });
        }

        function applyPromo() {
            const code = document.getElementById('promo-input').value;
            if(code === "FREE500") {
                db.ref('users/'+currentUser).once('value', s => {
                    db.ref('users/'+currentUser).update({wallet: s.val().wallet + 500});
                    alert("Rs 500 Bonus Added!");
                });
            } else { alert("Invalid Code"); }
        }

        function adminAddFund() {
            const target = document.getElementById('adm-target').value;
            const amt = parseInt(document.getElementById('adm-amount').value);
            db.ref('users/'+target).once('value', s => {
                if(!s.exists()) return alert("User not found");
                db.ref('users/'+target).update({wallet: s.val().wallet + amt});
                alert("Funds Injected!");
            });
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
        }
    </script>
</body>
</html>
