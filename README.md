<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
:root{
  --primary:#ff5c5c;
  --secondary:#ffb86c;
  --background:#f5f5f5;
  --card-bg:#ffffff;
  --text:#222;
  --accent:#ff75a0;
}
*{box-sizing:border-box;}
body{margin:0;font-family:'Arial',sans-serif;background:var(--background);color:var(--text);}
header{background:linear-gradient(90deg,var(--primary),var(--secondary));color:#fff;text-align:center;padding:20px;font-size:28px;font-weight:700;border-radius:0 0 20px 20px;}
.page{max-width:500px;margin:20px auto;padding:20px;background:var(--card-bg);border-radius:15px;box-shadow:0 5px 15px rgba(0,0,0,0.1);}
input, select, button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid #ccc;outline:none;font-size:14px;}
button{background:var(--accent);color:#fff;font-weight:700;cursor:pointer;border:none;}
button:hover{opacity:0.9;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:12px;background:var(--card-bg);box-shadow:0 -2px 10px rgba(0,0,0,0.1);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.box{background:#f0f0f0;padding:15px;margin-bottom:15px;border-radius:12px;box-shadow:0 3px 10px rgba(0,0,0,0.05);}
.box h3{margin:0 0 8px 0;font-size:16px;}
.box p{margin:2px 0;font-size:14px;}
.plan-box{display:flex;justify-content:space-between;align-items:center;padding:12px;margin:10px 0;border-radius:12px;background:#fff;box-shadow:0 3px 10px rgba(0,0,0,0.05);}
.plan-box .meta{flex:1;}
.plan-box .meta b{display:block;margin-bottom:5px;}
.plan-box .actions button{padding:8px 12px;}
img.dashboard-photo{width:100%;border-radius:12px;margin-bottom:12px;}

/* Animations for photos */
@keyframes slide{
  0%{opacity:0;}
  50%{opacity:1;}
  100%{opacity:0;}
}
.slide-img{animation:slide 10s infinite;}

</style>
</head>
<body>

<header>NEXA EARN</header>

<!-- LOGIN / SIGNUP -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <input id="username" placeholder="Username" />
  <input id="password" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="box"><h3>Username</h3><p id="dashUser">—</p></div>
  <div class="box"><h3>Balance</h3><p>Rs <span id="dashBalance">0</span></p></div>
  <div class="box"><h3>Daily Profit</h3><p>Rs <span id="dashDaily">0</span></p></div>
  <div class="box"><h3>Total Profit</h3><p>Rs <span id="dashTotal">0</span></p></div>
  <div class="box"><h3>Active Members</h3><p id="activeMembers">0</p></div>

  <div class="box">
    <h3>Company Highlights</h3>
    <p>NEXA EARN has been empowering digital investors since 2022. Fast, secure, and reliable platform.</p>
    <img class="dashboard-photo slide-img" src="https://picsum.photos/400/150?random=1" />
    <img class="dashboard-photo slide-img" src="https://picsum.photos/400/150?random=2" />
    <img class="dashboard-photo slide-img" src="https://picsum.photos/400/150?random=3" />
  </div>
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
  <div style="display:flex; gap:8px; align-items:center; margin-top:10px">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Amount" />
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

<!-- NAV -->
<div class="nav hidden" id="bottomNav">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let userPlans = JSON.parse(localStorage.getItem('nexa_plans')||'[]');
let activeMembers = Math.floor(Math.random()*5000)+500;

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}
function login(){
  const u=document.getElementById('username').value.trim();
  const p=document.getElementById('password').value.trim();
  if(!u||!p){alert('Enter Username & Password');return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0; totalProfit=0;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  updateDashboard();
}
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
  document.getElementById('activeMembers').innerText=activeMembers;
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
}

// ===== PLANS =====
const plansData=[];
for(let i=1;i<=50;i++){
  const invest=200 + (i-1)*100;
  const days=25 + i;
  const multiplier=i<=5?2.4:2.2; // first 5 plans special
  plansData.push({id:i,name:'Plan '+i,invest,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),days,special:i<=5});
}
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    let timer='';
    if(p.special) timer=`<div>Special Offer: <span id="timer${p.id}">24:00:00</span></div>`;
    div.innerHTML=`<div class='meta'><b>${p.name}</b>
      <div>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div>
      ${timer}
    </div>
    <div class='actions'><button onclick='buyNow(${p.id})'>Buy Now</button></div>`;
    list.appendChild(div);
    if(p.special) startTimer(p.id,24*60*60);
  });
}
function buyNow(id){
  const plan=plansData.find(p=>p.id===id);
  if(!plan) return;
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
}
function startTimer(id,seconds){
  const el=document.getElementById('timer'+id);
  if(!el) return;
  let sec=seconds;
  setInterval(()=>{
    const h=Math.floor(sec/3600).toString().padStart(2,'0');
    const m=Math.floor((sec%3600)/60).toString().padStart(2,'0');
    const s=(sec%60).toString().padStart(2,'0');
    el.innerText=`${h}:${m}:${s}`;
    if(sec>0) sec--;
  },1000);
}

// ===== DEPOSIT / WITHDRAW =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){
  alert('Deposit submitted for approval');
  balance += parseFloat(document.getElementById('depositAmount').value||0);
  totalProfit += parseFloat(document.getElementById('depositAmount').value||0);
  dailyProfit += Math.round((document.getElementById('depositAmount').value||0)*0.02);
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  updateDashboard();
}
function submitWithdraw(){
  alert('Withdrawal requested');
  const amt=parseFloat(document.getElementById('withdrawAmount').value||0);
  balance-=amt;
  localStorage.setItem('nexa_balance',balance);
  updateDashboard();
}

// Auto-increment active members randomly
setInterval(()=>{activeMembers=Math.floor(Math.random()*5000)+500; if(document.getElementById('activeMembers')) document.getElementById('activeMembers').innerText=activeMembers;},5000);

</script>

</body>
</html>
