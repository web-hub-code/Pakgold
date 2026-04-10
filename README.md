<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Premium Mining & Investment</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-light: #f1c40f; --dark: #2d3436; --glass: rgba(255, 255, 255, 0.9); }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        
        body { background: #f0f2f5; color: var(--dark); overflow-x: hidden; }

        /* --- Animations --- */
        @keyframes fadeInUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes slideIn { from { transform: translateX(-100%); } to { transform: translateX(0); } }

        /* --- Section Toggle Logic --- */
        .page-section { display: none; padding: 20px; animation: fadeInUp 0.5s ease; }
        .active { display: block; }

        /* --- Hero / Landing --- */
        .hero { height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center; background: radial-gradient(circle at top right, #fff9e6, #ffffff); }
        .gold-text { background: linear-gradient(90deg, var(--gold), var(--gold-light)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; font-size: 2.5rem; font-weight: 800; }

        /* --- Cards & Buttons --- */
        .card { background: white; padding: 25px; border-radius: 25px; box-shadow: 0 10px 30px rgba(0,0,0,0.05); margin-bottom: 20px; text-align: center; }
        .btn-gold { background: linear-gradient(135deg, var(--gold), var(--gold-light)); color: white; border: none; padding: 15px 35px; border-radius: 50px; font-weight: bold; cursor: pointer; transition: 0.3s; width: 100%; box-shadow: 0 10px 20px rgba(212, 175, 55, 0.3); }
        .btn-gold:hover { transform: scale(1.03); }

        /* --- Mining Dashboard --- */
        .balance-header { background: linear-gradient(135deg, var(--dark), #4b4b4b); color: white; padding: 40px 20px; border-radius: 30px; margin-top: 10px; }
        .payout-status { display: inline-block; background: rgba(46, 204, 113, 0.2); color: #2ecc71; padding: 5px 15px; border-radius: 20px; font-size: 0.7rem; margin-top: 10px; border: 1px solid #2ecc71; }

        /* --- Fake Notifications --- */
        #notif-container { position: fixed; bottom: 100px; left: 20px; z-index: 1000; }
        .fake-notif { background: var(--glass); padding: 12px; border-radius: 12px; border-left: 5px solid var(--gold); margin-top: 10px; font-size: 0.8rem; box-shadow: 0 5px 15px rgba(0,0,0,0.1); animation: slideIn 0.5s ease; width: 250px; }

        /* --- Floating WhatsApp --- */
        .wa-float { position: fixed; bottom: 30px; right: 20px; background: #25d366; color: white; padding: 12px 20px; border-radius: 50px; text-decoration: none; font-weight: bold; display: flex; align-items: center; gap: 10px; box-shadow: 0 10px 20px rgba(0,0,0,0.2); z-index: 2000; }
    </style>
</head>
<body onload="initApp()">

    <!-- 1. LANDING PAGE -->
    <section id="landing" class="page-section active">
        <div class="hero">
            <div style="font-size: 50px;">🏆</div>
            <h1 class="gold-text">PAK GOLD</h1>
            <p style="color: #666; margin-bottom: 30px;">Premium Cloud Mining Solutions</p>
            <button class="btn-gold" onclick="showPage('login')" style="max-width: 250px;">GET STARTED</button>
            <p style="margin-top: 20px; font-size: 0.8rem; color: #aaa;">Trusted by 25,000+ Investors</p>
        </div>
    </section>

    <!-- 2. LOGIN PAGE -->
    <section id="login" class="page-section">
        <div class="card" style="margin-top: 10vh;">
            <h2>Secure Access</h2>
            <div style="margin: 20px 0; text-align: left;">
                <label style="font-size: 0.8rem; color: #999;">Phone Number</label>
                <input type="text" placeholder="03xxxxxxxxx" style="width: 100%; padding: 12px; border-radius: 10px; border: 1px solid #ddd; margin-top: 5px;">
            </div>
            <button class="btn-gold" onclick="showPage('dashboard')">LOGIN NOW</button>
            <p style="margin-top: 15px; font-size: 0.8rem;">New here? <a href="#" style="color: var(--gold);">Create Account</a></p>
        </div>
    </section>

    <!-- 3. MAIN DASHBOARD -->
    <section id="dashboard" class="page-section">
        <div class="balance-header">
            <p style="opacity: 0.8; font-size: 0.9rem;">Mining Balance</p>
            <h1 id="live-bal" style="font-size: 2.5rem; margin: 10px 0;">₨ 1250.0000</h1>
            <div class="payout-status">Mining Node: Active ⚡</div>
        </div>

        <div class="card" style="margin-top: 20px;">
            <h3>🎡 Lucky Spin</h3>
            <p style="font-size: 0.75rem; color: #777; margin-bottom: 15px;">Win daily gold bonuses</p>
            <button class="btn-gold" onclick="spinAlert()">SPIN NOW</button>
        </div>

        <div class="card">
            <h3>👥 Referral Link</h3>
            <p style="font-size: 0.75rem; color: #777; margin-bottom: 10px;">Earn 10% from every friend!</p>
            <input type="text" value="https://pakgold.com/ref/user786" readonly style="width:100%; padding:10px; background:#f9f9f9; border:none; text-align:center; border-radius:10px; font-size:0.8rem;">
            <button onclick="alert('Link Copied, Sweetie!')" style="background:none; border:none; color:var(--gold); font-weight:bold; margin-top:10px; cursor:pointer;">Copy Link</button>
        </div>

        <div class="card" style="background: #fff9e6; border-left: 5px solid var(--gold);">
            <h4 style="color: #b8860b;">📜 Terms & Policy</h4>
            <p style="font-size: 0.7rem; color: #888; margin-top: 5px;">Withdrawals processed 10AM-6PM. Daily limit: 1 withdrawal. SSL Protected.</p>
        </div>
    </section>

    <!-- NOTIFICATION CONTAINER -->
    <div id="notif-container"></div>

    <!-- WHATSAPP SUPPORT -->
    <a href="https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3" class="wa-float" target="_blank">
        <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" width="25">
        Join Community
    </a>

    <script>
        let balance = 1250.0000;

        function showPage(pageId) {
            document.querySelectorAll('.page-section').forEach(s => s.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
            window.scrollTo(0,0);
        }

        function initApp() {
            // Live Mining Counter
            setInterval(() => {
                balance += 0.0009;
                if(document.getElementById('live-bal')) {
                    document.getElementById('live-bal').innerText = "₨ " + balance.toFixed(4);
                }
            }, 1000);

            // Fake Payout Notifications
            const users = ["Saba_7", "Rizwan_Win", "Mehak_Pro", "Ali_Z", "King_Mining", "Zain_88", "Hina_Official"];
            const amounts = [500, 1200, 2500, 5000, 150];
            
            setInterval(() => {
                if(document.getElementById('dashboard').classList.contains('active')) {
                    const u = users[Math.floor(Math.random() * users.length)];
                    const a = amounts[Math.floor(Math.random() * amounts.length)];
                    showPopup(`${u}*** just withdrew ₨ ${a}! 💰`);
                }
            }, 7000);
        }

        function showPopup(msg) {
            const container = document.getElementById('notif-container');
            const n = document.createElement('div');
            n.className = "fake-notif";
            n.innerText = msg;
            container.appendChild(n);
            setTimeout(() => n.remove(), 4000);
        }

        function spinAlert() {
            alert("Searching for rewards... Better luck tomorrow, sweetie!");
        }
    </script>
</body>
</html>
