<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA Earn</title>
<style>
:root{
  --neon1:#ff4d6d;
  --neon2:#1fd1f9;
  --neon3:#ffb347;
  --dark:#0d0d0d;
}
*{box-sizing:border-box;}
body{
  margin:0;
  font-family:Arial,sans-serif;
  overflow-x:hidden;
  background:#0d0d0d;
  color:#fff;
  animation:bgAnim 40s ease-in-out infinite;
}
@keyframes bgAnim{
  0%{background:linear-gradient(0deg,var(--neon1),var(--neon2));}
  25%{background:linear-gradient(45deg,var(--neon2),var(--neon3));}
  50%{background:linear-gradient(90deg,var(--neon3),var(--neon1));}
  75%{background:linear-gradient(135deg,var(--neon1),var(--neon2));}
  100%{background:linear-gradient(180deg,var(--neon2),var(--neon3));}
}
header{
  text-align:center;
  font-size:28px;
  font-weight:800;
  padding:20px;
  color:#fff;
  text-shadow:0 0 15px var(--neon1),0 0 25px var(--neon2);
}
.page{
  max-width:480px;
  margin:20px auto;
  background:rgba(255,255,255,0.05);
  padding:20px;
  border-radius:12px;
  border:1px solid rgba(255,255,255,0.1);
  box-shadow:0 0 15px var(--neon1),0 0 25px var(--neon2);
}
button,input,select{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:none;
  background:transparent;
  color:#fff;
  outline:none;
  font-size:14px;
}
button{
  background:linear-gradient(90deg,var(--neon1),var(--neon2));
  color:#000;
  font-weight:700;
  cursor:pointer;
  transition:0.2s all;
  box-shadow:0 0 10px var(--neon1),0 0 20px var(--neon2);
}
button:hover{
  transform:translateY(-2px);
  box-shadow:0 0 20px var(--neon1),0 0 30px var(--neon2);
}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:12px 6px;font-size:14px;background:rgba(255,255,255,0.1);}
.nav div{text-align:center;cursor:pointer;width:64px;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.small{font-size:13px;color:rgba(255,255,255,0.7);}
.plan-box,.alert-box,.user-box,.referral-box,.support-icon{
  border-radius:10px;padding:12px;margin-bottom:12px;
}
.plan-box,.user-box,.referral-box,.support-icon{
  background:rgba(255,255,255,0.05);
  box-shadow:0 0 10px var(--neon1),0 0 20px var(--neon2) inset;
}
.alert-box{background:rgba(255,0,136,0.12);color:#fff;box-shadow:0 0 10px #ff00ff inset;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;cursor:pointer;width:fit-content;font-weight:700;transition:0.15s all;}
.support-icon:hover{transform:translateY(-2px);box-shadow:0 0 20px var(--neon1),0 0 30px var(--neon2);}
.countdown{font-weight:700;color:var(--neon2);}
#popup{position:fixed;top:20%;left:50%;transform:translateX(-50%);background:linear-gradient(90deg,var(--neon1),var(--neon2));padding:20px;border-radius:12px;color:#000;font-weight:800;z-index:9999;text-align:center;}
#popup button{margin-top:10px;padding:8px 12px;border:none;border-radius:8px;background:#000;color:#fff;cursor:pointer;}
@media(max-width:480px){.page{margin:12px;padding:14px}.nav div{width:48px}header{font-size:22px}}
</style>
</head>
<body>
<header>NEXA Earn</header>

<!-- DASHBOARD -->
<div id="dashboard" class="page">
  <div class="alert-box">
    Deposit/Withdrawal issues? Contact Administration. Users will receive notification & screenshot sent to Admin.
  </div>
  <div class="user-box">
    <div style="display:flex;justify-content:space-between;align-items:center;">
      <div>
        <div style="font-weight:800;font-size:16px;">Welcome User</div>
        <div class="small">Member since: <span id="dashSince">—</span></div>
      </div>
      <div>
        <div class="small">Balance</div>
        <div style="font-size:18px;font-weight:900">Rs <span id="dashBalance">0</span></div>
        <div class="small">Daily Profit: Rs <span id="dashDaily">0</span></div>
      </div>
    </div>
  </div>

  <div class="alert-box">Active Members: <span id="activeMembers">0</span></div>

  <div id="bottomNav" class="nav">
    <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
    <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
    <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdrawal</div>
    <div onclick="showPage('history')"><span class="ico">📜</span>History</div>
    <div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
    <div onclick="showPage('support')"><span class="ico">🛠️</span>Support</div>
  </div>
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
  <div style="display:flex;gap:8px;align-items:center;margin-top:10px;">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()">Copy</button>
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
  <h2>About NEXA Earn</h2>
  <p>Safe and professional investment platform with automatic daily profits.</p>
</div>

<!-- SUPPORT -->
<div id="support" class="page hidden">
  <h2>Support</h2>
  <div class="support-icon" onclick="window.open('mailto:rock.earn92@gmail.com','_blank')"><span class="ico">📧</span>Email</div>
  <div class="support-icon" onclick="window.open('https://chat.whatsapp.com/yourlink','_blank')"><span class="ico">💬</span>WhatsApp Group</div>
</div>

<script>
// ===== STORAGE =====
let balance = parseFloat(localStorage.getItem('balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('dailyProfit')||'10');
let historyData = JSON.parse(localStorage.getItem('historyData')||'[]');

// ===== ACTIVE MEMBERS =====
function updateActiveMembers(){
  document.getElementById('activeMembers').innerText = Math.floor(Math.random()*500+50);
  setTimeout(updateActiveMembers,5000);
}
updateActiveMembers();

// ===== DASHBOARD =====
document.getElementById('dashBalance').innerText = balance.toFixed(2);
document.getElementById('dashDaily').innerText = dailyProfit.toFixed(2);
document.getElementById('dashSince').innerText = new Date().toLocaleDateString();

// ===== NAVIGATION =====
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

// ===== PLANS =====
let plansData = [];
for(let i=1;i<=50;i++){
  let invest = 200*i;
  let days = 25+i*2;
  let total = Math.round(invest*2.2);
  let daily = Math.round(total/days);
  plansData.push({id:i,name:'Plan '+i,invest,total,daily,days});
}
function renderPlans(){
  const list = document.getElementById('plansList');
  list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<b>${p.name}</b> | Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}
    <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}
renderPlans();
function buyPlan(id){
  let plan = plansData.find(p=>p.id===id);
  if(!plan) return;
  document.getElementById('depositAmount').value=plan.invest;
  document.getElementById('depositMethod').value='jazzcash';
  updateDepositNumber();
  showPage('deposit');
}

// ===== DEPOSIT =====
function updateDepositNumber(){
  const method = document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value = method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){
  navigator.clipboard.writeText(document.getElementById('depositNumber').value);
  alert('Deposit number copied!');
}
function submitDeposit(){
  let amt = document.getElementById('depositAmount').value;
  let tx = document.getElementById('depositTxId').value;
  historyData.push(`Deposit Rs ${amt} TX:${tx} Approved`);
  localStorage.setItem('historyData',JSON.stringify(historyData));
  renderHistory();
  alert('Deposit submitted and approved!');
}

// ===== WITHDRAWAL =====
function submitWithdraw(){
  let amt = document.getElementById('withdrawAmount').value;
  let acct = document.getElementById('withdrawAccount').value;
  historyData.push(`Withdrawal Rs ${amt} Account:${acct} Approved`);
  localStorage.setItem('historyData',JSON.stringify(historyData));
  renderHistory();
  alert('Withdrawal request submitted!');
}

// ===== HISTORY =====
function renderHistory(){
  const list = document.getElementById('historyList');
  list.innerHTML='';
  historyData.forEach(h=>{
    const div=document.createElement('div');
    div.className='plan-box';
    div.innerText=h;
    list.appendChild(div);
  });
}
renderHistory();

// ===== DAILY PROFIT =====
setInterval(()=>{
  balance += dailyProfit;
  localStorage.setItem('balance',balance);
  document.getElementById('dashBalance').innerText = balance.toFixed(2);
},1000*60*60*24);
</script>
</body>
</html>
