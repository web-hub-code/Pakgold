<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS God-Mode Elite | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f0f4f8; color: #1e293b; transition: 0.3s; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: zoomIn 0.3s ease; }
        @keyframes zoomIn { from { opacity: 0; transform: scale(0.98); } to { opacity: 1; transform: scale(1); } }
        
        /* Advanced Chat */
        #chatBox { height: 450px; overflow-y: auto; scroll-behavior: smooth; display: flex; flex-direction: column; gap: 10px; padding: 15px; }
        .msg-bubble { background: white; padding: 12px 16px; border-radius: 20px 20px 20px 5px; max-width: 85%; position: relative; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05); }
        .reply-box { font-size: 11px; background: #f1f5f9; padding: 5px 10px; border-left: 3px solid #1e3a8a; border-radius: 5px; margin-bottom: 5px; color: #64748b; }
        .react-btn { font-size: 14px; cursor: pointer; transition: 0.2s; }
        .react-btn:hover { transform: scale(1.3); }

        /* Lock Screen */
        #lockOverlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.95); z-index: 5000; color: white; flex-direction: column; align-items: center; justify-content: center; }
        .banned-msg { display: none; position: fixed; inset: 0; background: white; z-index: 4000; flex-direction: column; align-items: center; justify-content: center; text-align: center; p: 20px; }
    </style>
</head>
<body class="pb-20">

    <div id="lockOverlay">
        <h1 class="text-6xl font-black mb-4 text-red-600 animate-pulse">LOCKDOWN</h1>
        <p class="text-xl font-bold uppercase tracking-widest">System Maintenance in Progress</p>
    </div>

    <div id="banScreen" class="banned-msg">
        <h1 class="text-7xl mb-4">🚫</h1>
        <h2 class="text-4xl font-black text-red-600 mb-2">ACCESS DENIED</h2>
        <p class="font-bold text-slate-500">Aapko administration ne block kar diya hai.<br>Mazeed maloomat ke liye helpline par raabta karein.</p>
    </div>

    <div id="loginScreen" class="fixed inset-0 bg-slate-900 z-[2000] flex items-center justify-center p-6 hidden">
        <div class="bg-white p-12 rounded-[50px] max-w-md w-full text-center shadow-2xl">
            <h2 class="text-4xl font-black text-blue-900 mb-6 italic">PRIME ACADEMY</h2>
            <input id="uInput" type="text" placeholder="Apna Name Likhein" class="w-full p-5 bg-gray-100 rounded-3xl mb-4 outline-none font-bold text-center border-2 border-transparent focus:border-blue-600">
            <button onclick="doLogin()" class="w-full bg-blue-900 text-white py-4 rounded-3xl font-black text-lg">ENTER HUB</button>
        </div>
    </div>

    <nav class="bg-white sticky top-0 z-50 p-4 border-b flex justify-between items-center px-8 shadow-sm">
        <h1 class="font-black text-xl italic text-blue-900">PRIME HUB</h1>
        <div class="flex gap-5">
            <button onclick="showTab('levelsTab')" class="text-[10px] font-black uppercase text-blue-900">Exams</button>
            <button onclick="showTab('chatTab')" class="text-[10px] font-black uppercase text-slate-500">Discussion</button>
            <button onclick="showTab('godPanel')" id="adminLink" class="hidden text-[10px] font-black uppercase text-red-600 border-2 border-red-600 px-2 rounded">God Mode</button>
            <button onclick="logout()" class="text-[10px] font-black uppercase text-red-500">Logout</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-4 mt-6">
        
        <div id="levelsTab" class="tab-content active-tab">
            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-5 gap-6"></div>
        </div>

        <div id="chatTab" class="tab-content">
            <div class="max-w-3xl mx-auto bg-slate-100 rounded-[40px] p-6 shadow-inner">
                <div id="replyPreview" class="hidden bg-blue-100 p-3 rounded-2xl mb-2 flex justify-between items-center">
                    <span id="replyingToText" class="text-xs font-bold text-blue-900 italic"></span>
                    <button onclick="cancelReply()" class="text-red-500 font-black">×</button>
                </div>
                <div id="chatBox"></div>
                <div class="flex gap-2 mt-4 bg-white p-2 rounded-3xl shadow-lg">
                    <input id="msgInput" type="text" placeholder="Type a message..." class="flex-1 p-4 outline-none font-medium">
                    <button onclick="pushMsg()" class="bg-blue-900 text-white px-8 rounded-2xl font-black">SEND</button>
                </div>
            </div>
        </div>

        <div id="godPanel" class="tab-content bg-slate-900 p-10 rounded-[50px] text-white">
            <div class="flex justify-between items-center mb-10">
                <h2 class="text-3xl font-black text-blue-400 italic">SYSTEM OVERRIDE</h2>
                <div class="flex gap-2">
                    <button onclick="toggleLock()" class="bg-red-600 px-4 py-2 rounded-xl text-xs font-bold">LOCKDOWN SITE</button>
                </div>
            </div>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="bg-slate-800 p-6 rounded-3xl">
                    <h4 class="text-yellow-500 font-black mb-4 uppercase text-xs">Approval Requests</h4>
                    <div id="reqList" class="space-y-3"></div>
                </div>
                <div class="bg-slate-800 p-6 rounded-3xl">
                    <h4 class="text-red-500 font-black mb-4 uppercase text-xs">User Management (Ban System)</h4>
                    <div class="flex gap-2 mb-4">
                        <input id="banInput" type="text" placeholder="User Name" class="flex-1 p-2 rounded bg-slate-700 text-white outline-none">
                        <button onclick="manageUser(true)" class="bg-red-600 px-3 rounded font-bold">BAN</button>
                        <button onclick="manageUser(false)" class="bg-green-600 px-3 rounded font-bold">UNBAN</button>
                    </div>
                </div>
            </div>
        </div>
    </main>

    <footer class="fixed bottom-0 left-0 right-0 p-4 text-center">
        <span onclick="triggerGod()" class="text-[8px] text-gray-400 opacity-20 cursor-default">PRIME_SOLUTIONS_GOD_KEY</span>
    </footer>

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
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();

        let me = { name: "", level: 1, status: "ready", isGod: false };
        let replyData = null;
        let taps = 0;

        window.onload = () => {
            const saved = localStorage.getItem('hub_user');
            if(saved) {
                me.name = saved;
                checkBanStatus();
                listenLock();
                listenChat();
                renderLevels();
            } else {
                document.getElementById('loginScreen').classList.remove('hidden');
            }
        };

        function doLogin() {
            const n = document.getElementById('uInput').value.trim();
            if(!n) return;
            localStorage.setItem('hub_user', n);
            location.reload();
        }

        function logout() { localStorage.clear(); location.reload(); }

        // --- CHAT WITH REPLIES & REACTIONS ---
        function pushMsg() {
            const txt = document.getElementById('msgInput').value.trim();
            if(!txt) return;
            db.ref('global_chat_v2').push({
                name: me.name,
                text: txt,
                replyTo: replyData,
                time: Date.now()
            });
            cancelReply();
            document.getElementById('msgInput').value = '';
        }

        function listenChat() {
            db.ref('global_chat_v2').limitToLast(30).on('value', snap => {
                const box = document.getElementById('chatBox');
                box.innerHTML = '';
                snap.forEach(c => {
                    const d = c.val();
                    const key = c.key;
                    box.innerHTML += `
                        <div class="msg-bubble ${d.name === me.name ? 'self-end' : 'self-start'}">
                            <span class="text-[10px] font-black text-blue-900 uppercase">${d.name}</span>
                            ${d.replyTo ? `<div class="reply-box">Replying to: ${d.replyTo}</div>` : ''}
                            <p class="text-sm font-medium">${d.text}</p>
                            <div class="mt-2 flex gap-2">
                                <span class="react-btn" onclick="addReact('${key}', '🔥')">🔥</span>
                                <span class="react-btn" onclick="addReact('${key}', '👍')">👍</span>
                                <span class="text-[10px] text-blue-400 font-bold ml-4 cursor-pointer" onclick="setReply('${d.name}', '${d.text}')">REPLY</span>
                            </div>
                            <div id="react-${key}" class="text-[10px] mt-1 font-bold text-orange-500"></div>
                        </div>
                    `;
                    // Fetch Reactions
                    db.ref('reactions/'+key).on('value', rSnap => {
                        const rArea = document.getElementById('react-'+key);
                        if(rArea && rSnap.val()) rArea.innerText = Object.values(rSnap.val()).join(' ');
                    });
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        function setReply(name, text) {
            replyData = name + ": " + text;
            document.getElementById('replyPreview').classList.remove('hidden');
            document.getElementById('replyingToText').innerText = "Replying to: " + name;
        }

        function cancelReply() {
            replyData = null;
            document.getElementById('replyPreview').classList.add('hidden');
        }

        function addReact(key, emoji) {
            db.ref('reactions/'+key).push(emoji);
        }

        // --- BAN & LOCKDOWN SYSTEM ---
        function checkBanStatus() {
            db.ref('banned_users/' + me.name).on('value', snap => {
                if(snap.val()) document.getElementById('banScreen').style.display = 'flex';
            });
        }

        function manageUser(ban) {
            const target = document.getElementById('banInput').value.trim();
            if(!target) return;
            db.ref('banned_users/' + target).set(ban ? true : null);
            alert(target + (ban ? " Banned!" : " Unbanned!"));
            document.getElementById('banInput').value = '';
        }

        function toggleLock() {
            db.ref('lockdown').once('value', snap => {
                db.ref('lockdown').set(!snap.val());
            });
        }

        function listenLock() {
            db.ref('lockdown').on('value', snap => {
                document.getElementById('lockOverlay').style.display = snap.val() ? 'flex' : 'none';
            });
        }

        // --- GOD MODE TRIGGER ---
        function triggerGod() {
            taps++;
            if(taps === 4) {
                const p = prompt("ENTER SECRET GOD KEY:");
                if(p === "prime786") {
                    me.isGod = true;
                    document.getElementById('adminLink').classList.remove('hidden');
                    showTab('godPanel');
                    alert("GOD MODE ACCESS GRANTED!");
                }
                taps = 0;
            }
        }

        function renderLevels() {
            const grid = document.getElementById('levelGrid');
            grid.innerHTML = '';
            for(let i=1; i<=10; i++) {
                let locked = i > me.level;
                grid.innerHTML += `
                    <div class="level-card p-10 text-center shadow-lg ${locked ? 'locked' : 'border-white'}">
                        <div class="text-5xl mb-3">${locked ? '🔒' : '🎖️'}</div>
                        <h4 class="font-black text-xs">LEVEL ${i}</h4>
                    </div>
                `;
            }
        }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }
    </script>
</body>
</html>
