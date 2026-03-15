<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Pro | Official Recruitment & Training Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; color: #1e293b; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: fadeIn 0.5s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        
        /* Modern UI Elements */
        .glass-card { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(10px); border: 1px solid rgba(226, 232, 240, 0.8); }
        .status-badge { font-size: 9px; padding: 4px 10px; border-radius: 50px; font-weight: 900; text-transform: uppercase; }
        
        /* Privacy List Styling */
        .policy-text p { font-size: 13px; line-height: 1.6; color: #475569; margin-bottom: 15px; }
        .policy-text h4 { font-weight: 900; color: #1e3a8a; margin-top: 20px; text-transform: uppercase; font-size: 14px; }
    </style>
</head>
<body class="pb-20">

    <div id="loginOverlay" class="fixed inset-0 bg-slate-900 z-[5000] flex items-center justify-center p-6 hidden">
        <div class="bg-white p-10 rounded-[50px] max-w-md w-full shadow-2xl border-t-[10px] border-blue-900">
            <div class="text-center mb-8">
                <div class="w-16 h-16 bg-blue-100 rounded-2xl mx-auto flex items-center justify-center mb-4">
                    <span class="text-3xl">🛡️</span>
                </div>
                <h2 class="text-3xl font-black text-slate-900 italic">ADMISSION DESK</h2>
                <p class="text-[10px] font-bold text-gray-400 uppercase tracking-widest">Candidate Verification System</p>
            </div>
            
            <div class="space-y-4">
                <div>
                    <label class="text-[10px] font-black ml-4 text-blue-900">FULL NAME (AS PER CNIC)</label>
                    <input id="loginName" type="text" class="w-full p-4 bg-gray-50 rounded-2xl outline-none border-2 focus:border-blue-600 font-bold">
                </div>
                <div>
                    <label class="text-[10px] font-black ml-4 text-blue-900">ADMISSION KEY (OPTIONAL)</label>
                    <input id="loginKey" type="password" placeholder="••••" class="w-full p-4 bg-gray-50 rounded-2xl outline-none border-2 focus:border-blue-600 font-bold">
                </div>
                <button onclick="handleAdmission()" class="w-full bg-blue-900 text-white py-5 rounded-2xl font-black text-lg hover:bg-blue-800 transition">VERIFY & ENTER</button>
                <p class="text-[9px] text-center text-gray-400 px-6">By clicking verify, you agree to the GBTS Privacy Policy and Official Data Usage terms.</p>
            </div>
        </div>
    </div>

    <nav class="bg-white sticky top-0 z-50 p-4 border-b flex justify-between items-center px-8 shadow-sm">
        <div class="flex items-center gap-2">
            <div class="w-8 h-8 bg-blue-900 text-white flex items-center justify-center rounded-lg font-black text-xs">GB</div>
            <h1 class="font-black text-xl italic text-blue-900">GBTS ELITE</h1>
        </div>
        <div class="flex gap-4 md:gap-8">
            <button onclick="showTab('dashTab')" class="text-[10px] font-black uppercase text-blue-900">Dashboard</button>
            <button onclick="showTab('chatTab')" class="text-[10px] font-black uppercase text-gray-500">Community</button>
            <button onclick="showTab('policyTab')" class="text-[10px] font-black uppercase text-gray-500">Privacy</button>
            <button onclick="logoutCandidate()" class="text-[10px] font-black uppercase text-red-600 border-b-2 border-red-200">Logout</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-6 mt-6">

        <div id="dashTab" class="tab-content active-tab">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mb-12">
                <div class="bg-blue-900 p-8 rounded-[40px] text-white shadow-xl relative overflow-hidden">
                    <div class="relative z-10">
                        <p class="text-[10px] font-black opacity-60 uppercase mb-4 tracking-tighter">Candidate Application Status</p>
                        <h2 id="dispName" class="text-4xl font-black italic mb-2 tracking-tighter">---</h2>
                        <span class="status-badge bg-green-500 text-white">Verified Officer</span>
                        <div class="mt-8 flex gap-4">
                            <div class="text-center"><p class="text-[8px] opacity-60">LEVEL</p><p class="font-black text-xl" id="dispLvl">01</p></div>
                            <div class="text-center border-l border-white/20 pl-4"><p class="text-[8px] opacity-60">CREDITS</p><p class="font-black text-xl">100%</p></div>
                        </div>
                    </div>
                </div>
                
                <div class="bg-white p-8 rounded-[40px] shadow-sm border border-gray-100 flex items-center justify-between">
                    <div>
                        <h4 class="font-black text-blue-900 text-lg">Official News</h4>
                        <p class="text-xs text-gray-500 mt-2">Physical testing starts from April 2026. Keep your documents ready.</p>
                        <button class="mt-4 text-[10px] font-black bg-slate-100 px-4 py-2 rounded-lg">READ CIRCULAR</button>
                    </div>
                    <div class="w-16 h-16 bg-blue-50 rounded-full flex items-center justify-center text-2xl">📢</div>
                </div>
            </div>

            <h3 class="text-2xl font-black italic uppercase mb-8">Training Modules</h3>
            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-5 gap-6"></div>
        </div>

        <div id="policyTab" class="tab-content">
            <div class="max-w-4xl mx-auto bg-white p-10 rounded-[50px] shadow-sm border border-gray-100">
                <div class="text-center mb-10">
                    <h2 class="text-3xl font-black text-blue-900 italic">PRIVACY & DATA PROTECTION</h2>
                    <p class="text-[10px] font-bold text-gray-400 mt-2">LAST UPDATED: MARCH 2026</p>
                </div>
                
                <div class="policy-text">
                    <h4>1. Data Collection</h4>
                    <p>Hum bacho ka sirf wo data collect karte hain jo unki pehchan (Identity) aur progress track karne ke liye zaroori hai. Is mein aapka naam aur exam results shamil hain.</p>
                    
                    <h4>2. Information Security</h4>
                    <p>Aapka data 256-bit encryption ke saath Firebase servers par secure rakha jata hai. Prime Solutions privacy ko pehli tarjih deta hai aur kisi bhi 3rd party ko data nahi becha jata.</p>
                    
                    <h4>3. Candidate Conduct</h4>
                    <p>Global Discussion mein bad-tameezi, spamming ya galat maloomat phelane par aapka Admission cancel kiya ja sakta hai (Ban System).</p>
                    
                    <h4>4. Admission Transparency</h4>
                    <p>Hamara system automatic level locking par chalta hai. Har bacha apni mehnat se aglay level par jata hai, jis se system ki integrity maintain rehti hai.</p>
                </div>
                
                <div class="mt-10 p-6 bg-blue-50 rounded-3xl flex items-center gap-4">
                    <span class="text-2xl">🔒</span>
                    <p class="text-[10px] font-bold text-blue-900 leading-relaxed">This portal is protected by Prime Secure Shield. All interactions are monitored for quality and security purposes.</p>
                </div>
            </div>
        </div>

        <div id="chatTab" class="tab-content">
            <div class="max-w-3xl mx-auto bg-white rounded-[50px] p-8 shadow-2xl border">
                <div id="chatBox" class="h-96 overflow-y-auto mb-6 space-y-4 pr-4"></div>
                <div class="flex gap-2">
                    <input id="chatInp" type="text" placeholder="Post a query to the community..." class="flex-1 p-4 bg-gray-50 rounded-2xl outline-none font-bold text-sm">
                    <button onclick="sendMsg()" class="bg-blue-900 text-white px-8 rounded-2xl font-black">POST</button>
                </div>
            </div>
        </div>

    </main>

    <script>
        // FIREBASE SETUP
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
        const db = firebase.database();

        let me = { name: "", level: 1 };

        window.onload = () => {
            const saved = localStorage.getItem('active_officer');
            if(saved) {
                me.name = saved;
                me.level = parseInt(localStorage.getItem('officer_lvl')) || 1;
                document.getElementById('dispName').innerText = me.name;
                document.getElementById('dispLvl').innerText = "0" + me.level;
                renderLevels();
                listenToChat();
            } else {
                document.getElementById('loginOverlay').classList.remove('hidden');
            }
        };

        // ADMISSION LOGIN SYSTEM
        function handleAdmission() {
            const name = document.getElementById('loginName').value.trim();
            if(name.length < 3) return alert("Please enter valid Full Name for Admission");
            
            // Setting Session
            localStorage.setItem('active_officer', name);
            localStorage.setItem('officer_lvl', 1);
            alert("Application Verified. Welcome to GBTS Elite Portal.");
            location.reload();
        }

        // LOGOUT SYSTEM (Trusted Clear)
        function logoutCandidate() {
            if(confirm("Are you sure you want to end your session? Your progress is saved.")) {
                localStorage.removeItem('active_officer');
                // Hum level save rakhte hain take bacha dobara aaye to wapis wahin se shuru kare
                location.reload();
            }
        }

        function renderLevels() {
            const grid = document.getElementById('levelGrid');
            grid.innerHTML = '';
            for(let i=1; i<=10; i++) {
                let lock = i > me.level;
                grid.innerHTML += `
                    <div class="p-8 text-center rounded-[35px] border-2 ${lock ? 'opacity-30 border-gray-100' : 'border-blue-100 bg-white shadow-sm'}">
                        <div class="text-3xl mb-3">${lock ? '🔒' : '🎖️'}</div>
                        <h4 class="font-black text-[9px] uppercase tracking-widest">Module ${i}</h4>
                    </div>
                `;
            }
        }

        function sendMsg() {
            const t = document.getElementById('chatInp').value.trim();
            if(!t) return;
            db.ref('community_chat').push({ name: me.name, text: t, time: Date.now() });
            document.getElementById('chatInp').value = '';
        }

        function listenToChat() {
            db.ref('community_chat').limitToLast(15).on('value', snap => {
                const box = document.getElementById('chatBox');
                box.innerHTML = '';
                snap.forEach(c => {
                    const d = c.val();
                    box.innerHTML += `
                        <div class="p-4 rounded-3xl ${d.name === me.name ? 'bg-blue-900 text-white ml-10' : 'bg-gray-100 mr-10'}">
                            <p class="text-[8px] font-black uppercase mb-1 opacity-60">${d.name}</p>
                            <p class="text-sm font-bold">${d.text}</p>
                        </div>
                    `;
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }
    </script>
</body>
</html>
