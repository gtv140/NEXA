<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA</title>
<style>
:root{--primary:#1e90ff;--secondary:#ff4081;--bg-dark:#111;}
*{box-sizing:border-box;}
body{
  margin:0;
  font-family:Arial,sans-serif;
  overflow-x:hidden;
  background:#111;
  color:#fff;
  animation:bgAnim 30s linear infinite;
}
@keyframes bgAnim{
  0%{background:linear-gradient(135deg,#1e90ff,#ff4081);}
  25%{background:linear-gradient(135deg,#00c3ff,#ff8a00);}
  50%{background:linear-gradient(135deg,#7b2ff7,#f107a3);}
  75%{background:linear-gradient(135deg,#12c2e9,#c471ed,#f64f59);}
  100%{background:linear-gradient(135deg,#1e90ff,#ff4081);}
}
header{
  text-align:center;
  font-size:28px;
  font-weight:800;
  padding:20px;
  color:#fff;
  text-shadow:0 0 10px var(--primary),0 0 20px var(--secondary);
}
.login-box,.page{
  max-width:480px;
  margin:20px auto;
  background:rgba(255,255,255,0.05);
  padding:20px;
  border-radius:12px;
  border:1px solid rgba(255,255,255,0.1);
  box-shadow:0 0 10px var(--primary),0 0 20px var(--secondary);
}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:none;background:transparent;color:#fff;outline:none;font-size:14px;}
input::placeholder{color:rgba(255,255,255,0.7);}
button{
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  color:#000;
  font-weight:700;
  cursor:pointer;
  transition:0.2s all;
  box-shadow:0 0 10px var(--primary),0 0 20px var(--secondary);
}
button:hover{
  transform:translateY(-2px);
  box-shadow:0 0 20px var(--primary),0 0 30px var(--secondary);
}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:12px 6px;font-size:14px;background:rgba(255,255,255,0.1);}
.nav div{text-align:center;cursor:pointer;width:64px;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.small{font-size:13px;color:rgba(255,255,255,0.7);}
.user-box,.plan-box,.referral-box,.help-box,.alert-box{border-radius:10px;padding:12px;margin-bottom:12px;}
.user-box,.plan-box,.referral-box,.help-box{background:rgba(255,255,255,0.05);box-shadow:0 0 10px var(--primary),0 0 20px var(--secondary) inset;}
.alert-box{background:rgba(255,0,136,0.12);color:#fff;box-shadow:0 0 10px #ff00ff inset;}
</style>
</head>
<body>
<header>NEXA</header>

<!-- LOGIN -->
<div id="loginPage" class="login-box">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User</option></select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
  <p class="small">Use the same device to keep your account saved.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="alert-box">Warning: Only use official NEXA channels. Never share passwords.</div>
  <div class="user-box">
    <div style="display:flex;justify-content:space-between;align-items:center;">
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
  </div>

  <div class="alert-box">Active Members: <span id="activeMembers">0</span></div>

  <div class="plan-box">
    <h3>Plans</h3>
    <div id="plansList"></div>
  </div>

  <div class="plan-box">
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
    <input id="depositTxId" placeholder="TX ID" />
    <label>Upload Proof</label>
    <input type="file" id="depositProof" />
    <button onclick="submitDeposit()">Submit Deposit</button>
  </div>

  <div class="plan-box">
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

  <div class="plan-box">
    <h3>History</h3>
    <div id="historyList"></div>
  </div>

  <div class="referral-box">
    <div style="display:flex;gap:8px;align-items:center">
      <input id="refLink" readonly style="flex:1" />
      <button onclick="copyReferral()">Copy Link</button>
    </div>
    <div class="small">Share this link to invite friends. Bonuses apply automatically.</div>
  </div>

  <button onclick="logout()">Logout</button>
</div>

<script>
// ===== STORAGE =====
let currentUser = null;
let balance = 0;
let dailyProfit = 0;
let plansData = [];
for(let i=1;i<=10;i++){
  let invest=200*i,days=25+i*2,mult=2.2;
  plansData.push({id:i,name:`Plan ${i}`,invest,total:Math.round(invest*mult),daily:Math.round((invest*mult)/days),days});
}

// ===== FUNCTIONS =====
function login(){
  const u = document.getElementById('user').value.trim();
  const option = document.getElementById('userOption').value;
  if(!u){alert('Enter username');return;}
  currentUser = u; balance = 0; dailyProfit=0;
  document.getElementById('dashUser').innerText = currentUser;
  document.getElementById('dashBalance').innerText = balance.toFixed(2);
  document.getElementById('dashDaily').innerText = dailyProfit.toFixed(2);
  document.getElementById('dashSince').innerText = new Date().toLocaleDateString();
  document.getElementById('refLink').value = `https://example.com/?ref=${currentUser}`;
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  updateActiveMembers();
  renderPlans();
}

function logout(){
  currentUser=null;
  document.getElementById('loginPage').classList.remove('hidden');
  document.getElementById('dashboard').classList.add('hidden');
}

function renderPlans(){
  const list = document.getElementById('plansList');
  list.innerHTML = '';
  plansData.forEach(p=>{
    const div = document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<b>${p.name}</b> | Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days} <button onclick="buyNow(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}

function buyNow(id){
  let plan = plansData.find(p=>p.id===id);
  if(!plan) return;
  document.getElementById('depositAmount').value = plan.invest;
  document.getElementById('depositMethod').value='jazzcash';
  updateDepositNumber();
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=(method==='jazzcash')?'03705519562':'03379827882';
}

function submitDeposit(){ alert('Deposit submitted!'); }
function submitWithdraw(){ alert('Withdrawal requested!'); }

function copyReferral(){ navigator.clipboard.writeText(document.getElementById('refLink').value); alert('Referral link copied!'); }

function updateActiveMembers(){
  document.getElementById('activeMembers').innerText = Math.floor(Math.random()*500 + 50);
  setTimeout(updateActiveMembers,5000);
}

</script>
</body>
</html>
