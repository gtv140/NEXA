<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --primary:#ff5c5c;
  --secondary:#ffb85c;
  --bg:#f9f9f9;
  --text:#111;
  --box:#fff;
}
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:var(--bg);
  color:var(--text);
}
header{
  text-align:center;
  padding:20px;
  font-size:28px;
  font-weight:700;
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
}
.page{
  max-width:500px;
  margin:20px auto;
  background:var(--box);
  padding:20px;
  border-radius:12px;
  box-shadow:0 6px 20px rgba(0,0,0,0.1);
}
input,select,button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid #ccc;
  font-size:14px;
}
button{
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  color:#fff;
  font-weight:700;
  cursor:pointer;
}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  padding:10px 0;
  background:#fff;
  border-top:1px solid #ccc;
}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.box{
  background:#fdfdfd;
  border:1px solid #eee;
  padding:12px;
  border-radius:10px;
  margin-bottom:12px;
  box-shadow:0 4px 15px rgba(0,0,0,0.05);
}
.box b{display:block;margin-bottom:6px;}
.photo-grid{
  display:grid;
  grid-template-columns:repeat(2,1fr);
  gap:10px;
  margin:10px 0;
}
.photo-grid img{
  width:100%;
  border-radius:10px;
}
.countdown{
  font-weight:700;
  color:var(--primary);
}
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
  <div class="box">
    <b>Username:</b> <span id="dashUser">-</span>
    <b>Balance:</b> Rs <span id="dashBalance">0</span>
    <b>Daily Profit:</b> Rs <span id="dashDaily">0</span>
    <b>Total Profit:</b> Rs <span id="dashTotal">0</span>
    <b>Active Members:</b> <span id="activeMembers">0</span>
  </div>
  <div class="box">
    <h3>About NEXA EARN</h3>
    <p>Since 2022, NEXA EARN has been providing safe and profitable digital investment opportunities. Join thousands of active users growing their earnings every day.</p>
    <div class="photo-grid">
      <img src="https://picsum.photos/200/150?random=1" alt="">
      <img src="https://picsum.photos/200/150?random=2" alt="">
      <img src="https://picsum.photos/200/150?random=3" alt="">
      <img src="https://picsum.photos/200/150?random=4" alt="">
    </div>
  </div>
  <button onclick="logout()">Logout</button>
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

<!-- NAVIGATION -->
<div class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');

// 50+ plans
let plansData=[];
for(let i=1;i<=50;i++){
  let invest = 200 + (i-1)*50;
  let days = 25 + Math.floor(i/5)*5;
  let multiplier = i<=5?2.4:2.2;
  plansData.push({
    id:i,
    name:`Plan ${i}`,
    invest,
    days,
    daily:Math.round(invest*multiplier/days),
    total:Math.round(invest*multiplier),
    special:i<=5,
    endTime:i<=5?Date.now()+24*60*60*1000:Date.now()+days*24*60*60*1000
  });
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username');return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')) balance=0;
  if(!localStorage.getItem('nexa_daily')) dailyProfit=0;
  if(!localStorage.getItem('nexa_total')) totalProfit=0;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  updateDashboard();
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
  showPage('dashboard');
  document.querySelector('.nav').classList.remove('hidden');
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){
  let amt = parseFloat(document.getElementById('depositAmount').value);
  if(isNaN(amt)||amt<=0){alert('Enter valid amount');return;}
  balance+=amt;
  totalProfit+=amt*0.02; // Example profit addition
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_total',totalProfit);
  alert('Deposit submitted and approved');
  updateDashboard();
}
function submitWithdraw(){
  let amt = parseFloat(document.getElementById('withdrawAmount').value);
  if(isNaN(amt)||amt<=0){alert('Enter valid amount');return;}
  if(amt>balance){alert('Insufficient balance'); return;}
  balance-=amt;
  localStorage.setItem('nexa_balance',balance);
  alert('Withdrawal requested');
  updateDashboard();
}

// Render plans page
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='box';
    let cd = p.special?`<div class="countdown" id="cd${p.id}">24h offer</div>`:'';
    div.innerHTML=`<b>${p.name}</b>
      <div>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div>
      ${cd}
      <button onclick="buyNow(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}
function buyNow(id){
  let plan = plansData.find(p=>p.id===id);
  if(!plan) return;
  document.getElementById('depositAmount').value = plan.invest;
  showPage('deposit');
}

window.onload=function(){
  if(currentUser) updateDashboard();
  renderPlans();
};
</script>
</body>
</html>
