<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --primary: #ff0044; /* red neon */
  --secondary: #00f7ff; /* blue neon */
  --accent: #ffcc00; /* golden */
  --bg: #0a0a0a; /* black background */
}
body {
  margin:0;
  font-family: Arial, sans-serif;
  background: var(--bg);
  color:#fff;
  overflow-x:hidden;
}
header {
  text-align:center;
  font-size:28px;
  padding:20px;
  background: linear-gradient(90deg,var(--primary),var(--secondary),var(--accent));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.page { max-width:480px; margin:20px auto; padding:20px; border-radius:12px; background:rgba(255,255,255,0.03);}
input,select,button { width:100%; padding:10px; margin-top:10px; border-radius:8px; border:none; background: rgba(255,255,255,0.05); color:#fff; }
button { background: linear-gradient(90deg,var(--primary),var(--secondary),var(--accent)); font-weight:700; cursor:pointer; transition:0.3s; }
button:hover { transform:scale(1.05); }
.box { padding:15px; margin:10px 0; background:rgba(255,255,255,0.05); border-radius:12px; border:1px solid rgba(255,204,0,0.2); box-shadow:0 0 10px rgba(0,255,255,0.4); transition:0.3s;}
.box:hover { box-shadow:0 0 20px var(--secondary),0 0 25px var(--primary);}
.icon-box { display:flex; flex-wrap:wrap; gap:10px; margin:15px 0;}
.icon-item { flex:1 1 45%; background:rgba(255,255,255,0.05); border-radius:12px; padding:15px; text-align:center; cursor:pointer; border:1px solid rgba(255,204,0,0.2); transition:0.3s; }
.icon-item:hover { box-shadow:0 0 15px var(--secondary), 0 0 20px var(--primary); transform:scale(1.05);}
img { width:100%; border-radius:12px; margin-top:10px;}
.carousel { display:flex; overflow-x:auto; scroll-behavior:smooth; gap:10px; animation:autoSlide 15s linear infinite;}
.carousel img { flex:0 0 300px; height:150px; object-fit:cover; border:1px solid rgba(255,255,255,0.2);}
@keyframes autoSlide { 0%{transform:translateX(0);} 25%{transform:translateX(-25%);} 50%{transform:translateX(-50%);} 75%{transform:translateX(-75%);} 100%{transform:translateX(0);} }
.ad-task { padding:10px; margin:10px 0; background: rgba(0,0,0,0.2); border-radius:12px; text-align:center; box-shadow:0 0 10px var(--primary);}
.ad-task button { margin-top:10px; background: linear-gradient(90deg,var(--secondary),var(--accent)); transition:0.3s;}
.ad-task button:hover { transform:scale(1.05);}
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
<div class="box">
<strong>Username:</strong> <span id="dashUser">-</span><br>
<strong>Balance:</strong> Rs <span id="dashBalance">0</span><br>
<strong>Daily Profit:</strong> Rs <span id="dashDaily">0</span><br>
<strong>Total Profit:</strong> Rs <span id="dashTotal">0</span><br>
<strong>Active Members:</strong> <span id="activeMembers">0</span>
</div>

<div class="box">
<h3>About NEXA EARN</h3>
<p>Operating since 2022, NEXA EARN provides fast digital investment opportunities. Earn daily by watching ads or buying plans.</p>
<div class="carousel">
<img src="https://picsum.photos/400/200?random=1"/>
<img src="https://picsum.photos/400/200?random=2"/>
<img src="https://picsum.photos/400/200?random=3"/>
<img src="https://picsum.photos/400/200?random=4"/>
<img src="https://picsum.photos/400/200?random=5"/>
</div>
</div>

<div class="icon-box">
<div class="icon-item" onclick="showPage('plans')">📦<br>Plans</div>
<div class="icon-item" onclick="showPage('ads')">🎬<br>Watch Ads</div>
<div class="icon-item" onclick="showPage('deposit')">💰<br>Deposit</div>
<div class="icon-item" onclick="showPage('withdrawal')">💵<br>Withdraw</div>
<div class="icon-item" onclick="showPage('history')">🕒<br>History</div>
<div class="icon-item" onclick="showPage('support')">🛠️<br>Support</div>
<div class="icon-item" onclick="invite()">👥<br>Invite Friends</div>
</div>

<button onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h2>Investment Plans</h2>
<div id="plansList"></div>
</div>

<!-- ADS -->
<div id="ads" class="page hidden">
<h2>Watch Ads</h2>
<div id="adsList"></div>
<div id="adsTasks"></div>
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
<p>Contact via WhatsApp: <a href="https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu" target="_blank">Join Group</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
</div>

<script>
// User Data
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let lastDaily = parseInt(localStorage.getItem('nexa_lastDay')||0);
let userAdsTasks = JSON.parse(localStorage.getItem('nexa_adsTasks')||'{}');

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
  if(id==='ads') renderAdsTasks();
}

function login(){
  const u = document.getElementById('user').value.trim();
  const p = document.getElementById('pass').value.trim();
  if(!u || !p){alert('Enter username and password'); return;}
  currentUser = u;
  localStorage.setItem('nexa_user', currentUser);
  if(!localStorage.getItem('nexa_balance')) localStorage.setItem('nexa_balance',0);
  if(!localStorage.getItem('nexa_daily')) localStorage.setItem('nexa_daily',0);
  if(!localStorage.getItem('nexa_total')) localStorage.setItem('nexa_total',0);
  balance = parseFloat(localStorage.getItem('nexa_balance'));
  dailyProfit = parseFloat(localStorage.getItem('nexa_daily'));
  totalProfit = parseFloat(localStorage.getItem('nexa_total'));
  const today = new Date().getDate();
  if(today!==lastDaily){ dailyProfit=0; lastDaily=today; localStorage.setItem('nexa_lastDay',lastDaily); localStorage.setItem('nexa_daily',dailyProfit);}
  if(!userAdsTasks[currentUser]) userAdsTasks[currentUser]=[];
  localStorage.setItem('nexa_adsTasks',JSON.stringify(userAdsTasks));
  updateDashboard();
}

function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}

