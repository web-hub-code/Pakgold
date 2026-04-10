<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Premium</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #d4af37; --gold-dark: #b8860b; --bg: #f8fafc; --text: #1e293b; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        
        body { background: var(--bg); color: var(--text); }

        /* FIX: Sections are now strictly hidden by default */
        .section { 
            display: none; 
            min-height: 100vh; 
            width: 100%; 
            position: absolute; /* Extra safety to prevent overlap */
            top: 0; 
            left: 0;
        }

        /* Show only the active section */
        .section.active { 
            display: block; 
            position: relative; 
        }

        /* Login Screen Styling */
        #login-page { 
            background: white; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            z-index: 9999;
        }

        .login-card { 
            width: 90%; 
            max-width: 380px; 
            padding: 40px 25px; 
            background: white; 
            border-radius: 30px; 
            box-shadow: 0 20px 60px rgba(0,0,0,0.1); 
            text-align: center;
        }

        /* Dashboard Styling */
        .header { 
            background: white; 
            padding: 40px 20px; 
            text-align: center; 
            border-bottom: 4px solid var(--gold); 
            border-radius: 0 0 40px 40px; 
            box-shadow: 0 10px 30px rgba(0,0,0,0.03); 
        }

        .container { padding: 20px; padding-bottom: 120px; }
        .card { 
            background: white; 
            border-radius: 20px; 
            padding: 20px; 
            margin-bottom: 15px; 
            border: 1px solid #eee; 
            box-shadow: 0 5px 15px rgba(0,0,0,0.02);
        }

        .input-field { width: 100%; padding: 15px; margin: 10px 0; border-radius: 12px; border: 1px solid #ddd; outline: none; }
        .btn-main { 
            background: linear-gradient(135deg, var(--gold), var(--gold-dark)); 
            color: white; border: none; padding: 15px; border-radius: 12px; 
            width: 100%; font-weight: 700; cursor: pointer; 
        }

        .bottom-nav { 
            position: fixed; bottom: 0; width: 100%; background: white; 
            display: none; justify-content: space-around; padding: 15px; 
            border-top: 1px solid #eee; border-radius: 25px 25px 0 0; 
            box-shadow: 0 -5px 20px rgba(0,0,0,0.05); 
        }
    </style>
</head>
<body onload="init()">

    <!-- LOGIN PAGE (Hidden if logged in) -->
    <section id="login-page" class="section active">
        <div class="login-card">
            <h1 style="color: var(--gold-dark); margin-bottom: 5px;">⚜️ PakGold</h1>
            <p style="color: #64748b; font-size: 0.9rem; margin-bottom: 25px;">Premium Mining & Investments</p>
            <input type="text" id="phone" class="input-field" placeholder="Phone Number">
            <input type="password" id="pass" class="input-field" placeholder="Password">
            <button class="btn-main" onclick="doLogin()">SECURE LOGIN</button>
        </div>
    </section>

    <!-- DASHBOARD PAGE -->
    <section id="dash-page" class="section">
        <div class="header">
            <p style="color: #94a3b8; font-size: 0.7rem; letter-spacing: 1px;">PAK GOLD SOLUTIONS</p>
            <p style="font-size: 0.9rem; margin-top: 10px;">Mining Balance</p>
            <h1 style="color: var(--gold-dark); font-size: 2.5rem;" id="bal">Rs 0.0000</h1>
            <p id="mid" style="font-size: 0.8rem; font-weight: bold; color: var(--gold);"></p>
        </div>

        <div class="container">
            <h3 style="margin-bottom: 15px;">Active Mining Plans</h3>
            <div id="plan-box">
                <!-- Nano Plan Example -->
                <div class="card">
                    <h4 style="color: var(--gold-dark);">Nano Plan</h4>
                    <p style="font-weight: 800; font-size: 1.2rem; margin: 5px 0;">Rs 500</p>
                    <p style="font-size: 0.7rem; color: #888;">Earning: Rs 60 Daily | 30 Days</p>
                    <button class="btn-main" style="margin-top: 10px;" onclick="buy(500, 60)">ACTIVATE NOW</button>
                </div>
            </div>
        </div>
    </section>

    <!-- Nav Bar -->
    <nav class="bottom-nav" id="menu">
        <div style="text-align:center" onclick="tab('dash-page')">🏠<br><small>Home</small></div>
        <div style="text-align:center" onclick="logout()">🚪<br><small>Logout</small></div>
    </nav>

    <script>
        function init() {
            if(localStorage.getItem('user')) {
                show('dash-page');
                run();
            }
        }

        function doLogin() {
            let p = document.getElementById('phone').value;
            if(p.length < 10) return alert("Enter valid phone sweetie! 💋");
            localStorage.setItem('user', p);
            localStorage.setItem('bal', 0.0);
            localStorage.setItem('speed', 0.0001);
            show('dash-page');
            run();
        }

        function show(id) {
            // Hide all sections first
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            // Show the target section
            document.getElementById(id).classList.add('active');
            // Show/Hide menu
            document.getElementById('menu').style.display = (id === 'login-page') ? 'none' : 'flex';
            if(id === 'dash-page') document.getElementById('mid').innerText = "Miner: " + localStorage.getItem('user');
        }

        function run() {
            setInterval(() => {
                let b = parseFloat(localStorage.getItem('bal')) + parseFloat(localStorage.getItem('speed'));
                localStorage.setItem('bal', b);
                if(document.getElementById('bal')) document.getElementById('bal').innerText = "Rs " + b.toFixed(4);
            }, 1000);
        }

        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
