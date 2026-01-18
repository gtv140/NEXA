<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --gold: #FFD700;
  --dark: #111111;
  --accent: #00FFFF;
  --red: #FF5555;
  --white: #ffffff;
}
body {
  margin:0;
  font-family: 'Arial', sans-serif;
  background: var(--dark);
  color: var(--white);
  overflow-x:hidden;
}
header {
  text-align:center;
  font-size:32px;
  font-weight:800;
  padding:20px;
  background: linear-gradient(90deg, var(--gold), var(--accent));
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
  animation: glowHeader 2s infinite alternate;
}
@keyframes glowHeader{
  0%{-webkit-text-fill-color: var(--gold);}
  50%{-webkit-text-fill-color: var(--accent);}
  100%{-webkit-text-fill-color: var(--red);}
}
.page {
  max-width: 480px;
  margin:20px auto;
  padding:20px;
  border-radius:12px;
  background: rgba(255,255,255,0.03);
  border:1px solid rgba(255,215,0,0.2);
}
input, select, button {
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid rgba(255,215,0,0.2);
  background:transparent;
  color: var(--white);
}
button {
  cursor:pointer;
  font-weight:700;
  background: linear-gradient(90deg,var(--gold),var(--accent));
  transition: transform 0.15s, box-shadow 0.15s;
}
button:hover {
  transform:translateY(-2px);
  box-shadow:0 8px 20px rgba(255,215,0,0.5);
}
.nav {
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  background: rgba(0,0,0,0.85);
  padding:12px 0;
}
.nav div{
  text-align:center;
  cursor:pointer;
}
.nav div .ico{
  font-size:22px;
  display:block;
  margin-bottom:4px;
}
.hidden{display:none;}
.plan-box{
  border:1px solid rgba(255,215,0,0.2);
  border-radius:10px;
  padding:12px;
  margin:10px 0;
  display:flex;
  justify-content:space-between;
  align-items:center;
  transition: transform 0.15s, box-shadow 0.15s;
}
.plan-box:hover{
  transform:translateY(-4px);
  box-shadow:0 10px 25px rgba(255,215,0,0.5);
}
.plan-meta b{display:block;font-size:16px;margin-bottom:6px;}
.countdown{color: var(--accent); font-weight:700;}
.card-box{
  border:1px solid rgba(255,215,0,0.2);
  border-radius:12px;
  padding:12px;
  margin:10px 0;
  background: rgba(255,255,255,0.02);
}
.support-icon{
  display:flex;
  align-items:center;
  gap:6px;
  padding:10px;
  border-radius:10px;
  background: rgba(255,215,0,0.05);
  cursor:pointer;
}
.support-icon:hover{
  box-shadow:0 6px 20px rgba(255,215,0,0.3);
  transform:translateY(-2px);
}
img.dashboard-photo{
  width:100%;
  border-radius:12px;
  margin:10px 0;
  object-fit:cover;
  height:120px;
}
.logout-btn{
  margin-top:10px;
  background: var(--red);
  color: var(--white);
}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN / SIGNUP -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Login / Signup</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="card-box">
    <div><b>Username:</b> <span id="dashUser">-</span></div>
    <div><b>Balance:</b> Rs <span id="dashBalance">0</span></div>
    <div><b>Daily Profit:</b> Rs <span id="dashDaily">0</span></div>
    <div><b>Total Profit:</b> Rs <span id="dashTotal">0</span></div>
    <div><b>Active Members:</b> <span id="activeMembers">0</span></div>
  </div>

  <h3>Company Info</h3>
  <p>NEXA EARN, operating since 2022, is your trusted platform for digital investments. Our team ensures fast & secure profit growth. Enjoy daily updates, exclusive special plans, and full transparency.</p>
  <img class="dashboard-photo" src="https://picsum.photos/400/120?random=1" />
  <img class="dashboard-photo" src="https://picsum.photos/400/120?random=2" />
  <img class="dashboard-photo" src="https://picsum.photos/400/120?random=3" />

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
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex; gap:8px; margin-top:10px;">
    <input id="depositNumber" readonly style="flex:1;" />
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount" />
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

<!-- ABOUT -->
<div id="about" class="page hidden">
  <h2>About NEXA EARN</h2>
  <p>Trusted since 2022 for digital investments. Daily profit updates, special plans, and secure transactions guaranteed.</p>
  <div class="support-icon" onclick="openSupport()"><span class="ico">🛠️</span> Support</div>
</div>

<!-- NAV -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let activeMembers = parseInt(localStorage.getItem('nexa_members')||Math.floor(Math.random()*5000));

let plansData=[];
for(let i=1;i<=50;i++){
  let invest=200 + (i-1)*50;
  let days=25 + Math.floor((i-1)/5)*5;
  let multiplier = i<=5 ? 2.4 : 2.2;
  plansData.push({id:i,name:`Plan ${i}`,invest,days,total:Math.round(invest*multiplier),daily:Math.round(invest*multiplier/days)});
}
localStorage.setItem('nexa_members', activeMembers);

// ===== FUNCTIONS =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert('Enter username & password'); return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0; totalProfit=0;
  localStorage.setItem('nexa_balance', balance);
  localStorage.setItem('nexa_daily', dailyProfit);
  localStorage.setItem('nexa_total', totalProfit);
  updateDashboard();
}
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); showPage('loginPage');}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashTotal').innerText=totalProfit;
  document.getElementById('activeMembers').innerText=activeMembers;
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
  renderPlans();
}
function renderPlans(){
  const list=document.getElementById('plansList');
  list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<div class="plan-meta"><b>${p.name}</b>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div>
    <button onclick="buyNow(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}
function buyNow(id){
  const plan=plansData.find(p=>p.id===id);
  if(!plan) return;
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
}
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){
  const amount=parseFloat(document.getElementById('depositAmount').value)||0;
  if(amount>0){balance+=amount; totalProfit+=amount*0.05; dailyProfit+=amount*0.02; 
    localStorage.setItem('nexa_balance', balance);
    localStorage.setItem('nexa_total', totalProfit);
    localStorage.setItem('nexa_daily', dailyProfit);
    alert('Deposit submitted!'); updateDashboard();
  }else{alert('Enter valid amount');}
}
function submitWithdraw(){alert('Withdrawal requested');}
function openSupport(){window.open('https://chat.whatsapp.com/Example','_blank');}

// Random active members & daily updates
setInterval(()=>{
  if(currentUser){
    dailyProfit += Math.round(Math.random()*10);
    totalProfit += Math.round(Math.random()*15);
    activeMembers = Math.floor(Math.random()*5000);
    localStorage.setItem('nexa_daily', dailyProfit);
    localStorage.setItem('nexa_total', totalProfit);
    localStorage.setItem('nexa_members', activeMembers);
    if(document.getElementById('dashBalance')) document.getElementById('dashBalance').innerText=balance;
    if(document.getElementById('dashDaily')) document.getElementById('dashDaily').innerText=dailyProfit;
    if(document.getElementById('dashTotal')) document.getElementById('dashTotal').innerText=totalProfit;
    if(document.getElementById('activeMembers')) document.getElementById('activeMembers').innerText=activeMembers;
  }
},3000);
</script>
</body>
</html>
