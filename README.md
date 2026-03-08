<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | Professional Node</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        :root { --gold: #fbbf24; --bg: #06090f; --glass: rgba(13, 17, 23, 0.8); }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: #f0f6fc; padding-bottom: 90px; }
        .glass { background: var(--glass); backdrop-filter: blur(15px); border: 1px solid rgba(255, 255, 255, 0.05); border-radius: 24px; }
        .page { display: none; }
        .active-page { display: block !important; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .nav-btn { display: flex; flex-direction: column; align-items: center; gap: 4px; opacity: 0.4; transition: 0.3s; }
        .nav-active { opacity: 1; color: var(--gold); }
        input, select { background: #161b22; border: 1px solid #30363d; border-radius: 12px; padding: 14px; color: white; width: 100%; outline: none; font-size: 13px; }
        .btn-gold { background: linear-gradient(135deg, #fbbf24, #d97706); color: #000; font-weight: 800; border-radius: 14px; }
    </style>
</head>
<body>

    <header class="p-6 sticky top-0 bg-[#06090f]/90 backdrop-blur-md z-[5000]">
        <div class="flex justify-between items-center">
            <div>
                <p class="text-[10px] font-bold text-yellow-500 uppercase tracking-widest">Welcome Back</p>
                <h2 id="display-user" class="text-xl font-black italic">User</h2>
            </div>
            <div class="text-right">
                <p class="text-[8px] opacity-40 uppercase font-black">Yield Balance</p>
                <h2 id="u-bal" class="text-2xl font-black text-white tracking-tighter">₨ 0.00</h2>
            </div>
        </div>
        
        <div class="mt-4 flex gap-2">
            <input type="text" id="promo-box" placeholder="PROMO CODE" class="!p-2 text-center font-bold">
            <button onclick="applyPromo()" class="px-6 btn-gold text-[10px] uppercase">Apply</button>
        </div>
    </header>

    <main class="p-4 space-y-6">

        <div id="p-home" class="page active-page space-y-6">
            <div class="glass p-6 border-l-4 border-yellow-500">
                <div class="flex justify-between items-center">
                    <div>
                        <p class="text-[9px] font-black opacity-50 uppercase">Next Profit Cycle</p>
                        <h3 id="timer" class="text-2xl font-black tracking-widest text-yellow-500">23:59:59</h3>
                    </div>
                    <div class="w-12 h-12 rounded-full bg-yellow-500/10 flex items-center justify-center">⚡</div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-3">
                <button onclick="showPage('deposit')" class="glass p-4 text-center font-bold text-[11px] uppercase border-b-2 border-green-500">Deposit</button>
                <button onclick="showPage('withdraw')" class="glass p-4 text-center font-bold text-[11px] uppercase border-b-2 border-red-500">Withdraw</button>
            </div>
        </div>

        <div id="p-nodes" class="page space-y-4">
            <h3 class="text-xs font-black uppercase text-yellow-500">Mining Infrastructure</h3>
            <div id="nodes-container" class="space-y-3">
                </div>
        </div>

        <div id="p-deposit" class="page space-y-4">
            <div class="glass p-6">
                <h3 class="text-xs font-black uppercase mb-4">Manual Deposit</h3>
                <div class="bg-white/5 p-4 rounded-xl mb-4 text-[11px] font-bold">
                    <p>EasyPaisa: 03379827882</p>
                    <p class="mt-2 text-yellow-500 italic font-black">Note: Take screenshot after payment!</p>
                </div>
                <input type="number" id="dep-amt" placeholder="Amount" class="mb-3">
                <input type="text" id="dep-tid" placeholder="TID Number" class="mb-3">
                <p class="text-[9px] opacity-50 mb-1 ml-1 uppercase">Upload Screenshot Proof:</p>
                <input type="file" id="dep-proof" class="mb-4 !p-2">
                <button onclick="submitDeposit()" class="w-full p-4 btn-gold uppercase text-[10px]">Verify Transaction</button>
            </div>
        </div>

        <div id="p-history" class="page space-y-4">
            <div class="flex justify-between items-center px-1">
                <h3 class="text-xs font-black uppercase">Recent Activity</h3>
                <button onclick="clearHistory()" class="text-[9px] font-black text-red-500 uppercase">Clear All</button>
            </div>
            <div id="hist-list" class="space-y-3">
                </div>
        </div>

        <div id="p-menu" class="page space-y-3">
            <div class="glass p-6 mb-4">
                <p class="text-[8px] opacity-40 uppercase font-black">Share & Earn</p>
                <p id="ref-link" class="text-[10px] text-blue-400 font-mono mt-2 break-all p-2 bg-white/5 rounded">Loading link...</p>
            </div>
            <button class="w-full glass p-4 text-left text-[11px] font-bold uppercase">Company Profile</button>
            <button class="w-full glass p-4 text-left text-[11px] font-bold uppercase">Privacy Policy</button>
            <button onclick="window.location.href='https://wa.me/923705519562'" class="w-full glass p-4 text-left text-[11px] font-bold uppercase text-yellow-500">Contact Admin (Help Desk)</button>
            <button onclick="logout()" class="w-full p-4 bg-red-500/10 text-red-500 rounded-2xl font-black uppercase text-[10px] mt-6">Logout Account</button>
        </div>

    </main>

    <nav class="fixed bottom-0 w-full h-20 glass rounded-t-[2.5rem] border-t border-white/5 flex justify-around items-center px-4 z-[10000]">
        <button onclick="showPage('home')" id="n-home" class="nav-btn nav-active">
            <span class="text-xl">🏠</span><span class="text-[7px] font-black uppercase">Home</span>
        </button>
        <button onclick="showPage('nodes')" id="n-nodes" class="nav-btn">
            <span class="text-xl">⚡</span><span class="text-[7px] font-black uppercase">Nodes</span>
        </button>
        <button onclick="showPage('history')" id="n-history" class="nav-btn">
            <span class="text-xl">📜</span><span class="text-[7px] font-black uppercase">History</span>
        </button>
        <button onclick="showPage('menu')" id="n-menu" class="nav-btn">
            <span class="text-xl">👤</span><span class="text-[7px] font-black uppercase">Menu</span>
        </button>
    </nav>

    <script>
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        let user = localStorage.getItem('pg_user');

        // --- 25 MACHINES GENERATOR ---
        function generateNodes() {
            const container = document.getElementById('nodes-container');
            container.innerHTML = '';
            // 20 Normal + 5 VIP Plans
            for(let i=1; i<=25; i++) {
                let isVIP = i > 20;
                let cost = isVIP ? (i-20)*50000 : 200 + (i*1000);
                container.innerHTML += `
                <div class="glass p-5 flex justify-between items-center border-r-4 ${isVIP?'border-green-500':'border-yellow-500'}">
                    <div>
                        <p class="text-[8px] font-black text-yellow-500 uppercase">${isVIP?'VIP NODE':'S-NODE'} 0${i}</p>
                        <h4 class="text-md font-black italic">₨ ${cost.toLocaleString()}</h4>
                    </div>
                    <button class="px-5 py-2 btn-gold text-[9px] uppercase">Activate</button>
                </div>`;
            }
        }

        // --- DEPOSIT WITH PROOF ---
        async function submitDeposit() {
            const amt = document.getElementById('dep-amt').value;
            const tid = document.getElementById('dep-tid').value;
            const file = document.getElementById('dep-proof').files[0];

            if(!amt || !tid || !file) return alert("Please fill all fields and upload proof!");

            await db.collection("requests").add({
                user: user,
                type: 'deposit',
                amount: parseFloat(amt),
                info: tid,
                proof_status: "file_attached",
                status: "pending",
                time: new Date().toLocaleString()
            });
            alert("Deposit sent! Admin will verify your screenshot and TID.");
        }

        // --- CORE LOGIC ---
        function checkUser() {
            if(user) {
                document.getElementById('display-user').innerText = user.toUpperCase();
                document.getElementById('ref-link').innerText = "https://pakgold.net/join?ref=" + user;
                db.collection("users").doc(user).onSnapshot(d => {
                    document.getElementById('u-bal').innerText = "₨ " + (d.data()?.balance || 0).toLocaleString();
                });
                generateNodes();
                startTimer();
                loadHistory();
            } else {
                let n = prompt("Login Username:");
                if(n) { localStorage.setItem('pg_user', n.toLowerCase()); location.reload(); }
            }
        }

        function startTimer() {
            setInterval(() => {
                let d = new Date();
                let h = 23 - d.getHours();
                let m = 59 - d.getMinutes();
                let s = 59 - d.getSeconds();
                document.getElementById('timer').innerText = `${h < 10 ? '0'+h : h}:${m < 10 ? '0'+m : m}:${s < 10 ? '0'+s : s}`;
            }, 1000);
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('nav-active'));
            document.getElementById('n-'+p)?.classList.add('nav-active');
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = checkUser;
    </script>
</body>
</html>