function updateDepositNumber(){document.getElementById('depositNumber').value=document.getElementById('depositMethod').value==='jazzcash'?'03705519562':'03379827882';}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){const amount = parseFloat(document.getElementById('depositAmount').value); if(!amount){alert('Enter amount'); return;} balance += amount; totalProfit += amount*0.1; dailyProfit += amount*0.05; localStorage.setItem('nexa_balance',balance); localStorage.setItem('nexa_total',totalProfit); localStorage.setItem('nexa_daily',dailyProfit); alert('Deposit submitted'); updateDashboard();}
function submitWithdraw(){const amount = parseFloat(document.getElementById('withdrawAmount').value); if(!amount || amount>balance){alert('Invalid amount'); return;} balance -= amount; localStorage.setItem('nexa_balance',balance); alert('Withdrawal requested'); updateDashboard();}
function invite(){prompt('Share this code with friends: NEXA123');}

// Plans
let plans=[];
for(let i=1;i<=50;i++){plans.push({id:i,name:'Plan '+i,invest:200+i*50,days:25+Math.floor(i*2),multiplier:i<=5?2.4:2.2,special:i<=5});}
function showPlans(){let html=''; plans.forEach(p=>{html+=`<div class="box"><strong>${p.name} ${p.special?'[Special Offer]':''}</strong><br>Invest: Rs ${p.invest}<br>Days: ${p.days}<br>Total: Rs ${Math.round(p.invest*p.multiplier)}<br><button onclick="buyPlan(${p.id})">Buy Now</button></div>`;}); document.getElementById('plansList').innerHTML=html;}
function buyPlan(id){const plan = plans.find(p=>p.id===id); document.getElementById('depositAmount').value=plan.invest; showPage('deposit');}

// Ads
let adsPlans=[];
for(let i=1;i<=7;i++){adsPlans.push({id:i,name:'Ads Plan '+i, invest:500+i*50, days:10, dailyAds:3});}
function showAds(){let html=''; adsPlans.forEach(a=>{html+=`<div class="box"><strong>${a.name}</strong><br>Invest: Rs ${a.invest}<br>Days: ${a.days}<br>Daily Ads: ${a.dailyAds}<br><button onclick="buyAds(${a.id})">Buy Now</button></div>`;}); document.getElementById('adsList').innerHTML=html;}
function buyAds(id){const ad = adsPlans.find(a=>a.id===id); document.getElementById('depositAmount').value=ad.invest; if(!userAdsTasks[currentUser]) userAdsTasks[currentUser]=[]; userAdsTasks[currentUser].push({planId:id, remainingDays:ad.days, dailyAds:ad.dailyAds, completed:0}); localStorage.setItem('nexa_adsTasks',JSON.stringify(userAdsTasks)); showPage('deposit');}
function renderAdsTasks(){const tasksDiv = document.getElementById('adsTasks'); tasksDiv.innerHTML=''; if(!userAdsTasks[currentUser] || userAdsTasks[currentUser].length===0){ tasksDiv.innerHTML='<p>No active ads plans. Buy plan to start tasks.</p>'; return;} userAdsTasks[currentUser].forEach((task,index)=>{const planInfo = adsPlans.find(a=>a.id===task.planId); const div = document.createElement('div'); div.className='ad-task'; div.innerHTML=`<strong>${planInfo.name}</strong><br>Remaining Days: ${task.remainingDays}<br>Daily Ads: ${task.dailyAds - task.completed}<br><button onclick="watchAd(${index})">Watch Ad</button><div style="margin-top:8px;"><strong>Coming Soon:</strong><br><img src="https://picsum.photos/100/100?random=${index+10}" style="border-radius:50%; margin-top:5px;"></div>`; tasksDiv.appendChild(div);});}
function watchAd(index){const task = userAdsTasks[currentUser][index]; if(task.completed >= task.dailyAds){alert('Daily ads completed'); return;} task.completed++; balance += 10; dailyProfit += 10; localStorage.setItem('nexa_balance',balance); localStorage.setItem('nexa_daily',dailyProfit); localStorage.setItem('nexa_adsTasks',JSON.stringify(userAdsTasks)); alert('Ad watched! Rs 10 added'); renderAdsTasks(); updateDashboard();}

// Init
if(currentUser) login();
showPlans(); showAds();
</script>
</body>
</html>
