<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PAKGOLD PWA FINAL</title>

<link rel="manifest" href="manifest.json">

<style>
body{
margin:0;
font-family:Arial;
background:#0b1220;
color:white;
}

.center{
height:100vh;
display:flex;
justify-content:center;
align-items:center;
flex-direction:column;
}

input,button{
padding:12px;
margin:6px;
width:220px;
border:none;
border-radius:10px;
}

button{
background:#fbbf24;
font-weight:bold;
cursor:pointer;
}

.app{
display:none;
padding:15px;
}

.card{
background:rgba(255,255,255,0.06);
padding:15px;
border-radius:12px;
margin:10px 0;
}

.nav{
display:flex;
gap:10px;
flex-wrap:wrap;
}

.nav button{
flex:1;
background:#111827;
color:white;
}

.nav button:hover{
background:#fbbf24;
color:black;
}

.badge{
color:#fbbf24;
font-weight:bold;
}

</style>
</head>

<body>

<!-- LOGIN -->
<div class="center" id="loginBox">
<h2 class="badge">PAKGOLD LOGIN</h2>
<input id="user" placeholder="Username">
<input id="pass" type="password" placeholder="Password">
<button onclick="login()">Login / Register</button>
</div>

<!-- APP -->
<div class="app" id="app">

<h2>PAKGOLD DASHBOARD 💎</h2>
<p>Welcome <span id="uname"></span></p>

<div class="card">
Balance: <span id="bal">0</span><br>
Nodes: <span id="nodes">0</span>
</div>

<div class="nav">
<button onclick="show('home')">Home</button>
<button onclick="show('deposit')">Deposit</button>
<button onclick="show('withdraw')">Withdraw</button>
<button onclick="show('requests')">Requests</button>
<button onclick="logout()">Logout</button>
</div>

<div id="home" class="card">
Premium Dashboard UI 🚀
</div>

<div id="deposit" class="card" style="display:none">
<h3>Deposit</h3>
<input id="damount" placeholder="Amount">
<button onclick="deposit()">Send Request</button>
</div>

<div id="withdraw" class="card" style="display:none">
<h3>Withdraw</h3>
<input id="wamount" placeholder="Amount">
<button onclick="withdraw()">Send Request</button>
</div>

<div id="requests" class="card" style="display:none">
<h3>Your Requests</h3>
<p>All deposit/withdraw requests go to admin panel</p>
</div>

</div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
import { getDatabase, ref, set, get, child, push } 
from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

const firebaseConfig = {
apiKey: "YOUR_KEY",
authDomain: "YOUR_DOMAIN",
databaseURL: "YOUR_DB",
projectId: "YOUR_PROJECT"
};

const app = initializeApp(firebaseConfig);
const db = getDatabase(app);

let currentUser = "";

/* LOGIN */
window.login = async () => {
let u = user.value;
let p = pass.value;

let snap = await get(child(ref(db), "users/" + u));

if (snap.exists()) {
let d = snap.val();
if (d.password !== p) return alert("Wrong Password");

currentUser = u;
load(d);

} else {
set(ref(db, "users/" + u), {
password: p,
balance: 0,
nodes: 0
});

currentUser = u;
load({ balance: 0, nodes: 0 });
}
};

/* LOAD USER */
function load(d){
loginBox.style.display = "none";
app.style.display = "block";

uname.innerText = currentUser;
bal.innerText = d.balance;
nodes.innerText = d.nodes;
}

/* DEPOSIT */
window.deposit = () => {
push(ref(db, "requests/deposit"), {
user: currentUser,
amount: damount.value,
status: "pending"
});
alert("Deposit Request Sent");
};

/* WITHDRAW */
window.withdraw = () => {
push(ref(db, "requests/withdraw"), {
user: currentUser,
amount: wamount.value,
status: "pending"
});
alert("Withdraw Request Sent");
};

/* NAV */
window.show = (id) => {
["home","deposit","withdraw","requests"].forEach(x=>{
document.getElementById(x).style.display="none";
});
document.getElementById(id).style.display="block";
};

/* LOGOUT */
window.logout = () => location.reload();

</script>

</body>
</html>
