<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
body {
  margin:0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background:#f2f2f2;
  color:#111;
}
header{
  padding:15px;
  text-align:center;
  font-size:26px;
  font-weight:bold;
  background:#ffd700;
  color:#000;
  box-shadow:0 3px 10px rgba(0,0,0,0.2);
}
.page{display:none; max-width:480px; margin:20px auto; padding:15px; background:#fff; border-radius:15px; box-shadow:0 6px 20px rgba(0,0,0,0.1);}
.page.active{display:block;}
.card{background:#ffe; margin:10px 0; padding:15px; border-radius:12px; box-shadow:0 4px 10px rgba(0,0,0,0.1);}
.card h3{margin:0 0 10px 0;}
button{padding:10px; border:none; border-radius:8px; background:#ffd700; font-weight:bold; cursor:pointer;}
button:hover{background:#ffb800;}
.nav{position:fixed; bottom:0; left:0; right:0; display:flex; justify-content:space-around; padding:10px 0; background:#fff; box-shadow:0 -3px 10px rgba(0,0,0,0.1);}
.nav div{text-align:center; cursor:pointer;}
.nav div:hover{color:#ffd700;}
input, select{width:100%; padding:10px; margin:5px 0 10px 0; border-radius:8px; border:1px solid #ccc;}
</style>
</head>
<body>

<header>NEXA EARN</header>

<div id="loginPage" class="page active">
  <h2>Login / Signup</h2>
  <input id="user" placeholder="Username"/>
  <input id="pass" type="password" placeholder="Password"/>
  <button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page">
  <div class="card"><b>Username:</b> <span id="dashUser"></span></div>
  <div class="card"><b>Balance:</b> Rs <span id="dashBalance">0</span></div>
  <div class="card"><b>Daily Profit:</b> Rs <span id="dashDaily">0</span></div>
  <div class="card"><b>Active Members:</b> <span id="activeMembers">0</span></div>
  <div class="card">
    <h3>Company Info</h3>
    <p>NEXA EARN has been providing digital earning opportunities since 2022. Invest safely and earn daily profits through plans and watch ads offers.</p>
    <div style="display:flex; gap:5px; flex-wrap:wrap;">
      <img src="https://picsum.photos/100/80?random=1">
      <img src="https://picsum.photos/100/80?random=2">
      <img src="https://picsum.photos/100/80?random=3">
      <img src="https://picsum.photos/100/80?random=4">
    </div>
  </div>
  <div id="plansList"></div>
  <div id="adsPlansList"></div>
  <button onclick="logout()">Logout</button>
</div>

<div id="deposit" class="page">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex; gap:8px; align-items:center; margin-top:10px;">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount"/>
  <input id="depositTxId" placeholder="Transaction ID"/>
  <input type="file" id="depositProof"/>
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawal" class="page">
  <h2>Withdrawal</h2>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number"/>
  <input id="withdrawAmount" placeholder="Amount"/>
  <button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div id="support" class="page">
  <h2>Support</h2>
  <p>Contact our support team for assistance.</p>
  <button onclick="window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank')">WhatsApp Support</button>
  <button onclick="window.location='mailto:rock.earn92@gmail.com'">Email Support</button>
</div>

<div class="nav">
  <div onclick="showPage('dashboard')">🏠 Home</div>
  <div onclick="showPage('plansList')">📦 Plans</div>
  <div onclick="showPage('adsPlansList')">🎬 Ads Earn</div>
  <div onclick="showPage('deposit')">💰 Deposit</div>
  <div onclick="showPage('withdrawal')">💵 Withdraw</div>
  <div onclick="showPage('support')">🛠️ Support</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user') || null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let activeMembers = Math.floor(Math.random()*5000);

const plans = [];
for(let i=1;i<=50;i++){
  plans.push({id:i, name:'Plan '+i, invest:200+i*100, days:25+Math.floor(i/2), multiplier:i<=5?2.4:2.2});
}

const adsPlans = [];
for(let i=1;i<=7;i++){
  adsPlans.push({id:i, name:'Ads Plan '+i, invest:500+i*50, days:10, dailyAds:3, multiplier:1.1});
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  if(id==='plansList'){renderPlans();}
  if(id==='adsPlansList'){renderAdsPlans();}
  if(document.getElementById(id)) document.getElementById(id).classList.add('active');
}

function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username');return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')) balance=0, localStorage.setItem('nexa_balance',balance);
  if(!localStorage.getItem('nexa_daily')) dailyProfit=0, localStorage.setItem('nexa_daily',dailyProfit);
  updateDashboard();
}

function logout(){
  currentUser=null;
  localStorage.removeItem('nexa_user');
  location.reload();
}

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('activeMembers').innerText=activeMembers;
  showPage('dashboard');
}

function renderPlans(){
  const container=document.getElementById('plansList');
  container.innerHTML='';
  plans.forEach(p=>{
    const div=document.createElement('div');
    div.className='card';
    div.innerHTML=`<h3>${p.name}</h3>
      Invest: Rs ${p.invest}<br>
      Days: ${p.days}<br>
      Total Profit: Rs ${Math.round(p.invest*p.multiplier)}<br>
      <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    container.appendChild(div);
  });
}

function renderAdsPlans(){
  const container=document.getElementById('adsPlansList');
  container.innerHTML='';
  adsPlans.forEach(p=>{
    const div=document.createElement('div');
    div.className='card';
    div.innerHTML=`<h3>${p.name}</h3>
      Invest: Rs ${p.invest}<br>
      Days: ${p.days}<br>
      Daily Ads: ${p.dailyAds}<br>
      <button onclick="buyAdsPlan(${p.id})">Buy Now</button>`;
    container.appendChild(div);
  });
}

function buyPlan(id){
  const plan=plans.find(p=>p.id===id);
  document.getElementById('depositAmount').value=plan.invest;
  showPage('deposit');
}
function buyAdsPlan(id){
  const plan=adsPlans.find(p=>p.id===id);
  document.getElementById('depositAmount').value=plan.invest;
  showPage('deposit');
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}

function submitDeposit(){
  if(!document.getElementById('depositTxId').value || !document.getElementById('depositProof').files.length){
    alert('Please fill transaction ID and upload proof'); return;
  }
  let amount=parseFloat(document.getElementById('depositAmount').value)||0;
  balance+=amount;
  dailyProfit+=amount*0.1;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  alert('Deposit successful');
  updateDashboard();
  showPage('dashboard');
}

function submitWithdraw(){
  let amount=parseFloat(document.getElementById('withdrawAmount').value)||0;
  if(amount>balance){alert('Insufficient balance'); return;}
  balance-=amount;
  localStorage.setItem('nexa_balance',balance);
  alert('Withdrawal requested');
  updateDashboard();
}

updateDepositNumber();
updateDashboard();
</script>

</body>
</html>
