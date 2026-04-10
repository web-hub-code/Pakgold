<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PAKGOLD v2 - Premium Dashboard</title>

<style>
body{
margin:0;
font-family:Arial;
background:linear-gradient(135deg,#0f172a,#020617);
color:white;
}

.sidebar{
position:fixed;
left:0;top:0;
width:250px;
height:100%;
background:rgba(255,255,255,0.05);
backdrop-filter:blur(12px);
padding:20px;
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

.main{
margin-left:270px;
padding:20px;
}

.card{
background:rgba(255,255,255,0.06);
padding:20px;
margin:10px;
border-radius:15px;
backdrop-filter:blur(10px);
animation:fade 0.4s ease;
}

@keyframes fade{
from{opacity:0;transform:translateY(10px)}
to{opacity:1;transform:translateY(0)}
}

input,button{
padding:10px;
margin:5px;
border-radius:10px;
border:none;
}

button{
cursor:pointer;
background:#fbbf24;
color:black;
font-weight:bold;
}

.hidden{display:none;}

table{
width:100%;
border-collapse:collapse;
}

td,th{
padding:10px;
border-bottom:1px solid #334155;
}
</style>
</head>

<body>

<div class="sidebar">
<h2>PAKGOLD v2</h2>
<button onclick="show('home')">Home</button>
<button onclick="show('requests')">Requests</button>
<button onclick="show('history')">History</button>
<button onclick="show('ref')">Referral</button>
<button onclick="show('admin')">Admin</button>
</div>

<div class="main">

<div id="home" class="card">
<h2>Welcome Sweetie 😘</h2>
<p>Premium Live Dashboard</p>
<p>Points: <span id="points">0</span></p>
</div>

<div id="requests" class="card hidden">
<h2>Send Request</h2>
<input id="amount" placeholder="Enter request value"/>
<button onclick="sendRequest()">Submit</button>
</div>

<div id="history" class="card hidden">
<h2>History</h2>
<div id="historyBox"></div>
</div>

<div id="ref" class="card hidden">
<h2>Referral System</h2>
<p>Your Code: <span id="refCode">-</span></p>
</div>

<div id="admin" class="card hidden">
<h2>Admin Panel</h2>
<div id="reqTable"></div>
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

let uid = "";

signInAnonymously(auth).then(u=>{
uid = u.user.uid;
initUser();
loadRequests();
});

window.show = (id)=>{
document.querySelectorAll(".card").forEach(c=>c.classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
};

function initUser(){
set(ref(db,"users/"+uid),{
points:0,
refCode: uid.slice(0,6)
});
document.getElementById("refCode").innerText = uid.slice(0,6);

onValue(ref(db,"users/"+uid),(snap)=>{
if(snap.exists()){
document.getElementById("points").innerText = snap.val().points;
}
});
}

window.sendRequest = ()=>{
let val = document.getElementById("amount").value;

let r = push(ref(db,"requests"));
set(r,{
uid:uid,
value:val,
status:"pending",
time:Date.now()
});

alert("Request Sent 🚀");
};

function loadRequests(){
onValue(ref(db,"requests"),snap=>{
let html = "<table><tr><th>User</th><th>Value</th><th>Status</th><th>Action</th></tr>";

snap.forEach(r=>{
let d = r.val();
html += `
<tr>
<td>${d.uid.slice(0,5)}</td>
<td>${d.value}</td>
<td>${d.status}</td>
<td>
<button onclick="approve('${r.key}')">Approve</button>
<button onclick="reject('${r.key}')">Reject</button>
</td>
</tr>
`;
});

document.getElementById("reqTable").innerHTML = html;
});
}

window.approve = (id)=>{
update(ref(db,"requests/"+id),{status:"approved"});
};

window.reject = (id)=>{
update(ref(db,"requests/"+id),{status:"rejected"});
};
</script>

</body>
</html>
