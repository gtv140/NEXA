<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA</title>
<style>
:root{
  --primary:#00f7ff;
  --secondary:#ff5cff;
  --bg-dark:#0a0a0a;
}
*{box-sizing:border-box;}
body{
  margin:0;
  font-family:Arial,sans-serif;
  overflow-x:hidden;
  background:#0a0a0a;
  color:#fff;
  animation:bgAnim 30s linear infinite;
}
@keyframes bgAnim{
  0%{background:linear-gradient(135deg,#0a0a0a,#1a1a1a);}
  25%{background:linear-gradient(135deg,#0a0a0a,#0f0f2a);}
  50%{background:linear-gradient(135deg,#0a0a0a,#1a0a2a);}
  75%{background:linear-gradient(135deg,#0a0a0a,#2a0a1a);}
  100%{background:linear-gradient(135deg,#0a0a0a,#1a1a1a);}
}
header{text-align:center;font-size:28px;font-weight:800;padding:20px;text-shadow:0 0 10px var(--primary),0 0 20px var(--secondary);}
.login-box,.page{max-width:480px;margin:20px auto;background:rgba(255,255,255,0.05);padding:20px;border-radius:12px;border:1px solid rgba(255,255,255,0.1);box-shadow:0 0 10px var(--primary),0 0 20px var(--secondary);}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:none;background:transparent;color:#fff;outline:none;font-size:14px;}
input::placeholder{color:rgba(255,255,255,0.6);}
button{background:linear-gradient(90deg,var(--primary),var(--secondary));color:#000;font-weight:700;cursor:pointer;transition:0.2s all;box-shadow:0 0 10px var(--primary),0 0 20px var(--secondary);}
button:hover{transform:translateY(-2px);box-shadow:0 0 15px var(--primary),0 0 25px var(--secondary);}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:12px 6px;font-size:14px;background:rgba(255,255,255,0.1);}
.nav div{text-align:center;cursor:pointer;width:64px;transition:0.2s all;position:relative;}
.nav div:hover{transform:translateY(-2px);text-shadow:0 0 15px var(--primary),0 0 25px var(--secondary);}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.small{font-size:13px;color:rgba(255,255,255,0.7);}
.user-box,.plan-box,.referral-box,.alert-box{border-radius:10px;padding:12px;margin-bottom:12px;}
.user-box,.plan-box,.referral-box{background:rgba(255,255,255,0.05);box-shadow:0 0 10px var(--primary),0 0 20px var(--secondary) inset;}
.alert-box{background:rgba(255,0,136,0.12);color:#fff;box-shadow:0 0 10px #ff00ff inset;}
#popup{position:fixed;top:20%;left:50%;transform:translateX(-50%);background:linear-gradient(90deg,var(--primary),var(--secondary));padding:20px;border-radius:12px;color:#000;font-weight:800;z-index:9999;text-align:center;box-shadow:0 0 15px var(--primary),0 0 25px var(--secondary);}
#popup button{margin-top:10px;padding:8px 12px;border:none;border-radius:8px;background:#000;color:#fff;cursor:pointer;}
@media(max-width:480px){.login-box,.page{margin:12px;padding:14px}.nav div{width:48px}header{font-size:22px}}
</style>
</head>
<body>
<header>NEXA</header>

<div id="loginPage" class="login-box">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User</option></select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
  <p class="small">Use the same device to keep your account saved.</p>
</div>

<div id="dashboard" class="page hidden">
  <div class="alert-box">Warning: Only use official NEXA channels. Never share passwords.</div>
  <div class="user-box" style="display:flex;justify-content:space-between;align-items:center;">
    <div>
      <div id="dashUser" style="font-size:16px;font-weight:800">—</div>
      <div class="small">Member since: <span id="dashSince">—</span></div>
    </div>
    <div>
      <div class="small">Balance</div>
      <div style="font-size:18px;font-weight:900">Rs <span id="dashBalance">0</span></div>
      <div class="small">Daily: Rs <span id="dashDaily">0</span></div>
    </div>
  </div>
  <div class="alert-box">Active Members: <span id="activeMembers">0</span></div>
</div>

<div id="plans" class="page hidden">
  <h3>Plans</h3>
  <div id="plansList"></div>
</div>

<div id="deposit" class="page hidden">
  <h3>Deposit</h3>
  <label>Method</label>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex;gap:8px;align-items:center;margin-top:10px">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()">Copy Number</button>
  </div>
  <label>Amount</label>
  <input id="depositAmount" placeholder="Enter Amount" />
  <label>Transaction ID</label>
  <input type="text" id="depositTxId" placeholder="TX ID" />
  <label>Upload Proof</label>
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawal" class="page hidden">
  <h3>Withdrawal</h3>
  <label>Method</label>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number" />
  <input id="withdrawAmount" placeholder="Amount" />
  <button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div id="history" class="page hidden">
  <h3>History</h3>
  <div id="historyList"></div>
</div>

<div id="about" class="page hidden">
  <h3>About NEXA</h3>
  <p>NEXA is a modern, safe platform where you can invest in small to large plans and earn daily profit. We provide a transparent and professional service for all users.</p>
</div>

<div class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('history')"><span class="ico">📜</span>History</div>
  <div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<div id="popup" class="hidden"><span id="popupText"></span><br><button onclick="closePopup()">Close</button></div>

<script>
let currentUser = localStorage.getItem('nexa_user') || null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let history = JSON.parse(localStorage.getItem('nexa_history')||'[]');
let referralCode = localStorage.getItem('nexa_referral')||'';

let plansData = [];
for(let i=200;i<=10000;i+=200){
    let days = Math.floor(25 + i/100);
    let multiplier = (i <= 1000) ? 2.4 : 2.2;
    let total = Math.round(i*multiplier);
    let daily = Math.round(total/days);
    plansData.push({id:i, invest:i, total, daily, days});
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u; balance=0; dailyProfit=0;
  referralCode = referralCode || Math.random().toString(36).substring(2,10);
  localStorage.setItem('nexa_user',currentUser);
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_referral',referralCode);
  updateDashboard();
  showPopup('Login successful!');
}

function logout(){currentUser=null; localStorage.removeItem('nexa_user'); showPage('loginPage');}

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashSince').innerText=new Date().toLocaleDateString();
  document.querySelector('.nav').classList.remove('hidden');
  showPage('dashboard');
  renderPlans(); renderHistory(); updateActiveMembers();
}

function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`<b>Plan Rs ${p.invest}</b> | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days} <button onclick="buyNow(${p.invest})">Buy Now</button>`;
    list.appendChild(div);
  });
}

function buyNow(invest){document.getElementById('depositAmount').value=invest; showPage('deposit');}

function updateDepositNumber(){
  document.getElementById('depositNumber').value = document.getElementById('depositMethod').value==='jazzcash'?'03705519562':'03379827882';
}

function submitDeposit(){showPopup('Deposit submitted!');}
function submitWithdraw(){showPopup('Withdrawal requested!');}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value);showPopup('Number copied!');}

function renderHistory(){const list=document.getElementById('historyList'); list.innerHTML='';history.forEach(h=>{const div=document.createElement('div'); div.className='plan-box'; div.innerText=h; list.appendChild(div);});}
function updateActiveMembers(){document.getElementById('activeMembers').innerText=Math.floor(Math.random()*500+50); setTimeout(updateActiveMembers,5000);}
function showPopup(msg){document.getElementById('popupText').innerText=msg; document.getElementById('popup').classList.remove('hidden');}
function closePopup(){document.getElementById('popup').classList.add('hidden');}

window.onload=()=>{if(currentUser) updateDashboard(); else showPage('loginPage');}
</script>
</body>
</html>
