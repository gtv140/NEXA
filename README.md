<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEXA EARN</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body{margin:0;font-family:Arial;background:#f4f6f8;color:#222}
header{padding:16px;text-align:center;font-size:26px;font-weight:700;background:#111;color:#fff}
.page{max-width:420px;margin:20px auto;background:#fff;border-radius:12px;padding:16px;box-shadow:0 6px 18px rgba(0,0,0,.08)}
.hidden{display:none}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid #ccc}
button{background:#111;color:#fff;font-weight:600;border:none;cursor:pointer}
.stats{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:10px}
.box{background:#f1f1f1;padding:12px;border-radius:10px;text-align:center}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;background:#111;color:#fff;padding:10px 0}
.nav div{cursor:pointer;text-align:center;font-size:13px}
.plan{border:1px solid #ddd;padding:12px;border-radius:10px;margin-top:10px}
img{width:100%;border-radius:10px;margin-top:10px}
</style>
</head>
<body>

<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
  <h3>Login / Signup</h3>
  <select id="mode">
    <option value="login">Login</option>
    <option value="signup">Signup</option>
  </select>
  <input id="username" placeholder="Username">
  <input id="password" type="password" placeholder="Password">
  <button onclick="login()">Continue</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <h3>Welcome, <span id="uName"></span></h3>

  <div class="stats">
    <div class="box">Balance<br><b>Rs <span id="bal">0</span></b></div>
    <div class="box">Daily Profit<br><b>Rs <span id="daily">0</span></b></div>
    <div class="box">Total Profit<br><b>Rs <span id="total">0</span></b></div>
    <div class="box">Active Users<br><b><span id="users">0</span></b></div>
  </div>

  <img src="https://picsum.photos/400/200?1">
  <img src="https://picsum.photos/400/200?2">
  <img src="https://picsum.photos/400/200?3">

  <button onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h3>Investment Plans</h3>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h3>Deposit</h3>
  <select id="depMethod" onchange="setNumber()">
    <option value="jazz">JazzCash</option>
    <option value="easy">EasyPaisa</option>
  </select>
  <input id="depNum" readonly>
  <button onclick="copyNum()">Copy Number</button>
  <input id="depAmt" placeholder="Amount">
  <input placeholder="Transaction ID">
  <input type="file">
  <button>Submit</button>
</div>

<!-- NAV -->
<div id="nav" class="nav hidden">
  <div onclick="show('dashboard')">Home</div>
  <div onclick="show('plans')">Plans</div>
  <div onclick="show('deposit')">Deposit</div>
</div>

<script>
let user = localStorage.getItem('nexa_user');
let balance = Number(localStorage.getItem('nexa_bal')||0);
let daily = Number(localStorage.getItem('nexa_daily')||0);
let total = Number(localStorage.getItem('nexa_total')||0);

function login(){
  const u=username.value.trim();
  if(!u) return alert('Enter username');
  localStorage.setItem('nexa_user',u);
  if(mode.value==='signup'){
    balance=0; daily=0; total=0;
    localStorage.setItem('nexa_bal',0);
    localStorage.setItem('nexa_daily',0);
    localStorage.setItem('nexa_total',0);
  }
  load();
}

function load(){
  user = localStorage.getItem('nexa_user');
  if(!user) return;
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('nav').classList.remove('hidden');
  show('dashboard');
  uName.innerText=user;
  bal.innerText=balance;
  daily.innerText=daily;
  total.innerText=total;
  users.innerText=Math.floor(1000+Math.random()*4000);
}

function logout(){
  localStorage.removeItem('nexa_user');
  location.reload();
}

function show(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function setNumber(){
  depNum.value = depMethod.value==='jazz'?'03705519562':'03379827882';
}
function copyNum(){navigator.clipboard.writeText(depNum.value)}

function loadPlans(){
  let html='';
  for(let i=1;i<=50;i++){
    let invest=200*i;
    let totalP=Math.round(invest*2.2);
    html+=`<div class="plan">
      <b>Plan ${i}</b><br>
      Invest: Rs ${invest}<br>
      Total: Rs ${totalP}<br>
      <button>Buy Now</button>
    </div>`;
  }
  plansList.innerHTML=html;
}

window.onload=function(){
  load();
  loadPlans();
  setNumber();
};
</script>
</body>
</html>
