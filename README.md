<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PakGold | Premium Asset Management</title>
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>
    <!-- UI Libraries -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <style>
        :root { 
            --primary: #c9a050; 
            --dark: #0a0f18; 
            --surface: #161d27;
            --text-muted: #94a3b8;
            --success: #10b981;
            --error: #ef4444;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Inter', sans-serif; }
        body { background: var(--dark); color: white; overflow-x: hidden; }

        /* Smooth Page Transitions */
        .page { display: none; min-height: 100vh; padding-bottom: 100px; animation: fadeIn 0.4s ease-out; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* Side Menu Drawer */
        .drawer { position: fixed; left: -100%; top: 0; width: 280px; height: 100%; background: var(--surface); z-index: 10001; transition: 0.4s cubic-bezier(0.4, 0, 0.2, 1); padding: 30px 20px; border-right: 1px solid rgba(255,255,255,0.1); }
        .drawer.open { left: 0; }
        .drawer-item { padding: 15px; margin-bottom: 5px; border-radius: 12px; display: flex; align-items: center; gap: 12px; cursor: pointer; transition: 0.2s; color: var(--text-muted); }
        .drawer-item:hover { background: rgba(255,255,255,0.05); color: white; }
        .overlay { position: fixed; display: none; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.7); z-index: 10000; backdrop-filter: blur(4px); }

        /* Premium Dashboard UI */
        .header { padding: 25px; display: flex; justify-content: space-between; align-items: center; background: rgba(22, 29, 39, 0.8); backdrop-filter: blur(10px); position: sticky; top: 0; z-index: 100; }
        .balance-card { background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%); margin: 20px; padding: 30px; border-radius: 24px; border: 1px solid rgba(201, 160, 80, 0.3); position: relative; overflow: hidden; }
        .balance-card::after { content: ''; position: absolute; top: -50%; right: -50%; width: 200px; height: 200px; background: var(--primary); filter: blur(100px); opacity: 0.1; }
        .crypto-val { font-size: 2.2rem; font-weight: 800; color: var(--primary); margin: 10px 0; }

        /* Grid System for 15+5 Plans */
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 20px; }
        .node-card { background: var(--surface); padding: 20px; border-radius: 20px; border: 1px solid rgba(255,255,255,0.05); transition: 0.3s; }
        .node-card:active { transform: scale(0.95); }
        .vip-node { grid-column: span 2; border: 1px solid var(--primary); background: linear-gradient(to right, #161d27, #1e2530); }

        /* Inputs & Action Buttons */
        .input-group { margin: 20px; padding: 25px; background: var(--surface); border-radius: 20px; }
        input { width: 100%; padding: 15px; margin-bottom: 15px; background: #0a0f18; border: 1px solid #2d3748; border-radius: 12px; color: white; outline: none; }
        .btn-main { background: var(--primary); color: #000; border: none; padding: 16px; width: 100%; border-radius: 12px; font-weight: 700; cursor: pointer; text-transform: uppercase; letter-spacing: 1px; }
        
        /* Navigation Bar */
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: var(--surface); display: flex; justify-content: space-around; padding: 15px; border-top: 1px solid rgba(255,255,255,0.05); z-index: 999; }
        .nav-item { text-align: center; color: var(--text-muted); font-size: 0.75rem; text-decoration: none; cursor: pointer; }
        .nav-item.active { color: var(--primary); }

        /* Admin Terminal */
        .admin-terminal { background: #000; color: #00ff00; font-family: 'Courier New', monospace; padding: 20px; font-size: 0.85rem; }
        .admin-btn { padding: 8px 12px; border-radius: 6px; border: none; cursor: pointer; margin-right: 5px; }
        .btn-approve { background: var(--success); color: white; }
        .btn-reject { background: var(--error); color: white; }
    </style>
</head>
<body onload="initializeCore()">

    <!-- Side Menu -->
    <div class="overlay" id="overlay" onclick="toggleDrawer()"></div>
    <div class="drawer" id="drawer">
        <h3 style="color:var(--primary); margin-bottom:30px;">PAK GOLD GLOBAL</h3>
        <div class="drawer-item" onclick="navigate('home')"><i class="fa fa-th-large"></i> Dashboard</div>
        <div class="drawer-item" onclick="navigate('team')"><i class="fa fa-users"></i> My Team</div>
        <div class="drawer-item" onclick="navigate('promo')"><i class="fa fa-ticket"></i> Promo Center</div>
        <div class="drawer-item" onclick="showLegal()"><i class="fa fa-file-contract"></i> Terms & Policy</div>
        <div class="drawer-item" style="color:var(--error); margin-top:40px;" onclick="logout()">
            <i class="fa fa-power-off"></i> Secure Logout
        </div>
    </div>

    <!-- Auth Page -->
    <section id="login" class="page active">
        <div style="height:100vh; display:flex; flex-direction:column; justify-content:center; padding:40px;">
            <div style="text-align:center; margin-bottom:40px;">
                <h1 style="font-weight:800; font-size:2.5rem; letter-spacing:-1px;">PAK<span style="color:var(--primary)">GOLD</span></h1>
                <p style="color:var(--text-muted)">Secure Investment Portal</p>
            </div>
            <input type="number" id="authPhone" placeholder="Mobile Number">
            <input type="text" id="authRef" placeholder="Referral ID (Optional)">
            <input type="password" id="adminKey" placeholder="Admin Terminal Key">
            <button class="btn-main" onclick="handleAuth()">Access System</button>
        </div>
    </section>

    <!-- Home Dashboard -->
    <section id="home" class="page">
        <div class="header">
            <div onclick="toggleDrawer()"><i class="fa fa-bars-staggered" style="font-size:1.4rem;"></i></div>
            <div id="u-id" style="font-weight:600; font-size:0.85rem; background:rgba(255,255,255,0.05); padding:5px 12px; border-radius:50px;">ID: ---</div>
        </div>

        <div class="balance-card">
            <p style="font-size:0.75rem; color:var(--text-muted); font-weight:600;">CLOUD MINING BALANCE</p>
            <div class="crypto-val" id="u-profit">0.0000</div>
            <div style="display:flex; justify-content:space-between; align-items:center; margin-top:20px; border-top:1px solid rgba(255,255,255,0.1); padding-top:15px;">
                <div><small style="color:var(--text-muted)">WALLET</small><p id="u-wallet" style="font-weight:700;">Rs 0</p></div>
                <div style="text-align:right;"><small style="color:var(--text-muted)">HASH RATE</small><p id="u-speed" style="color:var(--success); font-weight:700;">0.00/s</p></div>
            </div>
        </div>

        <h4 style="padding:0 25px; margin-bottom:15px;">STANDARD NODES</h4>
        <div class="grid" id="std-nodes"></div>

        <h4 style="padding:0 25px; margin:25px 0 15px;">ENTERPRISE SERVERS (VIP)</h4>
        <div class="grid" id="vip-nodes"></div>
    </section>

    <!-- Team Page -->
    <section id="team" class="page">
        <div style="padding:25px;">
            <h3>Networking Hub</h3>
            <div class="input-group" style="margin:20px 0;">
                <p style="font-size:0.8rem; color:var(--text-muted);">Share your link to earn 10% commission on every deposit made by your team.</p>
                <div id="ref-link" style="background:#0a0f18; padding:15px; border-radius:10px; font-size:0.7rem; margin:15px 0; border:1px dashed var(--primary); word-break: break-all;">---</div>
                <button class="btn-main" onclick="copyRef()">Copy Referral Link</button>
            </div>
            <h4>Active Team Members</h4>
            <div id="team-list" style="margin-top:15px;"></div>
        </div>
    </section>

    <!-- Admin Page -->
    <section id="admin" class="page admin-terminal">
        <h3>[ TERMINAL ACCESS GRANTED ]</h3>
        <hr style="border:1px solid #111; margin:15px 0;">
        <div id="admin-summary">Loading stats...</div>
        
        <h4 style="margin:20px 0;">PENDING VERIFICATIONS</h4>
        <div id="admin-queue"></div>

        <h4 style="margin:20px 0;">PROMO MANAGEMENT</h4>
        <input type="text" id="newPromo" placeholder="Promo Code Name" style="background:#111; border:1px solid #333; color:#0f0;">
        <button class="btn-main" onclick="createPromo()">GENERATE PROMO</button>
    </section>

    <!-- Deposit Page -->
    <section id="deposit" class="page">
        <div class="input-group">
            <h3>Add Capital</h3>
            <div style="background:rgba(201, 160, 80, 0.1); padding:15px; border-radius:10px; margin:15px 0; border:1px solid var(--primary); font-size:0.85rem;">
                Account: JazzCash / EasyPaisa<br>
                Number: 0300-XXXXXXX<br>
                Name: Official PakGold Merchant
            </div>
            <input type="number" id="depAmt" placeholder="Amount (PKR)">
            <input type="text" id="depTid" placeholder="Transaction ID (TID)">
            <button class="btn-main" onclick="submitDeposit()">Submit For Verification</button>
        </div>
    </section>

    <nav class="nav-bar" id="bottomNav" style="display:none;">
        <div class="nav-item active" onclick="navigate('home')"><i class="fa fa-home"></i><br>Home</div>
        <div class="nav-item" onclick="navigate('deposit')"><i class="fa fa-wallet"></i><br>Deposit</div>
        <div class="nav-item" onclick="navigate('team')"><i class="fa fa-network-wired"></i><br>Team</div>
        <div class="nav-item" onclick="toggleDrawer()"><i class="fa fa-bars"></i><br>Menu</div>
    </nav>

    <script>
        // --- SECURE SYSTEM CONFIG ---
        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();
        let sessionPhone = localStorage.getItem('pg_session');

        function initializeCore() {
            if(sessionPhone) {
                navigate('home');
                startDataSync();
            }
            generatePlanGrid();
        }

        function handleAuth() {
            let p = document.getElementById('authPhone').value;
            let r = document.getElementById('authRef').value;
            let k = document.getElementById('adminKey').value;

            if(k === "pakgold786") return navigate('admin'), loadAdminTerminal();
            if(p.length < 10) return alert("System Error: Invalid Identifier.");

            sessionPhone = p;
            localStorage.setItem('pg_session', p);
            db.ref('users/' + p).once('value', s => {
                if(!s.exists()) {
                    db.ref('users/' + p).set({
                        wallet: 0, profit: 0, speed: 0, plan: 'Basic Node',
                        u_id: 'PG' + Math.floor(10000 + Math.random() * 90000),
                        inviter: r || 'System'
                    });
                    if(r) db.ref('users/' + r + '/team').push(p);
                }
                location.reload();
            });
        }

        function startDataSync() {
            db.ref('users/' + sessionPhone).on('value', s => {
                let d = s.val();
                document.getElementById('u-wallet').innerText = "Rs " + d.wallet;
                document.getElementById('u-profit').innerText = parseFloat(d.profit).toFixed(4);
                document.getElementById('u-id').innerText = "ID: " + d.u_id;
                document.getElementById('u-speed').innerText = d.speed + "/s";
                document.getElementById('ref-link').innerText = window.location.href.split('?')[0] + "?ref=" + sessionPhone;
            });

            // Mining Execution
            setInterval(() => {
                db.ref('users/' + sessionPhone).once('value', s => {
                    let d = s.val();
                    if(d.speed > 0) {
                        db.ref('users/' + sessionPhone).update({
                            profit: (parseFloat(d.profit) + parseFloat(d.speed)).toFixed(4)
                        });
                    }
                });
            }, 1000);

            // Team Sync
            db.ref('users/' + sessionPhone + '/team').on('value', s => {
                let list = document.getElementById('team-list'); list.innerHTML = "";
                s.forEach(child => {
                    list.innerHTML += `<div style="background:var(--surface); padding:15px; border-radius:12px; margin-bottom:10px; border-left:4px solid var(--primary);">User Account: ${child.val()}</div>`;
                });
            });
        }

        function generatePlanGrid() {
            let std = document.getElementById('std-nodes');
            for(let i=1; i<=15; i++) {
                std.innerHTML += `<div class="node-card" onclick="purchaseNode(${i*800}, ${0.0005 * i})">
                    <p style="font-size:0.65rem; color:var(--text-muted);">NODE X-${i}</p>
                    <h3 style="margin:5px 0;">Rs ${i*800}</h3>
                    <small style="color:var(--success)">+${(0.0005*i*3600).toFixed(2)}/hr</small>
                </div>`;
            }

            let vip = document.getElementById('vip-nodes');
            const servers = [
                {n: 'Alpha Cluster', p: 50000, s: 0.1}, {n: 'Sigma Prime', p: 100000, s: 0.25},
                {n: 'Omega Grid', p: 250000, s: 0.7}, {n: 'Global Mainframe', p: 500000, s: 1.5},
                {n: 'Infinity Core', p: 1000000, s: 4.0}
            ];
            servers.forEach(srv => {
                vip.innerHTML += `<div class="node-card vip-node" onclick="purchaseNode(${srv.p}, ${srv.s})">
                    <div style="display:flex; justify-content:space-between; align-items:center;">
                        <div><h4>${srv.n}</h4><p style="color:var(--primary); font-weight:800;">Rs ${srv.p}</p></div>
                        <div style="text-align:right;"><small>VIP Speed</small><br><b>${srv.s}/s</b></div>
                    </div>
                </div>`;
            });
        }

        function purchaseNode(p, s) {
            db.ref('users/' + sessionPhone).once('value', d => {
                let u = d.val();
                if(u.wallet < p) return alert("Insufficient Capital.");
                db.ref('users/' + sessionPhone).update({ wallet: u.wallet - p, speed: s });
                alert("Node Deployment Successful.");
            });
        }

        function submitDeposit() {
            let a = document.getElementById('depAmt').value;
            let t = document.getElementById('depTid').value;
            if(!a || !t) return alert("Fill all data points.");
            db.ref('v_requests/').push({ user: sessionPhone, amt: a, tid: t, status: 'pending', type: 'deposit' });
            alert("Application Logged. Waiting for verification.");
        }

        // --- ADMIN GOD-EYE LOGIC ---
        function loadAdminTerminal() {
            db.ref('v_requests/').on('value', s => {
                let q = document.getElementById('admin-queue'); q.innerHTML = "";
                s.forEach(child => {
                    let r = child.val();
                    if(r.status === 'pending') {
                        q.innerHTML += `<div style="border:1px solid #333; padding:15px; margin-bottom:10px;">
                            ID: ${r.user} | Amount: ${r.amt} | TID: ${r.tid} <br><br>
                            <button class="admin-btn btn-approve" onclick="terminalApprove('${child.key}', '${r.user}', ${r.amt})">VERIFY</button>
                            <button class="admin-btn btn-reject" onclick="terminalReject('${child.key}')">DISMISS</button>
                        </div>`;
                    }
                });
            });
        }

        function terminalApprove(k, ph, amt) {
            db.ref('users/' + ph).once('value', s => {
                let d = s.val();
                db.ref('users/' + ph).update({ wallet: d.wallet + parseInt(amt) });
                
                // Automatic Referral Commission
                if(d.inviter !== 'System') {
                    db.ref('users/' + d.inviter).once('value', inv => {
                        db.ref('users/' + d.inviter).update({ wallet: inv.val().wallet + (amt * 0.1) });
                    });
                }
                db.ref('v_requests/' + k).remove();
            });
        }

        function terminalReject(k) { db.ref('v_requests/' + k).remove(); }
        function createPromo() { let c = document.getElementById('newPromo').value; db.ref('p_codes/' + c).set(true); alert("Promo Logic Updated."); }

        // --- UI UTILS ---
        function toggleDrawer() { 
            document.getElementById('drawer').classList.toggle('open');
            let ov = document.getElementById('overlay');
            ov.style.display = (ov.style.display === 'block') ? 'none' : 'block';
        }
        function navigate(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('bottomNav').style.display = (id === 'login' || id === 'admin') ? 'none' : 'flex';
            if(document.getElementById('drawer').classList.contains('open')) toggleDrawer();
        }
        function copyRef() { navigator.clipboard.writeText(document.getElementById('ref-link').innerText); alert("Referral Linked Copied."); }
        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
