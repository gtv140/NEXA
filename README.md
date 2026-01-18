<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root{
  --primary:#1f1f1f;
  --accent:#00cfff;
  --accent2:#ff5caa;
  --text:#fff;
  --bg:#111;
}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:var(--text);overflow-x:hidden;}
header{text-align:center;padding:20px;font-size:28px;font-weight:700;background:linear-gradient(90deg,var(--accent),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:500px;margin:20px auto;background:rgba(255,255,255,0.03);padding:20px;border-radius:12px;border:1px solid rgba(0,255,240,0.06);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(0,255,240,0.1);background:transparent;color:var(--text);}
button{background:linear-gradient(90deg,var(--accent),var(--accent2));color:#000;font-weight:700;cursor:pointer;transition:0.2s;}
button:hover{opacity:0.9;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px 0;background:rgba(0,0,0,0.9);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.box{background:rgba(255,255,255,0.05);padding:15px;border-radius:12px;margin-bottom:12px;border:1px solid rgba(0,255,240,0.06);}
.box h3{margin:0;font-size:16px;margin-bottom:6px;}
.photo-grid{display:flex;flex-wrap:wrap;gap:6px;}
.photo-grid img{width:calc(50% - 3px);border-radius:10px;}
.plan-box{background:rgba(255,255,255,0.05);padding:10px;margin:10px 0;border-radius:10px;border:1px solid rgba(0,255,240,0.06);display:flex;justify-content:space-between;align-items:center;cursor:pointer;}
.plan-box:hover{box-shadow:0 5px 20px rgba(0,255,240,0.2);}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<input id="username" placeholder="Username">
<input id="password" placeholder="Password" type="password">
<select id="userOption"><option value="login">Login</option><option value="signup">Signup</option></select>
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="box">
<h3>Username: <span id="dashUser"></span></h3>
<p>Balance: Rs <span id="dashBalance">0</span></p>
<p>Daily Profit: Rs <span id="dashDaily">0</span></p>
<p>Total Profit: Rs <span id="dashTotal">0</span></p>
<p>Active Members: <span id="activeMembers">0</span></p>
</div>

<div class="box">
<h3>Company Info</h3>
<p>NEXA EARN provides fast, secure, and reliable digital investment plans. Join and grow your profits safely.</p>
<div class="photo-grid" id="photoGrid"></div>
</div>

<button onclick="showPage('plans')">View Plans</button>
<button onclick="showPage('deposit')">Deposit</button>
<button onclick="showPage('withdrawal')">Withdraw</button>
<button onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h2>Investment Plans</h2>
<div id="plansList"></div>
<button onclick="showPage('dashboard')">Back</button>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h2>Deposit</h2>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<div style="display:flex;gap:6px;margin-top:10px">
<input id="depositNumber" readonly style="flex:1">
<button onclick="copyDepositNumber()">Copy</button>
</div>
<input id="depositAmount" placeholder="Enter Amount">
<input id="depositTxId" placeholder="Transaction ID">
<input type="file" id="depositProof">
<button onclick="submitDeposit()">Submit Deposit</button>
<button onclick="showPage('dashboard')">Back</button>
</div>

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
<h2>Withdrawal</h2>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="withdrawAccount" placeholder="Account Number">
<input id="withdrawAmount" placeholder="Amount">
<button onclick="submitWithdraw()">Request Withdrawal</button>
<button onclick="showPage('dashboard')">Back</button>
</div>

<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let activeMembers = parseInt(localStorage.getItem('nexa_activeMembers')||Math.floor(Math.random()*5000)+100);
let plansData=[];

// ===== RANDOM PHOTOS =====
const photoUrls=[
"https://picsum.photos/200/150?random=1",
"https://picsum.photos/200/150?random=2",
"https://picsum.photos/200/150?random=3",
"https://picsum.photos/200/150?random=4",
"https://picsum.photos/200/150?random=5"
];
function showPhotos(){
const grid=document.getElementById('photoGrid');
grid.innerHTML='';
photoUrls.forEach(u=>{const img=document.createElement('img');img.src=u;grid.appendChild(img);});
}

// ===== PLANS =====
for(let i=1;i<=50;i++){
let invest=200+(i-1)*200;
let days=25+Math.floor((i-1)/5);
let multiplier=i<=5?2.4:2.2;
plansData.push({id:i,name:`Plan ${i}`,invest,total:Math.round(invest*multiplier),daily:Math.round(invest*multiplier/days),days});
}
function renderPlans(){
const list=document.getElementById('plansList');
list.innerHTML='';
plansData.forEach(p=>{
let div=document.createElement('div');
div.className='plan-box';
div.innerHTML=`<div><b>${p.name}</b><br>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div>
<div><button onclick="buyPlan(${p.id})">Buy Now</button></div>`;
list.appendChild(div);
});
}

// ===== LOGIN / DASHBOARD =====
function login(){
const u=document.getElementById('username').value.trim();
const p=document.getElementById('password').value.trim();
const option=document.getElementById('userOption').value;
if(!u||!p){alert('Enter username & password');return;}
currentUser=u; localStorage.setItem('nexa_user',currentUser);
balance=dailyProfit=totalProfit=0;
localStorage.setItem('nexa_balance',balance);
localStorage.setItem('nexa_daily',dailyProfit);
localStorage.setItem('nexa_total',totalProfit);
localStorage.setItem('nexa_activeMembers',activeMembers);
updateDashboard();
}
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}
function updateDashboard(){
document.getElementById('dashUser').innerText=currentUser;
document.getElementById('dashBalance').innerText=balance;
document.getElementById('dashDaily').innerText=dailyProfit;
document.getElementById('dashTotal').innerText=totalProfit;
document.getElementById('activeMembers').innerText=activeMembers;
document.getElementById('loginPage').classList.add('hidden');
document.getElementById('dashboard').classList.remove('hidden');
document.getElementById('bottomNav').classList.remove('hidden');
showPhotos();
}

// ===== PLANS BUY =====
function buyPlan(id){
let plan=plansData.find(p=>p.id===id);
if(balance<plan.invest){alert('Insufficient balance!');return;}
balance-=plan.invest;
dailyProfit+=plan.daily;
totalProfit+=plan.total;
localStorage.setItem('nexa_balance',balance);
localStorage.setItem('nexa_daily',dailyProfit);
localStorage.setItem('nexa_total',totalProfit);
alert(`You bought ${plan.name}`);
}

// ===== DEPOSIT =====
function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value);alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}

// ===== WITHDRAW =====
function submitWithdraw(){alert('Withdrawal requested');}

// ===== PAGE NAV =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
</script>
</body>
</html>
