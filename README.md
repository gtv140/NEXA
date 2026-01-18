<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
:root{
  --gold:#FFD700;
  --accent:#FF4500;
  --dark:#111;
  --bg:#1a1a1a;
}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:#fff;overflow-x:hidden;}
header{text-align:center;padding:20px;font-size:30px;font-weight:800;background:linear-gradient(90deg,var(--gold),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:480px;margin:20px auto;padding:20px;background:rgba(255,255,255,0.03);border-radius:15px;box-shadow:0 8px 20px rgba(0,0,0,0.5);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.1);background:transparent;color:#fff;outline:none;}
button{cursor:pointer;font-weight:700;background:linear-gradient(90deg,var(--gold),var(--accent));color:#111;transition:.2s;}
button:hover{transform:translateY(-2px);box-shadow:0 6px 15px rgba(0,0,0,0.5);}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:12px;background:rgba(0,0,0,0.85);border-top:1px solid rgba(255,255,255,0.05);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:22px;margin-bottom:4px;}
.hidden{display:none;}
.user-box{display:flex;justify-content:space-between;align-items:center;padding:15px;border-radius:12px;background:linear-gradient(90deg,rgba(255,215,0,0.1),rgba(255,69,0,0.1));margin-bottom:15px;}
.plan{padding:12px;border-radius:12px;margin-bottom:10px;background:rgba(255,255,255,0.02);border:1px solid rgba(255,215,0,0.1);}
.plan img{width:100%;border-radius:8px;margin-bottom:6px;}
.ad-box{padding:10px;margin:10px 0;background:rgba(255,215,0,0.1);border-radius:10px;text-align:center;}
.countdown{font-weight:700;color:#FFD700;}
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
<div class="user-box">
<span>Username: <b id="dashUser">-</b></span>
<span>Balance: Rs <b id="dashBalance">0</b></span>
</div>
<div class="user-box">
<span>Daily Profit: Rs <b id="dashDaily">0</b></span>
<span>Total Profit: Rs <b id="dashTotal">0</b></span>
</div>
<div class="user-box">
<span>Active Members: <b id="activeMembers">0</b></span>
</div>
<div id="adsBox"></div>
<div id="plansList"></div>
<button onclick="logout()">Logout</button>
</div>

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
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

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

<div id="about" class="page hidden">
<h2>About NEXA EARN</h2>
<p>NEXA EARN is running since 2022, providing fast & secure digital investment with daily profits. Enjoy premium features, random ads, and active member tracking.</p>
<div class="ad-box">Random Photo / Ad Space</div>
<div class="ad-box">Random Photo / Ad Space</div>
<div class="ad-box">Random Photo / Ad Space</div>
</div>

<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span class='ico'>🏠</span>Home</div>
<div onclick="showPage('plans')"><span class='ico'>📦</span>Plans</div>
<div onclick="showPage('deposit')"><span class='ico'>💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class='ico'>💵</span>Withdraw</div>
<div onclick="showPage('about')"><span class='ico'>ℹ️</span>About</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let activeMembers = Math.floor(Math.random()*5000+1000);

function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  balance = parseFloat(localStorage.getItem('nexa_balance_'+u)||'0');
  dailyProfit = parseFloat(localStorage.getItem('nexa_daily_'+u)||'0');
  totalProfit = parseFloat(localStorage.getItem('nexa_total_'+u)||'0');
  updateDashboard();
}
function logout(){localStorage.setItem('nexa_balance_'+currentUser,balance);localStorage.setItem('nexa_daily_'+currentUser,dailyProfit);localStorage.setItem('nexa_total_'+currentUser,totalProfit);currentUser=null;location.reload();}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
  document.getElementById('activeMembers').innerText=activeMembers;
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
}
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted'); balance+=parseFloat(document.getElementById('depositAmount').value||0); totalProfit+=parseFloat(document.getElementById('depositAmount').value||0); dailyProfit+=parseFloat(document.getElementById('depositAmount').value||0); localStorage.setItem('nexa_balance_'+currentUser,balance); localStorage.setItem('nexa_total_'+currentUser,totalProfit); localStorage.setItem('nexa_daily_'+currentUser,dailyProfit); updateDashboard();}
function submitWithdraw(){alert('Withdrawal requested');}

function loadPlans(){
  const plansList = document.getElementById('plansList');
  plansList.innerHTML='';
  for(let i=1;i<=50;i++){
    const plan=document.createElement('div');
    plan.className='plan';
    let multiplier = i<=5?2.4:2.2;
    let days = 25 + i;
    let invest = 200 + i*100;
    plan.innerHTML=`<img src='https://picsum.photos/400/150?random=${i}'><b>Plan ${i}</b><br>Invest: Rs ${invest}<br>Days: ${days}<br>Total Profit: Rs ${Math.round(invest*multiplier)}<br><button onclick="buyPlan(${invest})">Buy Now</button>`;
    plansList.appendChild(plan);
  }
}
function buyPlan(amount){
  showPage('deposit');
  document.getElementById('depositAmount').value=amount;
  updateDepositNumber();
}

// Load dashboard or login
if(currentUser){updateDashboard(); loadPlans();} else{showPage('loginPage');loadPlans();}

// Ads-watch example
const adsBox=document.getElementById('adsBox');
for(let i=1;i<=3;i++){
  const ad=document.createElement('div');
  ad.className='ad-box';
  ad.innerText=`Watch Ad ${i}: Earn Rs ${50*i}`;
  adsBox.appendChild(ad);
}
</script>
</body>
</html>
