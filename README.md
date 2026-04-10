<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PAK GOLD | Ultimate</title>

<script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.10.0/firebase-database-compat.js"></script>

<style>
body{margin:0;font-family:Arial;background:#f5f7fa;color:#111}
.page{display:none;padding:20px}
.page.active{display:block}
.card{background:#fff;padding:15px;margin:10px 0;border-radius:12px;box-shadow:0 2px 10px rgba(0,0,0,0.08)}
button{padding:10px;border:none;border-radius:8px;background:#d4af37;color:#fff;font-weight:bold}
input,select{width:100%;padding:10px;margin:5px 0;border:1px solid #ddd;border-radius:8px}
.nav{position:fixed;bottom:0;left:0;right:0;background:#fff;display:flex;justify-content:space-around;padding:10px}
.nav div{font-size:12px;cursor:pointer}
</style>
</head>

<body onload="checkSession()">

<!-- LOGIN -->
<div id="auth" class="page active">
<h2>PAK GOLD LOGIN</h2>
<input id="ph" placeholder="Phone">
<input id="ps" type="password" placeholder="Password">
<button onclick="login()">Login / Signup</button>
</div>

<!-- DASHBOARD -->
<div id="dash" class="page">
<h3>Dashboard</h3>

<div class="card">
<h4>Wallet: Rs <span id="wallet">0</span></h4>
<p>Profit: <span id="profit">0</span></p>
<p>Team: <span id="team">0</span></p>
<button onclick="dailyBonus()">Daily Bonus</button>
</div>

<div class="card">
<h4>Referral Code</h4>
<p id="ref"></p>
</div>

<h4>Transactions</h4>
<div id="tx"></div>

<h4>Plans</h4>
<div id="plans"></div>
</div>

<!-- ADMIN -->
<div id="admin" class="page">
<h3>ADMIN PANEL</h3>
<button onclick="stats()">Show Stats</button>
<div id="users"></div>
<div id="req"></div>
</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dash')">Home</div>
<div onclick="nav('admin')">Admin</div>
<div onclick="logout()">Logout</div>
</div>

<script>
const fbConfig = {
apiKey:"demo",
databaseURL:"https://dark-web-9-default-rtdb.firebaseio.com"
};

firebase.initializeApp(fbConfig);
const db = firebase.database();

let user=null;
let userData=null;

/* NAV */
function nav(p){
document.querySelectorAll('.page').forEach(e=>e.classList.remove('active'));
document.getElementById(p).classList.add('active');
if(p==='dash') load();
if(p==='admin') loadAdmin();
}

/* SESSION */
function checkSession(){
user=localStorage.getItem("u");
if(user) nav('dash');
renderPlans();
}

/* LOGIN */
function login(){
let ph=document.getElementById('ph').value;
let ps=document.getElementById('ps').value;

db.ref('users/'+ph).once('value',s=>{
if(!s.exists()){
db.ref('users/'+ph).set({
phone:ph,
pass:ps,
wallet:0,
profit:0,
speed:0,
ref:"PG"+Math.floor(Math.random()*99999),
refBy:""
});
}
localStorage.setItem("u",ph);
location.reload();
});
}

/* LOAD USER */
function load(){
db.ref('users/'+user).on('value',s=>{
userData=s.val();

document.getElementById('wallet').innerText=userData.wallet;
document.getElementById('profit').innerText=userData.profit;
document.getElementById('ref').innerText=userData.ref;
});

loadTx();
loadTeam();
}

/* TRANSACTIONS */
function loadTx(){
db.ref('requests').orderByChild('user').equalTo(user).on('value',s=>{
let h="";
s.forEach(c=>{
h+=`<div class="card">${c.val().type} - ${c.val().raw} (${c.val().status})</div>`;
});
document.getElementById('tx').innerHTML=h;
});
}

/* TEAM */
function loadTeam(){
db.ref('users').orderByChild('refBy').equalTo(userData?.ref).once('value',s=>{
let c=0;
s.forEach(()=>c++);
document.getElementById('team').innerText=c;
});
}

/* DAILY BONUS */
function dailyBonus(){
let t=new Date().toDateString();
if(localStorage.getItem("d")==t)return alert("Already Claimed");

db.ref('users/'+user).once('value',s=>{
db.ref('users/'+user).update({
wallet:s.val().wallet+50
});
});

localStorage.setItem("d",t);
}

/* PLANS */
function renderPlans(){
let d=document.getElementById('plans');
for(let i=1;i<=5;i++){
let cost=200*i;
d.innerHTML+=`<div class="card">
Node ${i} - Rs ${cost}
<button onclick="buy(${cost})">Buy</button>
</div>`;
}
}

/* BUY */
function buy(c){
db.ref('users/'+user).once('value',s=>{
if(s.val().wallet<c)return alert("Low balance");

db.ref('users/'+user).update({
wallet:s.val().wallet-c,
speed:(s.val().speed||0)+1
});
});
}

/* REQUESTS */
function deposit(){
db.ref('requests').push({user,type:"Deposit",raw:100,status:"Pending"});
}

/* ADMIN */
function loadAdmin(){
db.ref('users').once('value',s=>{
let h="";
s.forEach(c=>{
h+=`<div class="card">
${c.val().phone} | ${c.val().wallet}
<button onclick="add('${c.key}')">+100</button>
</div>`;
});
document.getElementById('users').innerHTML=h;
});

db.ref('requests').once('value',s=>{
let h="";
s.forEach(c=>{
h+=`<div class="card">
${c.val().type} - ${c.val().user}
<button onclick="approve('${c.key}','${c.val().user}')">OK</button>
</div>`;
});
document.getElementById('req').innerHTML=h;
});
}

function add(u){
db.ref('users/'+u).once('value',s=>{
db.ref('users/'+u).update({wallet:s.val().wallet+100});
});
}

function approve(k,u){
db.ref('users/'+u).once('value',s=>{
db.ref('users/'+u).update({wallet:s.val().wallet+100});
});
db.ref('requests/'+k).update({status:"Approved"});
}

/* STATS */
function stats(){
db.ref('users').once('value',s=>{
let t=0,c=0;
s.forEach(x=>{t+=x.val().wallet;c++;});
alert("Users:"+c+" Total:"+t);
});
}

/* LOGOUT */
function logout(){
localStorage.clear();
location.reload();
}
</script>

</body>
</html>
