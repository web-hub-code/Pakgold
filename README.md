<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PAKGOLD - Premium SaaS Dashboard</title>

<style>
body{
margin:0;
font-family:Arial;
background:#0b1220;
color:white;
overflow-x:hidden;
}

/* HERO */
.hero{
height:100vh;
display:flex;
flex-direction:column;
justify-content:center;
align-items:center;
text-align:center;
background:radial-gradient(circle at top,#fbbf24,#0b1220);
}

.hero h1{
font-size:55px;
animation:fade 1s ease;
color:#fbbf24;
}

.hero p{
opacity:0.8;
}

.btn{
padding:12px 22px;
margin-top:20px;
background:#fbbf24;
border:none;
border-radius:10px;
cursor:pointer;
font-weight:bold;
}

/* SIDEBAR */
.sidebar{
position:fixed;
left:0;top:0;
width:250px;
height:100%;
background:rgba(255,255,255,0.05);
backdrop-filter:blur(12px);
padding:20px;
display:none;
}

.sidebar h2{
color:#fbbf24;
text-align:center;
}

.sidebar button{
width:100%;
margin:8px 0;
padding:12px;
border:none;
border-radius:10px;
background:#1e293b;
color:white;
cursor:pointer;
transition:0.3s;
}

.sidebar button:hover{
background:#fbbf24;
color:black;
}

/* MAIN */
.main{
margin-left:270px;
padding:20px;
display:none;
}

.card{
background:rgba(255,255,255,0.06);
padding:20px;
border-radius:15px;
margin:10px;
backdrop-filter:blur(10px);
animation:fade 0.4s ease;
}

@keyframes fade{
from{opacity:0;transform:translateY(10px)}
to{opacity:1;transform:translateY(0)}
}

input,button{
padding:10px;
border-radius:10px;
border:none;
margin:5px;
}

button{
cursor:pointer;
font-weight:bold;
}

/* TABLE */
table{
width:100%;
border-collapse:collapse;
}

td,th{
padding:10px;
border-bottom:1px solid #334155;
}

.hidden{display:none;}

@media(max-width:768px){
.sidebar{width:200px}
.main{margin-left:0}
.hero h1{font-size:32px}
}
</style>
</head>

<body>

<!-- HERO -->
<div class="hero" id="hero">
<h1>PAKGOLD 💰</h1>
<p>Premium SaaS Dashboard System</p>
<button class="btn" onclick="start()">Enter Dashboard</button>
</div>

<!-- SIDEBAR -->
<div class="sidebar" id="sidebar">
<h2>PAKGOLD</h2>
<button onclick="show('home')">Dashboard</button>
<button onclick="show('requests')">Requests</button>
<button onclick="show('admin')">Admin</button>
<button onclick="logout()">Logout</button>
</div>

<!-- MAIN -->
<div class="main" id="main">

<div id="home" class="card">
<h2>Welcome to PAKGOLD 🚀</h2>
<p>User ID: <span id="uid"></span></p>
<p>Status: Active Session</p>
</div>

<div id="requests" class="card hidden">
<h2>Create Request</h2>
<input id="msg" placeholder="Enter request message"/>
<button onclick="send()">Submit</button>
<div id="list"></div>
</div>

<div id="admin" class="card hidden">
<h2>Admin Panel</h2>
<div id="adminList"></div>
</div>

</div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
import { getDatabase, ref, push, set, onValue, update } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";
import { getAuth, signInAnonymously } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";

const firebaseConfig = {
apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5qK5MDHk",
authDomain: "dark-web-9.firebaseapp.com",
databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com",
projectId: "dark-web-9",
storageBucket: "dark-web-9.firebasestorage.app",
messagingSenderId: "564328425161",
appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
};

const app = initializeApp(firebaseConfig);
const db = getDatabase(app);
const auth = getAuth();

let uid="";

window.start=()=>{
document.getElementById("hero").style.display="none";
document.getElementById("sidebar").style.display="block";
document.getElementById("main").style.display="block";

signInAnonymously(auth).then(u=>{
uid=u.user.uid;
document.getElementById("uid").innerText=uid.slice(0,8);
load();
loadAdmin();
});
};

window.show=(id)=>{
document.querySelectorAll(".card").forEach(c=>c.classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
};

window.send=()=>{
let msg=document.getElementById("msg").value;

let r=push(ref(db,"requests"));
set(r,{
uid:uid,
msg:msg,
status:"pending",
time:Date.now()
});

document.getElementById("msg").value="";
};

function load(){
onValue(ref(db,"requests"),snap=>{
let html="";
snap.forEach(r=>{
let d=r.val();
if(d.uid===uid){
html+=`<p>📩 ${d.msg} - ${d.status}</p>`;
}
});
document.getElementById("list").innerHTML=html;
});
}

function loadAdmin(){
onValue(ref(db,"requests"),snap=>{
let html=`<table>
<tr><th>User</th><th>Message</th><th>Status</th><th>Action</th></tr>`;

snap.forEach(r=>{
let d=r.val();

html+=`
<tr>
<td>${d.uid.slice(0,5)}</td>
<td>${d.msg}</td>
<td>${d.status}</td>
<td>
<button onclick="approve('${r.key}')">✔</button>
<button onclick="reject('${r.key}')">❌</button>
</td>
</tr>
`;
});

html+="</table>";
document.getElementById("adminList").innerHTML=html;
});
}

window.approve=(id)=>{
update(ref(db,"requests/"+id),{status:"approved"});
};

window.reject=(id)=>{
update(ref(db,"requests/"+id),{status:"rejected"});
};

window.logout=()=>{
location.reload();
};
</script>

</body>
</html>
