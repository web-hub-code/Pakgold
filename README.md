<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Ultimate Premium Investment</title>
    <!-- Modern Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { 
            --gold: #d4af37; --gold-dark: #b8860b; --dark: #1e272e; 
            --light: #f1f2f6; --success: #2ecc71; --glass: rgba(255, 255, 255, 0.95);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; scroll-behavior: smooth; }
        body { background: var(--light); color: var(--dark); overflow-x: hidden; }

        /* Page Management */
        .page { display: none; min-height: 100vh; animation: fadeIn 0.5s ease-in-out; }
        .active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* UI Elements */
        .card { background: white; padding: 25px; border-radius: 25px; box-shadow: 0 10px 30px rgba(0,0,0,0.05); margin-bottom: 20px; text-align: center; border: 1px solid #eee; }
        .btn-gold { 
            background: linear-gradient(135deg, var(--gold), var(--gold-dark)); 
            color: white; border: none; padding: 15px; border-radius: 50px; 
            width: 100%; font-weight: bold; cursor: pointer; transition: 0.3s;
            box-shadow: 0 10px 20px rgba(184, 134, 11, 0.3); font-size: 1rem;
        }
        .btn-gold:hover { transform: scale(1.02); box-shadow: 0 15px 25px rgba(184, 134, 11, 0.5); }

        /* --- 1. LANDING PAGE --- */
        #landing-page { background: radial-gradient(circle at top right, #fff9e6, #ffffff); display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center; padding: 20px; }
        .hero-title { font-size: 3.5rem; font-weight: 800; background: linear-gradient(90deg, var(--gold), #f1c40f); -webkit-background-clip: text; -webkit-text-fill-color: transparent; line-height: 1.1; }
        .floating-deco { position: absolute; font-size: 40px; opacity: 0.2; animation: float 3s infinite ease-in-out; }

        /* --- 2. LOGIN PAGE --- */
        #login-page { padding: 40px 20px; background: #fff; }
        .input-group { text-align: left; margin-bottom: 15px; }
        .input-group label { font-size: 0.8rem; color: #777; margin-left: 10px; }
        input { width: 100%; padding: 15px; border-radius: 15px; border: 2px solid #f1f1f1; outline: none; transition: 0.3s; font-size: 1rem; }
        input:focus { border-color: var(--gold); }

        /* --- 3. DASHBOARD --- */
        .dash-header { background: var(--dark); color: white; padding: 50px 20px; border-radius: 0 0 40px 40px; text-align: center; }
        .balance-box h1 { font-size: 2.8rem; margin: 10px 0; color: var(--gold-dark); }
        .mining-status { display: inline-block; background: rgba(46, 204, 113, 0.2); color: var(--success); padding: 5px 15px; border-radius: 20px; font-size: 0.8rem; border: 1px solid var(--success); }
        
        /* Stats Grid */
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin: -25px 20px 20px 20px; }
        .stat-card { background: white; padding: 15px; border-radius: 20px; box-shadow: 0 10px 20px rgba(0,0,0,0.05); text-align: center; }

        /* --- 4. REFERRAL & WITHDRAWAL --- */
        .ref-box { background: #f9f9f9; padding: 15px; border-radius: 15px; margin-top: 10px; font-size: 0.8rem; color: #555; word-break: break-all; }
        .policy-box { background: #fff9e6; border-left: 5px solid var(--gold); text-align: left; padding: 15px; font-size: 0.75rem; color: #888; line-height: 1.6; }

        /* Notifications & Floating Widgets */
        #notif-area { position: fixed; bottom: 100px; left: 20px; z-index: 1000; }
        .popup { background: var(--glass); padding: 12px 20px; border-radius: 15px; margin-top: 10px; box-shadow: 0 10px 25px rgba(0,0,0,0.1); border-left: 5px solid var(--success); font-size: 0.85rem; animation: slideIn 0.5s ease-out; }
        @keyframes slideIn { from { transform: translateX(-110%); } to { transform: translateX(0); } }

        .wa-btn { position: fixed; bottom: 30px; right: 20px; background: #25d366; color: white; padding: 15px 25px; border-radius: 50px; text-decoration: none; font-weight: bold; display: flex; align-items: center; gap: 10px; box-shadow: 0 10px 20px rgba(0,0,0,0.2); z-index: 1001; }

        @keyframes float { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-20px); } }
    </style>
</head>
<body onload="startSystem()">

    <!-- LANDING SECTION -->
    <section id="landing" class="page active">
        <div class="floating-deco" style="top:10%; left:10%;">💰</div>
        <div class="floating-deco" style="top:20%; right:15%; animation-delay:1s;">✨</div>
        <div class="hero-title">PAK GOLD<br>SOLUTIONS</div>
        <p style="margin: 20px 0 40px 0; color: #666; max-width: 300px;">The most trusted premium gold mining platform in the world.</p>
        <button class="btn-gold" style="width: 250px;" onclick="nav('login')">ENTER VAULT</button>
        <p style="margin-top: 30px; font-size: 0.7rem; color: #bbb;">Trusted by 25,000+ Active Miners</p>
    </section>

    <!-- LOGIN SECTION -->
    <section id="login" class="page">
        <div style="padding: 60px 25px;">
            <h2 style="font-size: 2rem; margin-bottom: 10px;">Login</h2>
            <p style="color: #888; margin-bottom: 30px;">Access your secure mining dashboard</p>
            <div class="input-group">
                <label>Phone Number</label>
                <input type="text" placeholder="03xxxxxxxxx">
            </div>
            <div class="input-group">
                <label>Account PIN</label>
                <input type="password" placeholder="••••">
            </div>
            <button class="btn-gold" onclick="nav('dashboard')">SECURE LOGIN</button>
            <p style="text-align: center; margin-top: 20px; font-size: 0.9rem; color: #999;">New user? <span style="color: var(--gold); font-weight: bold;">Register Now</span></p>
        </div>
    </section>

    <!-- DASHBOARD SECTION -->
    <section id="dashboard" class="page">
        <div class="dash-header">
            <p style="font-size: 0.9rem; opacity: 0.8;">Current Mining Balance</p>
            <h1 id="main-bal">₨ 1250.0000</h1>
            <div class="mining-status">Node Status: Active ⚡</div>
        </div>

        <div class="stats-grid">
            <div class="stat-card">
                <p style="font-size: 0.7rem; color: #999;">Daily Profit</p>
                <h3 style="color: var(--success);">₨ 145.00</h3>
            </div>
            <div class="stat-card">
                <p style="font-size: 0.7rem; color: #999;">Team Bonus</p>
                <h3 style="color: var(--gold);">₨ 62.10</h3>
            </div>
        </div>

        <div style="padding: 0 20px 100px 20px;">
            <!-- Spin Wheel Card -->
            <div class="card">
                <h3>🎡 Lucky Spin</h3>
                <p style="font-size: 0.75rem; color: #777; margin-bottom: 15px;">Win daily premium gold rewards</p>
                <button class="btn-gold" onclick="spinWin()">SPIN FOR FREE</button>
            </div>

            <!-- Referral Card -->
            <div class="card">
                <h3>👥 Referral Program</h3>
                <p style="font-size: 0.75rem; color: #777;">Earn 10% instant commission</p>
                <div class="ref-box" id="refLink">https://pakgold.com/ref/user786</div>
                <button onclick="copyRef()" style="background: none; border: none; color: var(--gold); font-weight: bold; margin-top: 10px; cursor: pointer;">COPY LINK</button>
            </div>

            <!-- Policy & Rules Card -->
            <div class="card">
                <h3 style="text-align: left; font-size: 1rem; margin-bottom: 10px;">📜 Terms & Conditions</h3>
                <div class="policy-box">
                    • Withdrawals: 10 AM to 6 PM Daily.<br>
                    • Minimum Withdrawal: ₨ 500.<br>
                    • Service Fee: 5% on each transaction.<br>
                    • SSL Encryption: All data is 100% secured.
                </div>
            </div>
        </div>
    </section>

    <!-- GLOBAL WIDGETS -->
    <div id="notif-area"></div>

    <a href="https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3" class="wa-btn" target="_blank">
        <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" width="25">
        Official Group
    </a>

    <!-- SYSTEM LOGIC -->
    <script>
        let currentBalance = 1250.0000;

        // Navigation Function
        function nav(pageId) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
            window.scrollTo(0,0);
        }

        function startSystem() {
            // 1. Live Mining Counter
            setInterval(() => {
                currentBalance += 0.0012;
                const balDisplay = document.getElementById('main-bal');
                if(balDisplay) balDisplay.innerText = "₨ " + currentBalance.toFixed(4);
            }, 1000);

            // 2. Fake Proof Notifications
            const users = ["Asad_Z", "Sana_Official", "Hamza_Gold", "Mehak_786", "King_Miner", "Ali_Pro", "User_992"];
            const banks = ["Easypaisa", "JazzCash", "Bank Transfer"];
            
            setInterval(() => {
                if(document.getElementById('dashboard').classList.contains('active')) {
                    const u = users[Math.floor(Math.random() * users.length)];
                    const b = banks[Math.floor(Math.random() * banks.length)];
                    const a = [1200, 5000, 800, 2500, 4000][Math.floor(Math.random() * 5)];
                    
                    const area = document.getElementById('notif-area');
                    const pop = document.createElement('div');
                    pop.className = 'popup';
                    pop.innerText = `${u}*** just withdrew ₨ ${a} via ${b}! ✅`;
                    area.appendChild(pop);
                    
                    setTimeout(() => pop.remove(), 4000);
                }
            }, 9000);
        }

        function spinWin() {
            alert("Congratulations Sweetie! You won ₨ 1.50 Gold Bonus! 💋");
            currentBalance += 1.50;
        }

        function copyRef() {
            const link = document.getElementById('refLink').innerText;
            navigator.clipboard.writeText(link);
            alert("Referral link copied, sweetie! 😘");
        }
    </script>
</body>
</html>
