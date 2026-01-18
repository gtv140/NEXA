<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
:root {
  --bg:#0f0f0f;
  --box:#1a1a1a;
  --text:#fff;
  --accent:#00cfff;
  --highlight:#ff5cff;
}
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:linear-gradient(135deg,#0f0f0f,#1a1a1a);
  color:var(--text);
}
header{
  text-align:center;
  font-size:30px;
  font-weight:700;
  padding:20px;
  background:linear-gradient(90deg,var(--accent),var(--highlight));
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
}
.page{
  max-width:480px;
  margin:20px auto;
  padding:20px;
  background:var(--box);
  border-radius:12px;
}
input,select,button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid rgba(0,255,255,0.08);
  background:transparent;
  color:var(--text);
}
button{
  background:linear-gradient(90deg,var(--accent),var(--highlight));
  font-weight:700;
  cursor:pointer;
  transition:all 0.2s ease;
}
button:hover{transform:translateY(-2px);}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  padding:12px 0;
  background:rgba(0,0,0,0.9);
}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.box{
  background: linear-gradient(135deg, rgba(0,255,255,0.05), rgba(255,92,255,0.02));
  padding:14px;
  border-radius:12px;
  margin-bottom:12px;
  border:1px solid rgba(0,255,255,0.06);
  font-weight:700;
}
.plan-box{
  display:flex;
  justify-content:space-between;
  align-items:center;
  gap:12px;
  margin:10px 0;
  padding:12px;
  border-radius:10px;
  background:linear-gradient(135deg, rgba(255,255,255,0.01), rgba(0,0,0,0.04));
  border:1px solid rgba(0,255,240,0.06);
  transition:0.2s;
}
.plan-box:hover{box-shadow:0 12px 30px rgba(0,255,240,0.06);transform:translateY(-2px);}
.countdown{font-weight:700;color:var(--accent);}
.photo-box{
  display:flex;gap:8px;margin:10px 0;overflow-x:auto;
}
.photo-box img{
  height:100px;border-radius:12px;flex-shrink:0;
}
</style>
</head>
<body>
<header>NEXA EARN</header>

<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">New User</option></select>
<input id="user" placeholder="Username"/>
<input id="pass" type="password" placeholder="Password"/>
<button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
<div class="box">Username: <span id="dashUser">-</span></div>
<div class="box">Balance: Rs <span id="dashBalance">0</span></div>
<div class="box">Daily Profit: Rs <span id="dashDaily">0</span></div>
<div class="box">Total Profit: Rs <span id="dashTotal">0</span></div>
<div class="box">Active Members: <span id="activeMembers">0</span></div>
<div class="photo-box" id="photoBox"></div>
<div id="plansList"></div>
<button onclick="logout()">Logout</button>
</div>

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

<div id="about" class="page hidden">
<h2>About NEXA EARN</h2>
<p>NEXA EARN provides secure automated profit solutions. Your balance grows daily. Support available 24/7.</p>
<div class="support-icon" onclick="openSupport()"><span class="ico">🛠️</span>Support</div>
</div>

<div class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
<div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
let currentUser=localStorage.getItem('nexa_user')||null;
let balance=parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit=parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit=parseFloat(localStorage.getItem('nexa_total')||'0');
let activeMembers=parseInt(localStorage.getItem('nexa_activeMembers')||'0');
let userPlans=JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let savedPhotos=JSON.parse(localStorage.getItem('nexa_photos')||'["https://picsum.photos/200/100?random=1","https://picsum.photos/200/100?random=2","https://picsum.photos/200/100?random=3","https://picsum.photos/200/100?random=4","https://picsum.photos/200/100?random=5"]');

let plansData=[];
for(let i=1;i<=50;i++){
  let invest=200+(i-1)*200;
  let days=25+Math.floor((i-1)/5)*5;
  let multiplier=i<=5?2.4:2.2;
  plansData.push({
    id:i,
    name:`Plan ${i}`,
    invest,
    days,
    total:Math.round(invest*multiplier),
    daily:Math.round(invest*multiplier/days),
    special:i<=5
  });
}

function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0; totalProfit=0;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  updateDashboard();
}
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashTotal').innerText=totalProfit;
  document.querySelector('.nav').classList.remove('hidden');
  renderPhotos();
  renderPlans();
}
function renderPhotos(){
  const box=document.getElementById('photoBox'); box.innerHTML='';
  savedPhotos.forEach(src=>{let img=document.createElement('img'); img.src=src; box.appendChild(img);});
}
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    let div=document.createElement('div'); div.className='plan-box';
    let countdownHTML='';
    if(p.special){countdownHTML=`<div class="countdown" id="countdown${p.id}"></div>`; startCountdown(p.id,24*60*60*1000);}
    div.innerHTML=`<div><b>${p.name}</b><div>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div>${countdownHTML}</div>
    <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}
function startCountdown(id,ms){
  const el=document.getElementById(`countdown${id}`);
  let endTime=localStorage.getItem(`countdown_${id}`)||(Date.now()+ms);
  localStorage.setItem(`countdown_${id}`,endTime);
  function update(){
    let remaining=endTime-Date.now();
    if(remaining<0){el.innerText='Offer expired'; return;}
    let h=Math.floor(remaining/3600000); let m=Math.floor((remaining%3600000)/60000); let s=Math.floor((remaining%60000)/1000);
    el.innerText=`Offer ends in ${h}h ${m}m ${s}s`;
    setTimeout(update,1000);
  }
  update();
}
function buyPlan(id){
  let plan=plansData.find(p=>p.id===id);
  if(!plan){alert('Plan not found'); return;}
  if(balance<plan.invest){alert('Insufficient balance'); return;}
  balance-=plan.invest; dailyProfit+=plan.daily; totalProfit+=plan.total;
  userPlans.push({id:plan.id,name:plan.name,daily:plan.daily,total:plan.total});
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  updateDashboard();
  alert('Plan purchased successfully!');
}
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}
function submitWithdraw(){alert('Withdrawal requested');}
function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}

setInterval(()=>{
  activeMembers=Math.floor(Math.random()*5000);
  localStorage.setItem('nexa_activeMembers',activeMembers);
  if(document.getElementById('activeMembers')) document.getElementById('activeMembers').innerHTML=`Active Members: <span>${activeMembers}</span>`;
},5000);
</script>
</body>
</html>
