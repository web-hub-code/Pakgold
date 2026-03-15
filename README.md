<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Pro | Official Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; overflow-x: hidden; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: slideUp 0.5s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .welcome-banner { background: linear-gradient(135deg, #0f172a 0%, #1e3a8a 100%); position: relative; overflow: hidden; }
        .glass-login { background: rgba(255, 255, 255, 0.95); backdrop-blur: 10px; border-radius: 50px; border: 1px solid #e2e8f0; }
        .level-card { transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); cursor: pointer; border-radius: 40px; background: white; border: 1px solid #e2e8f0; }
        .level-card:hover:not(.locked) { transform: scale(1.07); border-color: #1e3a8a; box-shadow: 0 25px 50px -12px rgba(0,0,0,0.1); }
        .locked { opacity: 0.4; filter: grayscale(1); cursor: not-allowed; background: #f1f5f9; }
        #loader { position: fixed; inset: 0; background: #0f172a; z-index: 10000; display: flex; flex-direction: column; items: center; justify-content: center; }
    </style>
</head>
<body>

    <div id="loader">
        <div class="w-16 h-16 border-4 border-blue-900 border-t-blue-400 rounded-full animate-spin mb-6"></div>
        <p class="text-white font-black tracking-[0.3em] uppercase italic">GBTS Academy</p>
    </div>

    <div id="loginOverlay" class="fixed inset-0 bg-slate-900 z-[9998] flex items-center justify-center p-6 hidden">
        <div class="glass-login p-10 md:p-14 max-w-md w-full text-center">
            <div class="w-20 h-20 bg-blue-900 text-white rounded-3xl flex items-center justify-center mx-auto mb-6 text-3xl font-black italic shadow-xl">GB</div>
            <h2 class="text-3xl font-black text-slate-900 mb-2 italic uppercase">Portal Access</h2>
            <p class="text-[10px] font-black text-blue-500 mb-10 tracking-[0.4em] uppercase italic">Username Authentication</p>

            <div class="space-y-4 mb-8">
                <input id="uName" type="text" placeholder="Username" class="w-full p-5 bg-slate-100 rounded-3xl font-bold text-center outline-none border-2 border-transparent focus:border-blue-900 transition-all">
                <input id="uPass" type="password" placeholder="Password" class="w-full p-5 bg-slate-100 rounded-3xl font-bold text-center outline-none border-2 border-transparent focus:border-blue-900 transition-all tracking-widest">
            </div>

            <button onclick="handleLogin()" class="w-full bg-blue-900 text-white py-5 rounded-[30px] font-black text-lg shadow-xl hover:bg-blue-800 transition-all active:scale-95">LOG IN NOW</button>
            <p class="mt-6 text-[9px] text-slate-400 font-bold uppercase italic">Secured by Prime Solutions</p>
        </div>
    </div>

    <nav class="bg-white/90 backdrop-blur-xl sticky top-0 z-50 p-5 px-10 flex justify-between items-center border-b">
        <div class="flex items-center gap-3">
            <div class="w-10 h-10 bg-blue-900 text-white flex items-center justify-center rounded-xl font-black italic shadow-md">G</div>
            <h1 class="font-black text-sm text-slate-900 uppercase italic">GBTS Portal</h1>
        </div>
        <div class="flex gap-6 items-center text-[10px] font-black uppercase italic">
            <button onclick="showTab('dashTab')" class="text-blue-900">Dashboard</button>
            <button onclick="showTab('rankTab')" class="text-slate-400">Merit List</button>
            <button onclick="logout()" class="text-red-500 bg-red-50 px-4 py-2 rounded-full">Exit</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-6 mt-6">
        <div id="dashTab" class="tab-content active-tab">
            <div class="welcome-banner p-12 md:p-16 rounded-[60px] text-white shadow-2xl mb-12">
                <div class="relative z-10">
                    <div class="flex items-center gap-4 mb-6">
                        <span class="bg-blue-500/20 text-blue-300 px-4 py-1 rounded-full text-[10px] font-black uppercase tracking-widest border border-blue-500/30">Official Candidate</span>
                        <span id="liveTag" class="flex items-center gap-2 text-[10px] font-bold text-green-400">
                            <span class="w-2 h-2 bg-green-500 rounded-full animate-ping"></span> LIVE
                        </span>
                    </div>
                    <h2 id="dispName" class="text-5xl md:text-6xl font-black italic uppercase tracking-tighter mb-6 leading-none">---</h2>
                    <div class="flex flex-wrap gap-4">
                        <div class="bg-white/10 backdrop-blur-md px-6 py-3 rounded-2xl border border-white/10">
                            <p class="text-[9px] uppercase font-bold opacity-60">Academic Rank</p>
                            <p class="text-xl font-black italic">Level <span id="dispLvl">01</span></p>
                        </div>
                        <div class="bg-white/10 backdrop-blur-md px-6 py-3 rounded-2xl border border-white/10">
                            <p class="text-[9px] uppercase font-bold opacity-60">Status</p>
                            <p class="text-xl font-black italic">ACTIVE</p>
                        </div>
                    </div>
                </div>
                <div class="absolute -right-20 -top-20 w-96 h-96 bg-blue-600/20 rounded-full blur-[100px]"></div>
                <div class="absolute -left-20 -bottom-20 w-80 h-80 bg-indigo-600/10 rounded-full blur-[80px]"></div>
            </div>
            
            <h3 class="text-xl font-black uppercase italic mb-8 px-4 text-slate-800">Training Roadmap</h3>
            <div id="levelGrid" class="grid grid-cols-2 sm:grid-cols-4 lg:grid-cols-6 gap-8 px-2"></div>
        </div>

        <div id="rankTab" class="tab-content">
            <div class="max-w-md mx-auto bg-white p-12 rounded-[60px] shadow-sm border border-slate-100">
                <h3 class="text-2xl font-black text-center text-blue-950 mb-10 italic uppercase tracking-tight">Real-Time Merit List</h3>
                <div id="leaderList" class="space-y-4"></div>
            </div>
        </div>
    </main>

    <div id="quizModal" class="fixed inset-0 bg-slate-950/95 z-[10000] hidden items-center justify-center p-6 backdrop-blur-xl">
        <div class="bg-white p-12 rounded-[60px] max-w-lg w-full text-center shadow-2xl">
            <h3 id="mTitle" class="text-3xl font-black text-blue-950 mb-6 italic uppercase">Security Check</h3>
            <div id="mStudy" class="bg-blue-50 p-8 rounded-[40px] mb-8 text-sm italic text-blue-900 border border-blue-100 leading-relaxed"></div>
            <input id="ansInp" type="text" placeholder="Security Code" class="w-full p-6 bg-slate-50 rounded-3xl mb-6 text-center font-black border-2 border-transparent focus:border-blue-900 outline-none transition-all">
            <button id="subBtn" class="w-full bg-blue-950 text-white py-5 rounded-[30px] font-black shadow-xl hover:bg-blue-800 transition-all">VERIFY & PROCEED</button>
            <button onclick="closeModal()" class="mt-6 text-[10px] font-black text-slate-300 uppercase tracking-widest">Cancel Assessment</button>
        </div>
    </div>

    <script>
        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com/",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();
        const db = firebase.database();

        let userData = null;

        // --- AUTH LOGIC ---
        window.onload = () => {
            setTimeout(() => {
                document.getElementById('loader').style.display = 'none';
                const savedU = localStorage.getItem('gbts_user');
                const savedP = localStorage.getItem('gbts_pass');
                if(savedU && savedP) processLogin(savedU, savedP);
                else document.getElementById('loginOverlay').style.display = 'flex';
            }, 2000);
        };

        async function handleLogin() {
            const u = document.getElementById('uName').value.trim();
            const p = document.getElementById('uPass').value.trim();
            if(!u || !p) return alert("Sweetie, Username aur Password likhein!");
            
            localStorage.setItem('gbts_user', u);
            localStorage.setItem('gbts_pass', p);
            await processLogin(u, p);
        }

        async function processLogin(name, pass) {
            const userKey = btoa(name.toLowerCase() + pass).substring(0, 20);
            try {
                const res = await auth.signInAnonymously();
                const userRef = db.ref('gbts_final_v1/' + userKey);
                const snap = await userRef.get();

                if(!snap.exists()) {
                    await userRef.set({ uid: res.user.uid, name, level: 1, pass, key: userKey });
                }
                
                document.getElementById('loginOverlay').style.display = 'none';
                loadUserPortal(userKey);
            } catch (e) {
                alert("Login Failed! Firebase Rules check karein sweetie.");
            }
        }

        function loadUserPortal(key) {
            db.ref('gbts_final_v1/' + key).on('value', snap => {
                userData = snap.val();
                if(userData) {
                    document.getElementById('dispName').innerText = userData.name;
                    document.getElementById('dispLvl').innerText = userData.level < 10 ? "0"+userData.level : userData.level;
                    renderMap();
                }
            });
            syncLeaderboard();
        }

        function renderMap() {
            const grid = document.getElementById('levelGrid'); grid.innerHTML = '';
            for(let i=1; i<=12; i++) {
                let lock = i > userData.level;
                grid.innerHTML += `
                    <div onclick="openTest(${i}, ${lock})" class="level-card p-12 text-center ${lock ? 'locked' : 'shadow-sm animate__animated animate__fadeInUp'}">
                        <div class="text-4xl mb-4">${lock ? '🔒' : '🎖️'}</div>
                        <p class="text-[10px] font-black uppercase text-slate-400 tracking-tighter">Module ${i}</p>
                    </div>`;
            }
        }

        function openTest(num, lock) {
            if(lock || num !== userData.level) return;
            document.getElementById('quizModal').style.display = 'flex';
            document.getElementById('mTitle').innerText = "Module " + num;
            document.getElementById('mStudy').innerText = "Verify your status to unlock Level " + (num + 1) + ". Type 'prime786' to complete.";
            
            document.getElementById('subBtn').onclick = () => {
                if(document.getElementById('ansInp').value.trim().toLowerCase() === 'prime786') {
                    confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 } });
                    db.ref('gbts_final_v1/' + userData.key).update({ level: userData.level + 1 });
                    db.ref('merit_v1/' + userData.key).set({ name: userData.name, level: userData.level + 1 });
                    closeModal();
                } else { alert("Incorrect Security Code!"); }
            };
        }

        function syncLeaderboard() {
            db.ref('merit_v1').orderByChild('level').limitToLast(10).on('value', snap => {
                const list = document.getElementById('leaderList'); list.innerHTML = '';
                let ranks = []; snap.forEach(c => ranks.push(c.val()));
                ranks.reverse().forEach((r, i) => {
                    list.innerHTML += `<div class="flex justify-between items-center p-5 bg-slate-50 rounded-3xl border border-slate-100">
                        <span class="font-black text-xs uppercase italic">#${i+1} ${r.name}</span>
                        <span class="bg-blue-900 text-white px-4 py-1 rounded-full text-[10px] font-black italic">LVL ${r.level}</span>
                    </div>`;
                });
            });
        }

        function closeModal() { document.getElementById('quizModal').style.display = 'none'; document.getElementById('ansInp').value = ''; }
        function showTab(id) { document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab')); document.getElementById(id).classList.add('active-tab'); }
        function logout() { localStorage.clear(); auth.signOut().then(() => location.reload()); }
    </script>
</body>
</html>
