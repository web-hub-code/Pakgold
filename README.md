<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Professional Mining Farm</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #fbbf24; --bg: #020617; --glass: rgba(255, 255, 255, 0.03); --green: #10b981; --red: #ef4444; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: white; min-height: 100vh; overflow-x: hidden; }
        
        .glass-card { background: var(--glass); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.08); border-radius: 24px; padding: 20px; width: 92%; max-width: 450px; margin: 15px auto; }
        .btn-main { width: 100%; padding: 15px; background: var(--gold); border: none; border-radius: 14px; color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        input, select { width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 14px; color: white; margin-bottom: 12px; outline: none; }
        
        #welcomeScreen, #authSection, #dashboardPage, #adminPage, #adminTrigger, #loadingOverlay { display: none; }
        .active { display: block !important; }
        .flex-active { display: flex !important; }
        
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        .spinner { width: 40px; height: 40px; border: 3px solid rgba(251, 191, 36, 0.1); border-top: 3px solid var(--gold); border-radius: 50%; animation: spin 1s linear infinite; }
    </style>
</head>
<body>

    <div id="welcomeScreen" class="flex-active" style="position:fixed; inset:0; background:#020617; z-index:10000; flex-direction:column; align-items:center; justify-content:center;">
        <h1 style="font-size:3.5rem; color:var(--gold);">PakGold</h1>
        <button onclick="enterApp()" class="btn-main" style="width:200px; margin-top:30px;">Get Started 🚀</button>
    </div>

    <div id="loadingOverlay" style="position:fixed; inset:0; background:rgba(2,6,23,0.9); z-index:20000; display:none; flex-direction:column; align-items:center; justify-content:center;">
        <div class="spinner"></div>
    </div>

    <div id="authSection" class="active">
        <h1 id="secretTap" style="text-align:center; margin:40px 0; color:var(--gold);">PakGold</h1>
        <div class="glass-card">
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Password">
            <button id="authBtn" class="btn-main">Continue</button>
            <p id="toggleAuth" style="text-align:center; font-size:0.8rem; margin-top:15px; cursor:pointer; color:#94a3b8;">New user? <span style="color:var(--gold)">Create Account</span></p>
            
            <div id="adminTrigger" style="margin-top:20px; display:none;">
                <input type="password" id="adminKey" placeholder="Admin Key" style="border-color:var(--red);">
                <button onclick="checkAdmin()" class="btn-main" style="background:var(--red); color:white;">Access God Mode</button>
            </div>
        </div>
    </div>

    <div id="dashboardPage">
        <div class="glass-card" style="border-color:var(--gold);">
            <div style="display:flex; justify-content:space-between; align-items:center;">
                <span id="timerBadge" style="font-size:0.7rem; color:var(--red);">00:00:00</span>
                <button onclick="logout()" style="background:none; border:none; color:var(--red); font-size:0.6rem;">Logout</button>
            </div>
            <div style="text-align:center; margin:20px 0;">
                <h1 style="color:var(--gold); font-size:2.5rem;">PKR <span id="bal">0.00</span></h1>
                <p style="font-size:0.6rem; color:#94a3b8;">Active Rig: <b id="machine">None</b></p>
            </div>
            <div style="width:100%; height:4px; background:rgba(255,255,255,0.05); border-radius:10px;">
                <div id="miningProgress" style="width:0%; height:100%; background:var(--gold); border-radius:10px; transition:1s;"></div>
            </div>
        </div>

        <div class="glass-card">
            <p style="font-size:0.7rem; margin-bottom:10px;"><b>EasyPaisa:</b> 03379827882 | <b>JazzCash:</b> 03705519562</p>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px;">
                <button onclick="togglePop('depPop')" class="btn-main" style="background:var(--glass); color:white; border:1px solid var(--gold); font-size:0.7rem;">Deposit</button>
                <button onclick="togglePop('witPop')" class="btn-main" style="background:var(--glass); color:white; border:1px solid var(--red); font-size:0.7rem;">Withdraw</button>
            </div>
        </div>

        <div id="depPop" class="glass-card" style="display:none;">
            <input type="number" id="depAmt" placeholder="Amount">
            <input type="text" id="depTID" placeholder="TID Number">
            <button onclick="submitDep()" class="btn-main">Submit</button>
        </div>

        <div id="witPop" class="glass-card" style="display:none;">
            <input type="number" id="witAmt" placeholder="Amount">
            <input type="text" id="witNum" placeholder="Account Number">
            <select id="witMethod"><option>EasyPaisa</option><option>JazzCash</option><option>SadaPay</option></select>
            <button onclick="submitWit()" class="btn-main" style="background:var(--red); color:white;">Request</button>
        </div>

        <div class="glass-card" style="border-color:#25d366;">
            <a href="YOUR_LINK" style="text-decoration:none;"><button class="btn-main" style="background:#25d366; color:white;">WhatsApp Group</button></a>
            <p style="text-align:center; font-size:0.7rem; margin-top:10px;">webhub262@gmail.com</p>
        </div>
    </div>

    <div id="adminPage">
        <h2 style="text-align:center; color:var(--red); margin:20px 0;">GOD MODE 👑</h2>
        <div id="adminList"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getFirestore, doc, getDoc, setDoc, updateDoc, onSnapshot, collection, query, where, addDoc, serverTimestamp, increment } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

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
        let currentUser = localStorage.getItem('pakgold_user');
        let isLogin = true;

        // --- AUTH LOGIC ---
        document.getElementById('authBtn').onclick = async () => {
            const user = document.getElementById('username').value.toLowerCase().trim();
            const pass = document.getElementById('password').value;
            if(!user || !pass) return alert("Enter details");
            
            const uRef = doc(db, "users", user);
            const snap = await getDoc(uRef);

            if(isLogin) {
                if(snap.exists() && snap.data().pass === pass) login(user);
                else alert("Wrong credentials");
            } else {
                if(snap.exists()) return alert("Exists");
                await setDoc(uRef, { user, pass, balance: 0, machine: "None", profit: 0, lastClaim: 0, machineExpiry: 0 });
                login(user);
            }
        };

        function login(u) { localStorage.setItem('pakgold_user', u); location.reload(); }
        window.logout = () => { localStorage.removeItem('pakgold_user'); location.reload(); }

        // --- CORE FEATURES (Auto-Mining & Bonus) ---
        async function runCore(data, uRef) {
            const today = new Date().toDateString();
            if(data.lastBonusDate !== today) {
                await updateDoc(uRef, { balance: increment(5), lastBonusDate: today });
            }
            if(data.machine !== "None" && (Date.now() - data.lastClaim > 86400000)) {
                await updateDoc(uRef, { balance: increment(data.profit), lastClaim: Date.now() });
            }
        }

        if(currentUser) {
            document.getElementById('authSection').style.display='none';
            document.getElementById('dashboardPage').classList.add('active');
            const uRef = doc(db, "users", currentUser);
            onSnapshot(uRef, (s) => {
                const d = s.data();
                document.getElementById('bal').innerText = d.balance.toFixed(2);
                document.getElementById('machine').innerText = d.machine;
                runCore(d, uRef);
            });
        }
        
        window.enterApp = () => document.getElementById('welcomeScreen').style.display='none';
        window.togglePop = (id) => { const e = document.getElementById(id); e.style.display = e.style.display==='none'?'block':'none'; };
    </script>
</body>
</html>
