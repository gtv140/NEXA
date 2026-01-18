<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
body {margin:0; font-family:Arial,sans-serif; background:#f4f4f9; color:#111;}
header {text-align:center; font-size:32px; padding:20px; background:linear-gradient(90deg,#f7c600,#ff4d4d); -webkit-background-clip:text; -webkit-text-fill-color:transparent;}
.page {max-width:480px; margin:20px auto; padding:20px; background:#fff; border-radius:12px; box-shadow:0 8px 20px rgba(0,0,0,0.1);}
input, select, button {width:100%; padding:10px; margin-top:10px; border-radius:8px; border:1px solid #ddd; font-size:16px;}
button {background:linear-gradient(90deg,#f7c600,#ff4d4d); color:#fff; font-weight:700; cursor:pointer;}
button:disabled {opacity:0.6; cursor:not-allowed;}
.nav {position:fixed; bottom:0; left:0; right:0; display:flex; justify-content:space-around; padding:10px 0; background:#fff; box-shadow:0 -3px 10px rgba(0,0,0,0.1);}
.nav div {text-align:center; cursor:pointer;}
.nav div .ico {font-size:24px; display:block; margin-bottom:4px;}
.hidden {display:none;}
.card {border:1px solid #eee; border-radius:10px; padding:10px; margin:10px 0; box-shadow:0 4px 8px rgba(0,0,0,0.05);}
.card img {width:100%; border-radius:8px; margin-bottom:8px;}
.cards-container {display:grid; grid-template-columns:1fr; gap:10px;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN / SIGNUP -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption">
    <option value="login">Login</option>
    <option value="signup">Signup</option>
  </select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="card"><strong>Username:</strong> <span id="dashUser"></span></div>
  <div class="card"><strong>Balance:</strong> Rs <span id="dashBalance">0</span></div>
  <div class="card"><strong>Daily Profit:</strong> Rs <span id="dashDaily">0</span></div>
  <div class="card"><strong>Active Members:</strong> <span id="activeMembers">0</span></div>
  <div class="cards-container" id="adsSection"></div>
  <div class="cards-container" id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
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
  <button id="submitDepositBtn" onclick="submitDeposit()" disabled>Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawal" class="page hidden">
  <h2>Withdrawal</h2>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number" />
  <input id="withdrawAmount" placeholder="Amount" />
  <button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<!-- COMPANY / SUPPORT -->
<div id="about" class="page hidden">
  <h2>About NEXA EARN</h2>
  <p>NEXA EARN provides secure digital earning opportunities since 2022. Watch ads, invest in plans, and earn daily profits. Our system is fully automated and safe.</p>
  <div class="card"><strong>Support:</strong> <button onclick="openSupport()">Contact Support</button></div>
</div>

<!-- NAV -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Dashboard</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
// LOCAL STORAGE DATA
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');

// SHOW/HIDE PAGES
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

// LOGIN / SIGNUP
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  balance = parseFloat(localStorage.getItem('nexa_balance_'+currentUser)||'0');
  dailyProfit = parseFloat(localStorage.getItem('nexa_daily_'+currentUser)||'0');
  updateDashboard();
}

// LOGOUT
function logout(){
  currentUser=null; 
  localStorage.removeItem('nexa_user'); 
  location.reload();
}

// DASHBOARD
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
  updateActiveMembers();
  loadPlans();
  loadAds();
}

// ACTIVE MEMBERS SIMULATION
function updateActiveMembers(){
  const active=document.getElementById('activeMembers');
  let count=Math.floor(Math.random()*5000)+1;
  active.innerText=count;
}

// PLANS
let plansData=[];
for(let i=1;i<=50;i++){
  let invest = 200 + (i-1)*100;
  plansData.push({id:i,name:`Plan ${i}`,invest:invest,days:25+Math.floor(i/2),multiplier:i<=5?2.4:2.2,offer:i<=5});
}

function loadPlans(){
  const container=document.getElementById('plansList');
  container.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='card';
    div.innerHTML=`<strong>${p.name}${p.offer?' (Special Offer)':''}</strong><br>
      Invest: Rs ${p.invest}<br>
      Days: ${p.days}<br>
      Total: Rs ${Math.round(p.invest*p.multiplier)}<br>
      <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    container.appendChild(div);
  });
}

// BUY PLAN
function buyPlan(planId){
  const plan=plansData.find(p=>p.id===planId);
  if(!plan) return;
  alert(`You selected ${plan.name}. Please complete deposit.`);
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
  updateDepositNumber();
}

// DEPOSIT
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
  checkDepositReady();
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}

function checkDepositReady(){
  const amount=document.getElementById('depositAmount').value.trim();
  const tx=document.getElementById('depositTxId').value.trim();
  const proof=document.getElementById('depositProof').files.length;
  document.getElementById('submitDepositBtn').disabled = !(amount && tx && proof);
}

document.getElementById('depositAmount').addEventListener('input',checkDepositReady);
document.getElementById('depositTxId').addEventListener('input',checkDepositReady);
document.getElementById('depositProof').addEventListener('change',checkDepositReady);

function submitDeposit(){
  const amount=parseFloat(document.getElementById('depositAmount').value);
  if(!amount){alert('Enter amount'); return;}
  balance+=amount;
  dailyProfit+=Math.round(amount*0.02);
  localStorage.setItem('nexa_balance_'+currentUser,balance);
  localStorage.setItem('nexa_daily_'+currentUser,dailyProfit);
  alert('Deposit successful!');
  updateDashboard();
  showPage('dashboard');
}

// WITHDRAW
function submitWithdraw(){alert('Withdrawal requested');}

// SUPPORT
function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}

// ADS
function loadAds(){
  const container=document.getElementById('adsSection');
  container.innerHTML='';
  if(balance<=0){container.innerHTML='<div class="card">Deposit to unlock ads and earn!</div>'; return;}
  for(let i=1;i<=3;i++){
    const div=document.createElement('div');
    div.className='card';
    div.innerHTML=`<strong>Ad ${i}</strong><br><img src="https://picsum.photos/400/200?random=${i}" alt="ad"><br>
      <button onclick="watchAd(${i})">Watch & Earn</button>`;
    container.appendChild(div);
  }
}

function watchAd(adId){
  alert(`You watched Ad ${adId}, daily profit added!`);
  let profit=Math.floor(Math.random()*20)+5;
  balance+=profit;
  dailyProfit+=profit;
  localStorage.setItem('nexa_balance_'+currentUser,balance);
  localStorage.setItem('nexa_daily_'+currentUser,dailyProfit);
  updateDashboard();
}
</script>
</body>
</html>
