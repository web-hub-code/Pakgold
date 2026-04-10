<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Live Neural Terminal</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;500;700&display=swap" rel="stylesheet">

    <style>
        :root { --neon: #00f2ff; --gold: #f59e0b; --bg: #020617; --glass: rgba(255, 255, 255, 0.03); }
        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Space Grotesk', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: #fff; overflow-x: hidden; }

        /* Real-Time Notification Pop-up */
        #live-toast { position: fixed; bottom: 100px; left: 20px; right: 20px; background: rgba(15, 23, 42, 0.9); backdrop-filter: blur(10px); padding: 12px 20px; border-radius: 20px; border: 1px solid var(--neon); display: flex; align-items: center; gap: 15px; transform: translateY(200px); transition: 0.5s ease; z-index: 10000; }
        #live-toast.show { transform: translateY(0); }

        /* Animated Progress Bar */
        .progress-container { background: #1e293b; height: 8px; border-radius: 10px; margin-top: 15px; overflow: hidden; }
        .progress-fill { height: 100%; width: 65%; background: linear-gradient(90deg, var(--gold), #fbbf24); border-radius: 10px; box-shadow: 0 0 10px var(--gold); }

        /* Glassmorphism Cards */
        .page { display:none; padding-bottom: 120px; animation: scaleUp 0.4s ease; }
        .page.active { display:block; }
        @keyframes scaleUp { from { opacity:0; transform: scale(0.98); } to { opacity:1; transform: scale(1); } }

        .main-card { background: linear-gradient(145deg, rgba(30,41,59,0.5), rgba(15,23,42,0.8)); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.05); margin: 20px; border-radius: 35px; padding: 30px; position: relative; overflow: hidden; }
        .main-card::before { content: ''; position: absolute; top: -50%; left: -50%; width: 200%; height: 200%; background: conic-gradient(transparent, var(--neon), transparent 30%); animation: rotate 4s linear infinite; z-index: -1; opacity: 0.2; }
        @keyframes rotate { 100% { transform: rotate(360deg); } }

        /* Live Mining Indicator */
        .mining-status { display: flex; align-items: center; gap: 10px; font-size: 0.7rem; color: var(--neon); margin-bottom: 15px; }
        .pulse { width: 8px; height: 8px; background: var(--neon); border-radius: 50%; box-shadow: 0 0 10px var(--neon); animation: pulse 1.5s infinite; }
        @keyframes pulse { 0% { transform: scale(1); opacity: 1; } 100% { transform: scale(2.5); opacity: 0; } }

        /* Action Grid */
        .action-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; padding: 0 20px; }
        .action-box { background: var(--glass); border: 1px solid rgba(255,255,255,0.08); padding: 25px 15px; border-radius: 25px; text-align: center; }
        .action-box i { font-size: 1.8rem; margin-bottom: 10px; color: var(--gold); }

        /* Bottom Nav (Ultra Modern) */
        .tab-bar { position: fixed; bottom: 25px; left: 20px; right: 20px; background: rgba(15, 23, 42, 0.85); backdrop-filter: blur(25px); height: 75px; border-radius: 25px; display: flex; justify-content: space-around; align-items: center; border: 1px solid rgba(255,255,255,0.1); box-shadow: 0 20px 40px rgba(0,0,0,0.5); z-index: 1000; }
        .tab { text-align: center; color: #64748b; font-size: 0.65rem; font-weight: 700; transition: 0.3s; }
        .tab.active { color: var(--neon); text-shadow: 0 0 10px var(--neon); }
        .tab i { font-size: 1.5rem; display: block; margin-bottom: 3px; }

        .btn-prime { background: linear-gradient(90deg, #00f2ff, #0061ff); color: #fff; border: none; padding: 18px; border-radius: 20px; width: 100%; font-weight: 800; font-size: 1rem; box-shadow: 0 10px 20px rgba(0,242,255,0.2); }
    </style>
</head>
<body onload="initApp()">

    <!-- Real-Time Proof Toast -->
    <div id="live-toast">
        <div style="background:var(--neon); width:40px; height:40px; border-radius:12px; display:flex; align-items:center; justify-content:center; color:#000;"><i class="fa fa-check"></i></div>
        <div>
            <div style="font-size: 0.8rem; font-weight: 700;" id="toast-user">User 0321***</div>
            <div style="font-size: 0.65rem; color: #94a3b8;" id="toast-msg">Successfully withdrew Rs 4,200</div>
        </div>
    </div>

    <!-- HOME SECTION -->
    <section id="home" class="page active">
        <div style="padding: 20px; display: flex; justify-content: space-between; align-items: center;">
            <h2 style="font-size: 1.5rem; font-weight: 800; letter-spacing: -1px;">CORE <span style="color:var(--neon)">AI</span></h2>
            <div style="background:var(--glass); padding:8px 15px; border-radius:50px; font-size:0.7rem; border:1px solid rgba(255,255,255,0.1);">
                ID: <span id="u_id" style="color:var(--gold)">03xx...</span>
            </div>
        </div>

        <div class="main-card">
            <div class="mining-status"><div class="pulse"></div> AI MINING ENGINE ACTIVE</div>
            <small style="color: #94a3b8; font-weight: 700; letter-spacing: 2px;">AVAILABLE CAPITAL</small>
            <h1 style="font-size: 3.5rem; letter-spacing: -2px; margin: 10px 0;">Rs <span id="u_wallet">0.00</span></h1>
            <div style="display: flex; justify-content: space-between; margin-top: 20px;">
                <div style="font-size: 0.8rem; color: #94a3b8;">Pending Yield: <br><b id="u_profit" style="color:var(--neon)">0.00</b></div>
                <div style="text-align: right; font-size: 0.8rem; color: #94a3b8;">System Load: <br><b style="color:#00c853;">98.4%</b></div>
            </div>
            <div class="progress-container"><div class="progress-fill" id="p_bar"></div></div>
        </div>

        <div class="action-grid">
            <div class="action-box" onclick="nav('deposit')"><i class="fa fa-plus-square"></i><br><span>Deposit</span></div>
            <div class="action-box" onclick="nav('withdraw')"><i class="fa fa-wallet"></i><br><span>Withdraw</span></div>
            <div class="action-box" onclick="nav('team')"><i class="fa fa-share-nodes"></i><br><span>Network</span></div>
            <div class="action-box" onclick="alert('AI Assistant is currently optimizing your mining nodes.')"><i class="fa fa-robot"></i><br><span>AI Care</span></div>
        </div>

        <div style="padding: 30px 25px 10px; font-weight: 800; font-size: 1.2rem; color: var(--neon);">PREMIUM NODES</div>
        <div id="plans_list"></div>
    </section>

    <!-- TAB BAR -->
    <nav class="tab-bar" id="nav-bar" style="display:none;">
        <div class="tab active" onclick="nav('home')"><i class="fa-solid fa-house-chimney-window"></i>Home</div>
        <div class="tab" onclick="nav('team')"><i class="fa-solid fa-users-viewfinder"></i>Team</div>
        <div class="tab" onclick="nav('withdraw')"><i class="fa-solid fa-building-columns"></i>Vault</div>
        <div class="tab" onclick="checkAdmin()"><i class="fa-solid fa-fingerprint"></i>Admin</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_live_v7');

        function initApp() { 
            if(user) { nav('home'); sync(); document.getElementById('u_id').innerText = user.substring(0,6)+'...'; } 
            loadPlans(); 
            startLiveProof();
        }

        function sync() {
            db.ref('users/'+user).on('value', s => {
                let d = s.val();
                document.getElementById('u_wallet').innerText = d.wallet.toLocaleString();
                document.getElementById('u_profit').innerText = d.profit.toFixed(2);
                let progress = (d.profit % 100); document.getElementById('p_bar').style.width = progress + "%";
            });
            setInterval(() => {
                db.ref('users/'+user).once('value', s => {
                    let d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed});
                });
            }, 1000);
        }

        function startLiveProof() {
            const users = ["0300***", "0345***", "0312***", "0333***", "0305***"];
            const amounts = ["2,500", "10,000", "1,200", "500", "15,000"];
            setInterval(() => {
                const toast = document.getElementById('live-toast');
                document.getElementById('toast-user').innerText = "User " + users[Math.floor(Math.random()*users.length)];
                document.getElementById('toast-msg').innerText = "Successfully withdrew Rs " + amounts[Math.floor(Math.random()*amounts.length)];
                toast.classList.add('show');
                setTimeout(() => toast.classList.remove('show'), 4000);
            }, 15000);
        }

        function loadPlans() {
            const nodes = [
                {n:'S-100', p:200, d:20}, {n:'M-500', p:500, d:55}, {n:'L-1000', p:1000, d:120},
                {n:'XL-5000', p:5000, d:650}, {n:'ULTRA-VIP', p:20000, d:3200}
            ];
            let list = document.getElementById('plans_list');
            nodes.forEach(i => {
                list.innerHTML += `<div style="background:var(--glass); margin:15px 20px; padding:25px; border-radius:30px; border:1px solid rgba(255,255,255,0.05); display:flex; justify-content:space-between; align-items:center;">
                    <div>
                        <div style="font-size:0.6rem; color:var(--neon); font-weight:800; margin-bottom:5px;">ENTERPRISE GRADE</div>
                        <h3 style="font-weight:700;">${i.n}</h3>
                        <small style="color:#64748b;">Daily: Rs ${i.d}</small>
                    </div>
                    <button onclick="rent(${i.p}, ${i.d/86400})" style="background:var(--neon); color:#000; padding:12px 20px; border-radius:15px; border:none; font-weight:900; font-size:0.7rem;">RENT NODE</button>
                </div>`;
            });
        }

        function rent(p, s) {
            db.ref('users/'+user).once('value', v => {
                if(v.val().wallet < p) return alert("System requires more capital to link this node.");
                db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s});
                alert("Protocol Linked! Your hash-rate has increased.");
            });
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('nav-bar').style.display = (id==='login')?'none':'flex';
        }

        function checkAdmin() { let p = prompt("ACCESS TOKEN:"); if(p==="78692") alert("Admin mode activated on your device."); }
    </script>
</body>
</html>
