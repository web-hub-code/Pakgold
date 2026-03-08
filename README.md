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
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: #06090f; color: #f0f6fc; padding-bottom: 90px; overflow-x: hidden; }
        .sky-card { background: #0d1117; border: 1px solid #30363d; border-radius: 24px; transition: 0.3s; }
        .page { display: none; animation: slideUp 0.4s ease forwards; }
        .active-page { display: block !important; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .btn-gold { background: linear-gradient(135deg, #fbbf24, #d97706); color: #000; font-weight: 800; border-radius: 16px; transition: 0.2s; }
        .nav-item { font-size: 20px; filter: grayscale(1); transition: 0.2s; color: #94a3b8; }
        .nav-active { filter: grayscale(0); color: #fbbf24 !important; border-top: 2px solid #fbbf24; padding-top: 5px; }
        input { background: #161b22; border: 1px solid #30363d; border-radius: 14px; padding: 14px; color: white; width: 100%; outline: none; font-size: 14px; }
    </style>
</head>
<body>

    <section id="auth-ui" class="fixed inset-0 z-[10000] bg-[#06090f] flex flex-col items-center justify-center p-8 active-page">
        <h1 class="text-4xl font-black italic text-yellow-500 mb-2 uppercase">PakGold</h1>
        <p class="text-[10px] font-bold opacity-40 tracking-[5px] uppercase mb-10">Cloud Infrastructure</p>
        
        <div class="w-full max-w-[320px] space-y-4">
            <div id="auth-title" class="text-xs font-black uppercase text-center mb-4">Login to your Node</div>
            <input type="text" id="user-id" placeholder="Username / Phone">
            <input type="password" id="user-pw" placeholder="Password">
            <button onclick="handleAuth()" id="auth-btn" class="w-full p-4 btn-gold uppercase text-[10px] tracking-widest shadow-xl">Authorize Access 🚀</button>
            <p onclick="toggleAuth()" id="auth-toggle" class="text-[10px] text-center opacity-50 font-bold cursor-pointer">New user? <span class="text-yellow-500">Create Account</span></p>
        </div>
        <p class="absolute bottom-10 text-[8px] font-bold opacity-20 uppercase tracking-widest">Powered by Prime Solutions</p>
    </section>

    <main id="app-ui" class="hidden">
        
        <header class="p-6 sticky top-0 bg-[#06090f]/90 backdrop-blur-xl z-[5000] border-b border-white/5">
            <div class="flex justify-between items-center mb-6">
                <span class="text-lg font-black italic text-yellow-500">PAK<span class="text-white font-bold">GOLD</span></span>
                <div class="flex items-center gap-3">
                    <div class="bg-yellow-500/10 text-yellow-500 text-[10px] font-black px-3 py-1 rounded-full border border-yellow-500/20">VIP 1</div>
                    <button onclick="logout()" class="text-xl">🚪</button>
                </div>
            </div>
            <div class="sky-card p-6 bg-gradient-to-br from-[#1c2128] to-[#0d1117]">
                <p class="text-[9px] font-bold opacity-50 uppercase tracking-widest">Total Yield Balance</p>
                <h2 class="text-4xl font-black mt-1" id="v-bal">₨ 0.00</h2>
                <div class="grid grid-cols-2 gap-3 mt-6">
                    <button onclick="showPage('wallet')" class="p-3 bg-yellow-500 text-black rounded-xl text-[10px] font-black uppercase">Deposit</button>
                    <button onclick="showPage('withdraw')" class="p-3 bg-white/5 rounded-xl text-[10px] font-black uppercase border border-white/10">Withdraw</button>
                </div>
            </div>
        </header>

        <div class="p-4">
            <div id="p-home" class="page active-page space-y-4">
                <div class="flex gap-2 p-1 bg-white/5 rounded-2xl mb-4">
                    <button onclick="renderNodes('std')" id="b-std" class="flex-1 py-3 text-[10px] font-black uppercase rounded-xl bg-yellow-500 text-black">Machines</button>
                    <button onclick="renderNodes('vip')" id="b-vip" class="flex-1 py-3 text-[10px] font-black uppercase rounded-xl opacity-40">VIP Nodes</button>
                </div>
                <div id="node-grid" class="space-y-4"></div>
            </div>

            <div id="p-team" class="page space-y-4 text-center">
                <div class="sky-card p-8">
                    <h3 class="text-yellow-500 font-black text-sm uppercase">Team Rewards</h3>
                    <p class="text-[10px] opacity-50 mt-2 leading-relaxed">Level 1: 10% | Level 2: 5% | Level 3: 2%</p>
                    <div class="mt-8 p-4 bg-white/5 rounded-2xl border-2 border-dashed border-white/10 font-mono text-lg tracking-widest text-yellow-500">PG-786-GOLD</div>
                    <button class="w-full mt-4 p-4 btn-gold text-[10px] uppercase">Copy Referral Link</button>
                </div>
            </div>

            <div id="p-wallet" class="page space-y-4">
                <div class="sky-card p-6">
                    <h4 class="text-yellow-500 font-black text-xs uppercase mb-6 tracking-widest">Deposit Center</h4>
                    <div class="space-y-3 mb-8">
                        <div class="flex justify-between p-4 bg-white/5 rounded-2xl border-l-4 border-yellow-500 text-[11px] font-bold"><span>EasyPaisa</span><span>03379827882</span></div>
                        <div class="flex justify-between p-4 bg-white/5 rounded-2xl border-l-4 border-yellow-600 text-[11px] font-bold"><span>JazzCash</span><span>03705519562</span></div>
                    </div>
                    <input type="number" id="dep-amt" placeholder="Amount (Minimum ₨ 200)" class="mb-3">
                    <input type="text" id="dep-tid" placeholder="Transaction ID (TID)" class="mb-4">
                    <button onclick="alert('Proof Submitted Sweetie! 😘')" class="w-full p-4 btn-gold uppercase text-[10px] tracking-widest">Submit Recharge</button>
                </div>
            </div>
        </div>

    </main>

    <nav id="bottom-nav" class="hidden fixed bottom-0 w-full p-4 flex justify-around items-center bg-[#0d1117]/95 backdrop-blur-xl border-t border-white/5 rounded-t-[2.5rem] z-[9000]">
        <button onclick="showPage('home')" id="n-home" class="flex flex-col items-center gap-1 nav-active pt-2"><span class="nav-item">⚡</span><span class="text-[7px] font-black uppercase">Nodes</span></button>
        <button onclick="showPage('team')" id="n-team" class="flex flex-col items-center gap-1 opacity-40 pt-2"><span class="nav-item">👥</span><span class="text-[7px] font-black uppercase">Team</span></button>
        <button onclick="showPage('wallet')" id="n-wallet" class="flex flex-col items-center gap-1 opacity-40 pt-2"><span class="nav-item">💰</span><span class="text-[7px] font-black uppercase">Fund</span></button>
        <button onclick="showPage('about')" id="n-about" class="flex flex-col items-center gap-1 opacity-40 pt-2"><span class="nav-item">🏢</span><span class="text-[7px] font-black uppercase">Policy</span></button>
    </nav>

    <script>
        // 🛠️ FIREBASE CONFIG (CloudSky Style)
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();

        let isLogin = true;

        // 🛠️ AUTH LOGIC (CloudSky)
        function toggleAuth() {
            isLogin = !isLogin;
            document.getElementById('auth-title').innerText = isLogin ? "Login to your Node" : "Create New Account";
            document.getElementById('auth-btn').innerText = isLogin ? "Authorize Access 🚀" : "Register Node 🛡️";
            document.getElementById('auth-toggle').innerHTML = isLogin ? 'New user? <span class="text-yellow-500">Create Account</span>' : 'Already registered? <span class="text-yellow-500">Login</span>';
        }

        async function handleAuth() {
            const u = document.getElementById('user-id').value.trim().toLowerCase();
            const p = document.getElementById('user-pw').value.trim();
            if(!u || !p) return alert("Fill all details sweetie! 😘");

            const ref = db.collection("users").doc(u);
            const doc = await ref.get();

            if(isLogin) {
                if(doc.exists && doc.data().password === p) {
                    localStorage.setItem('pakgold_user', u);
                    location.reload();
                } else { alert("Invalid Credentials! 😘"); }
            } else {
                if(doc.exists) return alert("User already exists! 😘");
                await ref.set({ name: u, password: p, balance: 0, team: 0, date: Date.now() });
                localStorage.setItem('pakgold_user', u);
                location.reload();
            }
        }

        // 🛠️ PAGE & DATA LOGIC
        function showPage(id) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+id).classList.add('active-page');
            document.querySelectorAll('nav button').forEach(b => {
                b.classList.remove('nav-active');
                b.classList.add('opacity-40');
            });
            document.getElementById('n-'+id).classList.add('nav-active');
            document.getElementById('n-'+id).classList.remove('opacity-40');
        }

        const STDS = [];
        for(let i=1; i<=15; i++) {
            let cost = 200 + (i-1)*1300;
            STDS.push({ id: `NODE-S${i}`, cost, daily: (cost*0.12).toFixed(0) });
        }
        const VIPS = [{ id: "VIP-GOLD 1", cost: 50000, daily: 6500 }, { id: "VIP-DIAMOND 2", cost: 100000, daily: 14000 }];

        function renderNodes(type) {
            const grid = document.getElementById('node-grid');
            const data = type === 'std' ? STDS : VIPS;
            grid.innerHTML = '';
            data.forEach(n => {
                grid.innerHTML += `
                <div class="sky-card p-5 border-l-4 ${type==='std'?'border-yellow-500':'border-green-500'}">
                    <div class="flex justify-between items-center mb-4 text-[9px] font-black uppercase text-yellow-500 tracking-widest">${n.id} Node</div>
                    <div class="flex justify-between items-end">
                        <div><p class="text-[7px] opacity-40 uppercase font-bold">Daily Profit</p><p class="text-sm font-black text-green-500">₨ ${n.daily}</p></div>
                        <button class="px-5 py-2 btn-gold text-[8px] uppercase">Rent ₨ ${n.cost.toLocaleString()}</button>
                    </div>
                </div>`;
            });
            document.getElementById('b-std').classList.toggle('bg-yellow-500', type==='std');
            document.getElementById('b-std').classList.toggle('text-black', type==='std');
            document.getElementById('b-std').classList.toggle('opacity-40', type!=='std');
            document.getElementById('b-vip').classList.toggle('bg-yellow-500', type==='vip');
            document.getElementById('b-vip').classList.toggle('text-black', type==='vip');
            document.getElementById('b-vip').classList.toggle('opacity-40', type!=='vip');
        }

        window.onload = () => {
            const user = localStorage.getItem('pakgold_user');
            if(user) {
                document.getElementById('auth-ui').classList.remove('active-page');
                document.getElementById('app-ui').classList.remove('hidden');
                document.getElementById('bottom-nav').classList.remove('hidden');
                db.collection("users").doc(user).onSnapshot(d => {
                    document.getElementById('v-bal').innerText = "₨ " + (d.data().balance || 0).toLocaleString();
                });
                renderNodes('std');
            }
        };

        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
