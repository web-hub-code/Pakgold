<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>MintCrest Gold | Enterprise Node</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        :root { --bg: #06090f; --card: #0d1117; --accent: #fbbf24; --text: #f0f6fc; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); padding-bottom: 100px; }
        .glass-card { background: rgba(13, 17, 23, 0.8); backdrop-filter: blur(12px); border: 1px solid rgba(255, 255, 255, 0.05); border-radius: 28px; }
        .page { display: none; animation: fadeIn 0.4s ease; }
        .active-page { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .btn-gold { background: linear-gradient(135deg, #fbbf24, #d97706); color: #000; font-weight: 800; border-radius: 16px; transition: 0.2s; }
        .btn-gold:active { transform: scale(0.95); }
        input { background: #161b22; border: 1px solid #30363d; border-radius: 14px; padding: 14px; color: white; width: 100%; outline: none; }
        .nav-active { color: var(--accent) !important; filter: drop-shadow(0 0 5px var(--accent)); opacity: 1 !important; }
        marquee { font-size: 10px; font-weight: 700; color: var(--accent); background: rgba(251, 191, 36, 0.05); padding: 8px 0; display: block; }
    </style>
</head>
<body>

    <marquee id="v-news" class="sticky top-0 z-[6000]">Welcome to MintCrest Gold. Secure your nodes today for maximum yield! 😘</marquee>

    <header class="p-6 flex justify-between items-center z-[5000]">
        <div onclick="adminTap()">
            <h1 class="text-xl font-black italic text-yellow-500 tracking-tighter uppercase">MINTCREST <span class="text-white">GOLD</span></h1>
            <p id="display-user" class="text-[8px] font-bold opacity-40 uppercase tracking-[2px]">Connecting...</p>
        </div>
        <div class="text-right">
            <p class="text-[8px] font-black opacity-40 uppercase">Assets Balance</p>
            <h2 id="top-bal" class="text-2xl font-black tracking-tighter text-white">₨ 0.00</h2>
        </div>
    </header>

    <main class="p-4 space-y-6">

        <div id="p-home" class="page active-page space-y-6">
            <div class="glass-card p-6 border-l-4 border-yellow-500 flex justify-between items-center">
                <div>
                    <p class="text-[9px] font-black opacity-50 uppercase">Next Node Cycle</p>
                    <h3 id="timer" class="text-2xl font-black text-yellow-500 tracking-widest">23:59:59</h3>
                </div>
                <div class="w-12 h-12 rounded-full bg-yellow-500/10 flex items-center justify-center animate-pulse">⚡</div>
            </div>

            <div class="glass-card p-4 flex gap-2">
                <input type="text" id="promo-input" placeholder="PROMO CODE" class="!p-2 text-center font-bold uppercase">
                <button onclick="applyPromo()" class="btn-gold px-6 text-[10px] uppercase">Apply</button>
            </div>

            <div class="grid grid-cols-2 gap-3">
                <button onclick="showPage('wallet')" class="glass-card p-4 text-center font-bold text-[10px] uppercase border-b-2 border-green-500">Deposit</button>
                <button onclick="showPage('withdraw')" class="glass-card p-4 text-center font-bold text-[10px] uppercase border-b-2 border-red-500">Withdraw</button>
            </div>
        </div>

        <div id="p-nodes" class="page space-y-4">
            <div class="flex gap-2 p-1 bg-white/5 rounded-2xl mb-4">
                <button onclick="renderNodes('standard')" class="flex-1 p-3 rounded-xl bg-yellow-500 text-black text-[9px] font-black uppercase">Standard</button>
                <button onclick="renderNodes('vip')" class="flex-1 p-3 rounded-xl text-[9px] font-black uppercase opacity-40">VIP Nodes</button>
            </div>
            <div id="nodes-grid" class="space-y-3"></div>
        </div>

        <div id="p-wallet" class="page space-y-4">
            <div class="glass-card p-6">
                <h3 class="text-xs font-black uppercase mb-4 text-yellow-500">Recharge Node</h3>
                <div class="space-y-2 mb-4">
                    <div class="flex justify-between p-3 bg-white/5 rounded-xl border-l-2 border-yellow-500">
                        <span class="text-[9px] font-bold">EasyPaisa/JazzCash</span>
                        <span class="text-xs font-black">03379827882</span>
                    </div>
                </div>
                <input type="number" id="dep-amt" placeholder="Amount (PKR)" class="mb-3">
                <input type="text" id="dep-tid" placeholder="Transaction ID (TID)" class="mb-3">
                <input type="file" id="dep-file" class="mb-4 !p-2 text-[10px]">
                <button onclick="submitDep()" class="w-full p-4 btn-gold uppercase text-[10px]">Confirm Payment</button>
            </div>
        </div>

        <div id="p-history" class="page space-y-4">
            <div class="flex justify-between items-center px-1">
                <h3 class="text-xs font-black uppercase opacity-50">Transaction Log</h3>
                <button onclick="clearHistory()" class="text-[9px] font-black text-red-500 uppercase tracking-widest">Clear</button>
            </div>
            <div id="hist-list" class="space-y-3"></div>
        </div>

        <div id="p-menu" class="page space-y-3">
            <div class="glass-card p-6 mb-4 text-center">
                <p class="text-[8px] font-black opacity-40 uppercase">Referral Network</p>
                <p id="ref-link" class="text-[10px] text-blue-400 mt-2 break-all p-3 bg-white/5 rounded-xl border border-dashed border-white/10">Loading...</p>
                <button onclick="copyRef()" class="mt-3 text-[9px] font-black uppercase text-yellow-500">Copy Link</button>
            </div>
            <button class="w-full glass-card p-4 text-left text-[11px] font-bold uppercase">Company Details</button>
            <button class="w-full glass-card p-4 text-left text-[11px] font-bold uppercase">Terms & Privacy</button>
            <button onclick="window.location.href='https://wa.me/923705519562'" class="w-full glass-card p-4 text-left text-[11px] font-bold uppercase text-green-500">Help Desk (Admin)</button>
            <button onclick="logout()" class="w-full p-4 bg-red-500/10 text-red-500 rounded-2xl font-black uppercase text-[10px] mt-6">Secure Logout</button>
        </div>

    </main>

    <nav class="fixed bottom-0 w-full h-20 glass-card rounded-t-[2.5rem] flex justify-around items-center px-4 z-[10000]">
        <button onclick="showPage('home')" id="n-home" class="nav-active flex flex-col items-center gap-1">
            <span class="text-xl">🏠</span><span class="text-[7px] font-black uppercase">Home</span>
        </button>
        <button onclick="showPage('nodes')" id="n-nodes" class="opacity-40 flex flex-col items-center gap-1">
            <span class="text-xl">⚡</span><span class="text-[7px] font-black uppercase">Nodes</span>
        </button>
        <button onclick="showPage('history')" id="n-history" class="opacity-40 flex flex-col items-center gap-1">
            <span class="text-xl">📜</span><span class="text-[7px] font-black uppercase">Log</span>
        </button>
        <button onclick="showPage('menu')" id="n-menu" class="opacity-40 flex flex-col items-center gap-1">
            <span class="text-xl">👤</span><span class="text-[7px] font-black uppercase">Profile</span>
        </button>
    </nav>

    <script>
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        let user = localStorage.getItem('mc_user');

        function renderNodes(type) {
            const grid = document.getElementById('nodes-grid');
            grid.innerHTML = '';
            const count = type === 'standard' ? 20 : 5;
            for(let i=1; i<=count; i++) {
                let cost = type === 'standard' ? 200 + (i*800) : 25000 + (i*10000);
                grid.innerHTML += `
                <div class="glass-card p-5 flex justify-between items-center border-r-4 ${type==='standard'?'border-yellow-500':'border-green-500'}">
                    <div>
                        <p class="text-[8px] font-black text-yellow-500 uppercase">${type==='standard'?'Node v':'VIP Node'} 0${i}</p>
                        <h4 class="text-md font-black italic text-white">₨ ${cost.toLocaleString()}</h4>
                    </div>
                    <button class="px-5 py-2 btn-gold text-[9px] uppercase">Rent</button>
                </div>`;
            }
        }

        async function submitDep() {
            const amt = document.getElementById('dep-amt').value;
            const tid = document.getElementById('dep-tid').value;
            const file = document.getElementById('dep-file').files[0];
            if(!amt || !tid || !file) return alert("All fields & Proof required! 😘");
            const r = new FileReader(); r.readAsDataURL(file);
            r.onload = async () => {
                await db.collection("requests").add({ user: user, type: 'deposit', amount: parseFloat(amt), tid: tid, proof: r.result, status: 'pending', time: Date.now() });
                alert("Deposit Submitted! Admin will verify soon. 😘");
            };
        }

        function checkUser() {
            if(user) {
                document.getElementById('display-user').innerText = "@" + user;
                document.getElementById('ref-link').innerText = window.location.origin + "?ref=" + user;
                db.collection("users").doc(user).onSnapshot(d => {
                    document.getElementById('top-bal').innerText = "₨ " + (d.data()?.balance || 0).toLocaleString();
                });
                renderNodes('standard');
                startTimer();
            } else {
                let n = prompt("Enter Username:");
                if(n) { localStorage.setItem('mc_user', n.toLowerCase()); location.reload(); }
            }
        }

        function startTimer() {
            setInterval(() => {
                let d = new Date();
                let h = 23 - d.getHours(); let m = 59 - d.getMinutes(); let s = 59 - d.getSeconds();
                document.getElementById('timer').innerText = `${h < 10 ? '0'+h : h}:${m < 10 ? '0'+m : m}:${s < 10 ? '0'+s : s}`;
            }, 1000);
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('nav button').forEach(b => b.classList.add('opacity-40'));
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('nav-active'));
            document.getElementById('n-'+p)?.classList.add('nav-active');
            document.getElementById('n-'+p)?.classList.remove('opacity-40');
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = checkUser;
    </script>
</body>
</html>
