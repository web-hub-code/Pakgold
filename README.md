<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily | Pro Trading System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { background-color: #020617; color: #f1f5f9; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; }
        .glass { background: rgba(30, 41, 59, 0.4); backdrop-filter: blur(16px); border: 1px solid rgba(255, 255, 255, 0.05); }
        .btn-glow:active { transform: scale(0.95); transition: 0.2s; }
        .tab-content { animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, onAuthStateChanged, signOut, signInWithEmailAndPassword, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, collection, addDoc, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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

        // --- AUTH & DATA SYNC ---
        onAuthStateChanged(auth, (user) => {
            const authSec = document.getElementById('authSection');
            const appBody = document.getElementById('appBody');
            if(user) {
                authSec.classList.add('hidden');
                appBody.classList.remove('hidden');
                onSnapshot(doc(db, "users", user.uid), (d) => {
                    if(d.exists()) {
                        window.userData = d.data();
                        document.getElementById('displayBal').innerText = "$" + window.userData.balance.toFixed(2);
                        document.getElementById('displayName').innerText = window.userData.username;
                    }
                });
            } else {
                authSec.classList.remove('hidden');
                appBody.classList.add('hidden');
            }
        });

        // --- CORE FUNCTIONS ---
        window.handleAuth = async (type) => {
            const u = document.getElementById('uInp').value;
            const p = document.getElementById('pInp').value;
            const email = u.includes('@') ? u : `${u}@apex.com`;
            try {
                if(type === 'signup') {
                    const res = await createUserWithEmailAndPassword(auth, email, p);
                    await setDoc(doc(db, "users", res.user.uid), { username: u, balance: 0, role: "user" });
                } else { await signInWithEmailAndPassword(auth, email, p); }
            } catch(e) { alert(e.message); }
        };

        window.submitDeposit = async () => {
            const amt = document.getElementById('dAmount').value;
            const tid = document.getElementById('dTid').value;
            if(!amt || !tid) return alert("Fill all fields, sweetie!");
            await addDoc(collection(db, "deposits"), {
                uid: auth.currentUser.uid,
                email: auth.currentUser.email,
                amount: parseFloat(amt),
                tid: tid,
                status: "pending",
                time: new Date()
            });
            alert("Request sent! Admin will verify soon.");
            showPage('homePage');
        };

        window.buyPlan = async (price) => {
            if(window.userData.balance < price) return alert("Insufficient Balance, sweetie!");
            const newBal = window.userData.balance - price;
            await updateDoc(doc(db, "users", auth.currentUser.uid), { balance: newBal });
            await addDoc(collection(db, "active_plans"), { uid: auth.currentUser.uid, price: price, date: new Date() });
            alert(`Plan $${price} activated successfully!`);
        };

        window.sendSupportMsg = async () => {
            const msg = document.getElementById('supportMsg').value;
            if(!msg) return;
            await addDoc(collection(db, "messages"), {
                uid: auth.currentUser.uid,
                username: window.userData.username,
                message: msg,
                time: new Date()
            });
            alert("Message sent to Help Desk! We will reply soon.");
            document.getElementById('supportMsg').value = "";
        };

        window.showPage = (id) => {
            ['homePage','plansPage','helpPage','walletPage'].forEach(p => document.getElementById(p).classList.add('hidden'));
            document.getElementById(id).classList.remove('hidden');
        };

        window.logout = () => signOut(auth);
    </script>
</head>
<body>

    <div id="authSection" class="min-h-screen flex items-center justify-center p-6">
        <div class="w-full max-w-sm glass p-10 rounded-[2.5rem] shadow-2xl border-t-2 border-blue-500/20">
            <h1 class="text-4xl font-black text-center text-blue-500 mb-8 italic">APEXDAILY</h1>
            <input id="uInp" type="text" placeholder="Username" class="w-full p-4 bg-white/5 rounded-2xl mb-4 outline-none border border-white/10 focus:border-blue-500">
            <input id="pInp" type="password" placeholder="Password" class="w-full p-4 bg-white/5 rounded-2xl mb-8 outline-none border border-white/10 focus:border-blue-500">
            <div class="flex gap-4">
                <button onclick="handleAuth('login')" class="flex-1 bg-blue-600 py-4 rounded-2xl font-bold btn-glow shadow-lg shadow-blue-500/20">Login</button>
                <button onclick="handleAuth('signup')" class="flex-1 bg-white/5 py-4 rounded-2xl font-bold border border-white/10">Signup</button>
            </div>
        </div>
    </div>

    <div id="appBody" class="hidden pb-32">
        <div class="p-6 flex justify-between items-center sticky top-0 bg-[#020617]/80 backdrop-blur-lg z-50">
            <h2 class="text-xl font-black italic text-blue-500">APEX.PRO</h2>
            <button onclick="logout()" class="text-gray-400 text-sm">Logout</button>
        </div>

        <div id="homePage" class="p-6 tab-content">
            <div class="glass p-8 rounded-[2.5rem] bg-gradient-to-br from-blue-600 to-indigo-900 border-none shadow-[0_20px_60px_rgba(37,99,235,0.3)] mb-8">
                <p class="text-xs text-blue-100 uppercase tracking-widest font-bold">Trading Balance</p>
                <h1 id="displayBal" class="text-6xl font-black my-3 tracking-tighter">$0.00</h1>
                <div class="flex gap-4 mt-6">
                    <button onclick="showPage('walletPage')" class="flex-1 bg-white/20 p-4 rounded-2xl font-bold backdrop-blur-md btn-glow"><i class="fa-solid fa-plus-circle mr-2"></i> Deposit</button>
                    <button class="flex-1 bg-black/20 p-4 rounded-2xl font-bold backdrop-blur-md btn-glow text-sm">Withdraw</button>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <div class="glass p-5 rounded-3xl"><p class="text-[10px] text-gray-400 uppercase">Status</p><h4 class="text-green-400 font-bold">Verified Account</h4></div>
                <div class="glass p-5 rounded-3xl"><p class="text-[10px] text-gray-400 uppercase">Live ROI</p><h4 class="text-blue-400 font-bold">2.8% / Day</h4></div>
            </div>
        </div>

        <div id="plansPage" class="hidden p-6 tab-content">
            <h2 class="text-2xl font-black mb-6 italic text-blue-400">Trading Plans</h2>
            <div class="grid grid-cols-2 gap-4">
                <script>
                    const plansArr = [5, 10, 20, 50, 100, 250, 500, 1000, 2500, 5000];
                    plansArr.forEach(val => {
                        document.write(`
                        <div class="glass p-6 rounded-3xl border-l-4 border-blue-600">
                            <i class="fa-solid fa-bolt text-yellow-500 mb-3"></i>
                            <h3 class="text-2xl font-black">$${val}</h3>
                            <p class="text-[10px] text-gray-500 mt-1">Premium Returns</p>
                            <button onclick="buyPlan(${val})" class="mt-4 w-full bg-blue-600/20 text-blue-400 py-2 rounded-xl text-xs font-bold border border-blue-500/20 btn-glow">Buy Now</button>
                        </div>`);
                    });
                </script>
            </div>
        </div>

        <div id="walletPage" class="hidden p-6 tab-content">
            <h2 class="text-2xl font-black mb-6 italic text-green-400">Add Funds</h2>
            <div class="glass p-6 rounded-[2rem] space-y-4">
                <div class="bg-black/40 p-4 rounded-2xl border border-white/5">
                    <p class="text-[10px] text-gray-500">JazzCash / EasyPaisa Number</p>
                    <div class="flex justify-between items-center mt-1">
                        <span class="font-bold text-lg">0300-1234567</span>
                        <i class="fa-solid fa-copy text-blue-500"></i>
                    </div>
                </div>
                <input id="dAmount" type="number" placeholder="Enter Amount ($)" class="w-full p-4 bg-black/20 rounded-2xl outline-none border border-white/5">
                <input id="dTid" type="text" placeholder="Transaction ID (TID)" class="w-full p-4 bg-black/20 rounded-2xl outline-none border border-white/5">
                <button onclick="submitDeposit()" class="w-full bg-green-600 py-4 rounded-2xl font-bold shadow-lg shadow-green-600/20">Submit Payment</button>
            </div>
        </div>

        <div id="helpPage" class="hidden p-6 tab-content text-center">
            <i class="fa-solid fa-headset text-6xl text-blue-500 mb-6 mt-10"></i>
            <h2 class="text-2xl font-black mb-2 italic">Admin Help Desk</h2>
            <p class="text-xs text-gray-500 mb-8">Directly message the owner for any help.</p>
            <div class="glass p-6 rounded-[2rem]">
                <textarea id="supportMsg" placeholder="Type your message here, sweetie..." class="w-full h-32 p-4 bg-black/20 rounded-2xl outline-none border border-white/5 mb-4"></textarea>
                <button onclick="sendSupportMsg()" class="w-full bg-blue-600 py-4 rounded-2xl font-bold">Send Message</button>
            </div>
        </div>

        <nav class="fixed bottom-0 left-0 right-0 glass p-6 flex justify-around items-center rounded-t-[2.5rem] z-50">
            <button onclick="showPage('homePage')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-house"></i><span class="text-[8px]">Home</span></button>
            <button onclick="showPage('plansPage')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-rocket"></i><span class="text-[8px]">Plans</span></button>
            <button onclick="showPage('walletPage')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-wallet"></i><span class="text-[8px]">Wallet</span></button>
            <button onclick="showPage('helpPage')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-headset"></i><span class="text-[8px]">Help</span></button>
        </nav>
    </div>

</body>
</html>
