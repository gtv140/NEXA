<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --black:#0a0a0a; --red:#ff0033; --blue:#00f0ff; --accent:#fff;
  --box-bg:#111; --glow-red:#ff0033aa; --glow-blue:#00f0ffaa;
}
body{margin:0;font-family:Arial,sans-serif;background:var(--black);color:var(--accent);}
header{text-align:center;font-size:28px;padding:15px;font-weight:bold;background:linear-gradient(90deg,var(--red),var(--blue));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:480px;margin:20px auto;padding:15px;background:var(--box-bg);border-radius:12px;box-shadow:0 0 10px var(--glow-red),0 0 20px var(--glow-blue);}
input,select,button{width:100%;padding:10px;margin:8px 0;border-radius:8px;border:none;background:#222;color:#fff;}
button{cursor:pointer;background:linear-gradient(90deg,var(--red),var(--blue));color:#000;font-weight:bold;box-shadow:0 0 10px var(--glow-red),0 0 20px var(--glow-blue);}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px 0;background:#111;box-shadow:0 0 10px var(--glow-red),0 0 20px var(--glow-blue);}
.nav div{text-align:center;cursor:pointer;color:#fff;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.box{background:#222;border-radius:10px;padding:12px;margin:8px 0;box-shadow:0 0 8px var(--glow-blue);}
.plan-card{background:#333;border-radius:10px;padding:12px;margin:8px 0;cursor:pointer;box-shadow:0 0 6px var(--glow-red);}
.plan-card:hover{background:#444;box-shadow:0 0 12px var(--glow-red),0 0 18px var(--glow-blue);}
img.random-photo{width:100%;height:150px;object-fit:cover;border-radius:10px;margin-bottom:10px;box-shadow:0 0 6px var(--glow-red),0 0 12px var(--glow-blue);}
</style>
</head>
<body>

<header>NEXA EARN</header>

<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="box" id="userBox">Username: <span id="dashUser">-</span></div>
  <div class="box" id="balanceBox">Balance: Rs <span id="dashBalance">0</span></div>
  <div class="box" id="dailyBox">Daily Profit: Rs <span id="dashDaily">0</span></div>
  <div class="box" id="activePlansBox">Active Plans: <span id="activePlans">0</span></div>
  <div class="box" id="activeMembersBox">Active Members: <span id="activeMembers">0</span></div>
  <h3>Company Info & Offers</h3>
  <p>NEXA EARN has been providing secure digital earning since 2022. Watch ads, buy plans, and earn daily profits!</p>
  <div id="photosContainer"></div>
  <h3>Ads Watch Plans</h3>
  <div id="adsPlansList"></div>
  <h3>Investment Plans</h3>
  <div id="plansList"></div>
  <button onclick="logout()">Logout</button>
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
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
</div>

<script>
// User data
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let activePlans = JSON.parse(localStorage.getItem('nexa_activePlans')||'[]');

// Pages
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}

// Login
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username');return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')){balance=0;dailyProfit=0;localStorage.setItem('nexa_balance',balance);localStorage.setItem('nexa_daily',dailyProfit);}
  updateDashboard();
}

// Logout
function logout(){currentUser=null;localStorage.removeItem('nexa_user');location.reload();}

// Dashboard
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('activePlans').innerText=activePlans.length;
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000+1);
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
}

// Deposit
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}

// Withdraw
function submitWithdraw(){alert('Withdrawal requested');}

// Photos
const photosContainer=document.getElementById('photosContainer');
for(let i=0;i<4;i++){let img=document.createElement('img');img.src=`https://picsum.photos/400/150?random=${i+1}`;img.classList.add('random-photo');photosContainer.appendChild(img);}

// Ads Plans
const adsPlansList=document.getElementById('adsPlansList');
for(let i=1;i<=7;i++){
  let adDiv=document.createElement('div');adDiv.classList.add('plan-card');
  adDiv.innerHTML=`<b>Ads Plan ${i}</b><br>Price: Rs ${500*i}<br>Daily Ads: ${3+i%3}<br>Days: 10<br><button onclick="buyAdsPlan(${500*i},'Ads Plan ${i}')">Buy Now</button>`;
  adsPlansList.appendChild(adDiv);
}
function buyAdsPlan(amount,name){alert(`Buy ${name} for Rs ${amount}`); showPage('deposit'); document.getElementById('depositAmount').value=amount;}

// Investment Plans
let plansList = document.getElementById('plansList');
for(let i=1;i<=50;i++){
  const planDiv=document.createElement('div');planDiv.classList.add('plan-card');
  planDiv.innerHTML=`<b>Plan ${i}</b><br>Investment: Rs ${200*i}<br>Days: ${25+Math.floor(Math.random()*46)}<br>Daily Profit: 2.2x<br>Total Profit: ${(200*i*2.2).toFixed(0)}<br><button onclick="buyPlan(${200*i},'Plan ${i}')">Buy Now</button>`;
  plansList.appendChild(planDiv);
}
function buyPlan(amount,name){alert(`Buy ${name} for Rs ${amount}`); showPage('deposit'); document.getElementById('depositAmount').value=amount;}

</script>
</body>
</html>
