<!DOCTYPE html><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA Dashboard</title>
<style>
:root {
  --neon: #00f7ff;
  --accent: #ff5cff;
  --dark: #0a0a0a;
}
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #0a0a0a;
  color: #fff;
  overflow-x: hidden;
}
header {
  text-align: center;
  font-size: 28px;
  padding: 20px;
  background: linear-gradient(90deg, var(--neon), var(--accent));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.page { max-width: 430px; margin: 20px auto; padding: 20px; background: rgba(255,255,255,0.02); border-radius: 12px; border: 1px solid rgba(0,255,240,0.06); }
input, select, button { width: 100%; padding: 10px; margin-top: 10px; border-radius: 8px; border: 1px solid rgba(0,255,240,0.08); background: transparent; color: #e6f7fb; }
button { background: linear-gradient(90deg, var(--neon), var(--accent)); color: #001; font-weight: 700; cursor: pointer; }
.nav { position: fixed; bottom: 0; left: 0; right: 0; display: flex; justify-content: space-around; padding: 10px 0; background: rgba(0,0,0,0.8); }
.nav div { text-align: center; cursor: pointer; }
.nav div .ico { font-size: 20px; display: block; margin-bottom: 4px; }
.hidden { display: none; }
.support-icon { display: flex; align-items: center; gap: 6px; padding: 10px; background: rgba(0,255,240,0.06); border-radius: 10px; cursor: pointer; }
.support-icon:hover { box-shadow: 0 6px 20px rgba(0,255,240,0.2); transform: translateY(-2px); }
</style>
</head>
<body>
<header>NEXA Dashboard</header><div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div><div id="dashboard" class="page hidden">
  <div class="alert-box">Warning: Only use official NEXA channels for deposits. Never share passwords.</div>
  <div id="balanceBox">Balance: Rs <span id="dashBalance">0</span></div>
  <div id="dailyBox">Daily Profit: Rs <span id="dashDaily">0</span></div>
  <div id="activeMembers">Active Members: <span>0</span></div>
  <div id="plansList"></div>
  <button onclick="logout()">Logout</button>
</div><div id="deposit" class="page hidden">
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
</div><div id="withdrawal" class="page hidden">
  <h2>Withdrawal</h2>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number" />
  <input id="withdrawAmount" placeholder="Amount" />
  <button onclick="submitWithdraw()">Request Withdrawal</button>
</div><div id="about" class="page hidden">
  <h2>About NEXA</h2>
  <p>NEXA is a premium digital investment platform providing fast, secure, and reliable profit growth opportunities. Our support team is always ready to assist you.</p>
  <div class="support-icon" onclick="openSupport()">
    <span class="ico">🛠️</span> Support
  </div>
</div><div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div><script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');

function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  balance = 0; dailyProfit = 0;
  localStorage.setItem('nexa_balance', balance);
  localStorage.setItem('nexa_daily', dailyProfit);
  updateDashboard();
}
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}
function updateDashboard(){
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
}
function openSupport(){window.open('https://chat.whatsapp.com/Example','_blank');}
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}
function submitWithdraw(){alert('Withdrawal requested');}
</script></body>
</html>
