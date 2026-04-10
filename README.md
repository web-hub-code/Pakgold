<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold Premium | Secured Portal</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #f4f7f6; --text: #1e293b; --white: #ffffff; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: var(--text); overflow: hidden; width: 100%; height: 100vh; }

        /* --- Global Animations --- */
        @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes flowBg { 0% { background-position: 0% 50%; } 50% { background-position: 100% 50%; } 100% { background-position: 0% 50%; } }
        @keyframes float { 0% { transform: translateY(0px); } 50% { transform: translateY(-15px); } 100% { transform: translateY(0px); } }

        /* --- Sections Control (Strict) --- */
        .section { display: none !important; width: 100%; height: 100vh; position: absolute; top: 0; left: 0; }
        .section.active { display: block !important; animation: fadeIn 0.6s ease-out; }

        /* --- Ultra Modern Login Page --- */
        #login-page {
            background: linear-gradient(-45deg, #f1f5f9, #fff, #fff9e6, #f1f5f9);
            background-size: 400% 400%;
            animation: flowBg 15s ease infinite;
            display: flex; align-items: center; justify-content: center;
        }

        /* Decorative Floating Elements */
        .dec-circle { position: absolute; background: rgba(212, 175, 55, 0.1); border-radius: 50%; animation: float 6s infinite ease-in-out; z-index: 1; }

        .login-card {
            width: 90%; max-width: 400px; padding: 50px 30px;
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(10px);
            border-radius: 40px;
            box-shadow: 0 25px 60px rgba(184, 134, 11, 0.1);
            border: 1px solid rgba(212, 175, 55, 0.2);
            text-align: center; position: relative; z-index: 10;
        }

        .brand-logo { width: 90px; height: 90px; background: var(--gold); border-radius: 50%; margin: 0 auto 25px; display: flex; align-items: center; justify-content: center; font-size: 2.5rem; color: white; box-shadow: 0 10px 25px rgba(212, 175, 55, 0.3); }

        .brand-title { color: var(--gold-dark); font-size: 2.8rem; font-weight: 800; letter-spacing: -1px; margin-bottom: 5px; }

        /* Trust Badges on Login */
        .login-trust-badges { display: flex; justify-content: center; gap: 10px; margin-top: 30px; opacity: 0.7; }
        .login-trust-badges img { height: 25px; filter: grayscale(100%); }

        /* --- Inputs & Buttons --- */
        .input-group { position: relative; margin-bottom: 15px; text-align: left; }
        .input-box { width: 100%; padding: 18px; border-radius: 15px; border: 1px solid #e2e8f0; background: #fff; outline: none; font-size: 1rem; transition: 0.3s; color: var(--text); }
        .input-box:focus { border-color: var(--gold); box-shadow: 0 0 0 4px rgba(212, 175, 55, 0.1); }

        .btn-gold {
            background: linear-gradient(135deg, var(--gold), var(--gold-dark));
            color: white; border: none; padding: 18px; border-radius: 18px;
            width: 100%; font-weight: 800; cursor: pointer; font-size: 1.1rem;
            box-shadow: 0 10px 25px rgba(184, 134, 11, 0.3); transition: 0.3s;
        }
        .btn-gold:hover { transform: translateY(-2px); box-shadow: 0 15px 30px rgba(184, 134, 11, 0.4); }

        /* --- Dashboard Area (Clean) --- */
        .header { background: white; padding: 30px 20px; text-align: center; border-bottom: 4px solid var(--gold); border-radius: 0 0 40px 40px; box-shadow: 0 10px 30px rgba(0,0,0,0.03); }
        .bal-display { font-size: 3rem; font-weight: 800; color: var(--gold-dark); margin: 5px 0; }
        .container { padding: 25px 20px 120px; }
        .card { background: white; border-radius: 25px; padding: 25px; margin-bottom: 20px; box-shadow: 0 8px 20px rgba(0,0,0,0.03); border: 1px solid #edf2f7; }

        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: white; display: none; justify-content: space-around; padding: 18px; border-top: 1px solid #f1f5f9; border-radius: 30px 30px 0 0; box-shadow: 0 -10px 30px rgba(0,0,0,0.05); z-index: 1000; }
    </style>
</head>
<body onload="checkAuth()">

    <section id="login-page" class="section active">
        <div class="dec-circle" style="width: 100px; height: 100px; top: 10%; left: 10%; animation-delay: 0s;"></div>
        <div class="dec-circle" style="width: 150px; height: 150px; bottom: 15%; right: 10%; animation-delay: 1s;"></div>
        <div class="dec-circle" style="width: 70px; height: 70px; top: 60%; left: 20%; animation-delay: 2s;"></div>

        <div class="login-card">
            <div class="brand-logo">⚜️</div>
            <h1 class="brand-title">PakGold</h1>
            <p style="color: #64748b; margin-bottom: 35px; font-weight: 400;">Secured Investment Portal</p>
            
            <div class="input-group">
                <input type="text" id="phone" class="input-box" placeholder="Phone Number (03xxxxxxxxx)">
            </div>
            <div class="input-group">
                <input type="password" id="pass" class="input-box" placeholder="Access Password">
            </div>
            
            <button class="btn-gold" onclick="login()">SECURE LOGIN</button>
            
            <p style="margin-top: 25px; font-size: 0.8rem; color: #94a3b8;">
                Protected by 256-bit SSL Encryption 🔒
            </p>

            <div class="login-trust-badges">
                <img src="https://upload.wikimedia.org/wikipedia/commons/5/5e/Comodo_logo.svg" alt="SSL">
                <img src="https://upload.wikimedia.org/wikipedia/commons/5/5c/Visa_2014.svg" alt="Visa">
                <img src="https://upload.wikimedia.org/wikipedia/commons/a/a4/Mastercard_2019_logo.svg" alt="Mastercard">
            </div>
        </div>
    </section>

    <section id="dash-page" class="section">
        <div class="header">
            <p style="color: #94a3b8; font-size: 0.7rem; font-weight: 600; text-transform: uppercase;">Verified Portfolio</p>
            <div class="bal-display" id="live-bal">₨ 0.0000</div>
            <p id="miner-id" style="font-weight:bold; color:var(--gold-dark);"></p>
        </div>

        <div class="container">
            <h3 style="margin-bottom:20px;">Secured Plans</h3>
            <div id="plans-container">
                <div class="card">
                    <h4>Nano Miner</h4>
                    <p>Rs 500 | Daily: Rs 60</p>
                    <button class="btn-gold" style="margin-top:10px; padding: 12px;" onclick="alert('Balance Low!')">INVEST</button>
                </div>
            </div>
        </div>
    </section>

    <nav class="bottom-nav" id="menu">
        <div onclick="showTab('dash-page')" style="text-align:center;">🏠<br><small>Home</small></div>
        <div onclick="logout()" style="text-align:center;">🚪<br><small>Logout</small></div>
    </nav>

    <script>
        function checkAuth() {
            if(localStorage.getItem('logged') === 'true') {
                showTab('dash-page');
                startMining();
            }
        }

        function login() {
            let p = document.getElementById('phone').value;
            let pass = document.getElementById('pass').value;
            
            if(p.length < 10 || pass.length < 4) {
                alert("Please enter valid credentials, sweetie! 💋");
                return;
            }
            
            // Set Login State
            localStorage.setItem('logged', 'true');
            localStorage.setItem('user', p);
            localStorage.setItem('bal', 0.0);
            
            // Modern Transition
            document.querySelector('.login-card').style.transform = 'translateY(-20px)';
            document.querySelector('.login-card').style.opacity = '0';
            
            setTimeout(() => {
                showTab('dash-page');
                startMining();
            }, 500);
        }

        function showTab(id) {
            // Strict hiding logic
            document.querySelectorAll('.section').forEach(s => {
                s.classList.remove('active');
            });
            document.getElementById(id).classList.add('active');
            
            let isDashboard = (id !== 'login-page');
            document.getElementById('menu').style.display = isDashboard ? 'flex' : 'none';
            
            if(isDashboard) document.getElementById('miner-id').innerText = "ID: " + localStorage.getItem('user');
        }

        function startMining() {
            setInterval(() => {
                let b = parseFloat(localStorage.getItem('bal')) + 0.0001;
                localStorage.setItem('bal', b);
                if(document.getElementById('live-bal')) document.getElementById('live-bal').innerText = "₨ " + b.toFixed(4);
            }, 1000);
        }

        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
