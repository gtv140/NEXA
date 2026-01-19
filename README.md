<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN APP LOOK</title>
<style>
:root {
  --primary:#ff0044;
  --secondary:#00fff7;
  --accent:#ffcc00;
  --bg:#111;
  --text:#fff;
  --box:#222;
}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:var(--text);}
header{font-size:28px;text-align:center;padding:20px;background:linear-gradient(90deg,var(--primary),var(--secondary));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:480px;margin:20px auto;padding:15px;border-radius:15px;background:rgba(255,255,255,0.03);border:1px solid rgba(255,204,0,0.2);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:10px;border:none;background: rgba(255,255,255,0.05);color:#fff;font-weight:600;}
button{background:linear-gradient(90deg,var(--primary),var(--accent));cursor:pointer;transition:0.3s;}
button:hover{opacity:0.85;}
.hidden{display:none;}
.box{padding:15px;margin:10px 0;background:var(--box);border-radius:15px;border:1px solid rgba(255,204,0,0.2);}
.box:hover{box-shadow:0 5px 20px rgba(255,204,0,0.4);}
.icon-box{display:flex;flex-wrap:wrap;gap:10px;margin:15px 0;}
.icon-item{flex:1 1 45%;background:var(--box);border-radius:15px;padding:20px;text-align:center;cursor:pointer;border:1px solid rgba(255,204,0,0.2);transition:0.3s;}
.icon-item:hover{box-shadow:0 6px 25px rgba(255,204,0,0.4);}
img{width:100%;border-radius:15px;margin-top:10px;}
.slide-container{display:flex;overflow-x:auto;scroll-snap-type:x mandatory;gap:10px;padding-bottom:10px;}
.slide-container::-webkit-scrollbar{display:none;}
.slide-box{min-width:200px;flex-shrink:0;background:var(--box);padding:15px;border-radius:15px;scroll-snap-align:start;transition:0.3s;}
.slide-box:hover{box-shadow:0 5px 20px rgba(255,204,0,0.4);}
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
<input id="user" placeholder="Username"/>
<input id="pass" placeholder="Password" type="password"/>
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="slide-container">
<div class="slide-box"><strong>Username:</strong> <span id="dashUser">-</span></div>
<div class="slide-box"><strong>Balance:</strong> Rs <span id="dashBalance">0</span></div>
<div class="slide-box"><strong>Daily Profit:</strong> Rs <span id="dashDaily">0</span></div>
<div class="slide-box"><strong>Total Profit:</strong> Rs <span id="dashTotal">0</span></div>
<div class="slide-box"><strong>Active Members:</strong> <span id="activeMembers">0</span></div>
</div>

<div class="box" id="companyInfo">
<h3>About NEXA EARN</h3>
<p>Operating since 2022, NEXA EARN provides fast digital investment growth. Buy plans or watch ads to earn daily rewards.</p>
<div class="slide-container">
<img src="https://picsum.photos/400/200?random=1"/>
<img src="https://picsum.photos/400/200?random=2"/>
<img src="https://picsum.photos/400/200?random=3"/>
<img src="https://picsum.photos/400/200?random=4"/>
</div>
</div>

<div class="icon-box">
<div class="icon-item" onclick="showPage('plans')">📦<br>Plans</div>
<div class="icon-item" onclick="showPage('ads')">🎬<br>Watch Ads</div>
<div class="icon-item" onclick="showPage('deposit')">💰<br>Deposit</div>
<div class="icon-item" onclick="showPage('withdrawal')">💵<br>Withdraw</div>
<div class="icon-item" onclick="showPage('history')">🕒<br>History</div>
<div class="icon-item" onclick="showPage('support')">🛠️<br>Support</div>
<div class="icon-item" onclick="invite()">👥<br>Invite</div>
</div>

<button onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h2>Investment Plans</h2>
<div id="plansList"></div>
</div>

<!-- ADS WATCH -->
<div id="ads" class="page hidden">
<h2>Watch Ads</h2>
<div id="adsList"></div>
</div>

<!-- DEPOSIT -->
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

<!-- WITHDRAWAL -->
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

<!-- HISTORY -->
<div id="history" class="page hidden">
<h2>History</h2>
<div id="historyList"></div>
</div>

<!-- SUPPORT -->
<div id="support" class="page hidden">
<h2>Support</h2>
<p>WhatsApp: <a href="https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu" target="_blank">Join Group</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
</div>

<script>
// LocalStorage user data
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser||'-';
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000)+100;
  showPage('dashboard');
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function login(){
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert('Enter username & password');return;}
  currentUser=u;localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')) localStorage.setItem('nexa_balance',0);
  if(!localStorage.getItem('nexa_daily')) localStorage.setItem('nexa_daily',0);
  if(!localStorage.getItem('nexa_total')) localStorage.setItem('nexa_total',0);
  balance=parseFloat(localStorage.getItem('nexa_balance'));
  dailyProfit=parseFloat(localStorage.getItem('nexa_daily'));
  totalProfit=parseFloat(localStorage.getItem('nexa_total'));
  updateDashboard();
}

function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}

function submitDeposit(){
  const amount=parseFloat(document.getElementById('depositAmount').value);
  if(!amount){alert('Enter amount');return;}
  balance+=amount;totalProfit+=amount*0.1;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_total',totalProfit);
  alert('Deposit submitted');updateDashboard();
}

function submitWithdraw(){
  const amount=parseFloat(document.getElementById('withdrawAmount').value);
  if(!amount||amount>balance){alert('Invalid amount');return;}
  balance-=amount;
  localStorage.setItem('nexa_balance',balance);
  alert('Withdrawal requested');updateDashboard();
}

function invite(){prompt('Share code: NEXA123');}

// Plans
let plans=[];for(let i=1;i<=50;i++){plans.push({id:i,name:'Plan '+i,invest:200+i*50,days:25+Math.floor(i*2),multiplier:i<=5?2.4:2.2,special:i<=5});}
function showPlans(){let html='';plans.forEach(p=>{html+=`<div class="box"><strong>${p.name} ${p.special?'[Special]':''}</strong><br>Invest: Rs ${p.invest}<br>Days: ${p.days}<br>Total: Rs ${Math.round(p.invest*p.multiplier)}<br><button onclick="buyPlan(${p.id})">Buy Now</button></div>`;});document.getElementById('plansList').innerHTML=html;}
function buyPlan(id){const plan=plans.find(p=>p.id===id);document.getElementById('depositAmount').value=plan.invest;showPage('deposit');}

// Ads watch plans
let adsPlans=[];for(let i=1;i<=7;i++){adsPlans.push({id:i,name:'Ads Plan '+i, invest:500+i*50, days:10});}
function showAds(){let html='';adsPlans.forEach(a=>{html+=`<div class="box"><strong>${a.name}</strong><br>Invest: Rs ${a.invest}<br>Days: ${a.days}<br><button onclick="buyAds(${a.id})">Buy Now</button></div>`;});document.getElementById('adsList').innerHTML=html;}
function buyAds(id){const ad=adsPlans.find(a=>a.id===id);document.getElementById('depositAmount').value=ad.invest;showPage('deposit');}

// Init
if(currentUser) updateDashboard();showPlans();showAds();
</script>
</body>
</html>
