<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
:root {
  --primary:#0a0a0a;
  --accent:#00bfff;
  --highlight:#ff5cdd;
}
body {
  margin:0;
  font-family:Arial, sans-serif;
  background:#0a0a0a;
  color:#fff;
  overflow-x:hidden;
}
header {
  text-align:center;
  font-size:28px;
  font-weight:800;
  padding:20px;
  color: var(--accent);
}
.page {
  max-width:480px;
  margin:20px auto;
  padding:20px;
  border-radius:12px;
  background:rgba(255,255,255,0.03);
  border:1px solid rgba(0,191,255,0.1);
}
input, select, button { width:100%; padding:10px; margin-top:10px; border-radius:8px; border:1px solid rgba(0,191,255,0.2); background:transparent; color:#fff; }
button { background: linear-gradient(90deg,var(--accent),var(--highlight)); color:#000; font-weight:700; cursor:pointer; }
button:hover { transform:translateY(-2px); }
.nav {
  position:fixed;
  bottom:0; left:0; right:0;
  display:flex;
  justify-content:space-around;
  padding:10px 0;
  background:rgba(0,0,0,0.8);
  border-top:1px solid rgba(0,191,255,0.2);
}
.nav div { text-align:center; cursor:pointer; }
.nav div .ico { font-size:20px; display:block; margin-bottom:4px; }
.hidden { display:none; }
.support-icon { display:flex; align-items:center; gap:6px; padding:10px; background:rgba(0,191,255,0.06); border-radius:10px; cursor:pointer; }
.support-icon:hover { box-shadow:0 6px 20px rgba(0,191,255,0.3); transform:translateY(-2px); }
.plan-box { border:1px solid rgba(0,191,255,0.2); padding:12px; margin:8px 0; border-radius:10px; display:flex; justify-content:space-between; align-items:center; transition:0.2s; }
.plan-box:hover { box-shadow:0 8px 25px rgba(0,191,255,0.3); transform:translateY(-3px);}
.plan-box .meta b{font-size:15px; display:block;}
.photos { display:flex; gap:6px; overflow-x:auto; margin:12px 0;}
.photos img { border-radius:12px; width:100px; height:70px; object-fit:cover;}
.alert-box { background:rgba(255,92,221,0.05); padding:10px; border-radius:10px; margin-bottom:12px; color:var(--highlight); border:1px solid rgba(255,92,221,0.2);}
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
  <div class="alert-box">Use official NEXA EARN channels. Never share passwords.</div>
  <div style="display:flex; justify-content:space-between; margin-bottom:10px;">
    <div>Username: <span id="dashUser">—</span></div>
    <div>Balance: Rs <span id="dashBalance">0</span></div>
  </div>
  <div style="display:flex; justify-content:space-between; margin-bottom:10px;">
    <div>Daily Profit: Rs <span id="dashDaily">0</span></div>
    <div>Total Profit: Rs <span id="dashTotal">0</span></div>
  </div>
  <div>Active Members: <span id="activeMembers">0</span></div>
  <div class="photos" id="dashboardPhotos"></div>
  <div id="plansList"></div>
  <button onclick="logout()">Logout</button>
</div>

<!-- DEPOSIT -->
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

<!-- ABOUT & SUPPORT -->
<div id="about" class="page hidden">
  <h2>About NEXA EARN</h2>
  <p>NEXA EARN provides fast and secure digital investment opportunities. Grow your profits daily with trusted plans. Support team available 24/7.</p>
  <div class="support-icon" onclick="openSupport()">
    <span class="ico">🛠️</span> Support
  </div>
</div>

<!-- NAVIGATION -->
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
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let activeMembers = Math.floor(Math.random()*5000)+500;

// ===== DASHBOARD PHOTOS =====
const dashboardPhotos = [
'https://picsum.photos/100/70?random=1',
'https://picsum.photos/100/70?random=2',
'https://picsum.photos/100/70?random=3',
'https://picsum.photos/100/70?random=4',
'https://picsum.photos/100/70?random=5'
];

// ===== PLANS DATA =====
let plansData=[];
for(let i=1;i<=50;i++){
  let invest = 200 + (i-1)*200;
  let days = 25 + Math.floor(i/5)*5;
  let multiplier = i<=5 ? 2.4 : 2.2;
  plansData.push({id:i,name:`Plan ${i}`,invest,days,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),special:i<=5});
}

// ===== FUNCTIONS =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser = u; localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0; totalProfit=0; userPlans=[]; 
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  updateDashboard();
}
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashTotal').innerText=totalProfit;
  document.getElementById('activeMembers').innerText=activeMembers;
  showPhotos();
  renderPlans();
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
}

function showPhotos(){
  const container = document.getElementById('dashboardPhotos');
  container.innerHTML='';
  dashboardPhotos.forEach(src=>{
    const img=document.createElement('img');
    img.src=src;
    container.appendChild(img);
  });
}

// ===== PLANS =====
function renderPlans(){
  const list=document.getElementById('plansList');
  list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='plan-box';
    let countdownHTML='';
    if(p.special){
      countdownHTML=`<div id="countdown${p.id}" class="small">Special offer: 24h left</div>`;
      startCountdown(p.id);
    }
    div.innerHTML=`<div class="meta"><b>${p.name}</b>
      <div class="small">Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div>
      ${countdownHTML}</div>
      <button onclick="buyNow(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}

function buyNow(id){
  let plan = plansData.find(p=>p.id===id);
  if(!plan) return;
  showPage('deposit');
  document.getElementById('depositAmount').value = plan.invest;
  updateDepositNumber();
}

function startCountdown(id){
  const endTime = new Date().getTime() + 24*60*60*1000;
  const interval = setInterval(()=>{
    const now = new Date().getTime();
    const distance = endTime-now;
    if(distance<0){document.getElementById(`countdown${id}`).innerText='Offer ended'; clearInterval(interval);}
    else{
      const h=Math.floor(distance/(1000*60*60));
      const m=Math.floor((distance%(1000*60*60))/(1000*60));
      const s=Math.floor((distance%(1000*60))/1000);
      document.getElementById(`countdown${id}`).innerText=`Special offer: ${h}h ${m}m ${s}s`;
    }
  },1000);
}

// ===== DEPOSIT =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value = method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){
  const amt = parseFloat(document.getElementById('depositAmount').value)||0;
  if(amt<=0){alert('Enter amount'); return;}
  balance+=amt; dailyProfit+=Math.round(amt*0.05); totalProfit+=Math.round(amt*0.05); 
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  alert('Deposit submitted!');
  updateDashboard();
}

// ===== WITHDRAW =====
function submitWithdraw(){
  const amt = parseFloat(document.getElementById('withdrawAmount').value)||0;
  if(amt<=0 || amt>balance){alert('Invalid amount'); return;}
  balance-=amt; localStorage.setItem('nexa_balance',balance); alert('Withdrawal requested!');
  updateDashboard();
}

// ===== SUPPORT =====
function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}

// ===== ACTIVE MEMBERS RANDOM UPDATE =====
setInterval(()=>{
  activeMembers = Math.floor(Math.random()*5000)+500;
  document.getElementById('activeMembers').innerText=activeMembers;
},5000);

</script>
</body>
</html>
