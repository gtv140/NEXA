<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
:root{
  --neon:#FFD700;
  --accent:#FF4500;
  --dark:#111;
  --bg:#1a1a1a;
}
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:var(--bg);
  color:#fff;
  overflow-x:hidden;
}
header{
  text-align:center;
  padding:20px;
  font-size:30px;
  font-weight:800;
  background:linear-gradient(90deg,var(--neon),var(--accent));
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
}
.page{
  max-width:480px;
  margin:20px auto;
  padding:20px;
  background:rgba(255,255,255,0.03);
  border-radius:15px;
  box-shadow:0 8px 20px rgba(0,0,0,0.5);
}
input,select,button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid rgba(255,255,255,0.1);
  background:transparent;
  color:#fff;
  outline:none;
}
button{
  cursor:pointer;
  font-weight:700;
  background:linear-gradient(90deg,var(--neon),var(--accent));
  color:#111;
  transition:all .2s;
}
button:hover{
  transform:translateY(-2px);
  box-shadow:0 6px 15px rgba(0,0,0,0.5);
}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  padding:12px;
  background:rgba(0,0,0,0.85);
  border-top:1px solid rgba(255,255,255,0.05);
}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:22px;margin-bottom:4px;}
.hidden{display:none;}
.user-box{
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:15px;
  border-radius:12px;
  background:linear-gradient(90deg,rgba(255,215,0,0.1),rgba(255,69,0,0.1));
  margin-bottom:10px;
}
.user-box div{font-weight:600;}
.plan-card{
  background:rgba(255,255,255,0.05);
  margin:10px 0;
  padding:15px;
  border-radius:12px;
  cursor:pointer;
  transition:all .2s;
}
.plan-card:hover{
  transform:translateY(-3px);
  box-shadow:0 8px 20px rgba(255,215,0,0.3);
}
.countdown{
  font-weight:700;
  color:var(--neon);
}
.photo-card{
  width:100%;
  border-radius:12px;
  margin:10px 0;
}
</style>
</head>
<body>
<header>NEXA EARN</header>

<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption">
    <option value="login">Login</option>
    <option value="signup">New User Signup</option>
  </select>
  <input id="user" placeholder="Username"/>
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="user-box">
    <div>Username: <span id="dashUser">-</span></div>
    <div>Balance: Rs <span id="dashBalance">0</span></div>
  </div>
  <div class="user-box">
    <div>Daily Profit: Rs <span id="dashDaily">0</span></div>
    <div>Total Profit: Rs <span id="dashTotal">0</span></div>
  </div>
  <div class="user-box">
    <div>Active Members: <span id="activeMembers">0</span></div>
  </div>
  <div id="photosSection"></div>
  <div id="plansList"></div>
  <button onclick="logout()">Logout</button>
</div>

<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex; gap:8px; align-items:center; margin-top:10px">
    <input id="depositNumber" readonly style="flex:1"/>
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount"/>
  <input id="depositTxId" placeholder="Transaction ID"/>
  <input type="file" id="depositProof"/>
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

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

<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('dashboard')"><span class="ico">ℹ️</span>Company</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}
function login(){
  const u = document.getElementById('user').value.trim();
  if(!u){alert('Enter username');return;}
  currentUser = u;
  localStorage.setItem('nexa_user',currentUser);
  balance = 0; dailyProfit=0; totalProfit=0;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  updateDashboard();
  populatePhotos();
  populatePlans();
}
function logout(){
  currentUser=null;
  localStorage.removeItem('nexa_user');
  location.reload();
}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashTotal').innerText=totalProfit;
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000+1);
  showPage('dashboard');
  document.getElementById('bottomNav').classList.remove('hidden');
}

// Photos
function populatePhotos(){
  const photosSection=document.getElementById('photosSection');
  photosSection.innerHTML='';
  for(let i=1;i<=4;i++){
    const img=document.createElement('img');
    img.src=`https://picsum.photos/400/200?random=${i}`;
    img.className='photo-card';
    photosSection.appendChild(img);
  }
}

// Plans
let plansData=[];
for(let i=1;i<=50;i++){
  const invest = 200 + (i-1)*50;
  const days = 25 + Math.floor((i-1)*(75-25)/49);
  const multiplier = i<=5?2.4:2.2;
  const special=i<=5;
  plansData.push({id:i,name:`Plan ${i}`,invest,days,total:Math.round(invest*multiplier),multiplier,special});
}

function populatePlans(){
  const plansList=document.getElementById('plansList');
  plansList.innerHTML='';
  plansData.forEach(plan=>{
    const div=document.createElement('div');
    div.className='plan-card';
    div.innerHTML=`<strong>${plan.name}${plan.special?' (Special Offer)':''}</strong><br>
      Invest: Rs ${plan.invest} | Days: ${plan.days}<br>
      Total Profit: Rs ${plan.total}<br>
      ${plan.special?`<span class="countdown" id="count${plan.id}">24:00:00</span>`:''}
      <button onclick="buyPlan(${plan.id})">Buy Now</button>`;
    plansList.appendChild(div);
    if(plan.special){
      startCountdown(plan.id);
    }
  });
}
function startCountdown(id){
  let sec=24*60*60;
  const el=document.getElementById('count'+id);
  setInterval(()=>{
    const h=Math.floor(sec/3600); const m=Math.floor((sec%3600)/60); const s=sec%60;
    el.innerText=`${h.toString().padStart(2,'0')}:${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`;
    if(sec>0) sec--;
  },1000);
}
function buyPlan(id){
  const plan=plansData.find(p=>p.id===id);
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
}

// Deposit/Withdraw
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted!'); balance+=parseFloat(document.getElementById('depositAmount').value); dailyProfit+=Math.round(parseFloat(document.getElementById('depositAmount').value)*0.05); totalProfit+=Math.round(parseFloat(document.getElementById('depositAmount').value)*0.05); localStorage.setItem('nexa_balance',balance); localStorage.setItem('nexa_daily',dailyProfit); localStorage.setItem('nexa_total',totalProfit); updateDashboard();}
function submitWithdraw(){alert('Withdrawal requested!');}
</script>
</body>
</html>
