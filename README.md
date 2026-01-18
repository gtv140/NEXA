<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root{--primary:#0d6efd;--secondary:#6c757d;--bg:#f5f5f5;--white:#fff;}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:#000;}
header{background:var(--primary);color:var(--white);text-align:center;padding:20px;font-size:26px;font-weight:bold;}
.page{max-width:480px;margin:20px auto;padding:20px;background:var(--white);border-radius:12px;box-shadow:0 4px 20px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid #ccc;font-size:14px;}
button{background:var(--primary);color:#fff;font-weight:700;cursor:pointer;transition:0.2s;}
button:hover{opacity:0.9;}
.hidden{display:none;}
.nav{position:fixed;bottom:0;left:0;right:0;background:var(--white);display:flex;justify-content:space-around;padding:10px 0;box-shadow:0 -2px 10px rgba(0,0,0,0.1);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.user-box{display:flex;justify-content:space-between;align-items:center;padding:12px;background:#f0f0f0;border-radius:10px;margin-bottom:12px;}
.user-box .left{font-weight:bold;}
.user-box .right{font-weight:bold;}
.plan-box{display:flex;justify-content:space-between;align-items:center;padding:12px;border:1px solid #ccc;border-radius:10px;margin-bottom:10px;}
.plan-box .meta{flex:1;}
.plan-box .meta b{display:block;}
.plan-box .actions button{width:100px;}
.support-box, .about-box{padding:12px;background:#f0f0f0;border-radius:10px;margin-bottom:12px;}
.ad-photo{width:100%;height:120px;margin-bottom:10px;border-radius:10px;object-fit:cover;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN / SIGNUP -->
<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
<input id="user" placeholder="Username"/>
<input id="pass" placeholder="Password" type="password"/>
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="user-box">
  <div class="left" id="dashUser">—</div>
  <div class="right">
    Balance: Rs <span id="dashBalance">0</span><br>
    Daily Profit: Rs <span id="dashDaily">0</span><br>
    Total Profit: Rs <span id="dashTotal">0</span><br>
    Active Members: <span id="activeMembers">0</span>
  </div>
</div>

<div id="adsPhotos"></div>

<button onclick="showPage('plans')">View Plans</button>
<button onclick="showPage('deposit')">Deposit</button>
<button onclick="showPage('withdrawal')">Withdraw</button>
<button onclick="showPage('about')">About</button>
<button onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h2>Plans</h2>
<div id="plansList"></div>
<button onclick="showPage('dashboard')">Back to Dashboard</button>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h2>Deposit</h2>
<select id="depositMethod" onchange="updateDepositNumber()">
  <option value="jazzcash">JazzCash</option>
  <option value="easypaisa">EasyPaisa</option>
</select>
<div style="display:flex;gap:8px;align-items:center;margin-top:10px;">
<input id="depositNumber" readonly style="flex:1"/>
<button onclick="copyDepositNumber()">Copy</button>
</div>
<input id="depositAmount" placeholder="Enter Amount"/>
<input id="depositTxId" placeholder="Transaction ID"/>
<input type="file" id="depositProof"/>
<button onclick="submitDeposit()">Submit Deposit</button>
<button onclick="showPage('dashboard')">Back to Dashboard</button>
</div>

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
<h2>Withdraw</h2>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="withdrawAccount" placeholder="Account Number"/>
<input id="withdrawAmount" placeholder="Amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
<button onclick="showPage('dashboard')">Back to Dashboard</button>
</div>

<!-- ABOUT -->
<div id="about" class="page hidden">
<h2>About NEXA EARN</h2>
<div class="about-box">
<p>NEXA EARN provides fast, secure, and reliable digital investment opportunities. Our platform is designed to maximize profits while keeping your investments safe. Join thousands of active members and start earning today!</p>
<div class="support-box" onclick="openSupport()">🛠️ Contact Support</div>
</div>
<button onclick="showPage('dashboard')">Back to Dashboard</button>
</div>

<!-- NAV -->
<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
<div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let activeMembers = parseInt(localStorage.getItem('nexa_activeMembers')||Math.floor(Math.random()*5000)+1);
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');

// ===== ADS & PHOTOS =====
const adsPhotos = [
"https://picsum.photos/400/120?random=1",
"https://picsum.photos/400/120?random=2",
"https://picsum.photos/400/120?random=3",
"https://picsum.photos/400/120?random=4",
"https://picsum.photos/400/120?random=5"
];
function renderAdsPhotos(){
let container=document.getElementById('adsPhotos');
container.innerHTML='';
adsPhotos.forEach(url=>{
  const img=document.createElement('img');
  img.src=url;
  img.className='ad-photo';
  container.appendChild(img);
});
}

// ===== PLANS DATA =====
let plansData=[];
for(let i=1;i<=50;i++){
let invest=200+(i-1)*200;
let days=25+Math.floor(i/5)*5;
let multiplier=i<=5?2.4:2.2;
plansData.push({id:i,name:`Plan ${i}`,invest,days,multiplier,total:invest*multiplier,daily:(invest*multiplier)/days});
}

// ===== FUNCTIONS =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
const u=document.getElementById('user').value.trim();
const p=document.getElementById('pass').value.trim();
if(!u||!p){alert('Enter username and password');return;}
currentUser=u;
localStorage.setItem('nexa_user',currentUser);
if(!localStorage.getItem('nexa_balance')){balance=0;dailyProfit=0;totalProfit=0;localStorage.setItem('nexa_balance',balance);localStorage.setItem('nexa_daily',dailyProfit);localStorage.setItem('nexa_total',totalProfit);}
updateDashboard();
renderAdsPhotos();
}
function logout(){currentUser=null;localStorage.removeItem('nexa_user');location.reload();}
function updateDashboard(){
document.getElementById('dashUser').innerText=currentUser;
document.getElementById('dashBalance').innerText=balance.toFixed(2);
document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
document.getElementById('activeMembers').innerText=activeMembers;
document.getElementById('bottomNav').classList.remove('hidden');
}
function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}
function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value);alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}
function submitWithdraw(){alert('Withdrawal requested');}

// ===== RENDER PLANS =====
function renderPlans(){
const list=document.getElementById('plansList');
list.innerHTML='';
plansData.forEach(p=>{
const div=document.createElement('div');div.className='plan-box';
div.innerHTML=`<div class='meta'><b>${p.name}</b>Invest: Rs ${p.invest} | Days: ${p.days} | Total: Rs ${p.total.toFixed(2)} | Daily: Rs ${p.daily.toFixed(2)}</div>
<div class='actions'><button onclick='buyPlan(${p.id})'>Buy Now</button></div>`;
list.appendChild(div);
});
}
function buyPlan(id){
let plan=plansData.find(p=>p.id===id);
if(balance<plan.invest){alert('Insufficient balance!');return;}
balance+=plan.daily;
totalProfit+=plan.daily;
dailyProfit+=plan.daily;
userPlans.push(plan);
localStorage.setItem('nexa_balance',balance);
localStorage.setItem('nexa_daily',dailyProfit);
localStorage.setItem('nexa_total',totalProfit);
localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
updateDashboard();
alert(`Plan ${plan.name} purchased!`);
}

// ===== AUTO UPDATE ACTIVE MEMBERS =====
setInterval(()=>{activeMembers=Math.floor(Math.random()*5000)+1;updateDashboard();},5000);
</script>
</body>
</html>
