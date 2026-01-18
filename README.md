<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEXA EARN</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body{margin:0;font-family:Arial;background:#f5f6f8;color:#333}
header{background:#fff;padding:15px;text-align:center;font-size:24px;font-weight:700;color:#c8a24a;box-shadow:0 4px 10px rgba(0,0,0,.05)}
.page{max-width:430px;margin:20px auto 90px;background:#fff;border-radius:14px;padding:18px;box-shadow:0 8px 25px rgba(0,0,0,.08)}
.hidden{display:none}
input,select,button{width:100%;padding:12px;margin-top:10px;border-radius:10px;border:1px solid #ddd;font-size:15px}
button{background:#c8a24a;color:#fff;font-weight:600;border:none;cursor:pointer}
.stats{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:14px}
.box{background:#f1f1f1;padding:14px;border-radius:12px;text-align:center}
.box b{color:#c8a24a;font-size:18px}
.nav{position:fixed;bottom:0;left:0;right:0;background:#fff;display:flex;justify-content:space-around;padding:10px 0;box-shadow:0 -4px 14px rgba(0,0,0,.08)}
.nav div{font-size:13px;font-weight:600;color:#c8a24a;cursor:pointer}
.plan{background:#f1f1f1;padding:14px;border-radius:12px;margin-top:12px}
.offer{border:2px dashed #c8a24a}
.small{font-size:13px;color:#666}
.timer{font-weight:700;color:#c0392b;margin-top:6px}
</style>
</head>
<body>

<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
  <h3>Login / Signup</h3>
  <input id="user" placeholder="Username">
  <input id="pass" type="password" placeholder="Password">
  <button onclick="login()">Continue</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <h3>Welcome, <span id="uname"></span></h3>
  <div class="stats">
    <div class="box">Balance<br><b>Rs <span id="bal">0</span></b></div>
    <div class="box">Daily Profit<br><b>Rs <span id="daily">0</span></b></div>
    <div class="box">Total Profit<br><b>Rs <span id="total">0</span></b></div>
    <div class="box">Active Users<br><b><span id="users">0</span></b></div>
  </div>
  <p class="small" style="margin-top:14px">
    NEXA EARN provides structured earning plans with automatic daily profit
    based on your active investments.
  </p>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h3>Investment Plans</h3>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h3>Deposit</h3>
  <select id="depMethod" onchange="setNum()">
    <option value="jazz">JazzCash</option>
    <option value="easy">EasyPaisa</option>
  </select>
  <input id="depNum" readonly>
  <input id="depAmt" placeholder="Amount">
  <button onclick="deposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdraw" class="page hidden">
  <h3>Withdraw</h3>
  <input id="wAmt" placeholder="Amount">
  <button onclick="withdraw()">Request Withdrawal</button>
</div>

<!-- NAV -->
<div id="nav" class="nav hidden">
  <div onclick="show('dashboard')">Home</div>
  <div onclick="show('plans')">Plans</div>
  <div onclick="show('deposit')">Deposit</div>
  <div onclick="show('withdraw')">Withdraw</div>
  <div onclick="logout()">Logout</div>
</div>

<script>
let balance=+localStorage.getItem('bal')||0
let daily=+localStorage.getItem('daily')||0
let total=+localStorage.getItem('total')||0
let activePlan=JSON.parse(localStorage.getItem('plan')||'null')
let offerEnd=localStorage.getItem('offerEnd')||Date.now()+3600000

function login(){
  if(!user.value)return alert('Enter username')
  localStorage.setItem('user',user.value)
  localStorage.setItem('pass',pass.value)
  load()
}

function load(){
  let u=localStorage.getItem('user')
  if(!u)return
  loginPage.classList.add('hidden')
  nav.classList.remove('hidden')
  uname.innerText=u
  users.innerText=Math.floor(1000+Math.random()*4000)
  update()
  show('dashboard')
}

function update(){
  bal.innerText=balance
  daily.innerText=daily
  total.innerText=total
  localStorage.setItem('bal',balance)
  localStorage.setItem('daily',daily)
  localStorage.setItem('total',total)
}

function logout(){
  localStorage.removeItem('user')
  location.reload()
}

function show(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'))
  document.getElementById(id).classList.remove('hidden')
}

function setNum(){
  depNum.value=depMethod.value==='jazz'?'03705519562':'03379827882'
}

function deposit(){
  let a=+depAmt.value
  if(a<=0)return alert('Invalid amount')
  balance+=a
  update()
  alert('Deposit Successful')
}

function withdraw(){
  let a=+wAmt.value
  if(a<=0||a>balance)return alert('Invalid amount')
  balance-=a
  update()
  alert('Withdrawal Requested')
}

function buyPlan(p,rate){
  if(balance<p)return alert('Insufficient balance')
  balance-=p
  activePlan={amount:p,rate:rate,last:Date.now()}
  localStorage.setItem('plan',JSON.stringify(activePlan))
  update()
  alert('Plan Activated')
}

function profitTick(){
  if(!activePlan)return
  let now=Date.now()
  let diff=Math.floor((now-activePlan.last)/86400000)
  if(diff>0){
    let earn=Math.floor(activePlan.amount*activePlan.rate)*diff
    balance+=earn
    total+=earn
    daily=Math.floor(activePlan.amount*activePlan.rate)
    activePlan.last+=diff*86400000
    localStorage.setItem('plan',JSON.stringify(activePlan))
    update()
  }
}

function loadPlans(){
  let h=''
  for(let i=1;i<=50;i++){
    let inv=200*i
    let rate=0.022
    let offer=i<=5
    if(offer)rate=0.024
    h+=`
    <div class="plan ${offer?'offer':''}">
      <b>Plan ${i}</b><br>
      <span class="small">Invest: Rs ${inv}</span><br>
      <span class="small">Daily Profit: ${Math.floor(inv*rate)}</span><br>
      ${offer?'<div class="timer" id="t'+i+'"></div>':''}
      <button onclick="buyPlan(${inv},${rate})">Buy Now</button>
    </div>`
  }
  plansList.innerHTML=h
}

function timers(){
  let left=Math.max(0,offerEnd-Date.now())
  let m=Math.floor(left/60000)
  let s=Math.floor(left/1000)%60
  document.querySelectorAll('.timer').forEach(t=>t.innerText=`Offer Ends: ${m}:${s<10?'0':''}${s}`)
}

setInterval(()=>{profitTick();timers()},1000)

window.onload=()=>{
  setNum()
  loadPlans()
  load()
}
</script>
</body>
</html>
