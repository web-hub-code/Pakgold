<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | Enterprise Node</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        :root { --bg: #06090f; --card: #0d1117; --accent: #fbbf24; --glass: rgba(13, 17, 23, 0.85); }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: #f0f6fc; padding-bottom: 100px; overflow-x: hidden; }
        
        /* Premium Glassmorphism */
        .glass-card { background: var(--glass); backdrop-filter: blur(12px); border: 1px solid rgba(255, 255, 255, 0.05); border-radius: 28px; transition: 0.3s; }
        .page { display: none; animation: slideUp 0.4s ease forwards; }
        .active-page { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        
        /* Luxury Buttons */
        .btn-premium { background: linear-gradient(135deg, #fbbf24, #d97706); color: #000; font-weight: 800; border-radius: 18px; box-shadow: 0 8px 15px rgba(217, 119, 6, 0.2); }
        input, select { background: #161b22; border: 1px solid #30363d; border-radius: 16px; padding: 15px; color: white; width: 100%; outline: none; }
        
        .nav-active { color: var(--accent) !important; filter: drop-shadow(0 0 10px var(--accent)); opacity: 1 !important; }
        marquee { background: rgba(251, 191, 36, 0.08); color: var(--accent); font-size: 11px; font-weight: 800; padding: 10px 0; border-bottom: 1px solid rgba(251, 191, 36, 0.1); }
    </style>
</head>
<body>

    <marquee id="v-news">Welcome to PakGold Official Node. Secure your future with our 25-tier mining infrastructure! 😘</marquee>

    <header class="p-6 flex justify-between items-center sticky top-0 bg-[#06090f]/90 backdrop-blur-md z-[5000]">
        <div onclick="adminTap()">
            <h1 class="text-xl font-black italic text-yellow-500 tracking-tighter uppercase">PAK<span class="text-white">GOLD</span></h1>
            <p id="display-user" class="text-[8px] font-bold opacity-50 uppercase tracking-[3px]">Guest</p>
        </div>
        <div class="text-right">
            <p class="text-[8px] font-black opacity-30 uppercase">Available Assets</p>
            <h2 id="u-bal" class="text-2xl font-black tracking-tighter text-white">₨ 0.00</h2>
        </div>
    </header>

    <main class="p-4 space-y-6">

        <div id="p-home" class="page active-page space-y-6">
            <div class="glass-card p-6 border-l-4 border-yellow-500 flex justify-between items-center">
                <div>
                    <p class="text-[9px] font-black opacity-40 uppercase tracking-widest">Next Yield Cycle</p>
                    <h3 id="timer" class="text-2xl font-black text-yellow-500 tracking-tighter">23:59:59</h3>
                </div>
                <div class="w-12 h-12 rounded-full bg-yellow-500/10 flex items-center justify-center animate-pulse text-xl">⚡</div>
            </div>

            <div class="glass-card p-4 flex gap-2">
                <input type="text" id="promo-box" placeholder="PROMO CODE" class="!p-3 text-center font-bold uppercase text-[12px]">
                <button onclick="applyPromo()" class="btn-premium px-8 text-[10px] uppercase">Apply</button>
            </div>

            <div class="grid grid-cols-2 gap-4">
                <button onclick="showPage('recharge')" class="glass-card p-5 text-center font-bold text-[11px] uppercase border-b-2 border-green-500">Deposit</button>
                <button onclick="showPage('withdraw')" class="glass-card p-5 text-center font-bold text-[11px] uppercase border-b-2 border-red-500">Withdraw</button>
            </div>
        </div>

        <div id="p-nodes" class="page space-y-4">
            <div class="flex gap-2 p-1 bg-white/5 rounded-2xl mb-4">
                <button onclick="renderNodes('normal')" id="btn-n" class="flex-1 p-3 rounded-xl bg-yellow-500 text-black text-[9px] font-black uppercase">Standard (20)</button>
                <button onclick="renderNodes('vip')" id="btn-v" class="flex-1 p-3 rounded-xl text-[9px] font-black uppercase opacity-40">VIP Plans (5)</button>
            </div>
            <div id="nodes-grid" class="space-y-4"></div>
        </div>

        <div id="p-recharge" class="page space-y-4">
            <div class="glass-card p-8">
                <h3 class="text-xs font-black uppercase text-yellow-500 mb-6 tracking-widest">Add Liquidity</h3>
                <div class="bg-white/5 p-4 rounded-2xl border-l-4 border-yellow-500 mb-6">
                    <p class="text-[10px] font-bold opacity-60 uppercase mb-1">EasyPaisa Account</p>
                    <p class="text-md font-black">03379827882</p>
                </div>
                <div class="space-y-4">
                    <input type="number" id="d-amt" placeholder="Amount (PKR)">
                    <input type="text" id="d-tid" placeholder="Transaction ID (TID)">
                    <p class="text-[8px] font-black opacity-40 uppercase ml-2">Upload Transaction Proof (Image):</p>
                    <input type="file" id="d-proof" class="!p-2 text-[10px]">
                    <button onclick="submitDeposit()" class="w-full p-4 btn-premium uppercase text-[11px] tracking-widest mt-4">Verify Deposit</button>
                </div>
            </div>
        </div>

        <div id="p-history" class="page space-y-4">
            <div class="flex justify-between items-center px-1">
                <h3 class="text-xs font-black uppercase opacity-50">Transaction Log</h3>
                <button onclick="clearHistory()" class="text-[9px] font-black text-red-500 uppercase tracking-widest">Clear All</button>
            </div>
            <div id="hist-list" class="space-y-3"></div>
        </div>

        <div id="p-menu" class="page space-y-4">
            <div class="glass-card p-6 text-center">
                <p class="text-[8px] font-black opacity-40 uppercase mb-2">My Network Link</p>
                <p id="ref-link" class="text-[10px] text-blue-400 break-all p-3 bg-white/5 rounded-2xl border border-dashed border-white/10">Generating...</p>
                <button onclick="copyRef()" class="mt-4 text-[9px] font-black uppercase text-yellow-500">Copy Invitation</button>
            </div>
            <button class="w-full glass-card p-5 text-left text-[11px] font-bold uppercase">Company Portfolio</button>
            <button class="w-full glass-card p-5 text-left text-[11px] font-bold uppercase">Privacy & Terms</button>
            <button onclick="window.location.href='https://wa.me/923705519562'" class="w-full glass-card p-5 text-left text-[11px] font-bold uppercase text-green-500">Contact Help Desk</button>
            <button onclick="logout()" class="w-full p-4 bg-red-500/10 text-red-500 rounded-2xl font-black uppercase text-[10px] mt-8">Secure Logout</button>
        </div>

    </main>

    <nav class="fixed bottom-0 w-full h-24 glass-card rounded-t-[3rem] border-t border-white/5 flex justify-around items-center px-6 z-[10000]">
        <button onclick="showPage('home')" id="n-home" class="nav-active flex flex-col items-center gap-1"><span class="text-2xl">🏠</span><span class="text-[8px] font-black uppercase">Home</span></button>
        <button onclick="showPage('nodes')" id="n-nodes" class="opacity-40 flex flex-col items-center gap-1"><span class="text-2xl">⚡</span><span class="text-[8px] font-black uppercase">Nodes</span></button>
        <button onclick="showPage('history')" id="n-history" class="opacity-40 flex flex-col items-center gap-1"><span class="text-2xl">📜</span><span class="text-[8px] font-black uppercase">Logs</span></button>
        <button onclick="showPage('menu')" id="n-menu" class="opacity-40 flex flex-col items-center gap-1"><span class="text-2xl">👤</span><span class="text-[8px] font-black uppercase">Menu</span></button>
    </nav>

    <script>
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        let user = localStorage.getItem('pg_user');

        // --- NODES RENDERER (25 TOTAL) ---
        function renderNodes(type) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = '';
            const btnN = document.getElementById('btn-n'); const btnV = document.getElementById('btn-v');
            if(type === 'normal') {
                btnN.className = "flex-1 p-3 rounded-xl bg-yellow-500 text-black text-[9px] font-black uppercase";
                btnV.className = "flex-1 p-3 rounded-xl text-[9px] font-black uppercase opacity-40";
                for(let i=1; i<=20; i++) {
                    let cost = 200 + (i*1000);
                    grid.innerHTML += `<div class="glass-card p-6 flex justify-between items-center border-r-4 border-yellow-500"><div class="flex items-center gap-4"><div class="w-10 h-10 rounded-full bg-yellow-500/10 flex items-center justify-center">⛏️</div><div><p class="text-[8px] font-black text-yellow-500 uppercase tracking-widest">S-Node v${i}</p><h4 class="text-xl font-black italic">₨ ${cost.toLocaleString()}</h4></div></div><button class="px-6 py-3 btn-premium text-[9px] uppercase">Rent</button></div>`;
                }
            } else {
                btnV.className = "flex-1 p-3 rounded-xl bg-yellow-500 text-black text-[9px] font-black uppercase";
                btnN.className = "flex-1 p-3 rounded-xl text-[9px] font-black uppercase opacity-40";
                const vips = [50000, 100000, 250000, 500000, 1000000];
                vips.forEach((v, idx) => {
                    grid.innerHTML += `<div class="glass-card p-6 flex justify-between items-center border-r-4 border-green-500"><div class="flex items-center gap-4"><div class="w-10 h-10 rounded-full bg-green-500/10 flex items-center justify-center">💎</div><div><p class="text-[8px] font-black text-green-500 uppercase tracking-widest">VIP Cluster 0${idx+1}</p><h4 class="text-xl font-black italic">₨ ${v.toLocaleString()}</h4></div></div><button class="px-6 py-3 btn-premium text-[9px] uppercase">Claim</button></div>`;
                });
            }
        }

        // --- DEPOSIT WITH PROOF ---
        async function submitDeposit() {
            const amt = document.getElementById('d-amt').value; const tid = document.getElementById('d-tid').value; const file = document.getElementById('d-proof').files[0];
            if(!amt || !tid || !file) return alert("All fields & Receipt required! 😘");
            const reader = new FileReader(); reader.readAsDataURL(file);
            reader.onload = async () => {
                await db.collection("requests").add({ user, amount: parseFloat(amt), tid, proof: reader.result, status: 'pending', type: 'deposit', time: new Date().toLocaleString() });
                alert("Deposit Request Sent to Admin! 😘"); showPage('home');
            };
        }

        // --- CORE LOGIC ---
        function initApp() {
            if(user) {
                document.getElementById('display-user').innerText = "@" + user.toUpperCase();
                document.getElementById('ref-link').innerText = "https://pakgold.net/join?ref=" + user;
                db.collection("users").doc(user).onSnapshot(d => {
                    document.getElementById('u-bal').innerText = "₨ " + (d.data()?.balance || 0).toLocaleString();
                });
                renderNodes('normal'); startCycleTimer();
            } else {
                let n = prompt("Enter Username:"); if(n) { localStorage.setItem('pg_user', n.toLowerCase()); location.reload(); }
            }
        }

        function startCycleTimer() {
            setInterval(() => {
                let now = new Date(); let h = 23 - now.getHours(); let m = 59 - now.getMinutes(); let s = 59 - now.getSeconds();
                document.getElementById('timer').innerText = `${h < 10 ? '0'+h : h}:${m < 10 ? '0'+m : m}:${s < 10 ? '0'+s : s}`;
            }, 1000);
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('nav button').forEach(b => { b.classList.add('opacity-40'); b.classList.remove('nav-active'); });
            document.getElementById('n-'+p)?.classList.add('nav-active');
            document.getElementById('n-'+p)?.classList.remove('opacity-40');
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = initApp;
    </script>
</body>
</html>
