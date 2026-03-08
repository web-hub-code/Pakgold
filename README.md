<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Prime Solutions Premium</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #fbbf24; --bg: #020617; --glass: rgba(255, 255, 255, 0.03); --green: #10b981; --red: #ef4444; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: white; min-height: 100vh; overflow-x: hidden; }
        
        .glass-card { background: var(--glass); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.08); border-radius: 24px; padding: 20px; width: 92%; max-width: 450px; margin: 15px auto; transition: 0.3s; }
        .btn-main { width: 100%; padding: 15px; background: var(--gold); border: none; border-radius: 14px; color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; transition: 0.3s; box-shadow: 0 4px 15px rgba(251, 191, 36, 0.2); }
        .btn-main:active { transform: scale(0.98); }
        input, select { width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 14px; color: white; margin-bottom: 12px; outline: none; }
        
        #authSection, #dashboardPage, #loadingOverlay { display: none; }
        .active { display: block !important; }
        .flex-active { display: flex !important; }
        
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        .spinner { width: 40px; height: 40px; border: 3px solid rgba(251, 191, 36, 0.1); border-top: 3px solid var(--gold); border-radius: 50%; animation: spin 1s linear infinite; }
    </style>
</head>
<body>

    <div id="loadingOverlay" style="position:fixed; inset:0; background:rgba(2,6,23,0.9); z-index:20000; display:none; flex-direction:column; align-items:center; justify-content:center;">
        <div class="spinner"></div>
    </div>

    <div id="authSection" class="active">
        <h1 style="text-align:center; margin:50px 0; color:var(--gold); font-size: 2.8rem; font-weight: 600;">PakGold</h1>
        <div class="glass-card">
            <h2 id="authTitle" style="font-size:1.1rem; margin-bottom:20px; text-align: center;">Welcome Back</h2>
            <input type="text" id="username" placeholder="Enter Username">
            <input type="password" id="password" placeholder="Enter Password">
            <button id="authBtn" class="btn-main">Continue 🚀</button>
            <p id="toggleAuth" style="text-align:center; font-size:0.8rem; margin-top:20px; cursor:pointer; color:#94a3b8;">New member? <span style="color:var(--gold)">Create Account</span></p>
        </div>
    </div>

    <div id="dashboardPage">
        <div class="glass-card" style="border-color:var(--gold); margin-top: 20px;">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom: 15px;">
                <span id="timerText" style="font-size:0.7rem; color:var(--gold); font-weight: bold;">⚡ MINING ACTIVE</span>
                <button onclick="logout()" style="background:none; border:none; color:var(--red); font-size:0.6rem; font-weight:bold; cursor:pointer;">LOGOUT</button>
            </div>
            <div style="text-align:center; margin:20px 0;">
                <p style="font-size:0.6rem; color:#94a3b8; text-transform: uppercase; letter-spacing: 1px;">Current Profit</p>
                <h1 style="color:var(--gold); font-size:3rem; margin: 10px 0;">PKR <span id="bal">0.00</span></h1>
                <div style="background: rgba(16, 185, 129, 0.1); color: var(--green); display: inline-block; padding: 4px 12px; border-radius: 20px; font-size: 0.65rem;">Auto-Mining Enabled</div>
            </div>
            <div style="width:100%; height:6px; background:rgba(255,255,255,0.05); border-radius:10px; overflow:hidden;">
                <div id="miningProgress" style="width:40%; height:100%; background:var(--gold); box-shadow: 0 0 15px var(--gold); transition: 1s;"></div>
            </div>
        </div>

        <div class="glass-card">
            <div style="display:grid; grid-template-columns: 1fr 1fr; gap: 12px;">
                <button onclick="toggleAction('dep')" style="background:var(--glass); color:white; border:1px solid var(--gold); padding:15px; border-radius:14px; font-size:0.7rem; font-weight:bold;">DEPOSIT</button>
                <button onclick="toggleAction('wit')" style="background:var(--glass); color:white; border:1px solid var(--red); padding:15px; border-radius:14px; font-size:0.7rem; font-weight:bold;">WITHDRAW</button>
            </div>
            
            <div id="depBox" style="display:none; margin-top: 20px; border-top: 1px solid rgba(255,255,255,0.1); padding-top: 15px;">
                <div style="background: rgba(255,255,255,0.02); padding: 12px; border-radius: 12px; margin-bottom: 15px; font-size: 0.7rem;">
                    <p style="margin-bottom: 5px;"><b>EasyPaisa:</b> 03379827882</p>
                    <p><b>JazzCash/SadaPay:</b> 03705519562</p>
                </div>
                <input type="number" id="depAmount" placeholder="Amount (PKR)">
                <input type="text" id="depTID" placeholder="Transaction ID (TID)">
                <button onclick="submitDeposit()" class="btn-main" style="font-size: 0.75rem;">Submit Payment</button>
            </div>
        </div>

        <div class="glass-card" style="border-color:#25d366;">
            <p style="text-align:center; font-size:0.7rem; margin-bottom:12px; color:#94a3b8;">Official Support: webhub262@gmail.com</p>
            <button class="btn-main" style="background:#25d366; color:white;">Join WhatsApp Community</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getFirestore, doc, getDoc, setDoc, updateDoc, increment } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        
        let isLoginMode = true;
        const currentUser = localStorage.getItem('pakgold_user');

        // Toggle Logic
        document.getElementById('toggleAuth').onclick = () => {
            isLoginMode = !isLoginMode;
            document.getElementById('authTitle').innerText = isLoginMode ? "Welcome Back" : "Create Account";
            document.getElementById('toggleAuth').innerHTML = isLoginMode ? 'New member? <span style="color:var(--gold)">Create Account</span>' : 'Already a member? <span style="color:var(--gold)">Login</span>';
        };

        // --- AUTH LOGIC (No Anonymous, Direct Firestore) ---
        document.getElementById('authBtn').onclick = async () => {
            const user = document.getElementById('username').value.toLowerCase().trim();
            const pass = document.getElementById('password').value;

            if(!user || !pass) return alert("Please fill all details");
            document.getElementById('loadingOverlay').style.display = 'flex';

            try {
                const userRef = doc(db, "users", user);
                const snap = await getDoc(userRef);

                if (isLoginMode) {
                    if (snap.exists() && snap.data().pass === pass) {
                        localStorage.setItem('pakgold_user', user);
                        location.reload();
                    } else { alert("Wrong Username or Password"); }
                } else {
                    if (snap.exists()) { alert("Username taken"); }
                    else {
                        await setDoc(userRef, { user, pass, balance: 0, lastBonusDate: "" });
                        localStorage.setItem('pakgold_user', user);
                        location.reload();
                    }
                }
            } catch (e) { alert("Database Connection Error!"); }
            document.getElementById('loadingOverlay').style.display = 'none';
        };

        // Dashboard Initializer
        if(currentUser) {
            document.getElementById('authSection').style.display = 'none';
            document.getElementById('dashboardPage').classList.add('active');
            
            const uRef = doc(db, "users", currentUser);
            getDoc(uRef).then(async (s) => {
                if(s.exists()) {
                    const data = s.data();
                    document.getElementById('bal').innerText = data.balance.toFixed(2);
                    
                    // Daily Login Bonus (Automatic)
                    const today = new Date().toDateString();
                    if(data.lastBonusDate !== today) {
                        await updateDoc(uRef, { balance: increment(5), lastBonusDate: today });
                        alert("Sweetie! PKR 5 Daily Bonus Added 🎁");
                        location.reload();
                    }
                }
            });
        }

        window.toggleAction = (type) => {
            const box = document.getElementById('depBox');
            box.style.display = box.style.display === 'none' ? 'block' : 'none';
        };

        window.logout = () => { localStorage.removeItem('pakgold_user'); location.reload(); }
    </script>
</body>
</html>
