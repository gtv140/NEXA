<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
:root{
  --primary:#1a1a1a;
  --accent:#00bfff;
  --accent2:#ff4081;
  --bg:#f5f5f5;
  --text:#111;
}
*{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif;}
body{background:var(--bg);color:var(--text);}
header{padding:20px;text-align:center;font-size:28px;font-weight:700;color:var(--accent);}
.page{max-width:480px;margin:20px auto;padding:20px;background:#fff;border-radius:12px;box-shadow:0 6px 20px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin:8px 0;border-radius:8px;border:1px solid #ccc;font-size:14px;}
button{background:linear-gradient(90deg,var(--accent),var(--accent2));color:#fff;font-weight:700;cursor:pointer;border:none;transition:0.2s;}
button:hover{opacity:0.9;}
.nav{display:flex;justify-content:space-around;position:fixed;bottom:0;width:100%;background:#eee;padding:10px 0;border-top:1px solid #ccc;}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.box{background:#f0f0f0;padding:14px;margin:10px 0;border-radius:12px;box-shadow:0 4px 12px rgba(0,0,0,0.05);}
.box b{display:block;margin-bottom:4px;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;background:#e0e0e0;border-radius:10px;cursor:pointer;}
.support-icon:hover{box-shadow:0 6px 20px rgba(0,0,0,0.15);}
.plan-box{border:1px solid #ccc;padding:12px;margin:10px 0;border-radius:10px;display:flex;justify-content:space-between;align-items:center;}
.plan-box:hover{box-shadow:0 6px 20px rgba(0,0,0,0.1);}
.countdown{color:#ff4081;font-weight:700;}
img.dashboard-img{width:100%;border-radius:12px;margin:10px 0;}
</style>
</head>
<body>

<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="box"><b>Username:</b> <span id="dashUser">-</span></div>
  <div class="box"><b>Balance:</b> Rs <span id="dashBalance">0</span></div>
  <div class="box"><b>Daily Profit:</b> Rs <span id="dashDaily">0</span></div>
  <div class="box"><b>Total Profit:</b> Rs <span id="dashTotal">0</span></div>
  <div class="box"><b>Active Members:</b> <span id="activeMembers">0</span></div>
  
  <h3>Photos & Ads</h3>
  <div id="photos"></div>

  <button onclick="showPage('plans')">View Plans</button>
  <button onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h2>Investment Plans</h2>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex;gap:8px;align-items:center;margin-top:10px">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount" />
  <input id="depositTxId" placeholder="Transaction ID" />
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAWAL -->
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

<!-- ABOUT -->
<div id="about" class="page hidden">
  <h2>About NEXA EARN</h2>
  <p>NEXA EARN is a simple digital investment platform providing fast and reliable profit growth. Always use official channels for deposits and withdrawals.</p>
  <div class="support-icon" onclick="openSupport()">🛠️ Support</div>
</div>

<!-- NAV -->
<div class="nav hidden">
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
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let activeMembers = parseInt(localStorage.getItem('nexa_active')||Math.floor(Math.random()*5000));

// ===== PHOTOS =====
const photosList=[
'https://picsum.photos/400/200?random=1',
'https://picsum.photos/400/200?random=2',
'https://picsum.photos/400/200?random=3',
'https://picsum.photos/400/200?random=4',
'https://picsum.photos/400/200?random=5'
];
function renderPhotos(){
  const div=document.getElementById('photos'); div.innerHTML='';
  photosList.forEach(p=>{
    const img=document.createElement('img'); img.src=p; img.className='dashboard-img';
    div.appendChild(img);
  });
}

// ===== PLANS =====
let plansData=[];
for(let i=1;i<=50;i++){
  let invest = 200 + (i-1)*100;
  let days = 25 + Math.floor((i-1)/5)*5;
  let multiplier = (i<=5)?2.4:2.2;
  let special = (i<=5);
  plansData.push({id:i,name:'Plan '+i,invest,days,multiplier,special});
}
function renderPlans(){
  const div=document.getElementById('plansList'); div.innerHTML='';
  plansData.forEach(p=>{
    const planDiv=document.createElement('div'); planDiv.className='plan-box';
    let endTime = p.special?new Date().getTime()+24*60*60*1000:0;
    let cd = p.special?`<div class="countdown" id="count${p.id}"></div>`:'';
    planDiv.innerHTML=`<div>
      <b>${p.name}</b>
      <div>Invest: Rs ${p.invest} | Total: Rs ${Math.round(p.invest*p.multiplier)} | Daily: Rs ${Math.round(p.invest*p.multiplier/p.days)} | Days: ${p.days}</div>
      ${cd}
    </div>
    <div><button onclick="buyNow(${p.id})">Buy Now</button></div>`;
    div.appendChild(planDiv);
    if(p.special) startCountdown(p.id,endTime);
  });
}

// ===== COUNTDOWN =====
function startCountdown(id,endTime){
  const el=document.getElementById('count'+id);
  if(!el) return;
  function update(){
    let now = new Date().getTime();
    let distance = endTime-now;
    if(distance<0){el.innerText='Expired';return;}
    let h=Math.floor(distance/(1000*60*60));
    let m=Math.floor((distance%(1000*60*60))/(1000*60));
    let s=Math.floor((distance%60000)/1000);
    el.innerText=`${h}h ${m}m ${s}s`;
    setTimeout(update,1000);
  }
  update();
}

// ===== FUNCTIONS =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert('Enter username & password');return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')){balance=0;dailyProfit=0;totalProfit=0;userPlans=[];saveData();}
  updateDashboard();renderPhotos();renderPlans();
}
function logout(){currentUser=null;localStorage.removeItem('nexa_user');location.reload();}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
  document.getElementById('activeMembers').innerText=activeMembers;
  document.querySelector('.nav').classList.remove('hidden');
  showPage('dashboard');
}
function openSupport(){window.open('https://chat.whatsapp.com/Example','_blank');}
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value);alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}
function submitWithdraw(){alert('Withdrawal requested');}
function buyNow(id){
  let plan=plansData.find(p=>p.id===id);
  if(balance<plan.invest){alert('Insufficient balance');return;}
  balance-=plan.invest;
  let daily=Math.round(plan.invest*plan.multiplier/plan.days);
  let total=Math.round(plan.invest*plan.multiplier);
  dailyProfit+=daily;
  totalProfit+=total;
  userPlans.push({id:plan.id,daily,total,start:new Date().getTime(),days:plan.days});
  saveData();updateDashboard();alert(`Plan ${plan.name} purchased!`);
}
function saveData(){
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
}
</script>
</body>
</html>
