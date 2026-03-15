<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Elite | Master Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getAuth, signInAnonymously } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
        import { getDatabase, ref, get, set, update, onValue } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com/",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        // Initialize Firebase
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getDatabase(app);

        // Global functions for HTML buttons
        window.handleLogin = async () => {
            const name = document.getElementById('uName').value.trim();
            if(!name) return alert("Please enter name, sweetie!");
            
            try {
                const userKey = name.toLowerCase().replace(/\s/g, '_').substring(0, 20);
                const cred = await signInAnonymously(auth);
                const userRef = ref(db, 'gbts_students/' + userKey);
                const snap = await get(userRef);

                if(!snap.exists()) {
                    await set(userRef, { name: name, level: 1, key: userKey, uid: cred.user.uid });
                }
                
                localStorage.setItem('gbts_user_key', userKey);
                document.getElementById('loginOverlay').classList.add('hidden');
                initPortal(userKey);
            } catch (e) {
                console.error(e);
                alert("Critical Error: " + e.message);
            }
        };

        function initPortal(key) {
            onValue(ref(db, 'gbts_students/' + key), (snap) => {
                const data = snap.val();
                if(data) {
                    document.getElementById('dispName').innerText = data.name;
                    document.getElementById('dispLvl').innerText = data.level;
                }
            });
        }

        // Auto-login check
        const savedKey = localStorage.getItem('gbts_user_key');
        if(savedKey) {
            document.getElementById('loginOverlay').classList.add('hidden');
            initPortal(savedKey);
        }
    </script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@400;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #0f172a; color: white; }
        .glass { background: rgba(255,255,255,0.05); backdrop-blur: 10px; border: 1px solid rgba(255,255,255,0.1); border-radius: 40px; }
    </style>
</head>
<body>

    <div id="loginOverlay" class="fixed inset-0 bg-[#0f172a] z-[9999] flex items-center justify-center p-6">
        <div class="glass p-12 max-w-md w-full text-center">
            <h2 class="text-3xl font-black mb-8 italic">GBTS LOGIN</h2>
            <input id="uName" type="text" placeholder="Your Name" class="w-full p-6 bg-white/5 rounded-3xl mb-6 text-center outline-none border-2 border-transparent focus:border-blue-500">
            <button onclick="handleLogin()" class="w-full bg-blue-600 py-6 rounded-3xl font-black text-xl shadow-lg">ENTER</button>
        </div>
    </div>

    <main class="p-10 max-w-5xl mx-auto">
        <div class="glass p-10 mb-10 text-center md:text-left">
            <p class="text-xs font-black text-blue-400 mb-2 tracking-widest uppercase">Student Portal</p>
            <h1 id="dispName" class="text-5xl font-black italic mb-4">---</h1>
            <div class="inline-block bg-blue-600/20 px-6 py-2 rounded-xl border border-blue-600/30 text-sm font-black italic">
                LEVEL: <span id="dispLvl">01</span>
            </div>
        </div>
    </main>

</body>
</html>
