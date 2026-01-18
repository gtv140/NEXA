<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
:root {
  --primary:#1e1e2f;
  --accent:#ff6b81;
  --text:#ffffff;
  --box:#2c2c3e;
}
body {
  margin:0;
  font-family:Arial, sans-serif;
  background:linear-gradient(135deg,#1e1e2f,#3b3b5c);
  color:var(--text);
}
header {
  text-align:center;
  font-size:28px;
  padding:20px;
  font-weight:800;
}
.page { max-width:480px; margin:20px auto; padding:20px; background:var(--box); border-radius:12px; }
input, select, button { width:100%; padding:10px; margin-top:10px; border-radius:8px; border:none; background:rgba(255,255,255,0.1); color:#fff; }
button { cursor:pointer; background:var(--accent); font-weight:700; transition:0.2s; }
button:hover { opacity:0.9; }
.nav { position:fixed; bottom:0; left:0; right:0; display:flex; justify-content:space-around; padding:10px 0; background:var(--box); }
.nav div { text-align:center; cursor:pointer; }
.nav div .ico { font-size:20px; display:block; margin-bottom:4px; }
.hidden { display:none; }
.dashboard-box { background:rgba(0,0,0,0.2); padding:12px; border-radius:12px; margin-bottom:12px; }
.support-icon { display:flex; align-items:center; gap:6px; padding:8px 12px; border-radius:10px; background:var(--accent); color:#001; font-weight:700; cursor:pointer; transition:0.2s; }
.support-icon:hover { opacity:0.9; }
.photos-box { display:flex; gap:8px; overflow-x:auto; margin-bottom:12px; }
.photos-box img { width:100px; height:70px; border-radius:8px; object-fit:cover; }
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <input id="user" placeholder="Username"/>
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="dashboard-box">
    <div>Username: <span id="dashUser">-</span></div>
    <div>Balance: Rs <span id="dashBalance">0</span></div>
    <div>Daily Profit: Rs <span id="dashDaily">0</span></div>
    <div>Total Profit: Rs <span id="dashTotal">0</span></div>
    <div>Active Members: <span id="activeMembers">0</span></div>
  </div>
  <div class="photos-box">
    <img src="https://source.unsplash.com/100x70/?business" alt="Photo1">
    <img src="https://source.unsplash.com/100x70/?office" alt="Photo2">
    <img src="https://source.unsplash.com/100x70/?team" alt="Photo3">
    <img src="https://source.unsplash.com/100x70/?work" alt="Photo4">
  </div>
  <div class="dashboard-box">
    <h3>About NEXA EARN</h3>
    <p>Operating since 2022, NEXA EARN provides secure digital investment opportunities. Fast, reliable profits with daily updates. Join our growing community!</p>
    <div class="support-icon" onclick="openSupport()">
      <span class="ico">🛠️</span> Support
    </div>
  </div>
  <button onclick="logout()">Logout</button>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
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

<script>
// STORAGE
let currentUser = localStorage.getItem('nexa_user') || null;
let balance = parseFloat(localStorage.getItem('nexa_balance') || '0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily') || '0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total') || '0');
let activeMembers = parseInt(localStorage.getItem('nexa_active') || '0');

function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}

function login(){
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert('Enter username and password'); return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  // New user starts with 0 balance
  balance = parseFloat(localStorage.getItem('nexa_balance') || '0');
  dailyProfit = parseFloat(localStorage.getItem('nexa_daily') || '0');
  totalProfit = parseFloat(localStorage.getItem('nexa_total') || '0');
  activeMembers = Math.floor(Math.random()*5000)+1;
  localStorage.setItem('nexa_active',activeMembers);
  updateDashboard();
}

function logout(){ currentUser=null; localStorage.removeItem('nexa_user'); location.reload(); }

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
  document.getElementById('activeMembers').innerText=activeMembers;
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
}

// DEPOSIT
function updateDepositNumber(){
  const method = document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value = method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){ navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied'); }
function submitDeposit(){ alert('Deposit submitted'); balance += parseFloat(document.getElementById('depositAmount').value||0); totalProfit += parseFloat(document.getElementById('depositAmount').value||0); dailyProfit += parseFloat(document.getElementById('depositAmount').value||0)*0.05; localStorage.setItem('nexa_balance',balance); localStorage.setItem('nexa_daily',dailyProfit); localStorage.setItem('nexa_total',totalProfit); updateDashboard(); }

// WITHDRAW
function submitWithdraw(){ alert('Withdrawal requested'); balance -= parseFloat(document.getElementById('withdrawAmount').value||0); localStorage.setItem('nexa_balance',balance); updateDashboard(); }

// SUPPORT
function openSupport(){ window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank'); }

</script>
</body>
</html>
