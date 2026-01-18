<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --neon-red: #ff073a;
  --neon-blue: #0af0ff;
  --neon-gold: #ffd700;
  --dark-bg: #0a0a0a;
  --card-bg: rgba(255,255,255,0.05);
  --card-border: rgba(255,215,0,0.2);
}
body {
  margin:0;
  font-family: Arial, sans-serif;
  background: linear-gradient(-45deg,#0a0a0a,#0a0a0a,#111,#0a0a0a);
  background-size: 400% 400%;
  animation: bgAnimation 20s ease infinite;
  color:#fff;
  overflow-x:hidden;
}
@keyframes bgAnimation {
  0%{background-position:0% 50%}
  50%{background-position:100% 50%}
  100%{background-position:0% 50%}
}
header{
  text-align:center;
  font-size:28px;
  padding:20px;
  font-weight:bold;
  color: var(--neon-gold);
  text-shadow:0 0 10px var(--neon-gold),0 0 20px var(--neon-red);
}
.page{max-width:430px;margin:20px auto;padding:20px;background:var(--card-bg);border-radius:12px;border:1px solid var(--card-border);}
input, select, button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.1);background:transparent;color:#fff;}
button{background: linear-gradient(90deg,var(--neon-red),var(--neon-blue)); color:#001;font-weight:700;cursor:pointer;transition:0.3s;}
button:hover{transform:scale(1.05);}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px 0;background:rgba(0,0,0,0.8);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:22px;display:block;margin-bottom:4px;color:#ffd700;text-shadow:0 0 8px #0af0ff;}
.hidden{display:none;}
.card{background:rgba(0,0,0,0.3);padding:10px;margin:10px 0;border-radius:10px;border:1px solid rgba(255,215,0,0.2);cursor:pointer;transition:0.3s;}
.card:hover{box-shadow:0 0 20px var(--neon-red);}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;background:rgba(255,215,0,0.1);border-radius:10px;cursor:pointer;}
.support-icon:hover{box-shadow:0 0 15px var(--neon-gold);transform:translateY(-2px);}
</style>
</head>
<body>

<header>NEXA EARN</header>

<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User</option></select>
  <input id="user" placeholder="Username"/>
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
  <div id="infoBoxes" style="display:flex;flex-direction:column;gap:10px;">
    <div class="card">Username: <span id="dashUser"></span></div>
    <div class="card">Balance: Rs <span id="dashBalance">0</span></div>
    <div class="card">Daily Profit: Rs <span id="dashDaily">0</span></div>
    <div class="card">Total Profit: Rs <span id="dashTotal">0</span></div>
    <div class="card">Active Members: <span id="activeMembers">0</span></div>
  </div>
  <h3>Company Highlights</h3>
  <div style="display:flex;gap:5px;overflow-x:auto;">
    <img src="https://picsum.photos/100/80?random=1" style="border-radius:8px;">
    <img src="https://picsum.photos/100/80?random=2" style="border-radius:8px;">
    <img src="https://picsum.photos/100/80?random=3" style="border-radius:8px;">
    <img src="https://picsum.photos/100/80?random=4" style="border-radius:8px;">
    <img src="https://picsum.photos/100/80?random=5" style="border-radius:8px;">
  </div>
  <button onclick="showPage('plansPage')">View Plans</button>
  <button onclick="showPage('deposit')">Deposit</button>
  <button onclick="showPage('withdrawal')">Withdraw</button>
  <button onclick="logout()">Logout</button>
</div>

<div id="plansPage" class="page hidden">
  <h2>Investment Plans</h2>
  <div id="plansList"></div>
  <button onclick="showPage('dashboard')">Back</button>
</div>

<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex; gap:8px; align-items:center; margin-top:10px">
    <input id="depositNumber" readonly style="flex:1"/>
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount"/>
  <input id="depositTxId" placeholder="Transaction ID"/>
  <input type="file" id="depositProof"/>
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawal" class="page hidden">
  <h2>Withdrawal</h2>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number"/>
  <input id="withdrawAmount" placeholder="Amount"/>
  <button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plansPage')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username');return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')){balance=0;dailyProfit=0;totalProfit=0;localStorage.setItem('nexa_balance',balance);localStorage.setItem('nexa_daily',dailyProfit);localStorage.setItem('nexa_total',totalProfit);}
  updateDashboard();
}
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); showPage('loginPage');}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashTotal').innerText=totalProfit;
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000+100);
  showPage('dashboard');
  document.getElementById('bottomNav').classList.remove('hidden');
}

let plansData=[];
for(let i=1;i<=50;i++){
  let invest=200+i*600; 
  let days=25+Math.floor(i*0.9); 
  let multiplier=i<=5?2.4:2.2; 
  plansData.push({id:i,name:'Plan '+i,invest:invest,days:days,multiplier:multiplier,total:Math.round(invest*multiplier),offer:i<=5});
}

function showPlans(){
  const container=document.getElementById('plansList');
  container.innerHTML='';
  plansData.forEach(p=>{
    let card=document.createElement('div');
    card.className='card';
    card.innerHTML=`${p.offer?'<span style="color:var(--neon-gold)">🔥Special Offer</span><br>':''}<b>${p.name}</b><br>Investment: Rs ${p.invest}<br>Days: ${p.days}<br>Total: Rs ${p.total}<br><button onclick="buyPlan(${p.id})">Buy Now</button>`;
    container.appendChild(card);
  });
}

function buyPlan(id){
  const plan=plansData.find(p=>p.id===id);
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){
  alert('Deposit submitted, pending approval'); 
  let amt=parseFloat(document.getElementById('depositAmount').value)||0;
  balance+=amt;dailyProfit+=amt*0.05;totalProfit+=amt*1.1;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  updateDashboard();
}
function submitWithdraw(){
  alert('Withdrawal request submitted');
}

showPlans();
if(currentUser){updateDashboard();}else{showPage('loginPage');}
</script>
</body>
</html>
