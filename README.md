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
}
header{
  text-align:center;
  padding:18px;
  font-size:26px;
  font-weight:700;
  color:var(--gold);
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
  box-shadow:0 6px 18px rgba(0,0,0,.4);
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
}
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
}
.stat b{display:block;font-size:15px;margin-top:3px}
.s-user{background:linear-gradient(135deg,#6366f1,#9333ea)}
.s-bal{background:linear-gradient(135deg,#16a34a,#4ade80)}
.s-day{background:linear-gradient(135deg,#ef4444,#f97316)}
.s-total{background:linear-gradient(135deg,#facc15,#fde047);color:#000}
.s-mem{background:linear-gradient(135deg,#334155,#475569)}

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

/* TASK HISTORY */
let history=JSON.parse(localStorage.getItem('nx_history')||'[]');

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
  history.push(`Deposit Rs ${a} - ${new Date().toLocaleString()}`);
  update();
  alert('Deposit Submitted');
  show('dashboard');
}

function submitWithdraw(){
  let a=parseFloat(wAmt.value);
  if(a>bal)return alert('Low balance');
  bal-=a;
  history.push(`Withdrawal Rs ${a} - ${new Date().toLocaleString()}`);
  update();
  alert('Withdraw Requested');
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
    <button onclick="buy(${inv})">Buy Now</button>
  </div>`;
}
planList.innerHTML=planHTML;

function buy(a){
  depAmt.value=a;
  show('deposit');
}

/* Ads Plans */
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

let adsBought={}; // key: planID, value: tasks left
function buyAds(id){
  let price=500+id*50;
  if(price>bal){alert('Insufficient balance'); return;}
  bal-=price;
  adsBought[id]={tasksLeft:id+2, lastClaim:new Date().toDateString()};
  history.push(`Bought Ads Plan ${id} - ${new Date().toLocaleString()}`);
  update();
  alert(`Ads Plan ${id} bought! Daily tasks unlocked.`);
  show('dashboard');
}

/* Daily Ads Task System */
function showAdsTasks(){
  let html='';
  let today=new Date().toDateString();
  for(let id in adsBought){
    let plan=adsBought[id];
    if(plan.lastClaim!==today){plan.tasksLeft=id*1+2; plan.lastClaim=today;}
    html+=`<div class="card">
      <b>Ads Plan ${id}</b><br>
      Tasks Left Today: ${plan.tasksLeft}<br>
      <button onclick="completeTask(${id})" ${plan.tasksLeft<=0?'disabled':''}>Watch Ad</button>
    </div>`;
  }
  adsList.innerHTML=html;
}

function completeTask(id){
  let plan=adsBought[id];
  if(plan.tasksLeft<=0)return alert('No tasks left today');
  plan.tasksLeft--;
  let profit=50; // example daily profit
  bal+=profit;
  day+=profit;
  total+=profit;
  history.push(`Ads Plan ${id} task completed, earned Rs ${profit} - ${new Date().toLocaleString()}`);
  update();
  showAdsTasks();
  alert(`Task completed! Rs ${profit} added to balance`);
}

/* Auto login and daily task init */
if(user){
  update();
  show('dashboard');
  showAdsTasks();
}
</script>

</body>
</html>
