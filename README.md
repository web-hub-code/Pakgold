<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PAKGOLD ULTRA FINAL</title>

<style>
body{
margin:0;
font-family:Arial;
background:#070b1a;
color:white;
}

.center{
height:100vh;
display:flex;
flex-direction:column;
justify-content:center;
align-items:center;
}

input,button{
padding:12px;
margin:6px;
width:240px;
border:none;
border-radius:10px;
}

button{
background:#fbbf24;
font-weight:bold;
cursor:pointer;
transition:0.3s;
}

button:hover{
transform:scale(1.05);
}

.app{
display:none;
padding:15px;
}

.top{
display:flex;
justify-content:space-between;
align-items:center;
}

.card{
background:rgba(255,255,255,0.06);
padding:15px;
border-radius:12px;
margin:10px 0;
backdrop-filter:blur(10px);
animation:fade 0.4s ease;
}

@keyframes fade{
from{opacity:0;transform:translateY(20px);}
to{opacity:1;transform:translateY(0);}
}

.grid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(150px,1fr));
gap:10px;
}

.box{
background:rgba(255,255,255,0.05);
padding:10px;
border-radius:10px;
text-align:center;
}

.nav{
display:flex;
flex-wrap:wrap;
gap:6px;
margin-top:10px;
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

.admin{
display:none;
position:fixed;
top:10px;
right:10px;
background:#111827;
padding:10px;
border-radius:10px;
width:250px;
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

<div class="top">
<h2 class="badge">PAKGOLD DASHBOARD</h2>
<button onclick="toggleAdmin()">Admin</button>
</div>

<div class="card">
User: <span id="uname"></span><br>
Balance: <span id="bal">0</span><br>
Nodes: <span id="nodes">0</span>
</div>

<div class="grid">
<div class="box">Referral UI</div>
<div class="box">History UI</div>
<div class="box">Profit UI</div>
<div class="box">Status</div>
</div>

<div class="nav">
<button onclick="show('home')">Home</button>
<button onclick="show('deposit')">Deposit</button>
<button onclick="show('withdraw')">Withdraw</button>
<button onclick="logout()">Logout</button>
</div>

<div id="home" class="card">Welcome to PAKGOLD 🚀 Premium PWA Dashboard</div>

<div id="deposit" class="card" style="display:none">
<h3>Deposit Request</h3>
<input id="damount" placeholder="Amount">
<button onclick="deposit()">Send</button>
</div>

<div id="withdraw" class="card" style="display:none">
<h3>Withdraw Request</h3>
<input id="wamount" placeholder="Amount">
<button onclick="withdraw()">Send</button>
</div>

</div>

<!-- ADMIN PANEL -->
<div class="admin" id="adminPanel">
<h3>ADMIN PANEL</h3>
<p>Requests Monitor</p>
<button onclick="alert('Demo: approve system')">Approve</button>
<button onclick="alert('Demo: reject system')">Reject</button>
<button onclick="toggleAdmin()">Close</button>
</div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
import { getDatabase, ref, set, get, child, push } 
from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

const firebaseConfig = {
apiKey:"YOUR_KEY",
authDomain:"YOUR_DOMAIN",
databaseURL:"YOUR_DB",
projectId:"YOUR_PROJECT"
};

const app = initializeApp(firebaseConfig);
const db = getDatabase(app);

let currentUser="";

/* LOGIN */
window.login = async () => {
let u=user.value;
let p=pass.value;

let snap = await get(child(ref(db),"users/"+u));

if(snap.exists()){
let d=snap.val();
if(d.password !== p) return alert("Wrong password");

currentUser=u;
load(d);

}else{
set(ref(db,"users/"+u),{
password:p,
balance:0,
nodes:0
});

currentUser=u;
load({balance:0,nodes:0});
}
};

/* LOAD */
function load(d){
loginBox.style.display="none";
app.style.display="block";

uname.innerText=currentUser;
bal.innerText=d.balance;
nodes.innerText=d.nodes;
}

/* DEPOSIT */
window.deposit=()=>{
push(ref(db,"requests/deposit"),{
user:currentUser,
amount:damount.value,
status:"pending"
});
alert("Deposit Sent");
}

/* WITHDRAW */
window.withdraw=()=>{
push(ref(db,"requests/withdraw"),{
user:currentUser,
amount:wamount.value,
status:"pending"
});
alert("Withdraw Sent");
}

/* NAV */
window.show=(id)=>{
["home","deposit","withdraw"].forEach(i=>{
document.getElementById(i).style.display="none";
});
document.getElementById(id).style.display="block";
}

/* ADMIN */
window.toggleAdmin=()=>{
adminPanel.style.display =
adminPanel.style.display==="block"?"none":"block";
}

/* LOGOUT */
window.logout=()=>location.reload();

</script>

</body>
</html>
