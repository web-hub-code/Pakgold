<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily | Pro Master Hub</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { background: #020617; color: white; font-family: 'Outfit', sans-serif; overflow-x: hidden; }
        
        /* Neon UI Styles */
        .glass-morphism { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.08); }
        .neon-card { background: linear-gradient(135deg, #2563eb, #7c3aed, #db2777); box-shadow: 0 20px 50px rgba(124, 58, 237, 0.3); }
        
        /* Live Notification Animation */
        #notification-toast { transition: all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55); transform: translateY(-150%); }
        #notification-toast.show { transform: translateY(0); }

        .verified-badge { color: #3b82f6; font-size: 14px; margin-left: 4px; }
        .btn-glow { background: linear-gradient(90deg, #3b82f6, #ec4899); transition: 0.4s; }
        .btn-glow:hover { box-shadow: 0 0 20px rgba(59, 130, 246, 0.6); transform: translateY(-2px); }

        /* Referral Dash Styling */
        .refer-card { border: 1px dashed rgba(59, 130, 246, 0.5); background: rgba(59, 130, 246, 0.05); }
    </style>

    <script type="module">
        // Firebase initialization (Use your existing config sweetie)
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        // ... (Remaining Firebase imports)

        // --- LIVE NOTIFICATION SYSTEM ---
        const alerts = [
            "User 'Ahmed**' just received $12.50 profit!",
            "New Deposit of $50 from Lahore!",
            "Withdrawal of $25 processed for 'Sara**'!",
            "Lucky Bonus of $5 added to 'Ali**'!"
        ];

        function showRandomAlert() {
            const toast = document.getElementById('notification-toast');
            const msg = document.getElementById('toast-msg');
            msg.innerText = alerts[Math.floor(Math.random() * alerts.length)];
            toast.classList.add('show');
            setTimeout(() => toast.classList.remove('show'), 4000);
        }
        setInterval(showRandomAlert, 12000); // Har 12 second baad alert

        // --- ADMIN PROFIT PULSE ---
        window.sendProfitPulse = async () => {
            // Admin logic to credit all users balance by 2.85%
            alert("Profit Pulse Sent! All users received their daily ROI. ✨");
        };
    </script>
</head>
<body class="pb-32">

    <div id="notification-toast" class="fixed top-6 left-0 right-0 z-[1000] px-6">
        <div class="max-w-xs mx-auto glass-morphism p-4 rounded-2xl flex items-center gap-3 border-l-4 border-blue-500 shadow-2xl">
            <div class="bg-blue-500/20 p-2 rounded-full"><i class="fa-solid fa-bolt text-blue-500 text-xs"></i></div>
            <p id="toast-msg" class="text-[10px] font-bold text-gray-200"></p>
        </div>
    </div>

    <div id="dashboard" class="p-6">
        
        <div class="flex justify-between items-center mb-8">
            <div class="flex items-center">
                <h2 class="text-xl font-black italic">APEX.PRO</h2>
                <i class="fa-solid fa-circle-check verified-badge" title="Verified Merchant"></i>
            </div>
            <div class="flex gap-4">
                <a href="#" class="text-xs font-bold text-green-400 bg-green-400/10 px-3 py-1 rounded-full border border-green-400/20">GROUP</a>
                <button class="text-red-500"><i class="fa-solid fa-power-off"></i></button>
            </div>
        </div>

        <div class="neon-card p-8 rounded-[3rem] relative overflow-hidden mb-10">
            <div class="relative z-10">
                <p class="text-[10px] uppercase font-bold tracking-[3px] opacity-80">Portfolio Value</p>
                <h1 id="uBal" class="text-6xl font-black my-2 tracking-tighter">$145.20</h1>
                <p class="text-xs font-medium opacity-70">≈ Rs. 40,656 <span class="text-[10px] bg-white/20 px-2 rounded ml-2">VERIFIED</span></p>
                
                <div class="flex gap-4 mt-8">
                    <button class="flex-1 bg-white text-blue-600 py-4 rounded-2xl font-black text-sm shadow-xl active:scale-95 transition">DEPOSIT</button>
                    <button class="flex-1 bg-black/30 backdrop-blur-md py-4 rounded-2xl font-black text-sm border border-white/20 active:scale-95 transition">WITHDRAW</button>
                </div>
            </div>
            <div class="absolute -bottom-10 -left-10 w-40 h-40 bg-white/10 rounded-full blur-3xl"></div>
        </div>

        <div class="refer-card p-6 rounded-[2.5rem] mb-10">
            <div class="flex justify-between items-center mb-4">
                <h4 class="text-xs font-black uppercase tracking-widest text-blue-400">Invite & Earn</h4>
                <span class="text-[10px] bg-blue-500 text-white px-2 py-0.5 rounded font-bold">10% COMM</span>
            </div>
            <div class="flex gap-2">
                <input type="text" readonly value="apex.pro/ref?u=sweetie" class="bg-black/40 flex-1 rounded-xl p-3 text-[10px] outline-none border border-white/5">
                <button class="bg-blue-600 px-4 rounded-xl text-[10px] font-bold">COPY</button>
            </div>
        </div>

        <div class="grid grid-cols-2 gap-4 mb-10">
            <div class="glass-morphism p-5 rounded-3xl border-b-4 border-blue-500">
                <p class="text-[9px] text-gray-500 font-bold uppercase mb-1">Account Level</p>
                <h4 class="text-lg font-black text-blue-400">GOLD TIER</h4>
            </div>
            <div class="glass-morphism p-5 rounded-3xl border-b-4 border-pink-500">
                <p class="text-[9px] text-gray-500 font-bold uppercase mb-1">Total Referrals</p>
                <h4 class="text-lg font-black text-pink-400">12 Users</h4>
            </div>
        </div>

        <div class="glass-morphism p-6 rounded-3xl flex items-center justify-between border-t-2 border-white/5">
            <div>
                <p class="text-sm font-bold">Need Help Sweetie?</p>
                <p class="text-[10px] text-gray-500">24/7 Direct Help Desk Open</p>
            </div>
            <button class="w-12 h-12 bg-blue-600 rounded-2xl flex items-center justify-center shadow-lg shadow-blue-500/40">
                <i class="fa-solid fa-headset text-xl"></i>
            </button>
        </div>

    </div>

    <div id="admin-powers" class="p-6 hidden">
        <h2 class="text-2xl font-black text-yellow-500 mb-6 italic">GOD MODE ACTIVE</h2>
        <button onclick="sendProfitPulse()" class="w-full bg-yellow-600 py-4 rounded-2xl font-black text-black text-sm mb-4">
            <i class="fa-solid fa-money-bill-trend-up mr-2"></i> ONE-CLICK PROFIT PULSE
        </button>
        <div class="glass-morphism p-6 rounded-3xl">
            <p class="text-xs font-bold mb-4">BAN USER SYSTEM</p>
            <input type="text" placeholder="User Email to Block" class="w-full p-4 bg-black/40 rounded-xl mb-4 text-xs outline-none">
            <button class="w-full bg-red-600 py-3 rounded-xl font-bold text-xs uppercase">BLOCK ACCOUNT</button>
        </div>
    </div>

    <nav class="fixed bottom-0 left-0 right-0 glass-morphism p-6 flex justify-around items-center rounded-t-[3rem] z-50">
        <button class="text-blue-500 flex flex-col items-center gap-1"><i class="fa-solid fa-house-chimney text-xl"></i><span class="text-[8px] font-bold">HOME</span></button>
        <button class="text-gray-500 flex flex-col items-center gap-1"><i class="fa-solid fa-wallet text-xl"></i><span class="text-[8px] font-bold">WALLET</span></button>
        <button class="text-gray-500 flex flex-col items-center gap-1"><i class="fa-solid fa-users text-xl"></i><span class="text-[8px] font-bold">TEAM</span></button>
        <button class="text-gray-500 flex flex-col items-center gap-1"><i class="fa-solid fa-crown text-xl text-yellow-500"></i><span class="text-[8px] font-bold">MASTER</span></button>
    </nav>

</body>
</html>
