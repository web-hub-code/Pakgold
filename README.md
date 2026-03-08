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
        
        /* UI Elements */
        .glass-card { background: var(--glass); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.08); border-radius: 24px; padding: 20px; width: 92%; max-width: 450px; margin: 15px auto; }
        .brand-name { font-size: 2.5rem; font-weight: 600; color: var(--gold); text-align: center; margin: 20px 0; }
        input, select { width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 14px; color: white; margin-bottom: 12px; outline: none; font-size: 0.9rem; }
        .btn-main { width: 100%; padding: 15px; background: var(--gold); border: none; border-radius: 14px; color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        
        /* Layout Control */
        #welcomeScreen, #authSection, #dashboardPage, #adminPage, #adminTrigger, #loadingOverlay { display: none; }
        .active { display: block !important; }
        .flex-active { display: flex !important; }
        
        /* Animations */
        .hide-welcome { opacity: 0 !important; transform: translateY(-100%) !important; transition: 0.8s ease-in-out; }
        .spinner { width: 40px; height: 40px; border: 3px solid rgba(251, 191, 36, 0.1); border-top: 3px solid var(--gold); border-radius: 50%; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    </style>
</head>
<body>

    <div id="welcomeScreen" class="flex-active" style="position:fixed; top:0; left:0; width:100%; height:100%; background:#020617; z-index:10000; flex-direction:column; align-items:center; justify-content:center; text-align:center;">
        <div class="brand-name" style="font-size: 3.5rem;">PakGold</div>
        <p style="color: #94a3b8; letter-spacing: 2px;">THE FUTURE OF MINING</p>
        <button onclick="enterApp()" class="btn-main" style="margin-top: 40px; width: 200px;">Get Started 🚀</button>
    </div>

    <div id="loadingOverlay" style="position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(2,6,23,0.9); z-index:20000; flex-direction:column; align-items:center; justify-content:center;">
        <div class="spinner"></div>
        <p style="color:var(--gold); margin-top:15px; font-size:0.7rem; letter-spacing:1px;">SECURE CONNECTION...</p>
    </div>

    <div id="authSection">
        <h1 class="brand-name" id="secretTap">PakGold</h1>
        <div class="glass-card">
            <h2 id="authTitle" style="margin-bottom: 20px; font-size: 1.1rem;">Sign In</h2>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Password">
            <div id="signupExtra" style="display:none;"><input type="text" id="refBy" placeholder="Referral Code (Optional)"></div>
            <button id="authBtn" class="btn-main">Continue</button>
            <p id="toggleAuth" style="font-size: 0.8rem; color: #94a3b8; margin-top: 20px; text-align: center; cursor: pointer;">New user? <span style="color: var(--gold)">Create Account</span></p>
            
            <div id="adminTrigger" style="margin-top: 15px;">
                <input type="password" id="adminKey" placeholder="System Access Key" style="border-color: var(--red);">
                <button onclick="checkAdmin()" class="btn-main" style="background:var(--red); color:white;">Access Vault</button>
            </div>
        </div>
    </div>

    <div id="dashboardPage">
        <div class="glass-card" style="margin-top:10px; padding:10px; border-color:#4facfe;">
            <marquee id="adminNotice" style="font-size: 0.7rem; color: #cbd5e1;">Welcome to PakGold Mining Platform.</marquee>
        </div>

        <div class="glass-card" style="border-color: var(--gold);">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
                <span style="font-size:0.7rem;">Status: <b id="timerBadge" style="color:var(--red);">00:00:00</b></span>
                <button onclick="logout()" style="background:none; border:none; color:var(--red); font-size:0.6rem;">Logout</button>
            </div>
            <div style="text-align:center;">
                <h1 style="color:var(--gold); font-size: 2rem;">PKR <span id="bal">0.00</span></h1>
                <p style="font-size:0.6rem; color:#94a3b8;">Mining Rig: <b id="machine" style="color:var(--green);">None</b></p>
            </div>
            <div style="width:100%; height:4px; background:rgba(255,255,255,0.05); border-radius:10px; margin:15px 0;">
                <div id="miningProgress" style="width:0%; height:100%; background:var(--gold); border-radius:10px; transition:1s;"></div>
            </div>
            <button id="claimBtn" class="btn-main" style="background:var(--green); color:white; font-size:0.75rem;">Collect Yield</button>
        </div>

        <div class="glass-card">
            <h4 style="font-size:0.8rem; margin-bottom:10px;">Funds Management</h4>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px;">
                <button onclick="showPop('depPop')" style="background:var(--glass); color:white; border:1px solid var(--gold); padding:10px; border-radius:10px; font-size:0.7rem;">Deposit</button>
                <button onclick="showPop('witPop')" style="background:var(--glass); color:white; border:1px solid var(--red); padding:10px; border-radius:10px; font-size:0.7rem;">Withdraw</button>
            </div>
        </div>

        <div id="depPop" class="glass-card" style="display:none; border-color:var(--gold);">
            <p style="font-size:0.6rem; margin-bottom:10px;">Send to: <b>03705519562</b> (Jazz/Sada)</p>
            <input type="number" id="depAmt" placeholder="Amount">
            <input type="text" id="depTID" placeholder="TID Number">
            <button onclick="submitDep()" class="btn-main" style="padding:10px;">Submit Proof</button>
        </div>

        <div id="witPop" class="glass-card" style="display:none; border-color:var(--red);">
            <input type="number" id="witAmt" placeholder="Amount">
            <input type="text" id="witNum" placeholder="Account (03xx...)">
            <select id="witMethod"><option>JazzCash</option><option>EasyPaisa</option><option>SadaPay</option></select>
            <button onclick="submitWit()" class="btn-main" style="background:var(--red); color:white; padding:10px;">Request Payout</button>
        </div>

        <div class="glass-card">
            <h4 style="font-size:0.75rem; margin-bottom:10px;">Payout History</h4>
            <div id="historyLogs" style="font-size:0.6rem; max-height:150px; overflow-y:auto;"></div>
        </div>

        <div class="glass-card">
            <h4 style="font-size:0.8rem; margin-bottom:10px;">Store</h4>
            <div id="mGrid" style="display:grid; grid-template-columns:1fr 1fr; gap:10px;"></div>
        </div>
    </div>

    <div id="adminPage">
        <h2 class="brand-name" style="color:var(--red);">God Mode 👑</h2>
        <div class="glass-card">
            <h4 style="font-size:0.8rem;">Pending Deposits</h4>
            <div id="adminDepList" style="margin-top:10px;"></div>
        </div>
        <div class="glass-card">
            <h4 style="font-size:0.8rem;">Pending Withdrawals</h4>
            <div id="adminWitList" style="margin-top:10px;"></div>
        </div>
        <button onclick="location.reload()" class="btn-main" style="background:#334155; color:white; margin-top:10px;">Exit</button>
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

        window.enterApp = () => {
            new Audio('https://www.soundjay.com/buttons/button-20.mp3').play().catch(()=>{});
            document.getElementById('welcomeScreen').classList.add('hide-welcome');
            setTimeout(() => {
                document.getElementById('welcomeScreen').style.display = 'none';
                if(!currentUser) document.getElementById('authSection').classList.add('active');
                else showDashboard();
            }, 800);
        };

        // --- AUTH & DASHBOARD ---
        document.getElementById('authBtn').onclick = async () => {
            const user = document.getElementById('username').value.toLowerCase().trim();
            const pass = document.getElementById('password').value;
            if(!user || !pass) return alert("Fill all fields.");
            document.getElementById('loadingOverlay').style.display = 'flex';
            setTimeout(async () => {
                const uRef = doc(db, "users", user);
                const snap = await getDoc(uRef);
                if(isLogin) {
                    if(snap.exists() && snap.data().pass === pass) login(user);
                    else { alert("Access Denied."); document.getElementById('loadingOverlay').style.display = 'none'; }
                } else {
                    if(snap.exists()) { alert("User exists."); document.getElementById('loadingOverlay').style.display = 'none'; return; }
                    await setDoc(uRef, { user, pass, balance: 0, machine: "None", profit: 0, lastClaim: 0, machineExpiry: 0 });
                    login(user);
                }
            }, 1500);
        };

        function login(u) { localStorage.setItem('pakgold_user', u); location.reload(); }
        window.logout = () => { localStorage.removeItem('pakgold_user'); location.reload(); }

        function showDashboard() {
            document.getElementById('authSection').classList.remove('active');
            document.getElementById('dashboardPage').classList.add('active');
            document.getElementById('displayUser').innerText = currentUser;
            
            onSnapshot(doc(db, "users", currentUser), (d) => {
                const data = d.data();
                document.getElementById('bal').innerText = data.balance.toFixed(2);
                document.getElementById('machine').innerText = data.machine;
                if(data.machineExpiry > 0) startTimer(data.machineExpiry);
            });
            loadHistory();
        }

        function startTimer(expiry) {
            setInterval(() => {
                const dist = expiry - Date.now();
                if(dist < 0) return document.getElementById('timerBadge').innerText = "EXPIRED";
                const d = Math.floor(dist / 86400000), h = Math.floor((dist % 86400000) / 3600000), m = Math.floor((dist % 3600000) / 60000);
                document.getElementById('timerBadge').innerText = `${d}d ${h}h ${m}m`;
                document.getElementById('miningProgress').style.width = ((Date.now() % 86400000) / 864000) + "%";
            }, 1000);
        }

        // --- STORE & PAYMENTS ---
        const rigs = [{n:"Nano",p:200,d:14}, {n:"Pro",p:2000,d:160}, {n:"Elite",p:10000,d:1500}];
        rigs.forEach(r => {
            document.getElementById('mGrid').innerHTML += `<div class="glass-card" style="margin:0; text-align:center; padding:10px; font-size:0.6rem;"><b>${r.n}</b><br>PKR ${r.p}<br><button onclick="buyR('${r.n}',${r.p},${r.d})" style="background:var(--gold); border:none; border-radius:5px; padding:4px; width:100%; margin-top:5px;">Buy</button></div>`;
        });

        window.buyR = async (n, p, d) => {
            const uRef = doc(db, "users", currentUser);
            const s = await getDoc(uRef);
            if(s.data().balance < p) return alert("Low balance.");
            await updateDoc(uRef, { balance: increment(-p), machine: n, profit: d, machineExpiry: Date.now() + (30*86400000) });
            alert(n + " Activated!");
        };

        window.showPop = (id) => { document.getElementById(id).style.display = document.getElementById(id).style.display === 'none' ? 'block' : 'none'; };
        window.submitDep = async () => {
            const a = Number(document.getElementById('depAmt').value), t = document.getElementById('depTID').value;
            if(!a || !t) return alert("Enter TID & Amount.");
            await addDoc(collection(db, "deposits"), { user: currentUser, amount: a, tid: t, status: "pending", time: serverTimestamp() });
            alert("Sent for approval.");
        };
        window.submitWit = async () => {
            const a = Number(document.getElementById('witAmt').value), n = document.getElementById('witNum').value, m = document.getElementById('witMethod').value;
            const s = await getDoc(doc(db, "users", currentUser));
            if(a < 100 || a > s.data().balance) return alert("Invalid amount.");
            await updateDoc(doc(db, "users", currentUser), { balance: increment(-a) });
            await addDoc(collection(db, "withdrawals"), { user: currentUser, amount: a, num: n, method: m, status: "pending", time: serverTimestamp() });
            alert("Withdrawal requested.");
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "withdrawals"), where("user", "==", currentUser)), (s) => {
                const l = document.getElementById('historyLogs'); l.innerHTML = "";
                s.forEach(d => { l.innerHTML += `<div style="background:rgba(255,255,255,0.03); padding:5px; border-radius:5px; margin-bottom:5px;">PKR ${d.data().amount} - ${d.data().status}</div>`; });
            });
        }

        // --- ADMIN GOD MODE ---
        let taps = 0;
        document.getElementById('secretTap').onclick = () => { taps++; if(taps===4) document.getElementById('adminTrigger').style.display='block'; };
        window.checkAdmin = () => {
            if(document.getElementById('adminKey').value === "admin123") {
                document.getElementById('authSection').style.display='none';
                document.getElementById('adminPage').classList.add('active');
                onSnapshot(query(collection(db, "deposits"), where("status", "==", "pending")), (s) => {
                    const l = document.getElementById('adminDepList'); l.innerHTML = "";
                    s.forEach(d => { l.innerHTML += `<div class="glass-card" style="font-size:0.6rem;">${d.data().user} | TID: ${d.data().tid} | PKR ${d.data().amount}<br><button onclick="appDep('${d.id}','${d.data().user}',${d.data().amount})" style="color:var(--green); border:none; background:none;">APPROVE</button></div>`; });
                });
                onSnapshot(query(collection(db, "withdrawals"), where("status", "==", "pending")), (s) => {
                    const l = document.getElementById('adminWitList'); l.innerHTML = "";
                    s.forEach(d => { l.innerHTML += `<div class="glass-card" style="font-size:0.6rem;">${d.data().user} | ${d.data().num} | PKR ${d.data().amount}<br><button onclick="markP('${d.id}')" style="color:var(--green); border:none; background:none;">MARK PAID</button></div>`; });
                });
            }
        };
        window.appDep = async (id, u, a) => { await updateDoc(doc(db, "users", u), { balance: increment(a) }); await updateDoc(doc(db, "deposits", id), { status: "success" }); };
        window.markP = async (id) => { await updateDoc(doc(db, "withdrawals", id), { status: "paid" }); };

        if(currentUser) enterApp();
    </script>
</body>
</html>
