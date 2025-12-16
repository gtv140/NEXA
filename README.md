<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root{
  --primary:#00ffff;
  --secondary:#ff00ff;
  --accent:#ff0000;
  --dark:#0b0b0b;
  --text:#ffffff;
  --support:#ff69b4;
  --whatsapp:#25d366;
}
*{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif;}
body{
  background:#0b0b0b;color:var(--text);overflow-x:hidden;
  animation:bgAnim 25s ease-in-out infinite alternate;
}
@keyframes bgAnim{
  0%{background:linear-gradient(120deg,#00ffff,#ff00ff);}
  25%{background:linear-gradient(120deg,#ff0000,#00ffff);}
  50%{background:linear-gradient(120deg,#ff00ff,#ff0000);}
  75%{background:linear-gradient(120deg,#00ffff,#ff0000);}
  100%{background:linear-gradient(120deg,#ff00ff,#00ffff);}
}
header{
  text-align:center;font-size:30px;font-weight:900;padding:20px;
  text-shadow:0 0 15px #00ffff,0 0 25px #ff00ff,0 0 35px #ff0000;
  letter-spacing:2px;
}
.page{
  max-width:480px;margin:20px auto;background:rgba(0,0,0,0.7);
  padding:20px;border-radius:15px;border:1px solid rgba(255,255,255,0.1);
  box-shadow:0 0 20px var(--primary),0 0 40px var(--secondary);transition:0.3s all;
}
.page:hover{box-shadow:0 0 25px var(--accent),0 0 50px var(--secondary);}
button,input,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:none;background:transparent;color:#fff;outline:none;font-size:14px;}
button{background:linear-gradient(90deg,var(--primary),var(--secondary));color:#000;font-weight:700;cursor:pointer;transition:0.3s all;box-shadow:0 0 10px var(--primary),0 0 20px var(--secondary);}
button:hover{transform:translateY(-3px);box-shadow:0 0 25px var(--accent),0 0 40px var(--secondary);}
.top-bar{display:flex;align-items:center;padding:10px 16px;background:rgba(0,0,0,0.5);border-radius:12px;max-width:480px;margin:20px auto 10px auto;}
.back-button{background:#ff4d4d;color:#fff;border:none;padding:8px 16px;border-radius:8px;cursor:pointer;font-weight:bold;font-size:16px;}
.hidden{display:none;}
.small{font-size:13px;color:rgba(255,255,255,0.7);}
.dashboard-box{border-radius:12px;padding:14px;margin-bottom:12px;text-align:center;transition:0.3s all; background:rgba(0,0,0,0.3); box-shadow:0 0 15px var(--primary),0 0 30px var(--secondary) inset;}
.dashboard-box:hover{transform:translateY(-3px);box-shadow:0 0 25px var(--accent),0 0 40px var(--secondary) inset;}
#dashboardMsg{font-weight:900;font-size:16px;text-align:center;padding:15px;border-radius:12px;background:linear-gradient(135deg,#00ffff,#ff00ff);color:#000;margin-bottom:15px;box-shadow:0 0 20px #00ffff,0 0 30px #ff00ff;}
.referral-box span{cursor:pointer;text-decoration:underline;}
#welcomePopup{position:fixed;top:20%;left:50%;transform:translateX(-50%);background:linear-gradient(90deg,#00ffff,#ff00ff);padding:20px;border-radius:15px;color:#000;font-weight:800;z-index:9999;text-align:center;display:none;box-shadow:0 0 25px #00ffff,0 0 40px #ff00ff;}
#welcomePopup button{margin-top:10px;padding:10px;border:none;border-radius:10px;background:#000;color:#fff;cursor:pointer;}
.nav{position:fixed;bottom:10px;left:50%;transform:translateX(-50%);display:flex;justify-content:space-around;background:rgba(20,20,20,0.95);padding:10px 14px;border-radius:40px;max-width:420px;box-shadow:0 5px 15px rgba(0,0,0,0.5);z-index:999;}
.nav div{text-align:center;cursor:pointer;width:60px;height:60px;border-radius:50%;display:flex;flex-direction:column;justify-content:center;align-items:center;transition:0.3s all;background:rgba(255,255,255,0.05);}
.nav div:hover{transform:translateY(-5px);box-shadow:0 4px 12px var(--accent);}
.nav div.active{background:linear-gradient(135deg,var(--primary),var(--secondary));color:#000;box-shadow:0 4px 12px var(--primary),0 0 15px var(--secondary);}
.nav div .ico{font-size:24px;display:block;margin-bottom:4px;}
a{color:var(--text);}
a:hover{text-decoration:underline;}
@media(max-width:480px){.page{margin:12px;padding:14px}.nav div{width:50px;height:50px}header{font-size:22px}}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN PAGE -->
<div id="loginPage" class="page">
  <h2>Login to NEXA Earn</h2>
  <label>Username</label>
  <input type="text" id="loginUsername" placeholder="Enter Username"/>
  <label>Password</label>
  <input type="password" id="loginPassword" placeholder="Enter Password"/>
  <button onclick="login()">Login</button>
</div>

<!-- WELCOME POPUP -->
<div id="welcomePopup">
  Welcome, <span id="welcomeUser">User</span>! To NEXA Earn 🚀
  <button onclick="closeWelcome()">Close</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div id="dashboardMsg">Welcome to <span style="color:#00ffff;text-shadow:0 0 10px #00ffff,0 0 20px #ff00ff,0 0 30px #ff0000;">NEXA EARN</span>! 🚀 Start growing your earnings today.</div>
  
  <div style="display:flex;gap:10px;flex-wrap:wrap;justify-content:space-between;">
    <div class="dashboard-box" style="flex:1;min-width:45%;">
      <div class="small">Balance</div>
      <div style="font-weight:900;font-size:20px">Rs <span id="dashBalance">0</span></div>
    </div>
    <div class="dashboard-box" style="flex:1;min-width:45%;">
      <div class="small">Daily Profit</div>
      <div style="font-weight:900;font-size:20px">Rs <span id="dashDaily">0</span></div>
    </div>
    <div class="dashboard-box" style="flex:1;min-width:45%;">
      <div class="small">Active Members</div>
      <div style="font-weight:900;font-size:18px"> <span id="activeMembers">0</span></div>
    </div>
    <div class="dashboard-box referral-box" style="flex:1;min-width:45%;">
      <div class="small">Referral Link</div>
      <div style="font-weight:900;font-size:14px;color:#0ff;word-break:break-all;">
        <span id="referralLink">—</span>
        <button onclick="copyReferral()">Copy</button>
      </div>
    </div>
    <div class="dashboard-box" style="flex:1;min-width:100%;">
      <div class="small">Support / About</div>
      <div style="font-weight:900;font-size:14px;word-break:break-word;">
        About: NEXA EARN is a trusted platform providing daily automated profits and secure investment plans.<br>
        Email: <a href="mailto:rock.earn92@gmail.com" style="color:var(--support);font-weight:bold;">rock.earn92@gmail.com</a><br>
        WhatsApp: <a href="https://whatsapp.com/channel/0029Vb7jo3eFnSz6S7mxgF29" target="_blank" style="color:var(--whatsapp);font-weight:bold;">Open WhatsApp Channel</a>
      </div>
    </div>
  </div>
  <div class="dashboard-box" id="countdownContainer">Plan Countdown: <span id="planCountdown">—</span></div>
</div>

<!-- PLANS PAGE -->
<div id="plans" class="page hidden">
  <div class="top-bar"><button class="back-button" onclick="goHome()">← Back</button></div>
  <h2>Plans</h2>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT PAGE -->
<div id="deposit" class="page hidden">
  <div class="top-bar"><button class="back-button" onclick="goHome()">← Back</button></div>
  <h2>Deposit</h2>
  <label>Method</label>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex;gap:8px;align-items:center;margin-top:10px;">
    <input id="depositNumber" readonly style="flex:1"/>
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <label>Amount</label>
  <input id="depositAmount" placeholder="Enter Amount"/>
  <label>Transaction ID</label>
  <input id="depositTxId" placeholder="TX ID"/>
  <label>Upload Proof</label>
  <input type="file" id="depositProof"/>
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAWAL PAGE -->
<div id="withdrawal" class="page hidden">
  <div class="top-bar"><button class="back-button" onclick="goHome()">← Back</button></div>
  <h2>Withdrawal</h2>
  <label>Method</label>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number"/>
  <input id="withdrawAmount" placeholder="Amount"/>
  <button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<!-- HISTORY PAGE -->
<div id="history" class="page hidden">
  <div class="top-bar"><button class="back-button" onclick="goHome()">← Back</button></div>
  <h2>History</h2>
  <div id="historyList"></div>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div id="homeIcon" onclick="setActive(this);showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div id="plansIcon" onclick="setActive(this);showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div id="depositIcon" onclick="setActive(this);showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div id="withdrawalIcon" onclick="setActive(this);showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div id="historyIcon" onclick="setActive(this);showPage('history')"><span class="ico">📜</span>History</div>
  <div id="logoutIcon" onclick="logout()"><span class="ico">🚪</span>Logout</div>
</div>

<script>
// ===== STORAGE & INIT =====
let balance=parseFloat(localStorage.getItem('balance')||'0');
let historyData=JSON.parse(localStorage.getItem('historyData')||'[]');
let loggedUser=localStorage.getItem('loggedUser')||'';
let activePlan=JSON.parse(localStorage.getItem('activePlan')||'null');
let planEndTime=localStorage.getItem('planEndTime')||'';
window.onload=function(){
  if(loggedUser){
    showDashboard();
  }
};
function showDashboard(){
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  document.getElementById('bottomNav').classList.remove('hidden');
  document.getElementById('dashUser').innerText=loggedUser;
  document.getElementById('referralLink').innerText=window.location.href+"?ref="+loggedUser;
  if(activePlan){startCountdown();document.getElementById('depositAmount').value=activePlan.invest;}
}

// ===== ACTIVE MEMBERS =====
function updateActiveMembers(){
  const container=document.getElementById('activeMembers');
  container.innerText=Math.floor(Math.random()*500+50);
  container.style.textShadow="0 0 5px #ff0,0 0 10px #0ff,0 0 20px #f0f";
  setTimeout(updateActiveMembers,4000);
}
updateActiveMembers();

// ===== NAVIGATION =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function goHome(){showPage('dashboard');}
function setActive(elem){document.querySelectorAll('.nav div').forEach(d=>d.classList.remove('active'));elem.classList.add('active');}

// ===== LOGIN =====
function login(){
  let u=document.getElementById('loginUsername').value.trim();
  let p=document.getElementById('loginPassword').value.trim();
  if(!u||!p){alert('Enter username and password!'); return;}
  loggedUser=u;
  localStorage.setItem('loggedUser',u);
  showWelcome(u);
}
function showWelcome(username){
  document.getElementById('welcomeUser').innerText=username;
  document.getElementById('welcomePopup').style.display='block';
  document.getElementById('dashboardMsg').style.display='none';
  setTimeout(()=>{
    document.getElementById('welcomePopup').style.display='none';
    document.getElementById('dashboardMsg').style.display='block';
    showDashboard();
  },3000);
}
function closeWelcome(){document.getElementById('welcomePopup').style.display='none';document.getElementById('dashboardMsg').style.display='block';}
function copyReferral(){navigator.clipboard.writeText(document.getElementById('referralLink').innerText);alert('Referral link copied!');}

// ===== PLANS =====
let plansData=[];
for(let i=1;i<=50;i++){
  let invest=200*i;
  let days=25+i;
  let multiplier=2.2;
  if(days>=26 && days<=35) multiplier=2.4;
  if(days>=36 && days<=50) multiplier=2.8;
  let total=Math.round(invest*multiplier);
  let daily=Math.round(total/days);
  plansData.push({id:i,name:'Plan '+i,invest,total,daily,days,multiplier});
}
function renderPlans(){
  const list=document.getElementById('plansList');list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='dashboard-box';
    div.innerHTML=`<b>${p.name}</b> | Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}
    <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}
renderPlans();
function buyPlan(id){
  let plan=plansData.find(p=>p.id===id); if(!plan) return;
  activePlan=plan;
  localStorage.setItem('activePlan',JSON.stringify(plan));
  planEndTime=Date.now()+24*60*60*1000;
  localStorage.setItem('planEndTime',planEndTime);
  document.getElementById('depositAmount').value=plan.invest;
  document.getElementById('depositMethod').value='jazzcash';
  updateDepositNumber();
  startCountdown();
  showPage('deposit');
}

// ===== DEPOSIT =====
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value);alert('Deposit number copied!');}
function submitDeposit(){let amt=document.getElementById('depositAmount').value;let tx=document.getElementById('depositTxId').value;historyData.push(`Deposit Rs ${amt} TX:${tx} Approved`);localStorage.setItem('historyData',JSON.stringify(historyData));renderHistory();alert('Deposit submitted & approved!');}

// ===== WITHDRAW =====
function submitWithdraw(){let amt=document.getElementById('withdrawAmount').value;let acct=document.getElementById('withdrawAccount').value;historyData.push(`Withdrawal Rs ${amt} Account:${acct} Approved`);localStorage.setItem('historyData',JSON.stringify(historyData));renderHistory();alert('Withdrawal submitted & approved!');}

// ===== HISTORY =====
function renderHistory(){const list=document.getElementById('historyList'); list.innerHTML='';historyData.forlist.innerHTML='';
  if(historyData.length===0){list.innerHTML='<p>No history yet.</p>'; return;}
  historyData.forEach(item=>{
    const div=document.createElement('div');
    div.className='dashboard-box';
    div.style.fontSize='14px';
    div.innerText=item;
    list.appendChild(div);
  });
}
renderHistory();

// ===== PLAN COUNTDOWN =====
function startCountdown(){
  if(!activePlan || !planEndTime) return;
  const countdown=document.getElementById('planCountdown');
  function updateTimer(){
    let now=Date.now();
    let diff=planEndTime-now;
    if(diff<=0){countdown.innerText='Plan Ended'; clearInterval(timer); return;}
    let hrs=Math.floor(diff/1000/60/60);
    let mins=Math.floor((diff/1000/60)%60);
    let secs=Math.floor((diff/1000)%60);
    countdown.innerText=`${hrs}h ${mins}m ${secs}s`;
  }
  updateTimer();
  let timer=setInterval(updateTimer,1000);
}
if(activePlan && planEndTime) startCountdown();

// ===== LOGOUT =====
function logout(){
  // localStorage.removeItem('loggedUser'); // comment this to prevent auto logout on refresh
  loggedUser=''; 
  showPage('loginPage'); 
  document.getElementById('bottomNav').classList.add('hidden');
}

// ===== PREVENT LOGOUT ON REFRESH =====
// page state saved in localStorage
window.addEventListener('beforeunload',()=>{
  localStorage.setItem('activePlan',JSON.stringify(activePlan));
  localStorage.setItem('planEndTime',planEndTime);
  localStorage.setItem('historyData',JSON.stringify(historyData));
  localStorage.setItem('balance',balance);
});

// ===== UTILITY =====
function copyText(text){navigator.clipboard.writeText(text); alert('Copied!');}
</script>
</body>
</html>
