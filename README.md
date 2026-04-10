<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PAK GOLD | Ultra All-In-One</title>

<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">

<script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>

<style>
body{
margin:0;
font-family:Inter;
background:radial-gradient(circle at top,#141a2e,#0b0f1a);
color:white;
}

/* HEADER */
.header{
display:flex;
justify-content:space-between;
padding:18px;
font-weight:800;
}
.logo span{color:#d4af37}

/* CARDS */
.card{
margin:12px;
padding:16px;
border-radius:20px;
background:rgba(255,255,255,0.06);
backdrop-filter:blur(15px);
border:1px solid rgba(255,255,255,0.1);
animation:fade .4s ease;
}

@keyframes fade{
from{opacity:0;transform:translateY(15px)}
to{opacity:1;transform:translateY(0)}
}

/* BUTTON */
.btn{
width:100%;
padding:14px;
border:none;
border-radius:14px;
background:linear-gradient(135deg,#d4af37,#b8860b);
font-weight:800;
}

/* INPUT */
.input{
width:100%;
padding:12px;
margin:6px 0;
border-radius:12px;
border:none;
background:rgba(255,255,255,0.08);
color:white;
}

/* NAV */
.nav{
position:fixed;
bottom:0;
width:100%;
display:flex;
background:rgba(0,0,0,0.6);
backdrop-filter:blur(10px);
}
.nav div{
flex:1;
text-align:center;
padding:10px;
font-size:12px;
}

/* PAGES */
.page{display:none;padding-bottom:80px}
.page.active{display:block}
</style>
</head>

<body onload="init()">

<!-- LOGIN -->
<div id="login" class="page active">
<div class="card">
<h2>PAK GOLD LOGIN</h2>
<input class="input" id="ph" placeholder="Phone">
<input class="input" id="pw" type="password" placeholder="Password">
<button class="btn" onclick="login()">Login</button>
</div>
</div>

<!-- HOME -->
<div id="home" class="page">
<div class="header">
<div class="logo">PAK <span>GOLD</span></div>
</div>

<div class="card">
<h3>Welcome <span id="u"></span></h3>
<h1>Balance: <span id="bal">0</span></h1>
</div>

<div class="card">
<button class="btn" onclick="nav('deposit')">Deposit</button><br><br>
<button class="btn" onclick="nav('withdraw')">Withdraw</button><br><br>
<button class="btn" onclick="nav('history')">History</button>
</div>

<div class="card">
<button class="btn" onclick="earn()">+ Demo Earn</button>
</div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page">
<div class="card">
<h3>Deposit Request</h3>
<input class="input" id="damt">
<input class="input" id="dtid">
<button class="btn" onclick="req('Deposit')">Submit</button>
</div>
</div>

<!-- WITHDRAW -->
<div id="withdraw" class="page">
<div class="card">
<h3>Withdraw Request</h3>
<input class="input" id="wamt">
<input class="input" id="wacc">
<button class="btn" onclick="req('Withdraw')">Submit</button>
</div>
</div>

<!-- HISTORY -->
<div id="history" class="page">
<div class="card">
<h2>History</h2>
<div id="hist"></div>
</div>
</div>

<!-- ADMIN -->
<div id="admin" class="page">
<div class="card">
<h2>ADMIN PANEL</h2>
<div id="reqs"></div>
</div>
</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('home')">Home</div>
<div onclick="nav('deposit')">Depo</div>
<div onclick="nav('withdraw')">Wth</div>
<div onclick="nav('admin')">Admin</div>
</div>

<script>
firebase.initializeApp({
apiKey:"demo",
databaseURL:"https://dark-web-9-default-rtdb.firebaseio.com"
});

const db=firebase.database();
let user=null;

function init(){
user=localStorage.getItem("u");
if(user){show("home");load();historyLoad();}
else show("login");
}

function show(p){
document.querySelectorAll(".page").forEach(x=>x.classList.remove("active"));
document.getElementById(p).classList.add("active");
}

function login(){
user=ph.value;
localStorage.setItem("u",user);
db.ref("users/"+user).update({bal:0});
init();
}

function load(){
db.ref("users/"+user).on("value",s=>{
let d=s.val()||{bal:0};
u.innerText=user;
bal.innerText=d.bal;
});
}

function earn(){
db.ref("users/"+user).once("value",s=>{
let d=s.val()||{bal:0};
db.ref("users/"+user).update({bal:d.bal+10});
log("Earned +10");
});
}

function req(t){
let obj={type:t,user:user,status:"Pending",time:Date.now()};
if(t=="Deposit"){obj.amount=damt.value;obj.tid=dtid.value;}
if(t=="Withdraw"){obj.amount=wamt.value;obj.acc=wacc.value;}
db.ref("req").push(obj);
log(t+" Sent");
}

function log(t){
db.ref("hist/"+user).push({t,time:Date.now()});
historyLoad();
}

function historyLoad(){
db.ref("hist/"+user).on("value",s=>{
let h="";
s.forEach(x=>{
h+=`<div class="card">${x.val().t}</div>`;
});
hist.innerHTML=h;
});
}

function nav(p){
show(p);
if(p=="admin")admin();
}

function admin(){
if(localStorage.getItem("admin")!=="true"){
alert("No Access");
show("home");
return;
}

db.ref("req").on("value",s=>{
let h="";
s.forEach(x=>{
let d=x.val();
if(d.status=="Pending"){
h+=`
<div class="card">
<b>${d.type}</b><br>
User:${d.user}<br>
Amt:${d.amount}<br><br>
<button onclick="a('${x.key}')">Approve</button>
<button onclick="r('${x.key}')">Reject</button>
</div>`;
}
});
reqs.innerHTML=h;
});
}

function a(k){db.ref("req/"+k).update({status:"Approved"})}
function r(k){db.ref("req/"+k).update({status:"Rejected"})}
</script>

</body>
</html>
