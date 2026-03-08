<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | Cloud Infrastructure</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        :root { --bg: #06090f; --card: #0d1117; --blue: #3b82f6; --gold: #fbbf24; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: #f0f6fc; padding-bottom: 90px; }
        .cloud-card { background: rgba(13, 17, 23, 0.8); backdrop-filter: blur(10px); border: 1px solid rgba(255,255,255,0.05); border-radius: 24px; }
        .page { display: none; animation: fadeIn 0.4s ease; }
        .active-page { display: block !important; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .btn-blue { background: linear-gradient(135deg, #3b82f6, #1d4ed8); color: white; font-weight: 800; border-radius: 16px; transition: 0.2s; }
        .btn-blue:active { transform: scale(0.96); }
        .nav-item { opacity: 0.4; transition: 0.3s; font-size: 8px; font-weight: 800; }
        .nav-active { opacity: 1; color: var(--blue); }
        input { background: #161b22; border: 1px solid #30363d; border-radius: 14px; padding: 14px; color: white; width: 100%; outline: none; }
        marquee { background: rgba(59, 130, 246, 0.1); color: var(--blue); font-size: 10px; font-weight: 800; padding: 8px 0; }
    </style>
</head>
<body>

    <marquee id="v-news">Welcome to PakGold! Secure your Cloud Nodes for high profit... 😘</marquee>

    <header class="p-6 flex justify-between items-center sticky top-0 bg-[#06090f]/90 z-[5000] backdrop-blur-md">
        <div>
            <h1 class="text-xl font-black text-blue-500 italic uppercase">PAK<span class="text-white">GOLD</span></h1>
            <p id="u-display" class="text-[8px] font-bold opacity-40 uppercase tracking-widest">Initialising...</p>
        </div>
        <div class="text-right">
            <p class="text-[8px] opacity-40 uppercase font-black">Cloud Balance</p>
            <h2 id="u-bal" class="text-2xl font-black text-white tracking-tighter">₨ 0.00</h2>
        </div>
    </header>

    <main class="p-4 space-y-6">

        <div id="p-home" class="page active-page space-y-6">
            <div class="cloud-card p-6 border-l-4 border-blue-500 flex justify-between items-center">
                <div>
                    <p class="text-[9px] font-black opacity-50 uppercase">Next Yield Countdown</p>
                    <h3 id="timer" class="text-2xl font-black text-blue-500 tracking-widest">23:59:59</h3>
                </div>
                <div class="w-12 h-12 rounded-full bg-blue-500/10 flex items-center justify-center animate-bounce">☁️</div>
            </div>

            <div class="cloud-card p-4 flex gap-2">
                <input type="text" id="promo-input" placeholder="ENTER PROMO CODE" class="text-center font-bold uppercase text-[10px]">
                <button onclick="applyPromo()" class="btn-blue px-6 text-[10px] uppercase">Apply</button>
            </div>

            <div class="grid grid-cols-2 gap-4">
                <button onclick="showPage('deposit')" class="cloud-card p-5 text-center font-black text-[10px] uppercase border-b-2 border-green-500">Deposit</button>
                <button onclick="showPage('withdraw')" class="cloud-card p-5 text-center font-black text-[10px] uppercase border-b-2 border-red-500">Withdraw</button>
            </div>
        </div>

        <div id="p-nodes" class="page space-y-4">
            <div class="flex gap-2 mb-4 p-1 bg-white/5 rounded-2xl">
                <button onclick="renderNodes('normal')" id="btn-n" class="flex-1 p-3 rounded-xl bg-blue-600 text-white text-[9px] font-black uppercase">Standard Nodes</button>
                <button onclick="renderNodes('vip')" id="btn-v" class="flex-1 p-3 rounded-xl text-[9px] font-black uppercase opacity-40">VIP Cluster</button>
            </div>
            <div id="nodes-grid" class="space-y-3"></div>
        </div>

        <div id="p-deposit" class="page space-y-4">
            <div class="cloud-card p-6">
                <h3 class="text-xs font-black uppercase text-blue-500 mb-4">Recharge Cloud</h3>
                <div class="bg-blue-900/10 p-4 rounded-xl mb-5 border border-blue-500/20">
                    <p class="text-[9px] font-bold opacity-60">JazzCash/EasyPaisa:</p>
                    <p class="text-lg font-black">03379827882</p>
                </div>
                <input type="number" id="d-amt" placeholder="Amount (PKR)" class="mb-3">
                <input type="text" id="d-tid" placeholder="Transaction ID (TID)" class="mb-3">
                <p class="text-[8px] font-black opacity-30 mb-1 ml-1 uppercase">Upload Screenshot Proof:</p>
                <input type="file" id="d-proof" class="mb-5 !p-2 text-[10px]">
                <button onclick="submitDeposit()" class="w-full p-4 btn-blue uppercase text-[11px] tracking-widest">Verify Payment</button>
            </div>
        </div>

        <div id="p-history" class="page space-y-4">
            <div class="flex justify-between items-center px-1">
                <h3 class="text-xs font-black uppercase opacity-50">Activity Log</h3>
                <button onclick="clearHistory()" class="text-[9px] font-black text-red-500 uppercase">Clear All</button>
            </div>
            <div id="h-list" class="space-y-3"></div>
        </div>

        <div id="p-menu" class="page space-y-4">
            <div class="cloud-card p-6 text-center">
                <p class="text-[8px] font-black opacity-40 uppercase mb-2">My Referral Link</p>
                <p id="ref-link" class="text-[10px] text-blue-400 break-all p-3 bg-white/5 rounded-2xl border border-dashed border-white/10">Loading...</p>
                <button onclick="copyRef()" class="mt-3 text-[9px] font-black uppercase text-blue-500">Copy Link</button>
            </div>
            <button class="w-full cloud-card p-5 text-left text-[11px] font-black uppercase">About PakGold</button>
            <button class="w-full cloud-card p-5 text-left text-[11px] font-black uppercase">Privacy Policy</button>
            <button onclick="window.location.href='https://wa.me/923705519562'" class="w-full cloud-card p-5 text-left text-[11px] font-black uppercase text-green-500">Contact Admin (Support)</button>
            <button onclick="logout()" class="w-full p-4 bg-red-500/10 text-red-500 rounded-2xl font-black uppercase text-[10px] mt-10">Logout Securely</button>
        </div>

    </main>

    <nav class="fixed bottom-0 w-full h-20 bg-[#0a0e17]/95 backdrop-blur-lg flex justify-around items-center px-6 z-[10000] border-t border-white/5">
        <button onclick="showPage('home')" id="n-home" class="nav-item nav-active flex flex-col items-center gap-1"><span class="text-2xl">🏠</span><span>HOME</span></button>
        <button onclick="showPage('nodes')" id="n-nodes" class="nav-item flex flex-col items-center gap-1"><span class="text-2xl">⚡</span><span>NODES</span></button>
        <button onclick="showPage('history')" id="n-history" class="nav-item flex flex-col items-center gap-1"><span class="text-2xl">📜</span><span>LOGS</span></button>
        <button onclick="showPage('menu')" id="n-menu" class="nav-item flex flex-col items-center gap-1"><span class="text-2xl">👤</span><span>MENU</span></button>
    </nav>

    <script>
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        let user = localStorage.getItem('pg_user');

        // --- NODES GENERATOR (20+5) ---
        function renderNodes(type) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = '';
            const count = type === 'normal' ? 20 : 5;
            document.getElementById('btn-n').className = type === 'normal' ? 'flex-1 p-3 rounded-xl bg-blue-600 text-white text-[9px] font-black uppercase' : 'flex-1 p-3 rounded-xl text-[9px] font-black uppercase opacity-40';
            document.getElementById('btn-v').className = type === 'vip' ? 'flex-1 p-3 rounded-xl bg-blue-600 text-white text-[9px] font-black uppercase' : 'flex-1 p-3 rounded-xl text-[9px] font-black uppercase opacity-40';

            for(let i=1; i<=count; i++) {
                let cost = type === 'normal' ? 200 + (i*1500) : 50000 + (i*20000);
                grid.innerHTML += `<div class="cloud-card p-6 flex justify-between items-center border-r-4 ${type==='normal'?'border-blue-500':'border-yellow-500'}"><div><p class="text-[8px] font-black text-blue-500 uppercase tracking-widest">${type==='normal'?'Standard':'VIP'} v0${i}</p><h4 class="text-xl font-black italic">₨ ${cost.toLocaleString()}</h4></div><button class="px-6 py-3 btn-blue text-[9px] uppercase">Rent</button></div>`;
            }
        }

        // --- DEPOSIT SYSTEM ---
        async function submitDeposit() {
            const a = document.getElementById('d-amt').value; const t = document.getElementById('d-tid').value; const f = document.getElementById('d-proof').files[0];
            if(!a || !t || !f) return alert("All fields and proof required, sweetie! 😘");
            const reader = new FileReader(); reader.readAsDataURL(f);
            reader.onload = async () => {
                await db.collection("requests").add({ user, amount: parseFloat(a), tid: t, proof: reader.result, type: 'deposit', status: 'pending', time: Date.now() });
                alert("Deposit Logged! Admin will verify. 😘"); showPage('home');
            };
        }

        // --- CORE SYNC ---
        function init() {
            if(user) {
                document.getElementById('u-display').innerText = "@" + user.toUpperCase();
                document.getElementById('ref-link').innerText = window.location.origin + "?ref=" + user;
                db.collection("users").doc(user).onSnapshot(d => {
                    document.getElementById('u-bal').innerText = "₨ " + (d.data()?.balance || 0).toLocaleString();
                });
                renderNodes('normal'); startTimer();
            } else {
                let n = prompt("Login ID:"); if(n) { localStorage.setItem('pg_user', n.toLowerCase()); location.reload(); }
            }
        }

        function startTimer() {
            setInterval(() => {
                let d = new Date(); let h = 23-d.getHours(); let m = 59-d.getMinutes(); let s = 59-d.getSeconds();
                document.getElementById('timer').innerText = `${h<10?'0'+h:h}:${m<10?'0'+m:m}:${s<10?'0'+s:s}`;
            }, 1000);
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('.nav-item').forEach(b => { b.classList.add('opacity-40'); b.classList.remove('nav-active'); });
            document.getElementById('n-'+p)?.classList.add('nav-active');
            document.getElementById('n-'+p)?.classList.remove('opacity-40');
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = init;
    </script>
</body>
</html>
