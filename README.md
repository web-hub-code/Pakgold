<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Elite | Master System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-auth.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@400;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #070b14; color: white; margin: 0; }
        .glass { background: rgba(255,255,255,0.03); backdrop-blur: 15px; border: 1px solid rgba(255,255,255,0.08); border-radius: 40px; }
        .tab-content { display: none; }
        .active-tab { display: block; }
        .lvl-card { background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 30px; transition: 0.3s; cursor: pointer; }
        .lvl-card:hover:not(.locked) { border-color: #3b82f6; transform: translateY(-5px); }
        .locked { opacity: 0.2; filter: grayscale(1); cursor: not-allowed; }
    </style>
</head>
<body>

    <div id="loader" class="fixed inset-0 bg-[#070b14] z-[10000] flex items-center justify-center font-black italic tracking-widest text-blue-500">
        LOADING ACADEMY...
    </div>

    <div id="loginOverlay" class="fixed inset-0 bg-[#070b14] z-[9999] flex items-center justify-center p-6 hidden">
        <div class="glass p-12 max-w-md w-full text-center">
            <h2 class="text-3xl font-black mb-8 italic">GBTS ENTRANCE</h2>
            <input id="uName" type="text" placeholder="Your Name" class="w-full p-6 bg-white/5 rounded-3xl mb-6 text-center outline-none border-2 border-transparent focus:border-blue-500 text-white">
            <button onclick="handleLogin()" class="w-full bg-blue-600 py-6 rounded-3xl font-black text-xl shadow-lg">ENTER PORTAL</button>
        </div>
    </div>

    <nav class="p-6 border-b border-white/5 flex justify-between items-center glass m-4">
        <div class="font-black italic text-blue-500">GBTS ELITE</div>
        <div class="flex gap-6 text-[10px] font-black uppercase italic">
            <button onclick="showTab('dashTab')">Training</button>
            <button onclick="showTab('rankTab')">Ranks</button>
            <button onclick="logout()" class="text-red-500">Exit</button>
        </div>
    </nav>

    <main class="p-6 max-w-6xl mx-auto">
        <div id="dashTab" class="tab-content active-tab">
            <div class="glass p-12 mb-10 relative overflow-hidden" onclick="checkAdminTrigger()">
                <p class="text-[10px] font-black text-blue-400 mb-2 uppercase tracking-widest">Active Student</p>
                <h1 id="dispName" class="text-5xl md:text-7xl font-black italic mb-6">---</h1>
                <div class="inline-block bg-blue-600/20 px-6 py-2 rounded-xl border border-blue-600/30 text-xs font-black italic">
                    LEVEL: <span id="dispLvl">01</span>
                </div>
            </div>
            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-6 gap-6"></div>
        </div>

        <div id="rankTab" class="tab-content">
            <div class="glass p-10 max-w-md mx-auto">
                <h3 class="text-2xl font-black text-center mb-8 italic">TOP CANDIDATES</h3>
                <div id="leaderList" class="space-y-3"></div>
            </div>
        </div>

        <div id="adminTab" class="tab-content">
            <div class="glass p-10">
                <h3 class="text-2xl font-black text-red-500 mb-6 italic">ADMIN PANEL</h3>
                <div id="adminUserList" class="space-y-4"></div>
            </div>
        </div>
    </main>

    <script>
        // CONFIG
        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com/",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        // FORCE INITIALIZE
        if (!firebase.apps.length) { firebase.initializeApp(firebaseConfig); }
        const db = firebase.database();
        const auth = firebase.auth();
        let userData = null;
        let adminClicks = 0;

        window.onload = () => {
            const saved = localStorage.getItem('gbts_key_vfinal');
            document.getElementById('loader').style.display = 'none';
            if(saved) initPortal(saved);
            else document.getElementById('loginOverlay').style.display = 'flex';
        };

        async function handleLogin() {
            const name = document.getElementById('uName').value.trim();
            if(!name) return;
            try {
                const userKey = name.toLowerCase().replace(/\s/g, '_');
                await auth.signInAnonymously();
                const userRef = db.ref('gbts_final/' + userKey);
                const snap = await userRef.once('value');
                if(!snap.exists()) {
                    await userRef.set({ name: name, level: 1, key: userKey });
                }
                localStorage.setItem('gbts_key_vfinal', userKey);
                document.getElementById('loginOverlay').style.display = 'none';
                initPortal(userKey);
            } catch (e) { alert("Error: Rules or Domain check!"); }
        }

        function initPortal(key) {
            db.ref('gbts_final/' + key).on('value', snap => {
                userData = snap.val();
                if(userData) {
                    document.getElementById('dispName').innerText = userData.name;
                    document.getElementById('dispLvl').innerText = userData.level;
                    renderLevels();
                }
            });
            loadRanks();
            loadAdminPanel();
        }

        function renderLevels() {
            const grid = document.getElementById('levelGrid'); grid.innerHTML = '';
            for(let i=1; i<=12; i++) {
                let lock = i > userData.level;
                grid.innerHTML += `<div onclick="unlockLvl(${i}, ${lock})" class="lvl-card p-8 text-center ${lock ? 'locked' : ''}">
                    <div class="text-3xl mb-2">${lock ? '🔒' : '🎖️'}</div>
                    <p class="text-[10px] font-black opacity-50 uppercase">Lvl ${i}</p>
                </div>`;
            }
        }

        function unlockLvl(n, lock) {
            if(lock || n !== userData.level) return;
            if(confirm("Unlock next level?")) {
                confetti();
                db.ref('gbts_final/' + userData.key).update({ level: userData.level + 1 });
                db.ref('gbts_ranks/' + userData.key).set({ name: userData.name, level: userData.level + 1 });
            }
        }

        function loadRanks() {
            db.ref('gbts_ranks').orderByChild('level').limitToLast(10).on('value', snap => {
                const list = document.getElementById('leaderList'); list.innerHTML = '';
                let arr = []; snap.forEach(c => arr.push(c.val()));
                arr.reverse().forEach((r, i) => {
                    list.innerHTML += `<div class="flex justify-between p-4 bg-white/5 rounded-2xl text-xs font-black">
                        <span>#${i+1} ${r.name}</span>
                        <span class="text-blue-500">LVL ${r.level}</span>
                    </div>`;
                });
            });
        }

        function checkAdminTrigger() {
            adminClicks++;
            if(adminClicks >= 5) {
                if(prompt("Pass?") === "prime786") showTab('adminTab');
                adminClicks = 0;
            }
        }

        function loadAdminPanel() {
            db.ref('gbts_final').on('value', snap => {
                const list = document.getElementById('adminUserList'); list.innerHTML = '';
                snap.forEach(c => {
                    const u = c.val();
                    list.innerHTML += `<div class="flex justify-between p-4 glass">
                        <span>${u.name} (Lvl ${u.level})</span>
                        <button onclick="db.ref('gbts_final/${u.key}').remove()" class="text-red-500">DEL</button>
                    </div>`;
                });
            });
        }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }
        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
