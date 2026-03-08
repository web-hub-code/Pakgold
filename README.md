<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <title>PakGold | Professional Cloud Mining</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@400;600;800&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #0b0e11; color: #eaecef; padding-bottom: 90px; }
        .cloud-card { background: #1e2329; border-radius: 16px; border: 1px solid #30363d; overflow: hidden; }
        .stat-box { background: linear-gradient(135deg, #f3ba2f, #c99400); color: #000; border-radius: 20px; }
        .nav-active { color: #f3ba2f !important; }
        .page { display: none; }
        .active { display: block; animation: fadeIn 0.3s ease; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .machine-img { width: 100%; height: 120px; background: #2b3139; display: flex; align-items: center; justify-content: center; font-size: 40px; }
    </style>
</head>
<body>

    <div id="p-home" class="page active p-4 space-y-4">
        <div class="stat-box p-6 shadow-2xl">
            <p class="text-[10px] font-bold opacity-70 uppercase tracking-widest">Total Balance</p>
            <h1 class="text-3xl font-black italic">PKR <span id="v-bal">0.00</span></h1>
            <div class="mt-4 flex justify-between text-[10px] font-bold border-t border-black/10 pt-3">
                <div><p class="opacity-70">DAILY INCOME</p><p>PKR 150.00</p></div>
                <div><p class="opacity-70">WITHDRAWN</p><p>PKR 0.00</p></div>
            </div>
        </div>

        <div class="bg-[#1e2329] p-2 rounded-lg flex items-center gap-2 border border-white/5">
            <span class="text-yellow-500">📢</span>
            <marquee class="text-[10px] font-bold text-gray-400">Welcome to PakGold by Prime Solutions! Minimum investment is only PKR 200. Secure your future now...</marquee>
        </div>

        <h3 class="text-sm font-bold ml-1">Mining Equipment</h3>
        <div id="machine-grid" class="grid grid-cols-1 gap-4">
            </div>
    </div>

    <div id="p-wallet" class="page p-4 space-y-4">
        <div class="cloud-card p-6 text-center">
            <h2 class="text-yellow-500 font-bold">RECHARGE WALLET</h2>
            <div class="my-4 space-y-2 text-xs text-left bg-black/20 p-4 rounded-xl">
                <p><b>EasyPaisa:</b> 03379827882</p>
                <p><b>JazzCash:</b> 03705519562</p>
            </div>
            <input type="number" placeholder="Enter Amount" class="w-full p-4 bg-[#2b3139] rounded-xl outline-none mb-3">
            <button class="w-full p-4 bg-yellow-500 text-black font-black rounded-xl text-xs uppercase">Submit Deposit</button>
        </div>
    </div>

    <div id="p-me" class="page p-4 space-y-4">
        <div class="cloud-card p-6 flex items-center gap-4">
            <div class="w-16 h-16 bg-yellow-500 rounded-full flex items-center justify-center text-2xl">👤</div>
            <div>
                <h2 id="u-name" class="font-bold text-lg">User_786</h2>
                <p class="text-[10px] text-gray-400 uppercase">VIP Level: 01</p>
            </div>
        </div>
        <div class="grid grid-cols-2 gap-3">
            <div class="cloud-card p-4 text-center"><p class="text-xl">📊</p><p class="text-[10px] font-bold">RECORDS</p></div>
            <div class="cloud-card p-4 text-center"><p class="text-xl">🛠️</p><p class="text-[10px] font-bold">SETTINGS</p></div>
            <div class="cloud-card p-4 text-center"><p class="text-xl">📄</p><p class="text-[10px] font-bold">LEGAL</p></div>
            <div onclick="location.reload()" class="cloud-card p-4 text-center border-red-500/30"><p class="text-xl">❌</p><p class="text-[10px] font-bold text-red-500">LOGOUT</p></div>
        </div>
    </div>

    <nav class="fixed bottom-0 w-full bg-[#1e2329] border-t border-white/5 p-4 flex justify-around items-center z-[5000]">
        <div onclick="showPage('home')" id="n-home" class="flex flex-col items-center gap-1 nav-active">
            <span class="text-xl">⚡</span><span class="text-[8px] font-black uppercase">Home</span>
        </div>
        <div onclick="showPage('wallet')" id="n-wallet" class="flex flex-col items-center gap-1 text-gray-500">
            <span class="text-xl">📥</span><span class="text-[8px] font-black uppercase">Fund</span>
        </div>
        <div onclick="showPage('me')" id="n-me" class="flex flex-col items-center gap-1 text-gray-500">
            <span class="text-xl">👤</span><span class="text-[8px] font-black uppercase">Me</span>
        </div>
    </nav>

    <script>
        // 🛠️ MACHINES DATA (200 Start, 15+5)
        const machines = [];
        for(let i=1; i<=15; i++) {
            let price = 200 + (i-1)*1200;
            machines.push({ name: `S-NODE 0${i}`, price: price, hourly: (price*0.005).toFixed(2), daily: (price*0.12).toFixed(0) });
        }
        // VIPs
        const vips = [
            { name: "GOLD VIP-1", price: 50000, hourly: 250, daily: 6000 },
            { name: "DIAMOND VIP-2", price: 100000, hourly: 550, daily: 13200 }
        ];

        function render() {
            const grid = document.getElementById('machine-grid');
            [...machines, ...vips].forEach(m => {
                grid.innerHTML += `
                <div class="cloud-card">
                    <div class="machine-img">${m.name.includes('VIP') ? '💎' : '⛏️'}</div>
                    <div class="p-4">
                        <div class="flex justify-between items-center mb-3">
                            <span class="text-[10px] font-black text-yellow-500 uppercase">${m.name}</span>
                            <span class="text-[10px] font-bold text-gray-400">Term: 30 Days</span>
                        </div>
                        <div class="grid grid-cols-2 gap-2 text-[10px] mb-4">
                            <div class="bg-black/20 p-2 rounded-lg"><p class="opacity-50">HOURLY</p><p class="text-green-500 font-bold">PKR ${m.hourly}</p></div>
                            <div class="bg-black/20 p-2 rounded-lg"><p class="opacity-50">DAILY</p><p class="text-green-500 font-bold">PKR ${m.daily}</p></div>
                        </div>
                        <button class="w-full p-3 bg-yellow-500 text-black font-black rounded-xl text-[10px] uppercase">Buy Machine (PKR ${m.price})</button>
                    </div>
                </div>`;
            });
        }

        function showPage(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById('p-'+id).classList.add('active');
            document.querySelectorAll('nav div').forEach(d => d.classList.remove('nav-active'));
            document.getElementById('n-'+id).classList.add('nav-active');
        }

        window.onload = render;
    </script>
</body>
</html>
