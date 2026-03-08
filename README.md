<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | CloudSky Infrastructure</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: #06090f; color: #f0f6fc; padding-bottom: 90px; }
        .php-card { background: #0d1117; border: 1px solid #30363d; border-radius: 20px; transition: 0.3s; position: relative; overflow: hidden; }
        .php-card:hover { border-color: #fbbf24; }
        .btn-gold { background: linear-gradient(135deg, #fbbf24, #d97706); color: #000; font-weight: 800; border-radius: 12px; }
        .page { display: none; }
        .active-page { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .nav-item { font-size: 20px; filter: grayscale(1); transition: 0.2s; }
        .nav-active { filter: grayscale(0) drop-shadow(0 0 5px #fbbf24); color: #fbbf24 !important; }
    </style>
</head>
<body>

    <header class="p-5 bg-[#0d1117] border-b border-white/5 sticky top-0 z-[5000]">
        <div class="flex justify-between items-center mb-6">
            <h1 class="text-xl font-black text-yellow-500 italic">PAK<span class="text-white">GOLD</span></h1>
            <button onclick="logout()" class="text-[10px] font-bold bg-red-500/10 text-red-500 px-3 py-1 rounded-lg">LOGOUT</button>
        </div>
        <div class="grid grid-cols-2 gap-4">
            <div class="p-4 bg-white/5 rounded-2xl border border-white/5">
                <p class="text-[8px] opacity-40 font-bold uppercase">Total Assets</p>
                <h2 id="v-bal" class="text-lg font-black text-yellow-500 tracking-tighter">₨ 0.00</h2>
            </div>
            <div class="p-4 bg-white/5 rounded-2xl border border-white/5">
                <p class="text-[8px] opacity-40 font-bold uppercase">Today Profit</p>
                <h2 class="text-lg font-black text-green-500 tracking-tighter">₨ 0.00</h2>
            </div>
        </div>
    </header>

    <div class="px-4 py-2 bg-yellow-500/5 border-y border-yellow-500/10 flex items-center gap-2 overflow-hidden">
        <span class="text-xs">📢</span>
        <marquee class="text-[9px] font-bold text-yellow-500 uppercase italic">Welcome to PakGold! Minimum Deposit ₨ 200 | Earn Daily Profit with Prime Solutions Infrastructure...</marquee>
    </div>

    <main class="p-4">

        <div id="p-home" class="page active-page space-y-4">
            <div class="flex gap-2 p-1 bg-white/5 rounded-xl">
                <button onclick="renderNodes('std')" id="b-std" class="flex-1 py-2 text-[9px] font-black uppercase rounded-lg bg-yellow-500 text-black">Machines</button>
                <button onclick="renderNodes('vip')" id="b-vip" class="flex-1 py-2 text-[9px] font-black uppercase rounded-lg opacity-40">VIP Nodes</button>
            </div>
            <div id="machine-grid" class="grid grid-cols-1 gap-4"></div>
        </div>

        <div id="p-wallet" class="page space-y-4">
            <div class="php-card p-6 space-y-4">
                <h3 class="text-xs font-black text-yellow-500 uppercase">Deposit Center</h3>
                <div class="p-4 bg-white/5 rounded-xl space-y-1 text-[11px] font-bold border-l-2 border-yellow-500">
                    <p class="flex justify-between"><span>EasyPaisa:</span><span>03379827882</span></p>
                    <p class="flex justify-between"><span>JazzCash:</span><span>03705519562</span></p>
                </div>
                <input type="number" placeholder="Enter Amount" class="w-full p-4 bg-black border border-white/10 rounded-xl outline-none text-sm">
                <button class="w-full p-4 btn-gold text-[10px] uppercase tracking-widest shadow-xl">Confirm Deposit Proof</button>
            </div>
        </div>

        <div id="p-logs" class="page space-y-4">
            <h3 class="text-xs font-black text-yellow-500 ml-1">TRANSACTION LOGS</h3>
            <div class="php-card p-8 text-center opacity-30 text-[10px] font-bold uppercase">No records found</div>
        </div>

        <div id="p-about" class="page space-y-4">
            <div class="php-card p-6 text-[11px] leading-relaxed">
                <h4 class="text-yellow-500 font-black mb-2 uppercase">Prime Solutions Official</h4>
                <p class="opacity-70 mb-4">PakGold is a professional mining hub. We offer secure nodes starting from ₨ 200.</p>
                <h4 class="text-yellow-500 font-black mb-2 uppercase tracking-widest text-[9px]">Privacy Policy</h4>
                <p class="opacity-60 italic">Your earnings are calculated hourly and can be withdrawn anytime to your linked JazzCash or EasyPaisa.</p>
            </div>
        </div>

    </main>

    <nav class="fixed bottom-0 w-full p-4 flex justify-around items-center bg-[#0d1117]/95 backdrop-blur-xl border-t border-white/5 rounded-t-[2.5rem] z-[5000]">
        <button onclick="showPage('home')" id="n-home" class="flex flex-col items-center gap-1 nav-active">
            <span class="nav-item">⚡</span><span class="text-[7px] font-black uppercase">Nodes</span>
        </button>
        <button onclick="showPage('wallet')" id="n-wallet" class="flex flex-col items-center gap-1 opacity-40">
            <span class="nav-item">💰</span><span class="text-[7px] font-black uppercase">Fund</span>
        </button>
        <button onclick="showPage('logs')" id="n-logs" class="flex flex-col items-center gap-1 opacity-40">
            <span class="nav-item">📜</span><span class="text-[7px] font-black uppercase">Logs</span>
        </button>
        <button onclick="showPage('about')" id="n-about" class="flex flex-col items-center gap-1 opacity-40">
            <span class="nav-item">🏢</span><span class="text-[7px] font-black uppercase">Legal</span>
        </button>
    </nav>

    <script>
        // --- MACHINE SYSTEM (15+5) ---
        const STDS = [];
        for(let i=1; i<=15; i++) {
            let cost = 200 + (i-1)*1300;
            STDS.push({ id: `S-NODE 0${i}`, cost, hourly: (cost*0.005).toFixed(2), daily: (cost*0.12).toFixed(0) });
        }
        const VIPS = [
            { id: "GOLD VIP-1", cost: 50000, hourly: 250, daily: 6000 },
            { id: "DIAMOND VIP-2", cost: 100000, hourly: 550, daily: 13200 },
            { id: "PLATINUM VIP-3", cost: 250000, hourly: 1500, daily: 36000 },
            { id: "MASTER VIP-4", cost: 500000, hourly: 3500, daily: 84000 },
            { id: "ULTRA VIP-5", cost: 1000000, hourly: 8000, daily: 192000 }
        ];

        function renderNodes(type) {
            const grid = document.getElementById('machine-grid');
            const data = type === 'std' ? STDS : VIPS;
            grid.innerHTML = '';
            data.forEach(n => {
                grid.innerHTML += `
                <div class="php-card p-5 border-l-4 ${type==='std'?'border-yellow-500':'border-green-500'}">
                    <div class="flex justify-between items-start mb-4">
                        <div><p class="text-[10px] font-black text-yellow-500 uppercase">${n.id}</p><p class="text-[7px] opacity-40 font-bold">TERM: 30 DAYS</p></div>
                        <span class="text-[8px] bg-green-500/10 text-green-500 px-2 py-1 rounded font-black italic">ACTIVE</span>
                    </div>
                    <div class="grid grid-cols-2 gap-3 mb-4 text-[9px] font-bold">
                        <div class="bg-white/5 p-2 rounded-xl"><p class="opacity-40 uppercase text-[6px]">Hourly</p>₨ ${n.hourly}</div>
                        <div class="bg-white/5 p-2 rounded-xl"><p class="opacity-40 uppercase text-[6px]">Daily</p>₨ ${n.daily}</div>
                    </div>
                    <button class="w-full p-3 btn-gold text-[9px] uppercase tracking-tighter">Rent Machine (₨ ${n.cost.toLocaleString()})</button>
                </div>`;
            });
            // Tab Toggle UI
            document.getElementById('b-std').classList.toggle('bg-yellow-500', type==='std');
            document.getElementById('b-std').classList.toggle('text-black', type==='std');
            document.getElementById('b-std').classList.toggle('opacity-40', type!=='std');
            document.getElementById('b-vip').classList.toggle('bg-yellow-500', type==='vip');
            document.getElementById('b-vip').classList.toggle('text-black', type==='vip');
            document.getElementById('b-vip').classList.toggle('opacity-40', type!=='vip');
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('nav button').forEach(b => b.classList.add('opacity-40'));
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('nav-active'));
            document.getElementById('n-'+p).classList.add('nav-active');
            document.getElementById('n-'+p).classList.remove('opacity-40');
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = () => { renderNodes('std'); }
    </script>
</body>
</html>
