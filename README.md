<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily | Sovereign Trading Hub</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { background: #020617; color: white; font-family: 'Outfit', sans-serif; overflow-x: hidden; }
        
        .glass { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.08); }
        .neon-card { background: linear-gradient(135deg, #1d4ed8, #7e22ce, #db2777); box-shadow: 0 20px 50px rgba(126, 34, 206, 0.3); }
        .btn-glow { background: linear-gradient(90deg, #3b82f6, #ec4899); transition: 0.3s; }
        .btn-glow:active { transform: scale(0.95); }

        #toast-alert { transition: 0.5s; transform: translateY(-150%); }
        #toast-alert.active { transform: translateY(0); }
        
        ::-webkit-scrollbar { width: 0; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, onAuthStateChanged, signOut, signInWithEmailAndPassword, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, collection, addDoc, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        // --- PASTE YOUR CONFIG HERE SWEETIE ---
        const firebaseConfig = {
            apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg",
            authDomain: "investment-84f4e.firebaseapp.com",
            projectId: "investment-84f4e",
            storageBucket: "investment-84f4e.firebasestorage.app",
            messagingSenderId: "975293889308",
            appId: "1:975293889308:web:6d034a99cc966c75ff58d9"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const PKR_RATE = 280;

        // --- AUTH TRACKER ---
        onAuthStateChanged(auth, (user) => {
            if(user) {
                document.getElementById('authWrap').classList.add('hidden');
                document.getElementById('appWrap').classList.remove('hidden');
                if(user.email === "admin@apexdaily.com") {
                    document.getElementById('masterBtn').classList.remove('hidden');
                    loadAdminSuite();
                }
                syncUserData(user.uid);
            } else {
                document.getElementById('authWrap').classList.remove('hidden');
                document.getElementById('appWrap').classList.add('hidden');
            }
        });

        // --- DATA SYNC ---
        function syncUserData(uid) {
            onSnapshot(doc(db, "users", uid), (d) => {
                if(d.exists()) {
                    const data = d.data();
                    document.getElementById('uBal').innerText = "$" + (data.balance || 0).toFixed(2);
                    document.getElementById('uPkr').innerText = "≈ Rs. " + ((data.balance || 0) * PKR_RATE).toLocaleString();
                    document.getElementById('uName').innerText = data.username;
                }
            });
        }

        // --- UI LOGIC ---
        window.switchTab = (id) => {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        };

        window.calcPkr = () => {
            const usd = document.getElementById('depAmt').value;
            document.getElementById('pkrLabel').innerText = usd ? `Total: Rs. ${usd * PKR_RATE}` : "Rs. 0";
        };

        // Live Alerts
        setInterval(() => {
            const alerts = ["User 'Ali**' earned $5.40!", "New Deposit of $100 from Karachi!", "Withdrawal of $20 Paid!"];
            const toast = document.getElementById('toast-alert');
            document.getElementById('toast-msg').innerText = alerts[Math.floor(Math.random() * alerts.length)];
            toast.classList.add('active');
            setTimeout(() => toast.classList.remove('active'), 3000);
        }, 10000);

        window.handleAuth = async (m) => {
            const e = document.getElementById('emailInp').value;
            const p = document.getElementById('passInp').value;
            try {
                if(m === 'up') {
                    const r = await createUserWithEmailAndPassword(auth, e, p);
                    await setDoc(doc(db, "users", r.user.uid), { username: e.split('@')[0], balance: 0, email: e, role: 'user' });
                } else { await signInWithEmailAndPassword(auth, e, p); }
            } catch(err) { alert(err.message); }
        };

        window.logout = () => signOut(auth);
    </script>
</head>
<body class="pb-32">

    <div id="toast-alert" class="fixed top-4 left-0 right-0 z-[1000] px-6">
        <div class="max-w-xs mx-auto glass p-3 rounded-2xl flex items-center gap-3 border-l-4 border-blue-500 shadow-2xl">
            <i class="fa-solid fa-circle-check text-blue-500"></i>
            <p id="toast-msg" class="text-[10px] font-bold"></p>
        </div>
    </div>

    <div id="authWrap" class="h-screen flex items-center justify-center p-6">
        <div class="w-full max-w-sm glass p-10 rounded-[3rem] text-center">
            <h1 class="text-4xl font-black italic text-blue-500 mb-8 tracking-tighter">APEXDAILY</h1>
            <input id="emailInp" type="email" placeholder="Email Address" class="w-full p-4 bg-white/5 rounded-2xl mb-4 outline-none border border-white/10">
            <input id="passInp" type="password" placeholder="Password" class="w-full p-4 bg-white/5 rounded-2xl mb-8 outline-none border border-white/10">
            <div class="flex gap-3">
                <button onclick="handleAuth('in')" class="flex-1 btn-glow py-4 rounded-2xl font-bold">Login</button>
                <button onclick="handleAuth('up')" class="flex-1 bg-white/5 py-4 rounded-2xl font-bold border border-white/10 text-xs">Sign Up</button>
            </div>
        </div>
    </div>

    <div id="appWrap" class="hidden">
        
        <div class="p-6 flex justify-between items-center">
            <div class="flex items-center gap-2">
                <div class="w-8 h-8 rounded-full bg-blue-600 flex items-center justify-center font-black text-xs">A</div>
                <h2 id="uName" class="text-sm font-bold">User</h2>
                <i class="fa-solid fa-circle-check text-blue-500 text-[10px]" title="Verified"></i>
            </div>
            <a href="#" class="text-[10px] font-bold text-green-400 bg-green-400/10 px-4 py-1.5 rounded-full border border-green-500/20">GROUP LINK</a>
        </div>

        <div id="home" class="tab-content active p-6">
            <div class="neon-card p-8 rounded-[2.5rem] relative overflow-hidden mb-8">
                <p class="text-[10px] font-bold opacity-80 uppercase tracking-widest">Total Capital</p>
                <h1 id="uBal" class="text-6xl font-black my-2 tracking-tighter">$0.00</h1>
                <p id="uPkr" class="text-xs font-medium opacity-70">Rs. 0</p>
                <div class="flex gap-3 mt-8">
                    <button onclick="switchTab('wallet')" class="flex-1 bg-white text-blue-600 py-3 rounded-xl font-black text-xs">DEPOSIT</button>
                    <button onclick="switchTab('wallet')" class="flex-1 bg-black/20 py-3 rounded-xl font-black text-xs border border-white/20">WITHDRAW</button>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <div class="glass p-5 rounded-3xl border-b-4 border-green-500">
                    <p class="text-[8px] text-gray-500 font-bold mb-1 uppercase">Daily ROI</p>
                    <h4 class="text-lg font-black text-green-400">+2.85%</h4>
                </div>
                <div class="glass p-5 rounded-3xl border-b-4 border-blue-500">
                    <p class="text-[8px] text-gray-500 font-bold mb-1 uppercase">Next Profit</p>
                    <h4 id="timer" class="text-lg font-black text-blue-400">23:59:10</h4>
                </div>
            </div>

            <div class="glass p-6 rounded-3xl flex items-center justify-between">
                <div><p class="text-sm font-bold">Help Desk</p><p class="text-[10px] text-gray-500 italic">Chat with Admin</p></div>
                <button class="w-12 h-12 bg-blue-600 rounded-2xl flex items-center justify-center"><i class="fa-solid fa-headset text-xl"></i></button>
            </div>
        </div>

        <div id="wallet" class="tab-content p-6">
            <h2 class="text-2xl font-black mb-6 text-blue-500 italic">WALLET CENTER</h2>
            <div class="space-y-4 mb-8">
                <div class="glass p-5 rounded-3xl border-l-4 border-yellow-500 flex justify-between items-center">
                    <div><p class="text-[10px] text-gray-500 uppercase">JazzCash / SadaPay</p><p class="font-black">03705519562</p></div>
                    <i class="fa-solid fa-bolt text-yellow-500"></i>
                </div>
                <div class="glass p-5 rounded-3xl border-l-4 border-green-500 flex justify-between items-center">
                    <div><p class="text-[10px] text-gray-500 uppercase">EasyPaisa</p><p class="font-black">03379827882</p></div>
                    <i class="fa-solid fa-copy text-green-500"></i>
                </div>
            </div>
            <div class="glass p-8 rounded-[2rem]">
                <input id="depAmt" oninput="calcPkr()" type="number" placeholder="Amount ($)" class="w-full p-4 bg-white/5 rounded-2xl mb-2 outline-none border border-white/5">
                <p id="pkrLabel" class="text-[10px] text-blue-400 font-bold mb-6 ml-1">Rs. 0</p>
                <input type="text" placeholder="Transaction ID (TID)" class="w-full p-4 bg-white/5 rounded-2xl mb-8 outline-none border border-white/5">
                <button class="w-full btn-glow py-4 rounded-2xl font-black">CONFIRM PAYMENT</button>
            </div>
        </div>

        <div id="master" class="tab-content p-6">
            <h2 class="text-2xl font-black text-yellow-500 mb-8 italic">MASTER CONTROL</h2>
            <div class="glass p-6 rounded-3xl border-t-4 border-yellow-500 mb-8">
                <p class="text-xs font-bold mb-4 uppercase">Pending Approvals</p>
                <p class="text-[10px] opacity-40 italic">System scanning for new requests...</p>
            </div>
            <button onclick="logout()" class="w-full bg-red-600/20 py-4 rounded-2xl text-red-500 font-bold text-xs">SECURE SYSTEM LOGOUT</button>
        </div>

        <nav class="fixed bottom-0 left-0 right-0 glass p-6 flex justify-around items-center rounded-t-[3rem] z-50 border-t border-white/5">
            <button onclick="switchTab('home')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-house text-xl"></i><span class="text-[8px] font-bold">HOME</span></button>
            <button onclick="switchTab('wallet')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-wallet text-xl"></i><span class="text-[8px] font-bold">WALLET</span></button>
            <button onclick="switchTab('home')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-users text-xl"></i><span class="text-[8px] font-bold">TEAM</span></button>
            <button id="masterBtn" onclick="switchTab('master')" class="hidden flex flex-col items-center gap-1 text-yellow-500"><i class="fa-solid fa-crown text-xl"></i><span class="text-[8px] font-bold">MASTER</span></button>
        </nav>
    </div>

</body>
</html>
