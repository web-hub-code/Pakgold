<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily Pro | Trade & Earn</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { background-color: #030712; color: #f8fafc; font-family: 'Outfit', sans-serif; }
        .glass-card { background: rgba(30, 41, 59, 0.5); backdrop-filter: blur(12px); border: 1px solid rgba(255, 255, 255, 0.05); }
        .gradient-text { background: linear-gradient(to right, #3b82f6, #60a5fa); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .nav-item { transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1); }
        .nav-item:active { transform: scale(0.9); color: #3b82f6; }
        .plan-card:hover { border-color: #3b82f6; transform: translateY(-5px); transition: 0.3s; }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg",
            authDomain: "investment-84f4e.firebaseapp.com",
            projectId: "investment-84f4e",
            storageBucket: "investment-84f4e.firebasestorage.app",
            messagingSenderId: "975293889308",
            appId: "1:975293889308:web:6d034a99cc966c75ff58d9"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // --- AUTH SYSTEM ---
        window.handleAuth = async (type) => {
            const u = document.getElementById('username').value.trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Fill all fields sweetie!");
            const email = u.includes('@') ? u : `${u}@apex.com`;

            try {
                if(type === 'signup') {
                    const res = await createUserWithEmailAndPassword(auth, email, p);
                    await setDoc(doc(db, "users", res.user.uid), { username: u, balance: 0, role: "user" });
                } else { await signInWithEmailAndPassword(auth, email, p); }
            } catch (e) { alert(e.message); }
        };

        // --- PAGE SWITCHER ---
        window.showPage = (id) => {
            ['homePage','plansPage','historyPage','morePage'].forEach(p => document.getElementById(p).classList.add('hidden'));
            document.getElementById(id).classList.remove('hidden');
        };

        onAuthStateChanged(auth, (user) => {
            if(user) {
                document.getElementById('authSection').classList.add('hidden');
                document.getElementById('appBody').classList.remove('hidden');
                onSnapshot(doc(db, "users", user.uid), (d) => {
                    if(d.exists()){
                        document.getElementById('uBal').innerText = "$" + d.data().balance.toFixed(2);
                        document.getElementById('uName').innerText = d.data().username;
                        document.getElementById('refLink').innerText = "apexdaily.com/r=" + d.data().username;
                    }
                });
            } else {
                document.getElementById('authSection').classList.remove('hidden');
                document.getElementById('appBody').classList.add('hidden');
            }
        });

        // --- FAKE NOTIFICATION ---
        setInterval(() => {
            const names = ["Zain", "Sara", "Hamza", "Ayesha", "Ali", "Fatima"];
            const msgs = ["just deposited $50", "earned $12 profit", "withdrew $100", "bought Pro Plan"];
            const toast = document.getElementById('fakeToast');
            toast.innerText = `${names[Math.floor(Math.random()*names.length)]} ${msgs[Math.floor(Math.random()*msgs.length)]} 🚀`;
            toast.classList.remove('hidden');
            setTimeout(() => toast.classList.add('hidden'), 3000);
        }, 12000);

        window.copyRef = () => {
            navigator.clipboard.writeText(document.getElementById('refLink').innerText);
            alert("Link Copied Sweetie! 😘");
        };

        window.logout = () => signOut(auth);
    </script>
</head>
<body class="p-0 m-0">

    <div id="fakeToast" class="hidden fixed top-8 left-1/2 -translate-x-1/2 bg-blue-600 px-6 py-2 rounded-full text-[10px] font-bold z-[100] shadow-xl border border-white/20"></div>

    <div id="authSection" class="min-h-screen flex items-center justify-center p-6">
        <div class="w-full max-w-sm glass-card p-10 rounded-[3rem] text-center border-t-2 border-blue-500/30">
            <h1 class="text-4xl font-black gradient-text mb-2 italic">APEXDAILY</h1>
            <p class="text-[10px] text-gray-500 uppercase tracking-[0.3em] mb-10">Wealth Redefined</p>
            <input id="username" type="text" placeholder="Username" class="w-full p-4 bg-white/5 rounded-2xl mb-4 outline-none border border-white/5 focus:border-blue-500">
            <input id="password" type="password" placeholder="Password" class="w-full p-4 bg-white/5 rounded-2xl mb-8 outline-none border border-white/5 focus:border-blue-500">
            <div class="flex gap-4">
                <button onclick="handleAuth('login')" class="flex-1 bg-blue-600 py-4 rounded-2xl font-bold active:scale-95 transition">Login</button>
                <button onclick="handleAuth('signup')" class="flex-1 bg-white/5 border border-white/10 py-4 rounded-2xl font-bold active:scale-95 transition">Join</button>
            </div>
        </div>
    </div>

    <div id="appBody" class="hidden pb-28">
        
        <div id="homePage" class="p-6 animate-in fade-in duration-500">
            <div class="flex justify-between items-center mb-8">
                <div>
                    <h2 class="text-2xl font-black">Hi, <span id="uName" class="gradient-text">...</span></h2>
                    <p class="text-[10px] text-gray-500">Global Profit Member</p>
                </div>
                <div class="w-12 h-12 rounded-2xl glass-card flex items-center justify-center text-blue-500"><i class="fa-solid fa-user-tie"></i></div>
            </div>

            <div class="bg-gradient-to-br from-blue-700 to-indigo-900 p-8 rounded-[2.5rem] shadow-[0_20px_50px_rgba(59,130,246,0.2)] mb-8 relative overflow-hidden">
                <p class="text-xs text-blue-200 uppercase tracking-widest font-semibold">Net Balance</p>
                <h1 id="uBal" class="text-6xl font-black my-3 tracking-tighter">$0.00</h1>
                <div class="flex gap-4 mt-6">
                    <button class="flex-1 bg-white/20 p-4 rounded-2xl font-bold backdrop-blur-md">Deposit</button>
                    <button class="flex-1 bg-black/30 p-4 rounded-2xl font-bold backdrop-blur-md text-sm">Withdraw</button>
                </div>
                <div class="absolute -right-10 -bottom-10 w-40 h-40 bg-white/10 rounded-full blur-3xl"></div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <div class="glass-card p-4 rounded-2xl"><p class="text-[10px] text-gray-500">Daily ROI</p><h4 class="text-green-400 font-bold">+2.8%</h4></div>
                <div class="glass-card p-4 rounded-2xl"><p class="text-[10px] text-gray-500">Referrals</p><h4 class="text-blue-400 font-bold">0 Earned</h4></div>
            </div>

            <div class="glass-card p-6 rounded-3xl border-l-4 border-blue-600">
                <p class="text-[10px] text-gray-400 mb-3">Copy Invite Link & Earn $1.00</p>
                <div class="flex gap-2">
                    <div id="refLink" class="bg-black/40 p-3 rounded-xl flex-1 text-[10px] text-gray-500 truncate">...</div>
                    <button onclick="copyRef()" class="bg-blue-600 px-5 rounded-xl text-xs font-bold">Copy</button>
                </div>
            </div>
        </div>

        <div id="plansPage" class="hidden p-6">
            <h2 class="text-3xl font-black mb-8 italic">Vip <span class="text-blue-500">Investment</span></h2>
            <div class="grid grid-cols-2 gap-4">
                <script>
                    const plansArr = [5, 10, 15, 20, 50, 80, 100, 150, 200, 250, 300, 400, 500, 700, 1000, 1500, 2000, 3000, 5000, 10000];
                    plansArr.forEach((val, i) => {
                        document.write(`
                        <div class="glass-card plan-card p-6 rounded-[2rem] relative">
                            <i class="fa-solid ${val < 100 ? 'fa-rocket text-green-500' : 'fa-crown text-blue-500'} text-xl mb-4"></i>
                            <p class="text-[8px] text-gray-500 uppercase font-bold">Plan Level ${i+1}</p>
                            <h3 class="text-2xl font-black mt-1">$${val}</h3>
                            <p class="text-[9px] text-green-400 mt-2 font-bold">Daily: 2.5% Profit</p>
                            <button class="mt-4 w-full bg-white/5 py-2 rounded-xl text-[10px] font-bold border border-white/5">Activate</button>
                        </div>`);
                    });
                </script>
            </div>
        </div>

        <div id="historyPage" class="hidden p-6">
            <h2 class="text-2xl font-black mb-8">Transactions</h2>
            <div class="text-center py-20">
                <i class="fa-solid fa-folder-open text-5xl text-gray-800 mb-4"></i>
                <p class="text-gray-500 text-sm italic">Nothing to show yet, sweetie.</p>
            </div>
        </div>

        <div id="morePage" class="hidden p-6">
            <h2 class="text-2xl font-black mb-8">Support Center</h2>
            <div class="space-y-4">
                <a href="https://chat.whatsapp.com/YOUR_GROUP_ID" target="_blank" class="glass-card p-6 rounded-3xl flex items-center gap-4 border-l-4 border-green-500">
                    <i class="fa-brands fa-whatsapp text-3xl text-green-500"></i>
                    <div><p class="font-bold">Official WhatsApp Group</p><p class="text-xs text-gray-500 text-nowrap">Join for latest signals and news</p></div>
                </a>
                <a href="https://wa.me/923000000000" target="_blank" class="glass-card p-6 rounded-3xl flex items-center gap-4 border-l-4 border-blue-500">
                    <i class="fa-solid fa-headset text-3xl text-blue-500"></i>
                    <div><p class="font-bold">Direct Admin Message</p><p class="text-xs text-gray-500">Chat with support for help</p></div>
                </a>
                
                <div class="glass-card p-5 rounded-3xl space-y-4 mt-8">
                    <div class="flex items-center gap-3 opacity-60"><i class="fa-solid fa-file-contract"></i> <p class="text-sm">Privacy Policy</p></div>
                    <div class="flex items-center gap-3 opacity-60"><i class="fa-solid fa-building"></i> <p class="text-sm">About ApexDaily Ltd.</p></div>
                    <div onclick="logout()" class="flex items-center gap-3 text-red-500 font-bold"><i class="fa-solid fa-power-off"></i> <p class="text-sm">Sign Out</p></div>
                </div>
            </div>
        </div>

        <nav class="fixed bottom-0 left-0 right-0 glass-card p-6 flex justify-around items-center rounded-t-[2.5rem] border-t border-white/10 z-50">
            <button onclick="showPage('homePage')" class="nav-item flex flex-col items-center gap-1"><i class="fa-solid fa-house-chimney text-xl"></i><span class="text-[8px] font-bold">Home</span></button>
            <button onclick="showPage('plansPage')" class="nav-item flex flex-col items-center gap-1"><i class="fa-solid fa-bolt-lightning text-xl"></i><span class="text-[8px] font-bold">Invest</span></button>
            <button onclick="showPage('historyPage')" class="nav-item flex flex-col items-center gap-1"><i class="fa-solid fa-clock-rotate-left text-xl"></i><span class="text-[8px] font-bold">History</span></button>
            <button onclick="showPage('morePage')" class="nav-item flex flex-col items-center gap-1"><i class="fa-solid fa-circle-nodes text-xl"></i><span class="text-[8px] font-bold">Support</span></button>
        </nav>

    </div>

</body>
</html>
