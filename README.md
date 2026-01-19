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
}
body{
  margin:0;
  font-family:Arial,Helvetica,sans-serif;
  background:linear-gradient(135deg,#0f1117,#14182a);
  color:#fff;
  overflow-x:hidden;
}
header{
  text-align:center;
  padding:18px;
  font-size:26px;
  font-weight:700;
  color:var(--gold);
  animation: neon 2s ease-in-out infinite alternate;
}
@keyframes neon{
  0%{text-shadow:0 0 5px var(--gold),0 0 10px var(--gold);}
  100%{text-shadow:0 0 20px var(--gold),0 0 30px var(--gold);}
}
.page{
  max-width:480px;
  margin:15px auto 90px;
  padding:15px;
  display:none;
  animation: fadeIn 0.5s ease forwards;
}
.show{display:block;}
@keyframes fadeIn{
  from{opacity:0; transform:translateY(10px);}
  to{opacity:1; transform:translateY(0);}
}
.card{
  background:var(--card);
  border-radius:14px;
  padding:14px;
  margin-bottom:12px;
  box-shadow:0 6px 18px rgba(0,0,0,.4);
  transition:0.3s;
}
.card:hover{
  transform:scale(1.02);
  box-shadow:0 8px 22px rgba(255,255,0,.5);
}
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
  transition:0.2s;
}
button:hover{opacity:0.85}
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
  animation: glow 2s ease-in-out infinite alternate;
}
.stat b{display:block;font-size:15px;margin-top:3px}
.s-user{background:linear-gradient(135deg,#6366f1,#9333ea)}
.s-bal{background:linear-gradient(135deg,#16a34a,#4ade80)}
.s-day{background:linear-gradient(135deg,#ef4444,#f97316)}
.s-total{background:linear-gradient(135deg,#facc15,#fde047);color:#000}
.s-mem{background:linear-gradient(135deg,#334155,#475569)}

@keyframes glow{
  0%{box-shadow:0 0 4px rgba(255,255,255,.2);}
  100%{box-shadow:0 0 12px rgba(255,255,0,.4);}
}

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
  transition:0.3s;
}
.icon:hover{
  transform:scale(1.05);
  box-shadow:0 0 12px #ff0;
}
.icon span{font-size:22px;display:block;margin-bottom:4px}

img.banner{
  width:100%;
  border-radius:12px;
  margin-top:10px;
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

.small{font-size:12px;opacity:.8}
.timer{
  font-size:12px;
  margin-top:5px;
  color:#facc15;
}
</style>
</head>
<body>

<header>NEXA EARN</header>

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
    <div class="icon" onclick="openPage('history')"><span>🕒</span>History</div>
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
  <div class="card">
    <p class="small">
      Buy Ads Plan → Daily ads unlock → Watch ads → Daily profit auto add.
    </p>
  </div>
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

<!-- HISTORY -->
<div id="history" class="page">
  <h3>History / Notifications</h3>
  <div id="historyList"></div>
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
// User data
let user=localStorage.getItem('nx_user');
let bal=parseFloat(localStorage.getItem('nx_bal')||0);
let day=parseFloat(localStorage.getItem('nx_day')||0);
let total=parseFloat(localStorage.getItem('nx_total')||0);
let history=JSON.parse(localStorage.getItem('nx_history')||'[]');
let adsBought=JSON.parse(localStorage.getItem('nx_ads')||'{}');
let tasksDone=JSON.parse(localStorage.getItem('nx_tasks')||'{}');

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
  addHistory('Logged in');
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
  localStorage.setItem('nx_history',JSON.stringify(history));
  localStorage.setItem('nx_ads',JSON.stringify(adsBought));
  localStorage.setItem('nx_tasks',JSON.stringify(tasksDone));
  renderHistory();
}

function logout(){
  addHistory('Logged out');
  localStorage.removeItem('nx_user');
  location.reload();
}

function openPage(p){show(p); renderHistory(); renderAds(); renderPlans();}

function addHistory(msg){
  history.unshift({time:new Date().toLocaleString(),msg});
  if(history.length>50) history.pop();
}

// Deposit
function setNumber(){ depNum.value = depMethod.value==='jazz'?'03705519562':'03379827882';}
setNumber();
function copyNum(){navigator.clipboard.writeText(depNum.value);alert('Copied');}
function submitDeposit(){
  let a=parseFloat(depAmt.value);
  if(!a)return alert('Enter amount');
  bal+=a;
  total+=a*0.1;
  addHistory(`Deposit Rs ${a}`);
  update();
  alert('Deposit Submitted');
  show('dashboard');
}
function submitWithdraw(){
  let a=parseFloat(wAmt.value);
  if(a>bal)return alert('Low balance');
  bal-=a;
  addHistory(`Withdraw Rs ${a}`);
  update();
  alert('Withdraw Requested');
  show('dashboard');
}

// Plans
let plans=[];
for(let i=1;i<=10;i++){
  plans.push({id:i,name:'Plan '+i,invest:200+i*50,mult:i<=3?2.5:2.2});
}
function renderPlans(){
  let html='';
  plans.forEach(p=>{
    html+=`<div class="card">
    <b>${p.name}</b><br>
    Invest: Rs ${p.invest}<br>
    Total: Rs ${Math.round(p.invest*p.mult)}<br>
    <button onclick="buyPlan(${p.id})">Buy</button>
    </div>`;
  });
  planList.innerHTML=html;
}
function buyPlan(id){
  let p=plans.find(pl=>pl.id===id);
  if(bal<p.invest)return alert('Low balance');
  bal-=p.invest;
  total+=p.invest*0.05;
  adsBought[id]=adsBought[id]||{tasks:[],last:new Date().getTime()};
  addHistory(`Bought ${p.name}`);
  update();
  show('dashboard');
}

// Ads
let ads=[];
for(let i=1;i<=5;i++){ads.push({id:i,name:'Ads '+i,price:200+i*50,daily:3});}
function renderAds(){
  let html='';
  ads.forEach(a=>{
    let remaining=0;
    if(adsBought[a.id]){
      let last=adsBought[a.id].last;
      let now=new Date().getTime();
      let diff=Math.max(0,24*60*60*1000-(now-last));
      remaining=Math.floor(diff/1000);
    }
    html+=`<div class="card">
      <b>${a.name}</b><br>
      Price: Rs ${a.price}<br>
      Daily Tasks: ${a.daily}<br>
      ${remaining>0?`Next task in: <span class="timer">${formatTime(remaining)}</span>`:
      `<button onclick="buyAds(${a.id})">Buy/Start</button>`}
    </div>`;
  });
  adsList.innerHTML=html;
}
function buyAds(id){
  let a=ads.find(ad=>ad.id===id);
  if(bal<a.price)return alert('Low balance');
  bal-=a.price;
  adsBought[id]={tasks:[],last:new Date().getTime()};
  addHistory(`Bought ${a.name}`);
  update();
  renderAds();
}

function formatTime(sec){
  let h=Math.floor(sec/3600); sec%=3600;
  let m=Math.floor(sec/60); sec%=60;
  let s=sec;
  return `${h}h ${m}m ${s}s`;
}

function renderHistory(){
  let html='';
  history.forEach(h=>{html+=`<div class="card"><small>${h.time}</small><br>${h.msg}</div>`});
  historyList.innerHTML=html;
}

// Auto login
if(user){update(); show('dashboard');}
</script>

</body>
</html>
