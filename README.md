<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | Cloud Infrastructure</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap');
        
        :root { 
            --cloud-dark: #0a0e17; 
            --cloud-blue: #1e3a8a; 
            --cloud-accent: #3b82f6; 
            --gold: #fbbf24;
        }

        body { 
            font-family: 'Inter', sans-serif; 
            background-color: var(--cloud-dark); 
            background-image: radial-gradient(circle at top right, #1e293b, #0a0e17);
            color: #e2e8f0; 
            margin: 0; padding-bottom: 100px;
        }

        /* CloudSky Card Style */
        .cloud-card {
            background: rgba(30, 41, 59, 0.5);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 20px;
            backdrop-filter: blur(10px);
        }

        .btn-cloud {
            background: linear-gradient(90deg, #3b82f6 0%, #1d4ed8 100%);
            color: white; font-weight: 700; border-radius: 12px;
            transition: 0.3s;
        }

        .btn-gold {
            background: linear-gradient(90deg, #fbbf24 0%, #d97706 100%);
            color: #000; font-weight: 800; border-radius: 12px;
        }

        .nav-item { opacity: 0.5; transition: 0.3s; font-size: 10px; font-weight: 700; }
        .nav-active { opacity: 1; color: var(--cloud-accent); border-top: 3px solid var(--cloud-accent); padding-top: 10px; }

        .page { display: none; animation: fadeIn 0.4s ease-in-out; }
        .active-page { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        input { background: #0f172a; border: 1px solid #334155; border-radius: 12px; color: white; padding: 12px; }
    </style>
</head>
<body>

    <header class="p-5 flex justify-between items-center sticky top-0 bg-slate-900/80 backdrop-blur-md z-[1000]">
        <div class="flex items-center gap-2">
            <div class="w-8 h-8 bg-blue-600 rounded-lg flex items-center justify-center font-bold">P</div>
            <h1 class="text-lg font-extrabold tracking-tight uppercase">Pak<span class="text-blue-500">Gold</span></h1>
        </div>
        <div class="text-right">
            <p id="bal" class="text-xl font-bold text-blue-400">₨ 0.00</p>
            <p class="text-[8px] uppercase font-bold opacity-50 tracking-widest">Live Balance</p>
        </div>
    </header>

    <main class="p-4 space-y-6">

        <div id="p-home" class="page active-page space-y-5">
            <div class="cloud-card p-6 flex justify-between items-center relative overflow-hidden">
                <div class="z-10">
                    <p class="text-[10px] font-bold text-blue-400 uppercase mb-1">Server Status: Active</p>
                    <h2 class="text-2xl font-black italic" id="u-display">Loading...</h2>
                </div>
                <div class="w-16 h-16 bg-blue-500/10 rounded-full flex items-center justify-center text-3xl animate-pulse">☁️</div>
            </div>

            <div class="grid grid-cols-2 gap-3">
                <div class="cloud-card p-4 text-center">
                    <p class="text-[9px] opacity-50 uppercase font-bold mb-1">Total Mining</p>
                    <p class="text-sm font-bold">₨ 0.00</p>
                </div>
                <div class="cloud-card p-4 text-center">
                    <p class="text-[9px] opacity-50 uppercase font-bold mb-1">Daily Profit</p>
                    <p class="text-sm font-bold text-green-400">₨ 0.00</p>
                </div>
            </div>

            <button onclick="showPage('recharge')" class="w-full p-4 btn-cloud uppercase text-xs tracking-widest shadow-lg shadow-blue-500/20">Add Funds</button>
        </div>

        <div id="p-nodes" class="page space-y-4">
            <div class="flex gap-2 mb-4">
                <button onclick="renderPlans('normal')" class="flex-1 p-3 bg-blue-600 rounded-xl text-[10px] font-bold uppercase">Cloud Nodes</button>
                <button onclick="renderPlans('vip')" class="flex-1 p-3 cloud-card rounded-xl text-[10px] font-bold uppercase">VIP Servers</button>
            </div>
            <div id="nodes-container" class="space-y-4"></div>
        </div>

        <div id="p-recharge" class="page space-y-5">
            <div class="cloud-card p-6">
                <h3 class="text-sm font-bold mb-4 uppercase text-blue-400">Recharge Gateway</h3>
                <div class="p-4 bg-blue-900/20 rounded-xl border border-blue-500/20 mb-5">
                    <p class="text-[10px] opacity-60">EasyPaisa Account:</p>
                    <p class="text-lg font-bold">03379827882</p>
                </div>
                <input type="number" id="d-amt" placeholder="Amount" class="w-full mb-3 outline-none">
                <input type="text" id="d-tid" placeholder="Transaction ID (TID)" class="w-full mb-3 outline-none">
                <p class="text-[9px] font-bold mb-1 ml-1 opacity-50">Upload Payment Proof:</p>
                <input type="file" id="d-file" class="w-full p-2 text-xs mb-5">
                <button onclick="handleDeposit()" class="w-full p-4 btn-cloud uppercase text-xs">Submit Request</button>
            </div>
        </div>

        <div id="p-menu" class="page space-y-3">
            <button onclick="alert('Coming Soon')" class="w-full cloud-card p-5 text-left text-xs font-bold uppercase flex justify-between">Account History <span>→</span></button>
            <button onclick="alert('Coming Soon')" class="w-full cloud-card p-5 text-left text-xs font-bold uppercase flex justify-between">About Company <span>→</span></button>
            <button onclick="window.location.href='https://wa.me/923705519562'" class="w-full cloud-card p-5 text-left text-xs font-bold uppercase text-blue-400 flex justify-between">Support Center <span>💬</span></button>
            <button onclick="logout()" class="w-full p-4 bg-red-500/10 text-red-500 font-bold rounded-xl text-xs uppercase mt-10">Logout</button>
        </div>

    </main>

    <nav class="fixed bottom-0 w-full h-20 bg-[#0a0e17]/95 border-t border-white/5 backdrop-blur-lg flex justify-around items-center px-4 z-[5000]">
        <button onclick="showPage('home')" id="n-home" class="nav-item nav-active flex flex-col items-center">
            <span class="text-xl">🏠</span><span>HOME</span>
        </button>
        <button onclick="showPage('nodes')" id="n-nodes" class="nav-item flex flex-col items-center">
            <span class="text-xl">☁️</span><span>MINING</span>
        </button>
        <button onclick="showPage('menu')" id="n-menu" class="nav-item flex flex-col items-center">
            <span class="text-xl">👤</span><span>MENU</span>
        </button>
    </nav>

    <script>
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        let user = localStorage.getItem('pg_user');

        function renderPlans(type) {
            const container = document.getElementById('nodes-container'); container.innerHTML = '';
            for(let i=1; i<=(type==='normal'?20:5); i++) {
                let cost = type==='normal' ? 200 + (i*1200) : 50000 + (i*25000);
                container.innerHTML += `
                <div class="cloud-card p-5 flex justify-between items-center">
                    <div>
                        <p class="text-[8px] font-bold text-blue-400 uppercase">Cloud Node v${i}</p>
                        <h4 class="text-lg font-bold">₨ ${cost.toLocaleString()}</h4>
                    </div>
                    <button class="px-5 py-2 btn-cloud text-[9px] uppercase">Activate</button>
                </div>`;
            }
        }

        async function handleDeposit() {
            const a = document.getElementById('d-amt').value; const t = document.getElementById('d-tid').value; const f = document.getElementById('d-file').files[0];
            if(!a || !t || !f) return alert("All fields & Proof are required, sweetie! 😘");
            const r = new FileReader(); r.readAsDataURL(f);
            r.onload = async () => {
                await db.collection("requests").add({ user, amount: parseFloat(a), tid: t, proof: r.result, type: 'deposit', status: 'pending', time: Date.now() });
                alert("Deposit submitted! Admin will verify soon. 😘"); showPage('home');
            };
        }

        function checkUser() {
            if(user) {
                document.getElementById('u-display').innerText = user.toUpperCase();
                db.collection("users").doc(user).onSnapshot(d => {
                    document.getElementById('bal').innerText = "₨ " + (d.data()?.balance || 0).toLocaleString();
                });
                renderPlans('normal');
            } else {
                let n = prompt("Enter Username:"); if(n) { localStorage.setItem('pg_user', n.toLowerCase()); location.reload(); }
            }
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('.nav-item').forEach(b => b.classList.remove('nav-active'));
            document.getElementById('n-'+p)?.classList.add('nav-active');
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = checkUser;
    </script>
</body>
</html>
