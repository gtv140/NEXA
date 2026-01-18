<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --primary:#FFD700;
  --secondary:#FF4500;
  --bg:#1a1a1a;
  --text:#fff;
  --box-bg:rgba(255,215,0,0.1);
}
body{
  margin:0;
  font-family: Arial,sans-serif;
  background: var(--bg);
  color: var(--text);
  overflow-x:hidden;
}
header{
  text-align:center;
  padding:15px;
  font-size:28px;
  font-weight:bold;
  background: linear-gradient(90deg,var(--primary),var(--secondary));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.page{
  max-width:450px;
  margin:20px auto;
  padding:20px;
  background:rgba(255,255,255,0.02);
  border-radius:12px;
  border:1px solid rgba(255,215,0,0.1);
}
input,select,button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:none;
  background:rgba(255,255,255,0.05);
  color:#fff;
}
button{
  cursor:pointer;
  font-weight:bold;
  background: linear-gradient(90deg,var(--primary),var(--secondary));
  color:#000;
}
.nav{display:flex;justify-content:space-around;margin-top:10px;}
.nav div{cursor:pointer;text-align:center;}
.nav div .ico{display:block;font-size:20px;margin-bottom:4px;}
.hidden{display:none;}
.box{background:var(--box-bg);padding:15px;margin:10px 0;border-radius:10px;cursor:pointer;transition:0.3s;}
.box:hover{box-shadow:0 0 10px var(--primary);transform:translateY(-3px);}
</style>
</head>
<body>

<header>NEXA EARN</header>

<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
  <input id="user" placeholder="Username"/>
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
  <h3>Welcome, <span id="dashUser"></span></h3>
  <div id="balanceBox" class="box">Balance: Rs <span id="dashBalance">0</span></div>
  <div id="dailyBox" class="box">Daily Profit: Rs <span id="dashDaily">0</span></div>
  <div id="totalBox" class="box">Total Profit: Rs <span id="dashTotal">0</span></div>
  <div id="activeMembers" class="box">Active Members: <span>0</span></div>

  <div class="box" onclick="showSection('plans')">📦 Plans</div>
  <div class="box" onclick="showSection('ads')">🎬 Watch Ads & Earn</div>
  <div class="box" onclick="showSection('deposit')">💰 Deposit</div>
  <div class="box" onclick="showSection('withdraw')">💵 Withdraw</div>
  <div class="box" onclick="showSection('special')">⭐ Special Offers</div>
  <div class="box" onclick="showSection('invite')">👥 Invite Friends</div>
  <div class="box" onclick="showSection('company')">🏢 Company Info</div>
  <div class="box" onclick="showSection('support')">🛠️ Support</div>
  <div class="box" onclick="logout()">🔓 Logout</div>
</div>

<div id="plans" class="page hidden">
  <h2>Plans</h2>
  <div id="plansList"></div>
  <button onclick="showSection('dashboard')">Back</button>
</div>

<div id="ads" class="page hidden">
  <h2>Watch Ads & Earn</h2>
  <div id="adsList"></div>
  <button onclick="showSection('dashboard')">Back</button>
</div>

<div id="special" class="page hidden">
  <h2>Special Offers</h2>
  <div id="specialList"></div>
  <button onclick="showSection('dashboard')">Back</button>
</div>

<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex;gap:8px;align-items:center;margin-top:10px">
    <input id="depositNumber" readonly style="flex:1"/>
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount"/>
  <input id="depositTxId" placeholder="Transaction ID"/>
  <input type="file" id="depositProof"/>
  <button onclick="submitDeposit()">Submit Deposit</button>
  <button onclick="showSection('dashboard')">Back</button>
</div>

<div id="withdraw" class="page hidden">
  <h2>Withdrawal</h2>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number"/>
  <input id="withdrawAmount" placeholder="Amount"/>
  <button onclick="submitWithdraw()">Request Withdrawal</button>
  <button onclick="showSection('dashboard')">Back</button>
</div>

<div id="invite" class="page hidden">
  <h2>Invite Friends</h2>
  <p>Share your referral link and earn bonuses!</p>
  <input readonly value="https://nexaearn.com/referral?user=" id="refLink"/>
  <button onclick="copyReferral()">Copy Link</button>
  <button onclick="showSection('dashboard')">Back</button>
</div>

<div id="company" class="page hidden">
  <h2>About NEXA EARN</h2>
  <p>NEXA EARN has been providing digital investment solutions since 2022. Fast, secure, reliable returns. Special offers and daily rewards available for active users.</p>
  <button onclick="showSection('dashboard')">Back</button>
</div>

<div id="support" class="page hidden">
  <h2>Support</h2>
  <p>Contact our team via WhatsApp: <a href="https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu" target="_blank">Click Here</a></p>
  <p>Email: rock.earn92@gmail.com</p>
  <button onclick="showSection('dashboard')">Back</button>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let activeMembers = 100+Math.floor(Math.random()*4900);

function showSection(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id==='dashboard'? 'dashboard': id).classList.remove('hidden');
  if(id==='dashboard'){updateDashboard();}
}

function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  balance = 0; dailyProfit = 0; totalProfit = 0;
  localStorage.setItem('nexa_balance', balance);
  localStorage.setItem('nexa_daily', dailyProfit);
  localStorage.setItem('nexa_total', totalProfit);
  updateDashboard();
}

function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
  document.getElementById('activeMembers').innerText='Active Members: '+activeMembers;
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}
function submitWithdraw(){alert('Withdrawal requested');}
function copyReferral(){navigator.clipboard.writeText(document.getElementById('refLink').value); alert('Referral link copied');}

// Plans Example
const plans = [];
for(let i=1;i<=50;i++){
  plans.push({
    name:'Plan '+i,
    invest:200+i*50,
    days:25+Math.floor(i/2),
    multiplier:i<=5?2.4:2.2
  });
}

function showPlans(){
  const list=document.getElementById('plansList');
  list.innerHTML='';
  plans.forEach((p,i)=>{
    const div=document.createElement('div');
    div.className='box';
    div.innerHTML=`<b>${p.name}</b><br>Invest: Rs ${p.invest}<br>Days: ${p.days}<br>Total: Rs ${Math.round(p.invest*p.multiplier)}<br><button onclick="alert('Deposit Rs ${p.invest}')">Buy Now</button>`;
    list.appendChild(div);
  });
}
showPlans();
</script>
</body>
</html>
