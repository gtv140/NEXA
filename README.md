<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
body {margin:0;font-family:Arial,sans-serif;background:#f8f9fa;color:#111;}
header {text-align:center;padding:20px;font-size:28px;font-weight:800;background:#0d6efd;color:#fff;}
.page {max-width:600px;margin:20px auto;padding:20px;background:#fff;border-radius:12px;box-shadow:0 4px 20px rgba(0,0,0,0.1);}
input,select,button {width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid #ccc;font-size:14px;}
button {background:#0d6efd;color:#fff;font-weight:700;cursor:pointer;border:none;transition:0.2s;}
button:hover{opacity:0.9;}
.nav {position:fixed;bottom:0;left:0;right:0;background:#fff;display:flex;justify-content:space-around;padding:10px 0;box-shadow:0 -2px 10px rgba(0,0,0,0.1);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{display:block;font-size:22px;margin-bottom:4px;}
.hidden{display:none;}
.user-box{display:flex;justify-content:space-between;align-items:center;background:#e9ecef;padding:14px;border-radius:12px;margin-bottom:12px;}
.user-box div{display:flex;flex-direction:column;}
.plan-box{border:1px solid #ccc;border-radius:10px;padding:12px;margin:10px 0;display:flex;justify-content:space-between;align-items:center;}
.plan-box button{width:120px;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;background:#0d6efd;color:#fff;border-radius:10px;cursor:pointer;margin-top:10px;}
.support-icon:hover{opacity:0.9;}
img.random-img{width:100%;border-radius:12px;margin-top:10px;}
.alert-box{background:#ffeeba;padding:10px;border-radius:8px;margin-bottom:12px;}
</style>
</head>
<body>

<header>NEXA EARN</header>

<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">Signup</option></select>
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
<div class="user-box">
  <div><b id="dashUser">Username</b><br>Active Members: <span id="activeMembers">0</span></div>
  <div>Balance: Rs <span id="dashBalance">0</span><br>Daily Profit: Rs <span id="dashDaily">0</span><br>Total Profit: Rs <span id="dashTotal">0</span></div>
</div>

<div id="randomPhotos"></div>

<div class="alert-box">Special Offers: Top 5 Plans x2.4, Others x2.2 | Countdown included for top 5</div>

<button onclick="showPage('plans')">View Plans</button>
<button onclick="showPage('deposit')">Deposit</button>
<button onclick="showPage('withdrawal')">Withdraw</button>
<button onclick="logout()">Logout</button>
</div>

<div id="plans" class="page hidden">
<h2>Investment Plans</h2>
<div id="plansList"></div>
</div>

<div id="deposit" class="page hidden">
<h2>Deposit</h2>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<div style="display:flex; gap:8px; align-items:center; margin-top:10px;">
<input id="depositNumber" readonly style="flex:1;">
<button onclick="copyDepositNumber()">Copy</button>
</div>
<input id="depositAmount" placeholder="Enter Amount">
<input id="depositTxId" placeholder="Transaction ID">
<input type="file" id="depositProof">
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawal" class="page hidden">
<h2>Withdrawal</h2>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="withdrawAccount" placeholder="Account Number">
<input id="withdrawAmount" placeholder="Amount">
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div id="about" class="page hidden">
<h2>About NEXA EARN</h2>
<p>NEXA EARN tracks your investments with real-time balance updates. Support team is available for all queries.</p>
<div class="support-icon" onclick="openSupport()">🛠️ Support</div>
</div>

<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
<div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');

const photoUrls = [
'https://source.unsplash.com/600x200/?finance',
'https://source.unsplash.com/600x200/?money',
'https://source.unsplash.com/600x200/?technology',
'https://source.unsplash.com/600x200/?business',
'https://source.unsplash.com/600x200/?investment'
];

function showRandomPhotos(){
  const container = document.getElementById('randomPhotos');
  container.innerHTML='';
  for(let i=0;i<4;i++){
    const img = document.createElement('img');
    img.src = photoUrls[Math.floor(Math.random()*photoUrls.length)];
    img.className='random-img';
    container.appendChild(img);
  }
}

let plansData=[];
for(let i=1;i<=50;i++){
  let invest=200 + (i-1)*100;
  let multiplier = i<=5 ? 2.4 : 2.2;
  let days=25+Math.floor(i/5)*5;
  plansData.push({id:i,name:`Plan ${i}`,invest,days,multiplier});
}

function renderPlans(){
  const list = document.getElementById('plansList');
  list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='plan-box';
    const total=Math.round(p.invest*p.multiplier);
    const daily=Math.round(total/p.days);
    div.innerHTML=`<div>${p.name}<br>Invest: Rs ${p.invest} | Total: Rs ${total} | Daily: Rs ${daily} | Days: ${p.days}</div>
    <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}

function buyPlan(id){
  const plan=plansData.find(p=>p.id===id);
  if(balance<plan.invest){alert('Insufficient Balance');return;}
  const total=Math.round(plan.invest*plan.multiplier);
  const daily=Math.round(total/plan.days);
  balance-=plan.invest;
  dailyProfit+=daily;
  userPlans.push({id:plan.id,name:plan.name,daily,total,start:Date.now(),days:plan.days});
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  updateDashboard();
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function login(){
  const u = document.getElementById('user').value.trim();
  const p = document.getElementById('pass').value.trim();
  if(!u || !p){alert('Enter username & password'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')){
    balance=0;dailyProfit=0;userPlans=[];localStorage.setItem('nexa_balance',balance);localStorage.setItem('nexa_daily',dailyProfit);localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  }
  updateDashboard();
}

function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashTotal').innerText=userPlans.reduce((a,b)=>a+b.total,0);
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
  showRandomPhotos();
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000)+1;
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value = method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}
function submitWithdraw(){alert('Withdrawal requested');}
function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}

window.onload=()=>{
  if(currentUser){updateDashboard();}
  renderPlans();
}
</script>
</body>
</html>
