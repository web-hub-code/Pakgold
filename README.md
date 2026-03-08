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
        :root { --bg: #020617; --card: #0f172a; --blue: #3b82f6; --gold: #fbbf24; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: #f8fafc; margin: 0; padding-bottom: 100px; overflow-x: hidden; }
        
        /* Modern Glassmorphism */
        .glass { background: rgba(30, 41, 59, 0.7); backdrop-filter: blur(16px); border: 1px solid rgba(255, 255, 255, 0.08); border-radius: 28px; }
        .page { display: none; animation: slideUp 0.5s cubic-bezier(0.4, 0, 0.2, 1) forwards; }
        .active-page { display: block !important; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }

        /* Buttons & Inputs */
        .btn-premium { background: linear-gradient(135deg, #3b82f6, #1d4ed8); color: white; font-weight: 800; border-radius: 18px; box-shadow: 0 10px 20px rgba(29, 78, 216, 0.3); transition: 0.3s; }
        .btn-premium:active { transform: scale(0.95); }
        input { background: #1e293b; border: 1px solid #334155; border-radius: 18px; padding: 18px; color: white; width: 100%; outline: none; transition: 0.3s; }
        input:focus { border-color: var(--blue); box-shadow: 0 0 0 4px rgba(59, 130, 246, 0.1); }

        /* Navigation */
        .nav-item { opacity: 0.4; transition: 0.4s; font-size: 9px; font-weight: 800; }
        .nav-active { opacity: 1; color: var(--blue); transform: translateY(-8px); }
        marquee { background: rgba(59, 130, 246, 0.05); color: var(--blue); font-size: 11px; font-weight: 800; padding: 12px 0; border-bottom: 1px solid rgba(255, 255, 255, 0.05); }
    </style>
</head>
<body>

    <div id="auth-screen" class="fixed inset-0 bg-[#020617] z-[99999] flex flex-col items-center justify-center p-8">
        <div class="mb-10 text-center animate-pulse">
            <h1 class="text-4xl font-black italic text-blue-500 tracking-tighter uppercase">PAK<span class="text-white">GOLD</span></h1>
            <p class="text-[10px] font-bold opacity-40 uppercase tracking-[4px] mt-2">Cloud Infrastructure</p>
        </div>
        <div class="glass w-full p-8 space-y-5">
            <h2 class="text-xl font-black text-center mb-4">Secure Login</h2>
            <input type="text" id="login-user" placeholder="Username">
            <input type="password" id="login-pass" placeholder="Password">
            <button onclick="handleAuth()" class="w-full p-5 btn-premium uppercase text-sm tracking-widest mt-2">Access Node</button>
            <p class="text-[10px] text-center opacity-40">Don't have an account? Contact Admin</p>
        </div>
    </div>

    <marquee>Verified Node Active • Daily Yield Distribution at 00:00 • Min Deposit ₨ 200 😘</marquee>

    <header class="p-6 flex justify-between items-center sticky top-0 bg-[#020617]/90 backdrop-blur-lg z-[5000]">
        <div onclick="tapAdmin()">
            <h1 class="text-xl font-black text-blue-500 italic uppercase tracking-tighter">PAK<span class="text-white">GOLD</span></h1>
            <p id="u-id" class="text-[8px] font-bold opacity-50 uppercase tracking-[2px]">Syncing...</p>
        </div>
        <div class="text-right">
            <p class="text-[8px] font-black opacity-30 uppercase">Vault Balance</p>
            <h2 id="u-bal" class="text-2xl font-black text-white tracking-tighter">₨ 0.00</h2>
        </div>
    </header>

    <main class="p-4 space-y-6">
        <div id="p-home" class="page active-page space-y-6">
            <div class="glass p-6 border-l-4 border-blue-500 flex justify-between items-center">
                <div>
                    <p class="text-[9px] font-black opacity-40 uppercase tracking-widest">Next Payout</p>
                    <h3 id="timer" class="text-2xl font-black text-blue-500 tracking-widest">23:59:59</h3>
                </div>
                <div class="w-14 h-14 rounded-full bg-blue-500/10 flex items-center justify-center text-2xl animate-spin-slow">💎</div>
            </div>

            <div class="grid grid-cols-2 gap-4">
                <button onclick="showPage('deposit')" class="glass p-6 text-center font-black text-[11px] uppercase border-b-2 border-green-500 bg-green-500/5">Deposit</button>
                <button onclick="showPage('withdraw')" class="glass p-6 text-center font-black text-[11px] uppercase border-b-2 border-red-500 bg-red-500/5">Withdraw</button>
            </div>
        </div>

        <div id="p-nodes" class="page space-y-4">
            <div class="flex gap-2 p-1.5 bg-slate-900 rounded-2xl mb-6">
                <button onclick="renderNodes('normal')" id="btn-n" class="flex-1 p-3.5 rounded-xl bg-blue-600 text-white text-[10px] font-black uppercase shadow-lg">Standard Nodes</button>
                <button onclick="renderNodes('vip')" id="btn-v" class="flex-1 p-3.5 rounded-xl text-[10px] font-black uppercase opacity-40">VIP Clusters</button>
            </div>
            <div id="nodes-grid" class="space-y-4"></div>
        </div>

        <div id="p-deposit" class="page space-y-4">
            <div class="glass p-8">
                <h3 class="text-xs font-black uppercase text-blue-500 mb-6">Deposit Infrastructure</h3>
                <div class="bg-blue-600/10 p-5 rounded-2xl border border-blue-500/20 mb-6">
                    <p class="text-[10px] font-bold opacity-50 mb-1">EasyPaisa / JazzCash</p>
                    <p class="text-xl font-black tracking-tight">03379827882</p>
                </div>
                <div class="space-y-4">
                    <input type="number" id="d-amt" placeholder="Amount (Min 200)">
                    <input type="text" id="d-tid" placeholder="Transaction ID (TID)">
                    <p class="text-[9px] font-bold opacity-30 ml-2">UPLOAD SCREENSHOT</p>
                    <input type="file" id="d-proof" class="!p-3 text-xs">
                    <button onclick="submitDeposit()" class="w-full p-5 btn-premium uppercase text-[11px] tracking-[2px] mt-4">Verify Funds</button>
                </div>
            </div>
        </div>

        <div id="p-history" class="page space-y-4">
            <div class="flex justify-between items-center px-2">
                <h3 class="text-xs font-black uppercase opacity-50 tracking-widest">Global Activity</h3>
                <button onclick="clearHistory()" class="text-[9px] font-bold text-red-500 uppercase">Clear Logs</button>
            </div>
            <div id="h-list" class="space-y-3"></div>
        </div>

        <div id="p-menu" class="page space-y-4 text-center">
            <div class="glass p-6">
                <p class="text-[9px] font-black opacity-40 uppercase mb-3">Referral Protocol</p>
                <p id="ref-link" class="text-[10px] text-blue-400 p-4 bg-slate-900 rounded-2xl border border-dashed border-slate-700 break-all">Generating link...</p>
                <button onclick="copyRef()" class="mt-4 text-[10px] font-black uppercase text-blue-500 tracking-widest">Copy Invitation</button>
            </div>
            <button onclick="window.location.href='https://wa.me/923705519562'" class="w-full glass p-5 text-left text-xs font-black uppercase text-green-500">Official Support 💬</button>
            <button onclick="logout()" class="w-full p-5 bg-red-500/10 text-red-500 rounded-2xl font-black uppercase text-xs mt-10 tracking-widest">Terminate Session</button>
        </div>
    </main>

    <nav class="fixed bottom-0 w-full h-24 glass rounded-t-[3rem] border-t border-white/5 flex justify-around items-center px-8 z-[10000]">
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
        let tapCount = 0;

        // --- 🤫 SECRET ADMIN TAP (4 TAPS) ---
        function tapAdmin() {
            tapCount++;
            if(tapCount >= 4) {
                let p = prompt("Admin Access Key:");
                if(p === "admin123") {
                    localStorage.setItem('is_admin', 'true');
                    alert("Admin Mode Unlocked, Sweetie! 😘");
                }
                tapCount = 0;
            }
            setTimeout(() => tapCount = 0, 2000);
        }

        // --- AUTH LOGIC ---
        function handleAuth() {
            const u = document.getElementById('login-user').value.toLowerCase().trim();
            const p = document.getElementById('login-pass').value;
            if(u.length < 3 || p.length < 3) return alert("Enter valid credentials! 😘");
            localStorage.setItem('pg_user', u);
            location.reload();
        }

        // --- NODES (20+5 Plans starting 200) ---
        function renderNodes(type) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = '';
            const isNormal = type === 'normal';
            document.getElementById('btn-n').className = isNormal ? 'flex-1 p-3.5 rounded-xl bg-blue-600 text-white text-[10px] font-black uppercase shadow-lg' : 'flex-1 p-3.5 rounded-xl text-[10px] font-black uppercase opacity-40';
            document.getElementById('btn-v').className = !isNormal ? 'flex-1 p-3.5 rounded-xl bg-blue-600 text-white text-[10px] font-black uppercase shadow-lg' : 'flex-1 p-3.5 rounded-xl text-[10px] font-black uppercase opacity-40';

            if(isNormal) {
                for(let i=1; i<=20; i++) {
                    let cost = i === 1 ? 200 : 200 + (i-1)*600;
                    grid.innerHTML += `<div class="glass p-6 flex justify-between items-center border-r-4 border-blue-500"><div class="flex items-center gap-4"><div class="w-12 h-12 rounded-2xl bg-blue-500/10 flex items-center justify-center">⛏️</div><div><p class="text-[8px] font-black text-blue-500 uppercase tracking-widest italic">Standard v${i}</p><h4 class="text-xl font-black">₨ ${cost.toLocaleString()}</h4></div></div><button class="px-7 py-3.5 btn-premium text-[10px] uppercase">Rent</button></div>`;
                }
            } else {
                const vips = [50000, 100000, 250000, 500000, 1000000];
                vips.forEach((v, idx) => {
                    grid.innerHTML += `<div class="glass p-6 flex justify-between items-center border-r-4 border-yellow-500"><div class="flex items-center gap-4"><div class="w-12 h-12 rounded-2xl bg-yellow-500/10 flex items-center justify-center">💎</div><div><p class="text-[8px] font-black text-yellow-500 uppercase tracking-widest italic">VIP Cluster 0${idx+1}</p><h4 class="text-xl font-black">₨ ${v.toLocaleString()}</h4></div></div><button class="px-7 py-3.5 btn-premium text-[10px] uppercase">Rent</button></div>`;
                });
            }
        }

        // --- DEPOSIT WITH PROOF ---
        async function submitDeposit() {
            const a = document.getElementById('d-amt').value; const t = document.getElementById('d-tid').value; const f = document.getElementById('d-proof').files[0];
            if(!a || !t || !f) return alert("Please provide TID and Screenshot proof! 😘");
            const r = new FileReader(); r.readAsDataURL(f);
            r.onload = async () => {
                await db.collection("requests").add({ user, amount: parseFloat(a), tid: t, proof: r.result, status: 'pending', type: 'deposit', time: Date.now() });
                alert("Deposit Logged! Verifying within 1 hour. 😘"); showPage('home');
            };
        }

        // --- SYSTEM SYNC ---
        function initApp() {
            if(user) {
                document.getElementById('auth-screen').style.display = 'none';
                document.getElementById('u-id').innerText = "@" + user.toUpperCase();
                document.getElementById('ref-link').innerText = window.location.origin + "/join?ref=" + user;
                db.collection("users").doc(user).onSnapshot(d => {
                    document.getElementById('u-bal').innerText = "₨ " + (d.data()?.balance || 0).toLocaleString();
                });
                renderNodes('normal'); startTimer();
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
            document.querySelectorAll('.nav-item').forEach(b => { b.classList.remove('nav-active'); b.classList.add('opacity-40'); });
            document.getElementById('n-'+p)?.classList.add('nav-active');
            document.getElementById('n-'+p)?.classList.remove('opacity-40');
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = initApp;
    </script>
</body>
</html>
