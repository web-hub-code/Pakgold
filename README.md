<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PAK GOLD v8 Ultra Pro</title>

<script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>

<style>
body{margin:0;font-family:Arial;background:#f5f6fb}
.page{display:none;padding-bottom:80px}
.page.active{display:block}

.card{
background:#fff;margin:12px;padding:15px;border-radius:15px;
box-shadow:0 2px 10px rgba(0,0,0,0.08)
}

.btn{
width:100%;padding:12px;border:none;border-radius:10px;
background:#d4af37;color:#fff;font-weight:bold
}

.input{
width:100%;padding:10px;margin:5px 0;border:1px solid #ddd;border-radius:10px
}

.nav{
position:fixed;bottom:0;width:100%;display:flex;background:#fff;border-top:1px solid #ddd
}
.nav div{flex:1;text-align:center;padding:10px;font-size:12px}
</style>
</head>

<body onload="init()">

<!-- LOGIN -->
<div id="login" class="page active">
<div class="card">
<h2>Login</h2>
<input class="input" id="ph" placeholder="Phone">
<input class="input" id="pw" type="password" placeholder="Password">
<button class="btn" onclick="login()">Login</button>
</div>
</div>

<!-- HOME -->
<div id="home" class="page">
<div class="card">
<h3>User: <span id="u"></span></h3>
<h2>Balance: <span id="bal">0</span></h2>
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
<h3>Deposit</h3>
<input class="input" id="damt">
<input class="input" id="dtid">
<button class="btn" onclick="req('Deposit')">Submit</button>
</div>
</div>

<!-- WITHDRAW -->
<div id="withdraw" class="page">
<div class="card">
<h3>Withdraw</h3>
<input class="input" id="wamt">
<input class="input" id="wacc">
<button class="btn" onclick="req('Withdraw')">Submit</button>
</div>
</div>

<!-- HISTORY -->
<div id="history" class="page">
<div class="card">
<h2>Transaction History</h2>
<div id="hist"></div>
</div>
</div>

<!-- ADMIN -->
<div id="admin" class="page">
<div class="card">
<h2>Admin Panel</h2>
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
const ADMIN_KEY="admin123";

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

if(t==="Deposit"){
obj.amount=damt.value;
obj.tid=dtid.value;
}

if(t==="Withdraw"){
obj.amount=wamt.value;
obj.acc=wacc.value;
}

db.ref("requests").push(obj);
log(t+" Sent");
}

function log(txt){
db.ref("history/"+user).push({text:txt,time:Date.now()});
historyLoad();
}

function historyLoad(){
db.ref("history/"+user).on("value",s=>{
let h="";
s.forEach(x=>{
h+=`<div class="card">${x.val().text}</div>`;
});
hist.innerHTML=h || "No History";
});
}

function nav(p){
show(p);
if(p==="admin")admin();
if(p==="history")historyLoad();
}

function admin(){
let isAdmin=localStorage.getItem("admin")==="true";
if(!isAdmin){
alert("Not Admin");
show("home");
return;
}

db.ref("requests").on("value",s=>{
let h="";
s.forEach(x=>{
let d=x.val();
if(d.status==="Pending"){
h+=`
<div class="card">
<b>${d.type}</b><br>
User:${d.user}<br>
Amt:${d.amount}<br>

<button onclick="ap('${x.key}')">Approve</button>
<button onclick="re('${x.key}')">Reject</button>
</div>`;
}
});
reqs.innerHTML=h;
});
}

function ap(k){
db.ref("requests/"+k).update({status:"Approved"});
}

function re(k){
db.ref("requests/"+k).update({status:"Rejected"});
}
</script>

</body>
</html>
