<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Professional Investment Portal</title>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
    
    <style>
        :root { 
            --primary: #d4af37; 
            --secondary: #2c3e50;
            --bg: #ffffff; 
            --surface: #f8f9fa;
            --text: #1a1a1a;
            --border: #e9ecef;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* LIGHT MODE UI */
        .page { display: none; min-height: 100vh; padding-bottom: 100px; animation: fadeIn 0.3s ease; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* HEADER & DRAWER */
        .header { background: #fff; padding: 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid var(--border); position: sticky; top: 0; z-index: 1000; }
        .drawer { position: fixed; left: -100%; top: 0; width: 300px; height: 100%; background: #fff; z-index: 2000; transition: 0.4s; padding: 40px 20px; box-shadow: 5px 0 25px rgba(0,0,0,0.1); }
        .drawer.open { left: 0; }
        .drawer-item { padding: 15px; border-bottom: 1px solid var(--border); cursor: pointer; display: flex; align-items: center; gap: 12px; font-weight: 500; color: var(--secondary); }
        .overlay { position: fixed; display: none; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.4); z-index: 1500; backdrop-filter: blur(2px); }

        /* DASHBOARD CARDS */
        .hero-card { background: linear-gradient(135deg, #fdfbfb 0%, #ebedee 100%); margin: 20px; padding: 30px; border-radius: 25px; border: 1px solid var(--border); box-shadow: 0 10px 20px rgba(0,0,0,0.02); }
        .profit-val { font-size: 2.5rem; font-weight: 700; color: var(--primary); margin: 10px 0; }

        /* GRID SYSTEM */
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; padding: 0 20px; }
        .item-card { background: #fff; padding: 20px; border-radius: 20px; border: 1px solid var(--border); text-align: center; transition: 0.3s; }
        .item-card:active { transform: scale(0.95); background: #fdfaf0; }

        /* FORMS & INPUTS */
        .input-group { margin: 20px; padding: 25px; background: var(--surface); border-radius: 25px; border: 1px solid var(--border); }
        input, select { width: 100%; padding: 15px; margin-bottom: 15px; border: 1px solid var(--border); border-radius: 12px; font-size: 1rem; outline: none; background: #fff; }
        .btn-gold { background: var(--primary); color: #fff; border: none; padding: 18px; width: 100%; border-radius: 15px; font-weight: 700; cursor: pointer; box-shadow: 0 8px 15px rgba(212, 175, 55, 0.2); }

        /* NAVIGATION */
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: #fff; display: flex; justify-content: space-around; padding: 15px; border-top: 1px solid var(--border); z-index: 1000; }
        .nav-item { text-align: center; color: #95a5a6; font-size: 0.75rem; cursor: pointer; }
        .nav-item.active { color: var(--primary); }

        /* ADMIN PANEL */
        .admin-box { background: #fff; border: 2px solid #34495e; padding: 15px; margin: 10px; border-radius: 12px; }
    </style>
</head>
<body onload="bootApp()">

    <div class="overlay" id="ov" onclick="toggleMenu()"></div>
    <div class="drawer" id="dr">
        <h2 style="color:var(--primary); margin-bottom:30px;">PAK GOLD ⚜️</h2>
        <div class="drawer-item" onclick="nav('home')"><i class="fa fa-home"></i> Home</div>
        <div class="drawer-item" onclick="nav('team')"><i class="fa fa-users"></i> My Team</div>
        <div class="drawer-item" onclick="nav('promo')"><i class="fa fa-gift"></i> Promo Code</div>
        <div class="drawer-item" onclick="nav('legal')"><i class="fa fa-shield"></i> Privacy & Policy</div>
        <div class="drawer-item" onclick="nav('legal')"><i class="fa fa-info-circle"></i> Company Details</div>
        <div class="drawer-item" style="color:#e74c3c; margin-top:30px;" onclick="logout()"><i class="fa fa-sign-out"></i> Logout</div>
    </div>

    <section id="login" class="page active">
        <div style="height:100vh; display:flex; flex-direction:column; justify-content:center; padding:40px; text-align:center;">
            <h1 style="font-size:2.5rem; margin-bottom:10px;">Login</h1>
            <p style="color:#7f8c8d; margin-bottom:30px;">Enter your credentials to access portal</p>
            <input type="number" id="ph" placeholder="Phone Number">
            <input type="password" id="ak" placeholder="Terminal Access Key">
            <button class="btn-gold" onclick="auth()">Sign In</button>
        </div>
    </section>

    <section id="home" class="page">
        <div class="header">
            <div onclick="toggleMenu()"><i class="fa fa-bars" style="font-size:1.2rem;"></i></div>
            <div id="u-id" style="font-weight:700; color:var(--primary);">ID: Loading...</div>
        </div>
        <div class="hero-card">
            <small style="color:#7f8c8d; font-weight:600;">MINING PROFIT (PKR)</small>
            <div class="profit-val" id="u-pro">0.0000</div>
            <div style="display:flex; justify-content:space-between; margin-top:15px; border-top:1px solid #eee; padding-top:15px;">
                <div><small>WALLET</small><p id="u-wal" style="font-weight:700;">Rs 0</p></div>
                <div style="text-align:right;"><small>NODES</small><p style="color:#27ae60; font-weight:700;">Active</p></div>
            </div>
        </div>
        <div class="grid" id="nodes-container"></div>
    </section>

    <section id="legal" class="page">
        <div style="padding:25px;">
            <h2>Company Details</h2>
            <div class="input-group">
                <p><b>PakGold Intl.</b> is a registered cloud asset firm. We provide high-end server access for digital mining. All investments are secured via gold-backed liquidity pools.</p>
                <br>
                <h3>Privacy Policy</h3>
                <p>We do not share your mobile numbers or financial data with third parties. Withdrawals are processed within 24 hours.</p>
            </div>
        </div>
    </section>

    <section id="withdraw" class="page">
        <div class="input-group">
            <h2>Withdraw Funds</h2>
            <select id="w-method">
                <option value="JazzCash">JazzCash</option>
                <option value="EasyPaisa">EasyPaisa</option>
                <option value="Bank Transfer">Bank Transfer</option>
                <option value="Binance (USDT)">Binance (USDT)</option>
            </select>
            <input type="text" id="w-acc" placeholder="Account Number / Address">
            <input type="text" id="w-name" placeholder="Account Title / Username">
            <input type="number" id="w-amt" placeholder="Amount (Min 500)">
            <button class="btn-gold" onclick="submitWithdraw()">Request Withdrawal</button>
        </div>
    </section>

    <section id="deposit" class="page">
        <div class="input-group">
            <h2>Deposit Assets</h2>
            <div style="background:#fff9e6; border:1px solid #ffeaa7; padding:15px; border-radius:12px; margin-bottom:20px; font-size:0.9rem;">
                <b>JazzCash/SADA:</b> 03705519562<br>
                <b>EasyPaisa:</b> 03379827882<br>
                <small>*Kindly upload proof after payment</small>
            </div>
            <input type="number" id="d-amt" placeholder="Deposit Amount">
            <input type="text" id="d-tid" placeholder="Transaction ID (TID)">
            <p style="font-size:0.8rem; margin-bottom:10px;">Upload Screenshot (Optional):</p>
            <input type="file" id="d-proof" style="border:none;">
            <button class="btn-gold" onclick="submitDep()">Submit Payment</button>
        </div>
    </section>

    <section id="promo" class="page">
        <div class="input-group">
            <h2>Claim Promo</h2>
            <input type="text" id="pc" placeholder="Enter Promo Code">
            <button class="btn-gold" onclick="applyPromo()">Claim Reward</button>
        </div>
    </section>

    <section id="admin" class="page" style="background:#f1f2f6; padding:20px;">
        <h2>Management Console</h2>
        <div id="admin-reqs"></div>
    </section>

    <nav class="nav-bar" id="nb" style="display:none;">
        <div class="nav-item active" onclick="nav('home')"><i class="fa fa-home"></i><br>Home</div>
        <div class="nav-item" onclick="nav('deposit')"><i class="fa fa-plus-circle"></i><br>Deposit</div>
        <div class="nav-item" onclick="nav('withdraw')"><i class="fa fa-money-bill"></i><br>Withdraw</div>
        <div class="nav-item" onclick="toggleMenu()"><i class="fa fa-ellipsis-h"></i><br>More</div>
    </nav>

    <script>
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
        let userPh = localStorage.getItem('up');

        function bootApp() {
            if(userPh) { nav('home'); sync(); }
            renderNodes();
        }

        function auth() {
            let p = document.getElementById('ph').value;
            let k = document.getElementById('ak').value;
            if(k === "pakgold786") return nav('admin'), loadAdmin();
            if(p.length < 10) return alert("Invalid Phone");
            userPh = p;
            localStorage.setItem('up', p);
            db.ref('users/'+p).once('value', s => {
                if(!s.exists()) db.ref('users/'+p).set({wallet:0, profit:0, speed:0, id:'PG'+Math.floor(Math.random()*90000)});
                location.reload();
            });
        }

        function sync() {
            db.ref('users/'+userPh).on('value', s => {
                let d = s.val();
                document.getElementById('u-wal').innerText = "Rs "+d.wallet;
                document.getElementById('u-pro').innerText = parseFloat(d.profit).toFixed(4);
                document.getElementById('u-id').innerText = "ID: "+d.id;
            });
            setInterval(() => {
                db.ref('users/'+userPh).once('value', s => {
                    let d = s.val();
                    if(d.speed > 0) db.ref('users/'+userPh).update({profit: (parseFloat(d.profit)+d.speed).toFixed(4)});
                });
            }, 1000);
        }

        function renderNodes() {
            let c = document.getElementById('nodes-container');
            for(let i=1; i<=15; i++) {
                c.innerHTML += `<div class="item-card" onclick="buy(${i*600}, ${0.0004*i})">
                    <p style="font-size:0.7rem; color:#7f8c8d;">NODE ${i}</p>
                    <b>Rs ${i*600}</b><br>
                    <small>Speed: ${i*0.0004}/s</small>
                </div>`;
            }
        }

        function buy(p, s) {
            db.ref('users/'+userPh).once('value', d => {
                if(d.val().wallet < p) return alert("Insufficient Balance");
                db.ref('users/'+userPh).update({wallet: d.val().wallet - p, speed: s});
                alert("Server Activated");
            });
        }

        function submitDep() {
            let a = document.getElementById('d-amt').value;
            let t = document.getElementById('d-tid').value;
            db.ref('admin_reqs/').push({user:userPh, amt:a, tid:t, status:'pending', type:'Deposit'});
            alert("Deposit Request Submitted");
        }

        function submitWithdraw() {
            let a = document.getElementById('w-amt').value;
            let m = document.getElementById('w-method').value;
            let acc = document.getElementById('w-acc').value;
            let n = document.getElementById('w-name').value;
            if(a < 500) return alert("Min Withdraw Rs 500");
            db.ref('admin_reqs/').push({user:userPh, amt:a, method:m, acc:acc, name:n, status:'pending', type:'Withdraw'});
            alert("Withdrawal Request Submitted");
        }

        function applyPromo() {
            let c = document.getElementById('pc').value;
            db.ref('promos/'+c).once('value', s => {
                if(s.exists()) {
                    db.ref('admin_reqs/').push({user:userPh, code:c, status:'pending', type:'Promo'});
                    alert("Promo request sent to Admin");
                } else alert("Invalid Code");
            });
        }

        function loadAdmin() {
            db.ref('admin_reqs/').on('value', s => {
                let cont = document.getElementById('admin-reqs'); cont.innerHTML = "";
                s.forEach(child => {
                    let r = child.val();
                    if(r.status === 'pending') {
                        cont.innerHTML += `<div class="admin-box">
                            <b>${r.type}</b> | User: ${r.user} | Val: ${r.amt || r.code}<br>
                            Details: ${r.tid || r.method + ' ' + r.acc}<br>
                            <button onclick="approve('${child.key}','${r.user}',${r.amt || 0},'${r.type}')" style="background:green; color:white;">Approve</button>
                            <button onclick="reject('${child.key}')" style="background:red; color:white;">Reject</button>
                        </div>`;
                    }
                });
            });
        }

        function approve(k, ph, amt, type) {
            db.ref('users/'+ph).once('value', s => {
                let bonus = (type === 'Promo') ? 100 : parseInt(amt);
                db.ref('users/'+ph).update({wallet: s.val().wallet + bonus});
                db.ref('admin_reqs/'+k).update({status:'approved'});
            });
        }

        function reject(k) { db.ref('admin_reqs/'+k).remove(); alert("Rejected"); }
        function toggleMenu() {
            document.getElementById('dr').classList.toggle('open');
            document.getElementById('ov').style.display = (document.getElementById('ov').style.display === 'block') ? 'none' : 'block';
        }
        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('nb').style.display = (id === 'login' || id === 'admin') ? 'none' : 'flex';
            if(document.getElementById('dr').classList.contains('open')) toggleMenu();
        }
        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
