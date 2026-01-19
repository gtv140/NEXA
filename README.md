<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root{
  --red:#ff2d2d;
  --blue:#2563ff;
  --gold:#f5c542;
  --dark:#0f1117;
  --card:#181b24;
  --green:#16a34a;
}
body{
  margin:0;
  font-family:Arial,Helvetica,sans-serif;
  background:linear-gradient(135deg,#0f1117,#14182a);
  color:#fff;
}
header{
  text-align:center;
  padding:18px;
  font-size:26px;
  font-weight:700;
  color:var(--gold);
  text-shadow:0 0 10px var(--gold);
}
.page{
  max-width:480px;
  margin:15px auto 90px;
  padding:15px;
  display:none;
}
.show{display:block;}
.card{
  background:var(--card);
  border-radius:14px;
  padding:14px;
  margin-bottom:12px;
  box-shadow:0 0 20px rgba(255,255,255,.2);
  transition:0.3s;
}
.card:hover{box-shadow:0 0 25px var(--gold);}
input,select,button{
  width:100%;
  padding:11px;
  margin-top:10px;
  border-radius:10px;
  border:none;
  outline:none;
}
input,select{
  background:#0f1320;
  color:#fff;
}
button{
  background:linear-gradient(90deg,var(--red),var(--gold));
  color:#000;
  font-weight:700;
  cursor:pointer;
  box-shadow:0 0 8px var(--gold);
  transition:0.2s;
}
button:hover{box-shadow:0 0 15px var(--gold); transform:scale(1.02);}
button:active{transform:scale(.98)}

.stats{
  display:grid;
  grid-template-columns:repeat(3,1fr);
  gap:10px;
}
.stat{
  padding:10px;
  border-radius:12px;
  text-align:center;
  font-size:13px;
  box-shadow:0 0 10px rgba(255,255,255,.1);
}
.stat b{display:block;font-size:15px;margin-top:3px; color:#fff;}
.s-user{background:linear-gradient(135deg,#6366f1,#9333ea);box-shadow:0 0 10px #9333ea;}
.s-bal{background:linear-gradient(135deg,#16a34a,#4ade80);box-shadow:0 0 10px #4ade80;}
.s-day{background:linear-gradient(135deg,#ef4444,#f97316);box-shadow:0 0 10px #f97316;}
.s-total{background:linear-gradient(135deg,#facc15,#fde047);color:#000;box-shadow:0 0 10px #fde047;}
.s-mem{background:linear-gradient(135deg,#334155,#475569);box-shadow:0 0 10px #475569;}

.icons{
  display:grid;
  grid-template-columns:repeat(3,1fr);
  gap:10px;
  margin-top:15px;
}
.icon{
  background:#0f1320;
  padding:14px 5px;
  border-radius:14px;
  text-align:center;
  cursor:pointer;
  border:1px solid rgba(255,255,255,.06);
  box-shadow:0 0 10px rgba(255,255,255,.1);
  transition:0.2s;
}
.icon:hover{box-shadow:0 0 15px var(--gold);}
.icon span{font-size:22px;display:block;margin-bottom:4px;}

img.banner{
  width:100%;
  border-radius:12px;
  margin-top:10px;
  animation:slide 10s infinite alternate;
}
@keyframes slide{
  0%{transform:translateX(0);}
  50%{transform:translateX(-5px);}
  100%{transform:translateX(0);}
}

.nav{
  position:fixed;
  bottom:0;left:0;right:0;
  display:flex;
  justify-content:space-around;
  background:#0b0e18;
  padding:8px 0;
}
.nav div{
  text-align:center;
  font-size:12px;
  cursor:pointer;
}
.nav span{font-size:20px;display:block}

.small{font-size:12px;opacity:.8;}
.alert{
  position:fixed;
  top:10px;
  left:50%;
  transform:translateX(-50%);
  background:var(--gold);
  color:#000;
  padding:10px 15px;
  border-radius:12px;
  box-shadow:0 0 15px var(--gold);
  display:none;
  z-index:9999;
}
.timer{color:#facc15;font-weight:700;}
</style>
</head>

<body>
<header>NEXA EARN</header>

<div id="alert" class="alert"></div>

<!-- LOGIN -->
<div id="login" class="page show">
  <div class="card">
    <h3>Login / Signup</h3>
    <input id="lUser" placeholder="Username">
    <input id="lPass" type="password" placeholder="Password">
    <button onclick="login()">Continue</button>
  </div>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page">
  <div class="stats">
    <div class="stat s-user"><small>User</small><b id="dUser">-</b></div>
    <div class="stat s-bal"><small>Balance</small><b>Rs <span id="dBal">0</span></b></div>
    <div class="stat s-day"><small>Daily</small><b>Rs <span id="dDay">0</span></b></div>
    <div class="stat s-total"><small>Total</small><b>Rs <span id="dTotal">0</span></b></div>
    <div class="stat s-mem"><small>Active</small><b id="dMem">0</b></div>
  </div>

  <div class="card">
    <h3>About NEXA EARN</h3>
    <p class="small">
      Since 2022, NEXA EARN provides digital earning opportunities through
      investment plans & ad-based tasks. Secure system, fast processing,
      user-friendly dashboard.
    </p>
    <img class="banner" src="https://picsum.photos/500/250?random=11">
    <img class="banner" src="https://picsum.photos/500/250?random=12">
  </div>

  <div class="icons">
    <div class="icon" onclick="openPage('plans')"><span>📦</span>Plans</div>
    <div class="icon" onclick="openPage('ads')"><span>🎬</span>Watch Ads</div>
    <div class="icon" onclick="openPage('deposit')"><span>💰</span>Deposit</div>
    <div class="icon" onclick="openPage('withdraw')"><span>💵</span>Withdraw</div>
    <div class="icon" onclick="openPage('support')"><span>🛠️</span>Support</div>
    <div class="icon" onclick="logout()"><span>🚪</span>Logout</div>
  </div>
</div>

<!-- PLANS -->
<div id="plans" class="page">
  <h3>Investment Plans</h3>
  <div id="planList"></div>
</div>

<!-- ADS -->
<div id="ads" class="page">
  <h3>Watch Ads & Earn</h3>
  <div id="adsList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page">
  <h3>Deposit</h3>
  <select id="depMethod" onchange="setNumber()">
    <option value="jazz">JazzCash</option>
    <option value="easy">EasyPaisa</option>
  </select>
  <input id="depNum" readonly>
  <button onclick="copyNum()">Copy Number</button>
  <input id="depAmt" placeholder="Amount">
  <input id="depTx" placeholder="Transaction ID">
  <button onclick="submitDeposit()">Submit</button>
</div>

<!-- WITHDRAW -->
<div id="withdraw" class="page">
  <h3>Withdraw</h3>
  <select>
    <option>JazzCash</option>
    <option>EasyPaisa</option>
  </select>
  <input id="wAcc" placeholder="Account Number">
  <input id="wAmt" placeholder="Amount">
  <button onclick="submitWithdraw()">Request</button>
</div>

<!-- SUPPORT -->
<div id="support" class="page">
  <div class="card">
    <h3>Support</h3>
    <p>WhatsApp Group</p>
    <a href="https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu" target="_blank">Join</a>
    <p>Email: rock.earn92@gmail.com</p>
  </div>
</div>

<!-- NAV -->
<div class="nav">
  <div onclick="openPage('dashboard')"><span>🏠</span>Home</div>
  <div onclick="openPage('plans')"><span>📦</span>Plans</div>
  <div onclick="openPage('ads')"><span>🎬</span>Ads</div>
  <div onclick="openPage('deposit')"><span>💰</span>Deposit</div>
</div>

<script>
let user=localStorage.getItem('nx_user');
let bal=parseFloat(localStorage.getItem('nx_bal')||0);
let day=parseFloat(localStorage.getItem('nx_day')||0);
let total=parseFloat(localStorage.getItem('nx_total')||0);

let adsTasks={}; // track ads per user

function show(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('show'));
  document.getElementById(id).classList.add('show');
}

function login(){
  let u=lUser.value.trim();
  let p=lPass.value.trim();
  if(!u||!p)return alert('Enter details');
  localStorage.setItem('nx_user',u);
  user=u;
  if(!localStorage.getItem('nx_adsTasks')) localStorage.setItem('nx_adsTasks',JSON.stringify({}));
  adsTasks=JSON.parse(localStorage.getItem('nx_adsTasks'));
  if(!adsTasks[user]) adsTasks[user]=[];
  update();
  show('dashboard');
}

function update(){
  dUser.innerText=user;
  dBal.innerText=bal.toFixed(0);
  dDay.innerText=day.toFixed(0);
  dTotal.innerText=total.toFixed(0);
  dMem.innerText=Math.floor(Math.random()*4000)+800;
  localStorage.setItem('nx_bal',bal);
  localStorage.setItem('nx_day',day);
  localStorage.setItem('nx_total',total);
  localStorage.setItem('nx_adsTasks',JSON.stringify(adsTasks));
}

function logout(){
  localStorage.removeItem('nx_user');
  location.reload();
}

function openPage(p){show(p);}

function setNumber(){
  depNum.value = depMethod.value==='jazz'?'03705519562':'03379827882';
}
setNumber();

function copyNum(){navigator.clipboard.writeText(depNum.value);alert('Copied');}

function submitDeposit(){
  let a=parseFloat(depAmt.value);
  if(!a)return alert('Enter amount');
  bal+=a;
  total+=a*0.1;
  showAlert('Deposit Submitted!');
  update();
  show('dashboard');
}

function submitWithdraw(){
  let a=parseFloat(wAmt.value);
  if(a>bal)return alert('Low balance');
  bal-=a;
  showAlert('Withdraw Requested!');
  update();
  show('dashboard');
}

/* Plans */
let planHTML='';
for(let i=1;i<=50;i++){
  let inv=200+i*50;
  let mul=i<=5?2.4:2.2;
  planHTML+=`
  <div class="card">
    <b>Plan ${i} ${i<=5?'(Special)':""}</b><br>
    Invest: Rs ${inv}<br>
    Days: ${25+i}<br>
    Total: Rs ${Math.round(inv*mul)}<br>
    <button onclick="buyPlan(${i})">Buy Now</button>
  </div>`;
}
planList.innerHTML=planHTML;

function buyPlan(id){
  let inv=200+id*50;
  depAmt.value=inv;
  show('deposit');
  showAlert(`Plan ${id} selected. Please deposit to unlock tasks.`);
}

/* Ads */
let adsHTML='';
for(let i=1;i<=7;i++){
  adsHTML+=`
  <div class="card">
    <b>Ads Plan ${i}</b><br>
    Price: Rs ${500+i*50}<br>
    Daily Ads: ${i+2}<br>
    <button onclick="buyAds(${i})">Buy Ads Plan</button>
  </div>`;
}
adsList.innerHTML=adsHTML;

function buyAds(id){
  if(!adsTasks[user]) adsTasks[user]=[];
  adsTasks[user].push({plan:id,remaining:id+2,lastClaim:Date.now()});
  localStorage.setItem('nx_adsTasks',JSON.stringify(adsTasks));
  showAlert(`Ads Plan ${id} bought! Daily tasks unlocked.`);
  show('ads');
  renderTasks();
}

function renderTasks(){
  let html='';
  let userTasks=adsTasks[user]||[];
  userTasks.forEach((t,i)=>{
    let now=Date.now();
    let last=new Date(t.lastClaim).getTime();
    let diff=Math.floor((now-last)/1000);
    let waitTime=86400-diff; // 24h cooldown
    if(waitTime<0) waitTime=0;
    html+=`
    <div class="card">
      <b>Ads Task Plan ${t.plan}</b><br>
      Remaining: ${t.remaining}<br>
      ${waitTime>0?`Next Task in: <span class="timer">${formatTime(waitTime)}</span>`:`<button onclick="claimTask(${i})">Watch Task</button>`}
    </div>`;
  });
  adsList.innerHTML=html;
}

function claimTask(index){
  let t=adsTasks[user][index];
  if(t.remaining<=0)return alert('No remaining tasks');
  let now=Date.now();
  if((now - t.lastClaim)<86400000)return alert('Task not yet available');
  t.remaining--;
  t.lastClaim=now;
  bal+=50; // profit per task
  day+=50;
  total+=50;
  showAlert(`Task completed! Rs 50 added.`);
  update();
  renderTasks();
}

function formatTime(sec){
  let h=Math.floor(sec/3600);
  let m=Math.floor((sec%3600)/60);
  let s=sec%60;
  return `${h}h ${m}m ${s}s`;
}

function showAlert(msg){
  let a=document.getElementById('alert');
  a.innerText=msg;
  a.style.display='block';
  setTimeout(()=>{a.style.display='none';},3000);
}

/* Auto login */
if(user){
  adsTasks=JSON.parse(localStorage.getItem('nx_adsTasks')||'{}');
  update();
  show('dashboard');
  renderTasks();
}
</script>
</body>
</html>
