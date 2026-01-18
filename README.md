<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root{
  --primary:#ff0080;
  --secondary:#00e6ff;
  --bg:#f8f8f8;
  --text:#111;
}
*{box-sizing:border-box;margin:0;padding:0;font-family:sans-serif;}
body{background:var(--bg);color:var(--text);}
header{text-align:center;padding:20px;font-size:32px;font-weight:800;background:linear-gradient(90deg,var(--primary),var(--secondary));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:500px;margin:20px auto;background:#fff;padding:20px;border-radius:12px;box-shadow:0 4px 20px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid #ccc;font-size:14px;}
button{background:linear-gradient(90deg,var(--primary),var(--secondary));color:#fff;font-weight:700;cursor:pointer;border:none;transition:0.2s; }
button:hover{transform:translateY(-2px);}
.hidden{display:none;}
.nav{display:flex;justify-content:space-around;padding:10px;margin-top:20px;border-top:1px solid #ddd;}
.nav div{text-align:center;cursor:pointer;}
.nav div span{display:block;font-size:20px;margin-bottom:4px;}
.card{background:#f0f0f0;padding:15px;border-radius:10px;margin:10px 0;box-shadow:0 2px 10px rgba(0,0,0,0.05);}
.card h3{margin-bottom:8px;}
.card .small{font-size:13px;color:#555;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;background:#eee;border-radius:10px;cursor:pointer;margin-top:10px;}
.support-icon span{font-size:18px;}
.plan-box{background:#fff3f8;padding:12px;margin:10px 0;border-radius:10px;display:flex;justify-content:space-between;align-items:center;box-shadow:0 2px 10px rgba(0,0,0,0.05);}
.countdown{color:var(--primary);font-weight:700;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption">
    <option value="login">Login</option>
    <option value="signup">New User Signup</option>
  </select>
  <input id="user" placeholder="Username"/>
  <input id="pass" type="password" placeholder="Password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="card">Username: <b id="dashUser">—</b></div>
  <div class="card">Balance: Rs <span id="dashBalance">0</span></div>
  <div class="card">Daily Profit: Rs <span id="dashDaily">0</span></div>
  <div class="card">Total Profit: Rs <span id="dashTotal">0</span></div>
  <div class="card">Active Members: <span id="activeMembers">0</span></div>

  <div id="companyBox" class="card">
    <h3>About NEXA EARN</h3>
    <p>Operating since 2022, NEXA EARN provides secure and reliable digital investment opportunities. Enjoy automatic profits, easy deposits, and withdrawals with JazzCash/EasyPaisa.</p>
    <img src="https://picsum.photos/200/100?random=1" alt="company1"/>
    <img src="https://picsum.photos/200/100?random=2" alt="company2"/>
    <div class="support-icon" onclick="openSupport()"><span>🛠️</span> Support</div>
  </div>

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
  <div style="display:flex; gap:8px; margin-top:10px;">
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

<div class="nav" id="bottomNav" class="hidden">
  <div onclick="showPage('dashboard')"><span>🏠</span>Home</div>
  <div onclick="showPage('plans')"><span>📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span>💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span>💵</span>Withdraw</div>
</div>

<script>
// STORAGE
let currentUser = localStorage.getItem('nexa_user') || null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let activeMembers = parseInt(localStorage.getItem('nexa_active')||'0');

// RANDOM ACTIVE MEMBERS
function randomMembers(){activeMembers = Math.floor(Math.random()*5000+1); document.getElementById('activeMembers').innerText=activeMembers; localStorage.setItem('nexa_active',activeMembers);}
setInterval(randomMembers,5000);

// PLANS
let plans=[];
for(let i=1;i<=50;i++){
  let invest=200 + (i-1)*50;
  let days=25+Math.floor(i/2);
  let multiplier=(i<=5)?2.4:2.2;
  plans.push({id:i,name:'Plan '+i,invest,days,daily:Math.round(invest*multiplier/days),total:Math.round(invest*multiplier),special:i<=5});
}

// SHOW PLANS
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plans.forEach(p=>{
    let countdownHTML=p.special?`<div class="countdown" id="countdown${p.id}"></div>`:'';
    const div=document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<div>
      <b>${p.name}</b><br/>
      Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}
      ${countdownHTML}
    </div>
    <div><button onclick="buyPlan(${p.id})">Buy Now</button></div>`;
    list.appendChild(div);
    if(p.special){startCountdown(p.id);}
  });
}

// COUNTDOWN
let specialEndTimes={};
function startCountdown(id){
  if(!specialEndTimes[id]) specialEndTimes[id]=new Date().getTime()+24*60*60*1000;
  setInterval(()=>{
    let now=new Date().getTime();
    let distance=specialEndTimes[id]-now;
    if(distance<0){document.getElementById('countdown'+id).innerText='Offer Ended';return;}
    let h=Math.floor(distance/(1000*60*60));
    let m=Math.floor((distance%(1000*60*60))/(1000*60));
    let s=Math.floor((distance%(1000*60))/1000);
    document.getElementById('countdown'+id).innerText=`${h}h ${m}m ${s}s`;
  },1000);
}

// LOGIN
function login(){
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert('Enter username & password'); return;}
  currentUser=u;
  localStorage.setItem('nexa_user',u);
  balance = 0; dailyProfit = 0; totalProfit =0;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashTotal').innerText=totalProfit;
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  document.getElementById('bottomNav').classList.remove('hidden');
  renderPlans();
}

// LOGOUT
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}

// SHOW PAGE
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

// DEPOSIT
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){
  let amt=parseFloat(document.getElementById('depositAmount').value);
  if(isNaN(amt)||amt<=0){alert('Enter valid amount'); return;}
  balance+=amt;
  localStorage.setItem('nexa_balance',balance);
  document.getElementById('dashBalance').innerText=balance;
  alert('Deposit Submitted & Balance Updated');
}

// WITHDRAWAL
function submitWithdraw(){
  let amt=parseFloat(document.getElementById('withdrawAmount').value);
  if(isNaN(amt)||amt<=0){alert('Enter valid amount'); return;}
  if(amt>balance){alert('Insufficient balance'); return;}
  balance-=amt;
  localStorage.setItem('nexa_balance',balance);
  document.getElementById('dashBalance').innerText=balance;
  alert('Withdrawal Requested & Balance Updated');
}

// SUPPORT
function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}
</script>
</body>
</html>
