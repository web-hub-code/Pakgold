<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PAK GOLD | Official Elite</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { --gold: #c5a059; --bg: #f8fafc; --text: #1e293b; --sub: #64748b; }
        * { margin:0; padding:0; box-sizing:border-box; font-family: 'Plus Jakarta Sans', sans-serif; }
        body { background: var(--bg); color: var(--text); overflow-x: hidden; }

        /* Premium Animations */
        @keyframes fadeInUp { from { opacity:0; transform: translateY(20px); } to { opacity:1; transform: translateY(0); } }
        .page { display:none; animation: fadeInUp 0.5s ease; padding: 20px 15px 120px; }
        .page.active { display:block; }

        /* Luxury Cards */
        .glass-card { 
            background: #ffffff; border-radius: 28px; padding: 25px; 
            border: 1px solid rgba(197, 160, 89, 0.1); box-shadow: 0 10px 30px rgba(0,0,0,0.03); 
            margin-bottom: 20px;
        }

        .wallet-box { 
            background: linear-gradient(135deg, #1e293b, #334155); color: #fff;
            padding: 35px; border-radius: 35px; box-shadow: 0 20px 40px rgba(0,0,0,0.1); text-align: center;
        }

        .elite-btn { 
            background: linear-gradient(135deg, #c5a059, #b08d48); color: white; border: none; 
            padding: 18px; border-radius: 20px; font-weight: 800; width: 100%; cursor: pointer; margin-top: 10px;
        }

        .input-elite { 
            width: 100%; padding: 18px; border-radius: 18px; border: 2px solid #edf2f7; 
            margin-bottom: 12px; outline: none; font-size: 1rem;
        }

        /* Bottom Nav */
        .nav-bar { 
            position: fixed; bottom: 20px; left: 20px; right: 20px;
            background: rgba(255,255,255,0.9); backdrop-filter: blur(15px);
            display: flex; justify-content: space-around; padding: 15px;
            border-radius: 25px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); z-index: 999; 
        }
        .nav-item { color: #94a3b8; text-align: center; font-size: 0.7rem; font-weight: 700; flex:1; }
        .nav-item.active { color: var(--gold); }
        .nav-item i { font-size: 1.4rem; display: block; margin-bottom: 4px; }
    </style>
</head>
<body onload="checkUser()">

    <section id="dash-pg" class="page">
        <div class="wallet-box">
            <small style="opacity:0.7;">TOTAL ASSETS</small>
            <h1 style="font-size:3rem; margin:10px 0;">Rs <span id="w-val">0.00</span></h1>
            <div style="display:flex; justify-content:space-between; background:rgba(255,255,255,0.1); padding:12px; border-radius:15px;">
                <span>Profit: <b id="p-val" style="color:var(--gold)">0.00</b></span>
                <span>Speed: <b id="s-val">0/h</b></span>
            </div>
        </div>

        <div style="display:grid; grid-template-columns:repeat(4, 1fr); gap:10px; margin-top:20px;">
            <div class="glass-card" style="margin:0; text-align:center; padding:15px;" onclick="claim()"><i class="fa fa-gift"></i><br><small>Bonus</small></div>
            <div class="glass-card" style="margin:0; text-align:center; padding:15px;" onclick="nav('dep-pg')"><i class="fa fa-plus"></i><br><small>Depo</small></div>
            <div class="glass-card" style="margin:0; text-align:center; padding:15px;" onclick="nav('with-pg')"><i class="fa fa-wallet"></i><br><small>Cash</small></div>
            <div class="glass-card" style="margin:0; text-align:center; padding:15px;" onclick="window.open('https://chat.whatsapp.com/EbfTbr66JQLFEmjnxrReE3')"><i class="fab fa-whatsapp"></i><br><small>Help</small></div>
        </div>

        <h3 style="margin:25px 0 15px;">Investment Nodes</h3>
        <div id="plans-container"></div>
    </section>

    <section id="dep-pg" class="page">
        <h2>Add Funds</h2>
        <div class="glass-card">
            <p style="margin-bottom:10px; font-size:0.9rem;">Send to: <b>03705519562</b></p>
            <select id="d-m" class="input-elite"><option>JazzCash</option><option>EasyPaisa</option></select>
            <input type="number" id="d-a" class="input-elite" placeholder="Amount">
            <input type="text" id="d-t" class="input-elite" placeholder="TID Number">
            <button class="elite-btn" onclick="sendD()">Verify Deposit</button>
        </div>
    </section>

    <section id="with-pg" class="page">
        <h2>Withdraw</h2>
        <div class="glass-card">
            <input type="number" id="w-a" class="input-elite" placeholder="Amount">
            <input type="text" id="w-ac" class="input-elite" placeholder="Account Number">
            <button class="elite-btn" onclick="sendW()">Request Payout</button>
        </div>
    </section>

    <section id="admin-pg" class="page">
        <h2 style="color:var(--gold)">Admin Panel</h2>
        <div id="admin-reqs"></div>
    </section>

    <nav class="nav-bar" id="nb" style="display:none;">
        <div class="nav-item active" onclick="nav('dash-pg')"><i class="fa fa-home"></i>Home</div>
        <div class="nav-item" onclick="nav('admin-pg')" id="adm-nav" style="display:none;"><i class="fa fa-shield"></i>Admin</div>
        <div class="nav-item" onclick="logout()"><i class="fa fa-sign-out"></i>Exit</div>
    </nav>

    <script>
        const config = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(config); const db = firebase.database();
        let uid = localStorage.getItem('pg_user') || prompt("Enter Phone:");

        function checkUser() {
            if(uid) {
                localStorage.setItem('pg_user', uid);
                document.getElementById('nb').style.display = 'flex';
                if(uid === "03705519562") { document.getElementById('adm-nav').style.display='block'; nav('admin-pg'); loadA(); }
                else { nav('dash-pg'); sync(); }
            }
            render();
        }

        function sync() {
            db.ref('users/'+uid).on('value', s => {
                const d = s.val() || {wallet:0, profit:0, speed:0};
                document.getElementById('w-val').innerText = d.wallet.toFixed(2);
                document.getElementById('p-val').innerText = d.profit.toFixed(2);
                document.getElementById('s-val').innerText = (d.speed*3600).toFixed(2)+"/h";
            });
            setInterval(() => { db.ref('users/'+uid).once('value', s => { if(s.val() && s.val().speed > 0) db.ref('users/'+uid).update({profit: s.val().profit + s.val().speed}); }); }, 1000);
        }

        function sendD() { db.ref('requests').push({type:'Deposit', user:uid, raw:document.getElementById('d-a').value, tid:document.getElementById('d-t').value, met:document.getElementById('d-m').value, status:'Pending'}); alert("Submitted!"); }
        function sendW() { 
            const a = document.getElementById('w-a').value;
            db.ref('users/'+uid).once('value', s => {
                if(s.val().wallet < a) return alert("Low funds!");
                db.ref('requests').push({type:'Withdraw', user:uid, raw:a, acc:document.getElementById('w-ac').value, status:'Pending'});
                db.ref('users/'+uid).update({wallet: s.val().wallet - a});
                alert("Withdrawal Logged!");
            });
        }

        function loadA() {
            db.ref('requests').on('value', s => {
                let h = ""; s.forEach(c => {
                    if(c.val().status === 'Pending') h += `<div class="glass-card"><b>${c.val().type}</b> | ${c.val().user}<br>Amt: ${c.val().raw}<br>ID/Acc: ${c.val().tid || c.val().acc}<br><button onclick="upd('${c.key}','Approved','${c.val().user}',${c.val().raw},'${c.val().type}')">Approve</button></div>`;
                });
                document.getElementById('admin-reqs').innerHTML = h;
            });
        }

        function upd(k, st, u, a, t) {
            if(st === 'Approved' && t === 'Deposit') db.ref('users/'+u).once('value', s => db.ref('users/'+u).update({wallet: (s.val().wallet||0) + parseFloat(a)}));
            db.ref('requests/'+k).update({status: st});
        }

        function render() {
            let h = ""; for(let i=1; i<=10; i++){ const c = 500*i; h += `<div class="glass-card" style="display:flex; justify-content:space-between;"><b>Level ${i}</b><button class="elite-btn" style="width:auto; padding:10px;" onclick="buy(${c}, ${c*0.15/86400})">Rs ${c}</button></div>`; }
            document.getElementById('plans-container').innerHTML = h;
        }
        function buy(p, s) { db.ref('users/'+uid).once('value', v => { if(v.val().wallet < p) return alert("Low balance!"); db.ref('users/'+uid).update({wallet: v.val().wallet - p, speed: (v.val().speed||0) + s}); alert("Active!"); }); }
        function nav(id) { document.querySelectorAll('.page').forEach(p => p.classList.remove('active')); document.getElementById(id).classList.add('active'); }
        function logout() { localStorage.clear(); location.reload(); }
        function claim() { alert("Bonus Rs 20 Added!"); db.ref('users/'+uid).once('value', s => db.ref('users/'+uid).update({wallet: (s.val().wallet||0) + 20})); }
    </script>
</body>
</html>
