<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Admin God Mode 👑</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #fbbf24; --bg: #020617; --glass: rgba(255, 255, 255, 0.03); --green: #10b981; --red: #ef4444; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: white; min-height: 100vh; }
        .glass-card { background: var(--glass); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.08); border-radius: 20px; padding: 15px; width: 92%; max-width: 500px; margin: 10px auto; }
        .brand-name { font-size: 2.5rem; font-weight: 600; color: var(--gold); text-align: center; margin: 15px 0; }
        input, select { width: 100%; padding: 12px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 12px; color: white; margin-bottom: 10px; outline: none; font-size: 0.8rem; }
        .btn-main { width: 100%; padding: 12px; background: var(--gold); border: none; border-radius: 12px; color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; }
        #welcomeScreen, #authSection, #dashboardPage, #adminPage, #adminTrigger, #loadingOverlay { display: none; }
        .active { display: block !important; }
        .flex-active { display: flex !important; }
        .admin-badge { background: var(--red); color: white; font-size: 0.5rem; padding: 2px 5px; border-radius: 4px; vertical-align: middle; }
    </style>
</head>
<body>

    <div id="welcomeScreen" class="flex-active" style="position:fixed; top:0; left:0; width:100%; height:100%; background:#020617; z-index:10000; flex-direction:column; align-items:center; justify-content:center; text-align:center; transition: 0.8s;">
        <div class="brand-name" style="font-size: 3.5rem;">PakGold</div>
        <p style="color: #94a3b8; letter-spacing: 2px;">THE FUTURE OF MINING</p>
        <button onclick="enterApp()" class="btn-main" style="margin-top: 40px; width: 200px;">Get Started 🚀</button>
    </div>

    <div id="authSection">
        <h1 class="brand-name" id="secretTap">PakGold</h1>
        <div class="glass-card">
            <h2 id="authTitle" style="margin-bottom: 15px; font-size: 1rem;">Sign In</h2>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Password">
            <div id="signupExtra" style="display:none;"><input type="text" id="refBy" placeholder="Referral Code (Optional)"></div>
            <button id="authBtn" class="btn-main">Continue</button>
            <p id="toggleAuth" style="font-size: 0.7rem; color: #94a3b8; margin-top: 15px; text-align: center; cursor: pointer;">New user? <span style="color: var(--gold)">Create Account</span></p>
            
            <div id="adminTrigger" style="margin-top: 15px;">
                <input type="password" id="adminKey" placeholder="Master Key" style="border-color: var(--red);">
                <button onclick="checkAdmin()" class="btn-main" style="background:var(--red); color:white;">Login as God 👑</button>
            </div>
        </div>
    </div>

    <div id="dashboardPage">
        <div class="glass-card" style="border-color:#4facfe; padding:8px;"><marquee id="adminNotice" style="font-size:0.7rem; color:#cbd5e1;">Welcome to PakGold Platform.</marquee></div>

        <div class="glass-card">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
                <span style="font-size:0.7rem;">Account: <b id="displayUser"></b></span>
                <button onclick="logout()" style="background:none; border:none; color:var(--red); font-size:0.6rem;">Logout</button>
            </div>
            <div style="text-align:center; background:rgba(251,191,36,0.05); padding:15px; border-radius:15px;">
                <p style="font-size:0.6rem; color:#94a3b8;">Mining Balance</p>
                <h1 style="color:var(--gold);">PKR <span id="bal">0.00</span></h1>
                <p style="font-size:0.55rem; color:var(--green);">Rig: <span id="machine">None</span></p>
            </div>
            <button id="claimBtn" class="btn-main" style="background:var(--green); color:white; margin-top:10px; font-size:0.7rem;">Collect Yield</button>
        </div>

        <div class="glass-card">
            <h4 style="font-size:0.75rem; margin-bottom:10px;">Deposit (JazzCash/SadaPay: 03705519562)</h4>
            <input type="number" id="depAmt" placeholder="Amount">
            <input type="text" id="depTID" placeholder="TID Number">
            <button onclick="submitDep()" class="btn-main" style="font-size:0.7rem;">Submit Proof</button>
        </div>

        <div class="glass-card">
            <h4 style="font-size:0.75rem; margin-bottom:10px;">Withdraw Funds</h4>
            <input type="number" id="witAmt" placeholder="Amount">
            <input type="text" id="witNum" placeholder="Account Number">
            <select id="witMethod"><option>JazzCash</option><option>EasyPaisa</option><option>SadaPay</option></select>
            <button onclick="submitWit()" class="btn-main" style="background:var(--red); color:white; font-size:0.7rem;">Request Payout</button>
        </div>
    </div>

    <div id="adminPage">
        <h2 class="brand-name" style="color:var(--red); font-size:1.5rem;">God Mode Active 👑</h2>
        
        <div class="glass-card">
            <h4 style="font-size:0.8rem; margin-bottom:10px;">Global Notice Update</h4>
            <input type="text" id="newNotice" placeholder="Enter important update...">
            <button onclick="updateNotice()" class="btn-main" style="padding:8px;">Broadcast</button>
        </div>

        <div class="glass-card">
            <h4 style="font-size:0.8rem;">Pending Deposits (TID Check)</h4>
            <div id="adminDepList" style="margin-top:10px;"></div>
        </div>

        <div class="glass-card">
            <h4 style="font-size:0.8rem;">Pending Withdrawals (Payouts)</h4>
            <div id="adminWitList" style="margin-top:10px;"></div>
        </div>

        <button onclick="location.reload()" class="btn-main" style="background:#334155; color:white; width:92%; margin:10px auto; display:block;">Exit God Mode</button>
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
            document.getElementById('welcomeScreen').style.transform = "translateY(-100%)";
            setTimeout(() => {
                document.getElementById('welcomeScreen').style.display = 'none';
                if(!currentUser) document.getElementById('authSection').classList.add('active');
                else showDashboard();
            }, 800);
        };

        // --- AUTH ---
        document.getElementById('authBtn').onclick = async () => {
            const user = document.getElementById('username').value.toLowerCase().trim();
            const pass = document.getElementById('password').value;
            const ref = document.getElementById('refBy').value;
            if(!user || !pass) return alert("Required fields missing.");

            const uRef = doc(db, "users", user);
            const snap = await getDoc(uRef);

            if(isLogin) {
                if(snap.exists() && snap.data().pass === pass) { localStorage.setItem('pakgold_user', user); location.reload(); }
                else alert("Invalid credentials.");
            } else {
                if(snap.exists()) return alert("User exists.");
                await setDoc(uRef, { user, pass, balance: 0, machine: "None", profit: 0, lastClaim: 0, referredBy: ref || null });
                localStorage.setItem('pakgold_user', user); location.reload();
            }
        };

        function showDashboard() {
            document.getElementById('authSection').classList.remove('active');
            document.getElementById('dashboardPage').classList.add('active');
            document.getElementById('displayUser').innerText = currentUser;
            onSnapshot(doc(db, "users", currentUser), (d) => {
                document.getElementById('bal').innerText = d.data().balance.toFixed(2);
                document.getElementById('machine').innerText = d.data().machine;
            });
            onSnapshot(doc(db, "settings", "global"), (d) => { if(d.exists()) document.getElementById('adminNotice').innerText = d.data().notice; });
        }

        // --- PAYMENTS ---
        window.submitDep = async () => {
            const a = Number(document.getElementById('depAmt').value);
            const t = document.getElementById('depTID').value;
            if(!a || !t) return alert("Enter Amount & TID.");
            await addDoc(collection(db, "deposits"), { user: currentUser, amount: a, tid: t, status: "pending", time: serverTimestamp() });
            alert("Deposit request sent for TID: " + t);
        };

        window.submitWit = async () => {
            const a = Number(document.getElementById('witAmt').value);
            const n = document.getElementById('witNum').value;
            const m = document.getElementById('witMethod').value;
            const snap = await getDoc(doc(db, "users", currentUser));
            if(a < 100 || a > snap.data().balance) return alert("Check balance/limit.");
            await updateDoc(doc(db, "users", currentUser), { balance: increment(-a) });
            await addDoc(collection(db, "withdrawals"), { user: currentUser, amount: a, num: n, method: m, status: "pending", time: serverTimestamp() });
            alert("Withdrawal request sent.");
        };

        // --- GOD MODE (ADMIN) ---
        let taps = 0;
        document.getElementById('secretTap').onclick = () => { taps++; if(taps===4) document.getElementById('adminTrigger').style.display='block'; };
        window.checkAdmin = () => {
            if(document.getElementById('adminKey').value === "admin123") {
                document.getElementById('authSection').style.display='none';
                document.getElementById('adminPage').classList.add('active');
                loadAdminData();
            }
        };

        function loadAdminData() {
            onSnapshot(query(collection(db, "deposits"), where("status", "==", "pending")), (s) => {
                const l = document.getElementById('adminDepList'); l.innerHTML = "";
                s.forEach(d => {
                    const data = d.data();
                    l.innerHTML += `<div class="glass-card" style="font-size:0.65rem;">User: ${data.user} | TID: ${data.tid} | Amt: ${data.amount}<br><button onclick="approveDep('${d.id}','${data.user}',${data.amount})" style="color:var(--green); background:none; border:none; margin-right:10px;">APPROVE</button><button onclick="reject('${d.id}','deposits')" style="color:var(--red); background:none; border:none;">REJECT</button></div>`;
                });
            });
            onSnapshot(query(collection(db, "withdrawals"), where("status", "==", "pending")), (s) => {
                const l = document.getElementById('adminWitList'); l.innerHTML = "";
                s.forEach(d => {
                    const data = d.data();
                    l.innerHTML += `<div class="glass-card" style="font-size:0.65rem;">User: ${data.user} | Pay: ${data.num} (${data.method}) | Amt: ${data.amount}<br><button onclick="markPaid('${d.id}')" style="color:var(--green); background:none; border:none;">MARK PAID</button></div>`;
                });
            });
        }

        window.approveDep = async (id, u, a) => { await updateDoc(doc(db, "users", u), { balance: increment(a) }); await updateDoc(doc(db, "deposits", id), { status: "success" }); };
        window.markPaid = async (id) => { await updateDoc(doc(db, "withdrawals", id), { status: "paid" }); };
        window.updateNotice = async () => { await setDoc(doc(db, "settings", "global"), { notice: document.getElementById('newNotice').value }); alert("Notice Broadcasted."); };
        window.logout = () => { localStorage.removeItem('pakgold_user'); location.reload(); };

        if(currentUser) enterApp();
    </script>
</body>
</html>
