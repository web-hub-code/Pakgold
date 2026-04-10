<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PAKGOLD ULTRA DASHBOARD</title>

<style>
body{
margin:0;
font-family:Arial;
background:#050816;
color:white;
overflow-x:hidden;
}

.glow{
color:#fbbf24;
text-shadow:0 0 10px #fbbf24;
}

.sidebar{
position:fixed;
left:0;
top:0;
width:220px;
height:100%;
background:rgba(255,255,255,0.05);
backdrop-filter:blur(12px);
padding:15px;
}

.sidebar h2{
text-align:center;
}

.sidebar button{
width:100%;
margin:8px 0;
padding:10px;
border:none;
border-radius:10px;
background:#111827;
color:white;
cursor:pointer;
transition:0.3s;
}

.sidebar button:hover{
background:#fbbf24;
color:black;
}

.main{
margin-left:240px;
padding:20px;
}

.card{
background:rgba(255,255,255,0.06);
padding:20px;
border-radius:15px;
margin-bottom:15px;
animation:fadeIn 0.6s ease-in-out;
}

@keyframes fadeIn{
from{opacity:0;transform:translateY(20px);}
to{opacity:1;transform:translateY(0);}
}

.grid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(200px,1fr));
gap:15px;
}

.box{
background:rgba(255,255,255,0.05);
padding:15px;
border-radius:12px;
text-align:center;
transition:0.3s;
}

.box:hover{
transform:scale(1.05);
background:rgba(251,191,36,0.1);
}

.badge{
color:#fbbf24;
font-weight:bold;
}

.topbar{
display:flex;
justify-content:space-between;
align-items:center;
margin-bottom:20px;
}

.pulse{
animation:pulse 1.5s infinite;
}

@keyframes pulse{
0%{box-shadow:0 0 0 0 rgba(251,191,36,0.7);}
70%{box-shadow:0 0 0 15px rgba(251,191,36,0);}
100%{box-shadow:0 0 0 0 rgba(251,191,36,0);}
}
</style>
</head>

<body>

<div class="sidebar">
<h2 class="glow">PAKGOLD</h2>
<button>Dashboard</button>
<button>Invest</button>
<button>Deposit</button>
<button>Withdraw</button>
<button>Referral</button>
<button>History</button>
<button>Logout</button>
</div>

<div class="main">

<div class="topbar">
<h1 class="glow">Dashboard</h1>
<span class="badge pulse">LIVE DEMO</span>
</div>

<div class="grid">

<div class="box">
<h3>Balance</h3>
<p>₨ 12,500</p>
</div>

<div class="box">
<h3>Nodes</h3>
<p>15 Active</p>
</div>

<div class="box">
<h3>Profit</h3>
<p>₨ 2,340</p>
</div>

<div class="box">
<h3>Referrals</h3>
<p>8 Users</p>
</div>

</div>

<div class="card">
<h2 class="glow">Activity Feed</h2>
<ul>
<li>✔ Deposit request pending</li>
<li>✔ Node activated</li>
<li>✔ Referral bonus added</li>
<li>✔ Withdrawal queued</li>
</ul>
</div>

<div class="card">
<h2 class="glow">Investment Plans</h2>
<div class="grid">
<div class="box">Basic Plan<br>₨200 Node</div>
<div class="box">Pro Plan<br>₨500 Node</div>
<div class="box">VIP Plan<br>₨1000 Node</div>
</div>
</div>

<div class="card">
<h2 class="glow">Referral System</h2>
<p>Your Code: <b>PAK12345</b></p>
<p>Earn rewards by inviting friends 💎</p>
</div>

</div>

</body>
</html>
