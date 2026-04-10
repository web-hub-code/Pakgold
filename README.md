<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Premium Investment Portal</title>
    <!-- Google Fonts for Premium Look -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        :root { 
            --gold: #d4af37; 
            --gold-dark: #b8860b;
            --dark: #1e272e; 
            --light: #f1f2f6;
            --success: #2ecc71;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: var(--light); color: var(--dark); overflow-x: hidden; }

        /* --- Global Animations --- */
        @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes slideIn { from { transform: translateX(-100%); } to { transform: translateX(0); } }

        /* --- Page Management --- */
        .section { display: none; min-height: 100vh; animation: fadeIn 0.5s ease-out; }
        .section.active { display: block; }

        /* --- Common UI Elements --- */
        .card { background: white; padding: 25px; border-radius: 20px; box-shadow: 0 10px 25px rgba(0,0,0,0.05); margin-bottom: 20px; }
        .btn-gold { 
            background: linear-gradient(135deg, var(--gold), var(--gold-dark)); 
            color: white; border: none; padding: 15px; border-radius: 50px; 
            width: 100%; font-weight: bold; cursor: pointer; transition: 0.3s;
            box-shadow: 0 8px 15px rgba(184, 134, 11, 0.3);
        }
        .btn-gold:hover { transform: translateY(-3px); box-shadow: 0 12px 20px rgba(184, 134, 11, 0.5); }

        /* --- 1. Hero / Landing Section --- */
        #landing { display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center; background: radial-gradient(circle at top right, #fff, #f9f9f9); }
        .logo-icon { font-size: 60px; margin-bottom: 10px; }
        .gold-title { font-size: 3rem; font-weight: 800; background: linear-gradient(90deg, #d4af37, #f1c40f); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }

        /* --- 2. Login Section --- */
        #login { padding-top: 10vh; }
        .login-box { max-width: 400px; margin: 0 auto; text-align: center; }
        input { width: 100%; padding: 15px; border-radius: 12px; border: 1px solid #ddd; margin: 10px 0; outline: none; transition: 0.3s; }
        input:focus { border-color: var(--gold); }

        /* --- 3. Dashboard Section --- */
        .dash-header { background: var(--dark); color: white; padding: 40px 20px; border-radius: 0 0 40px 40px; text-align: center; margin-bottom: 30px; }
        .mining-badge { display: inline-block; background: rgba(46, 204, 113, 0.2); color: var(--success); padding: 5px 15px; border-radius: 20px; font-size: 0.8rem; margin-top: 10px; border: 1px solid var(--success); }
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; padding: 0 20px; }

        /* --- WhatsApp Floating Button --- */
        .whatsapp-btn { 
            position: fixed; bottom: 30px; right: 20px; background: #25d366; 
            color: white; padding: 12px 25px; border-radius: 50px; text-decoration: none; 
            font-weight: bold; display: flex; align-items: center; gap: 10px; 
            box-shadow: 0 10px 25px rgba(0,0,0,0.2); z-index: 9999;
        }

        /* --- Fake Notifications --- */
        #notif-box { position: fixed; bottom: 100px; left: 20px; z-index: 9998; }
        .fake-popup { background: white; padding: 10px 15px; border-radius: 10px; border-left: 5px solid var(--gold); margin-top: 10px; font-size: 0.8rem; box-shadow: 0 5px 15px rgba(0,0,0,0.1); animation: slideIn 0.5s ease; }
    </style>
</head>
<body onload="initializeSite()">

    <!-- LANDING SECTION -->
    <section id="landing" class="section active">
        <div class="logo-icon">⚜️</div>
        <h1 class="gold-title">PAK GOLD</h1>
        <p style="color: #777; margin-bottom: 40px;">Professional Gold Mining Portal</p>
        <button class="btn-gold" style="max-width: 280px;" onclick="goTo('login')">START EARNING</button>
        <div style="margin-top: 30px; color: #bbb; font-size: 0.8rem;">🔒 Secured by 256-bit Encryption</div>
    </section>

    <!-- LOGIN SECTION -->
    <section id="login" class="section">
        <div class="login-box">
            <div class="card">
                <h2>Welcome Back</h2>
                <p style="color: #888; margin-bottom: 20px; font-size: 0.9rem;">Login to your gold vault</p>
                <input type="text" placeholder="Phone Number" id="phone">
                <input type="password" placeholder="Account PIN" id="pin">
                <button class="btn-gold" onclick="goTo('dashboard')">SECURE LOGIN</button>
                <p style="margin-top: 15px; font-size: 0.8rem; color: #999;">Don't have an account? <span style="color: var(--gold); font-weight: bold; cursor: pointer;">Register</span></p>
            </div>
        </div>
    </section>

    <!-- DASHBOARD SECTION -->
    <section id="dashboard" class="section">
        <div class="dash-header">
            <p style="opacity: 0.8; font-size: 0.9rem;">Total Gold Earnings</p>
            <h1 id="live-balance" style="font-size: 2.8rem; margin: 10px 0;">₨ 1250.0000</h1>
            <div class="mining-badge">Mining Active ⚡</div>
        </div>

        <div class="stats-grid">
            <div class="card" style="padding: 15px;">
                <p style="font-size: 0.7rem; color: #888;">Daily Profit</p>
                <h3 style="color: var(--success);">₨ 45.00</h3>
            </div>
            <div class="card" style="padding: 15px;">
                <p style="font-size: 0.7rem; color: #888;">Team Bonus</p>
                <h3 style="color: var(--gold);">₨ 12.50</h3>
            </div>
        </div>

        <div style="padding: 0 20px;">
            <!-- Spin Wheel Card -->
            <div class="card">
                <h3>🎡 Lucky Spin</h3>
                <p style="font-size: 0.75rem; color: #777; margin-bottom: 15px;">Try your luck for extra gold</p>
                <button class="btn-gold" onclick="luckySpin()">SPIN NOW</button>
            </div>

            <!-- Referral Card -->
            <div class="card">
                <h3>👥 Referral Link</h3>
                <input type="text" value="https://pakgold.com/ref/user786" readonly style="background: #f8f8f8; text-align: center; border: none; font-size: 0.8rem; color: #555;">
                <button onclick="alert('Link Copied, Sweetie!')" style="background: none; border: none; color: var(--gold); font-weight: bold; cursor: pointer; font-size: 0.9rem;">Copy Link</button>
            </div>

            <!-- Policy Card -->
            <div class="card" style="background: #fff9e6; border: 1px dashed var(--gold);">
                <h4 style="color: var(--gold-dark);">📌 Official Rules</h4>
                <ul style="font-size: 0.7rem; color: #888; text-align: left; padding-left: 15px; margin-top: 10px;">
                    <li>Withdrawals are processed 10 AM - 6 PM daily.</li>
                    <li>Referral commission is credited instantly.</li>
                    <li>Multiple accounts will result in a permanent ban.</li>
                </ul>
            </div>
        </div>
    </section>

    <!-- UI ELEMENTS -->
    <div id="notif-box"></div>

    <a href="https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3" class="whatsapp-btn" target="_blank">
        <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" width="25">
        Join Official Group
    </a>

    <!-- LOGIC SCRIPTS -->
    <script>
        let myBalance = 1250.0000;

        function goTo(id) {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            window.scrollTo(0,0);
        }

        function initializeSite() {
            // 1. Live Mining Animation
            setInterval(() => {
                myBalance += 0.0011;
                const balEl = document.getElementById('live-balance');
                if(balEl) balEl.innerText = "₨ " + myBalance.toFixed(4);
            }, 1000);

            // 2. Fake Payout Popups
            const users = ["Ali_Pro", "Sana_786", "Hammad_Win", "Zain_Gold", "Mehak_Mining", "Khurram_88"];
            setInterval(() => {
                if(document.getElementById('dashboard').classList.contains('active')) {
                    const u = users[Math.floor(Math.random()*users.length)];
                    const amt = [450, 1000, 2500, 5000, 200][Math.floor(Math.random()*5)];
                    const box = document.getElementById('notif-box');
                    const pop = document.createElement('div');
                    pop.className = 'fake-popup';
                    pop.innerText = `${u}*** just received ₨ ${amt} via Easypaisa! ✅`;
                    box.appendChild(pop);
                    setTimeout(() => pop.remove(), 4000);
                }
            }, 8000);
        }

        function luckySpin() {
            alert("Calculating rewards... Sweetie, your balance increased by ₨ 0.50! 🔥");
            myBalance += 0.50;
        }
    </script>
</body>
</html>
