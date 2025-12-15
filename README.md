<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA Dashboard</title>
<style>
:root{
  --primary:#1e1e2f;
  --secondary:#ff4081;
  --bg-dark:#0b0b0b;
  --bg-light:#1c1c1c;
}
*{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif;}
body{
  background:#0b0b0b;
  color:#fff;
  overflow-x:hidden;
  animation:bgAnim 30s linear infinite;
}
@keyframes bgAnim{
  0%{background:linear-gradient(135deg,#0b0b0b,#1e1e2f);}
  25%{background:linear-gradient(135deg,#111,#2b2b3f);}
  50%{background:linear-gradient(135deg,#0d0d0d,#3a1e3f);}
  75%{background:linear-gradient(135deg,#111,#2b2b3f);}
  100%{background:linear-gradient(135deg,#0b0b0b,#1e1e2f);}
}
header{
  text-align:center;font-size:28px;font-weight:800;padding:20px;
  color:#fff;text-shadow:0 0 10px var(--secondary),0 0 20px var(--primary);
}
.login-box,.page{
  max-width:500px;margin:20px auto;
  background:rgba(255,255,255,0.05);padding:20px;border-radius:12px;
  border:1px solid rgba(255,255,255,0.1);
  box-shadow:0 0 10px var(--primary),0 0 20px var(--secondary);
}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:none;background:transparent;color:#fff;font-size:14px;outline:none;}
input::placeholder{color:rgba(255,255,255,0.7);}
button{
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  color:#000;font-weight:700;cursor:pointer;transition:0.2s all;
  box-shadow:0 0 10px var(--primary),0 0 20px var(--secondary);
}
button:hover{transform:translateY(-2px);box-shadow:0 0 20px var(--primary),0 0 35px var(--secondary);}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:12px 6px;font-size:14px;background:rgba(255,255,255,0.1);}
.nav div{text-align:center;cursor:pointer;width:64px;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.small{font-size:13px;color:rgba(255,255,255,0.7);}
.user-box,.plan-box,.referral-box,.help-box,.alert-box,.support-icon{border-radius:10px;padding:12px;margin-bottom:12px;}
.user-box,.plan-box,.referral-box,.help-box{background:rgba(255,255,255,0.05);box-shadow:0 0 10px var(--primary),0 0 20px var(--secondary) inset;}
.alert-box{background:rgba(255,0,136,0.12);color:#fff;box-shadow:0 0 10px #ff00ff inset;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;cursor:pointer;width:fit-content;font-weight:700;transition:0.15s all;}
.support-icon:hover{transform:translateY(-2px);box-shadow:0 0 20px var(--primary),0 0 30px var(--secondary);}
.countdown{font-weight:700;color:var(--secondary);}
#popup{position:fixed;top:20%;left:50%;transform:translateX(-50%);background:linear-gradient(90deg,var(--primary),var(--secondary));padding:20px;border-radius:12px;color:#000;font-weight:800;z-index:9999;text-align:center;}
#popup button{margin-top:10px;padding:8px 12px;border:none;border-radius:8px;background:#000;color:#fff;cursor:pointer;}
@media(max-width:480px){.login-box,.page{margin:12px;padding:14px}.nav div{width:48px}header{font-size:22px}}
</style>
</head>
<body>
<header>NEXA Dashboard</header>

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

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h2>Plans</h2>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
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

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let referralCode = localStorage.getItem('nexa_referral')||'';
let totalUsers = parseInt(localStorage.getItem('nexa_totalUsers')||'1');

// ===== PLANS =====
let plansData=[];
for(let i=1;i<=50;i++){
  let invest = 200*i;
  let days = 25 + i;
  let multiplier = (i<=5)?2.4:2.2;
  plansData.push({id:i,name:`Plan ${i}`,invest,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),days});
}

// ===== FUNCTIONS =====
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}
function login(){
  const option=document.getElementById('userOption').value;
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert('Please enter username & password');return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  referralCode=referralCode||'https://gtv140.github.io/NEXA/';
  localStorage.setItem('nexa_referral',referralCode);
  balance=0; dailyProfit=0; userPlans=[]; localStorage.setItem('nexa_balance',balance); 
  localStorage.setItem('nexa_daily',dailyProfit); 
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  totalUsers++; localStorage.setItem('nexa_totalUsers',totalUsers);
  updateDashboard();
}
function logout(){ currentUser=null; localStorage.removeItem('nexa_user'); showPage('loginPage'); }
function copyReferral(){navigator.clipboard.writeText(referralCode); alert('Referral link copied!');}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Deposit number copied!');}

// ===== DASHBOARD =====
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashSince').innerText=new Date().toLocaleDateString();
  showPage('dashboard');
  document.getElementById('bottomNav').classList.remove('hidden');
  updateActiveMembers();
  renderPlans();
}

// ===== PLANS =====
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`<b>${p.name}</b> | Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days} 
    <button onclick='buyNow(${p.id})'>Buy Now</button>`;
    list.appendChild(div);
  });
}
function buyNow(id){
  let plan = plansData.find(p=>p.id===id);
  if(!plan) return;
  document.getElementById('depositAmount').value=plan.invest;
  showPage('deposit');
}

// ===== DEPOSIT =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=(method==='jazzcash')?'03705519562':'03379827882';
}
function submitDeposit(){ alert('Deposit submitted!'); }

// ===== ACTIVE MEMBERS =====
function updateActiveMembers(){
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*500+50);
  setTimeout(updateActiveMembers,5000);
}

window.onload=()=>{ if(currentUser) updateDashboard(); else showPage('loginPage'); updateDepositNumber(); }
</script>
</body>
</html>
