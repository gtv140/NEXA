<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA Dashboard</title>
<style>
:root{--primary:#1e90ff;--secondary:#ff4081;--bg-dark:#111;--bg-light:#f2f2f2;}
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
  <div class="alert-box">Warning: Only use official NEXA channels for deposits. Never share passwords.</div>
  <div class="user-box">
    <div class="left">
      <div style="display:flex;gap:12px;align-items:center">
        <div style="width:56px;height:56px;border-radius:12px;background:linear-gradient(90deg,var(--primary),var(--secondary));display:flex;align-items:center;justify-content:center;color:#000;font-weight:900">N</div>
        <div>
          <div id="dashUser" style="font-size:16px;font-weight:800">—</div>
          <div class="small">Member since: <span id="dashSince">—</span></div>
        </div>
      </div>
    </div>
    <div class="right">
      <div class="small">Balance</div>
      <div style="font-size:18px;font-weight:900">Rs <span id="dashBalance">0</span></div>
      <div class="badge">Daily: Rs <span id="dashDaily">0</span></div>
    </div>
  </div>

  <div class="alert-box">Active Members: <span id="activeMembers">0</span></div>
  <div id="activePlansBox"></div>

  <div class="referral-box">
    <div style="display:flex;gap:8px;align-items:center">
      <input id="refLink" readonly style="flex:1" />
      <button onclick="copyReferral()">Copy Link</button>
    </div>
    <div class="small">Share this link to invite friends. Bonuses apply automatically.</div>
  </div>

  <button class="logout-btn" onclick="logout()">Logout</button>
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

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
  <h2>Withdrawal</h2>
  <label>Method</label>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number" />
  <input id="withdrawAmount" placeholder="Amount" />
  <button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<!-- HISTORY -->
<div id="history" class="page hidden">
  <h2>History</h2>
  <div id="historyList"></div>
</div>

<!-- ABOUT -->
<div id="about" class="page hidden">
  <h2>About NEXA</h2>
  <p>NEXA is a modern, safe platform where you can invest in small to large plans and earn daily profit. We provide a transparent and professional service for all users.</p>
</div>

<!-- POPUP -->
<div id="popup" class="hidden">
  <span id="popupText"></span><br>
  <button onclick="closePopup()">Close</button>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('history')"><span class="ico">📜</span>History</div>
  <div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let referralCode = localStorage.getItem('nexa_referral')||'';
let history = JSON.parse(localStorage.getItem('nexa_history')||'[]');
let totalUsers = parseInt(localStorage.getItem('nexa_totalUsers')||'1');

// ===== PLANS =====
let plansData=[];
for(let i=1;i<=25;i++){
  let invest = 200*i;
  let days = 25 + i*2;
  let multiplier = (i<=5)?2.4:2.2;
  plansData.push({id:i,name:`Plan ${i}`,invest,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),days,special:(i<=5),endTime:(i<=5?Date.now()+24*60*60*1000:0)});
}

// ===== FUNCTIONS =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const option=document.getElementById('userOption').value;
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert('Please enter username & password');return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  referralCode=referralCode||Math.random().toString(36).substring(2,10); localStorage.setItem('nexa_referral',referralCode);
  if(option==='signup'){ balance=0; dailyProfit=0; userPlans=[]; totalUsers++; localStorage.setItem('nexa_totalUsers',totalUsers);
    localStorage.setItem('nexa_balance',balance); localStorage.setItem('nexa_daily',dailyProfit); localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans)); }
  updateDashboard();
}
function logout(){ currentUser=null; localStorage.removeItem('nexa_user'); updateDashboard(); }
function copyReferral(){navigator.clipboard.writeText(document.getElementById('refLink').value); alert('Referral link copied!');}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Deposit number copied!');}

// ===== DASHBOARD =====
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashSince').innerText=new Date().toLocaleDateString();
  document.getElementById('refLink').value=`https://example.com/?ref=${referralCode}`;
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  document.getElementById('bottomNav').classList.remove('hidden');
  renderPlans(); renderHistory(); updateActiveMembers(); checkSpecialOffers();
}

// ===== PLANS =====
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    let countdownHTML = '';
    if(p.special){
      countdownHTML=`<div class='small countdown' id='countdown${p.id}'></div>`;
      startCountdown(p.id,p.endTime);
    }
    div.innerHTML=`<b>${p.name}</b> | Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days} ${countdownHTML}
    <button onclick='buyNow(${p.id})'>Buy Now</button>`;
    list.appendChild(div);
  });
}
function buyNow(id){
  let plan = plansData.find(p=>p.id===id);
  if(!plan) return;
  document.getElementById('depositAmount').value=plan.invest;
  document.getElementById('depositMethod').value='jazzcash';
  updateDepositNumber();
  showPage('deposit');
}

// ===== DEPOSIT & WITHDRAW =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=(method==='jazzcash')?'03705519562':'03379827882';
}
function submitDeposit(){ alert('Deposit submitted!'); }
function submitWithdraw(){ alert('Withdrawal request submitted!'); }

// ===== HISTORY =====
function renderHistory(){
  const list=document.getElementById('historyList'); list.innerHTML='';
  history.forEach(h=>{
    const div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`${h}`;
    list.appendChild(div);
  });
}

// ===== ACTIVE MEMBERS =====
function updateActiveMembers(){
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*500+50);
  setTimeout(updateActiveMembers,5000);
}

// ===== POPUP =====
function closePopup(){ document.getElementById('popup').classList.add('hidden'); }

// ===== SPECIAL OFFERS COUNTDOWN =====
function startCountdown(id,endTime){
  let interval = setInterval(()=>{
    const now=Date.now();
    const diff=endTime-now;
    if(diff<=0){ document.getElementById('countdown'+id).innerText='Offer ended'; clearInterval(interval); return;}
    const hrs=Math.floor(diff/1000/60/60);
    const min=Math.floor((diff/1000/60)%60);
    const sec=Math.floor((diff/1000)%60);
    document.getElementById('countdown'+id).innerText=`Special Offer ends in: ${hrs}h ${min}m ${sec}s`;
  },1000);
}
function checkSpecialOffers(){ plansData.filter(p=>p.special).forEach(p=>startCountdown(p.id,p.endTime)); }

window.onload=()=>{ if(currentUser) updateDashboard(); }
</script>
</body>
</html>
