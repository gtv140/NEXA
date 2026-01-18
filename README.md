<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --primary:#ff9800;
  --secondary:#ffeb3b;
  --bg:#f0f0f0;
  --card-bg:#fff;
  --text-dark:#333;
}
body {
  margin:0; font-family:'Segoe UI',sans-serif; background:var(--bg); color:var(--text-dark);
}
header { background:var(--primary); color:#fff; text-align:center; padding:20px; font-size:24px; font-weight:bold; }
.page { max-width:500px; margin:20px auto; padding:15px; border-radius:12px; }
.hidden{display:none;}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid #ccc;}
button{cursor:pointer;background:linear-gradient(90deg,var(--primary),var(--secondary));color:#fff;font-weight:bold;}

/* App-style cards & icons */
.cards { display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-top:10px;}
.card { background:var(--card-bg); border-radius:12px; padding:15px; text-align:center; box-shadow:0 3px 12px rgba(0,0,0,0.15); cursor:pointer; transition:0.3s;}
.card:hover{transform:translateY(-5px); box-shadow:0 6px 20px rgba(0,0,0,0.2);}
.card h3{margin:0;font-size:16px;}
.card p{margin:5px 0 0; font-size:12px; color:#555;}
.nav { display:flex; justify-content:space-around; padding:10px 0; background:#eee; position:fixed; bottom:0; left:0; right:0; border-top:1px solid #ccc;}
.nav div{cursor:pointer;text-align:center;}
</style>
</head>
<body>

<header>NEXA EARN</header>

<!-- Login Page -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <input id="username" placeholder="Username" />
  <input id="password" type="password" placeholder="Password" />
  <button onclick="login()">Login / Signup</button>
</div>

<!-- Dashboard -->
<div id="dashboard" class="page hidden">
  <div class="cards">
    <div class="card">Username<br><strong id="dashUser">-</strong></div>
    <div class="card">Balance<br><strong>Rs <span id="dashBalance">0</span></strong></div>
    <div class="card">Daily Profit<br><strong>Rs <span id="dashDaily">0</span></strong></div>
    <div class="card">Total Profit<br><strong>Rs <span id="dashTotal">0</span></strong></div>
    <div class="card">Active Members<br><strong><span id="dashActive">0</span></strong></div>
    <div class="card" onclick="showPage('supportPage')">Support<br>Contact Us</div>
  </div>
  <h3>Company Info</h3>
  <p>NEXA EARN provides digital investment & ads watch plans since 2022. Safe, fast & reliable profits.</p>
</div>

<!-- Plans Page -->
<div id="plansPage" class="page hidden">
  <h2>Investment Plans</h2>
  <div id="plansList" class="cards"></div>
</div>

<!-- Ads Page -->
<div id="adsPage" class="page hidden">
  <h2>Ads Watch Plans</h2>
  <div id="adsList" class="cards"></div>
</div>

<!-- Deposit Page -->
<div id="depositPage" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex; gap:8px; align-items:center; margin-top:10px">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount" />
  <input id="depositTxId" placeholder="Transaction ID" />
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- Withdraw Page -->
<div id="withdrawPage" class="page hidden">
  <h2>Withdraw</h2>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number" />
  <input id="withdrawAmount" placeholder="Amount" />
  <button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<!-- Support Page -->
<div id="supportPage" class="page hidden">
  <h2>Support</h2>
  <p>Email: rock.earn92@gmail.com</p>
  <p><a href="https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu" target="_blank">WhatsApp Group</a></p>
</div>

<div class="nav">
  <div onclick="showPage('dashboard')">🏠 Home</div>
  <div onclick="showPage('plansPage')">📦 Plans</div>
  <div onclick="showPage('adsPage')">🎬 Ads</div>
  <div onclick="showPage('depositPage')">💰 Deposit</div>
  <div onclick="showPage('withdrawPage')">💵 Withdraw</div>
  <div onclick="showPage('supportPage')">🛠️ Support</div>
</div>

<script>
// User data
let currentUser = localStorage.getItem('nexa_user')||'';
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let activeMembers = Math.floor(Math.random()*5000);

// Pages
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

// Login/Signup
function login(){
  const u = document.getElementById('username').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')){
    balance=0; dailyProfit=0; totalProfit=0;
    localStorage.setItem('nexa_balance', balance);
    localStorage.setItem('nexa_daily', dailyProfit);
    localStorage.setItem('nexa_total', totalProfit);
  }else{
    balance=parseFloat(localStorage.getItem('nexa_balance'));
    dailyProfit=parseFloat(localStorage.getItem('nexa_daily'));
    totalProfit=parseFloat(localStorage.getItem('nexa_total'));
  }
  updateDashboard();
}

// Dashboard Update
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
  document.getElementById('dashActive').innerText=activeMembers;
  showPage('dashboard');
}

// Deposit
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}

// Withdraw
function submitWithdraw(){alert('Withdrawal requested');}

// Plans
const plans=[];
for(let i=1;i<=50;i++){
  plans.push({id:i,name:'Plan '+i,invest:200+i*100,days:25+Math.floor(i/2),multiplier:i<=5?2.4:2.2});
}
function loadPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plans.forEach(p=>{
    const div=document.createElement('div');
    div.className='card';
    div.innerHTML=`<h3>${p.name}</h3>
    Invest: Rs ${p.invest} | Days: ${p.days} | Total: Rs ${Math.round(p.invest*p.multiplier)} 
    <br><button onclick="buyPlan(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}
function buyPlan(id){
  const plan=plans.find(p=>p.id===id);
  if(plan){
    showPage('depositPage');
    document.getElementById('depositAmount').value=plan.invest;
    updateDepositNumber();
  }
}

// Ads Watch Plans
const adsPlans=[];
for(let i=1;i<=7;i++){
  adsPlans.push({id:i,name:'Ads Plan '+i,invest:500+i*50,days:10,adsPerDay:3});
}
function loadAds(){
  const list=document.getElementById('adsList'); list.innerHTML='';
  adsPlans.forEach(a=>{
    const div=document.createElement('div');
    div.className='card';
    div.innerHTML=`<h3>${a.name}</h3>
    Invest: Rs ${a.invest} | Days: ${a.days} | Daily Ads: ${a.adsPerDay}
    <br><button onclick="buyAds(${a.id})">Buy Plan</button>`;
    list.appendChild(div);
  });
}
function buyAds(id){
  const plan=adsPlans.find(a=>a.id===id);
  if(plan){
    showPage('depositPage');
    document.getElementById('depositAmount').value=plan.invest;
    updateDepositNumber();
  }
}

// Init
window.onload=function(){
  if(currentUser){updateDashboard();}
  loadPlans(); loadAds();
}
</script>
</body>
</html>
