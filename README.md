<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEXA EARN</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
:root{
  --bg:#f4f6f8;
  --card:#ffffff;
  --gold:#c8a24a;
  --soft:#f1f1f1;
  --text:#333;
}

*{box-sizing:border-box}

body{
  margin:0;
  font-family:Arial,Helvetica,sans-serif;
  background:var(--bg);
  color:var(--text);
}

header{
  background:#fff;
  padding:16px;
  text-align:center;
  font-size:24px;
  font-weight:700;
  color:var(--gold);
  box-shadow:0 4px 10px rgba(0,0,0,.05);
}

.page{
  max-width:430px;
  margin:20px auto 90px;
  background:var(--card);
  border-radius:14px;
  padding:18px;
  box-shadow:0 8px 25px rgba(0,0,0,.08);
}

.hidden{display:none}

h3{margin-top:0}

input,select,button{
  width:100%;
  padding:12px;
  margin-top:12px;
  border-radius:10px;
  border:1px solid #ddd;
  font-size:15px;
}

button{
  background:var(--gold);
  color:#fff;
  font-weight:600;
  border:none;
  cursor:pointer;
}

button:active{transform:scale(.98)}

.stats{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:12px;
  margin-top:14px;
}

.box{
  background:var(--soft);
  padding:14px;
  border-radius:12px;
  text-align:center;
}

.box b{
  font-size:18px;
  color:var(--gold);
}

.section{
  margin-top:18px;
}

img{
  width:100%;
  border-radius:14px;
  margin-top:12px;
}

.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  background:#fff;
  display:flex;
  justify-content:space-around;
  padding:10px 0;
  box-shadow:0 -4px 14px rgba(0,0,0,.08);
}

.nav div{
  font-size:13px;
  font-weight:600;
  color:var(--gold);
  cursor:pointer;
}

.plan{
  background:var(--soft);
  padding:14px;
  border-radius:12px;
  margin-top:12px;
}

.plan b{color:var(--gold)}

.small{font-size:13px;color:#666}
</style>
</head>

<body>

<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
  <h3>Account Access</h3>
  <select id="mode">
    <option value="login">Login</option>
    <option value="signup">Create Account</option>
  </select>
  <input id="username" placeholder="Username">
  <input id="password" type="password" placeholder="Password">
  <button onclick="login()">Continue</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <h3>Welcome, <span id="uName"></span></h3>

  <div class="stats">
    <div class="box">Balance<br><b>Rs <span id="bal">0</span></b></div>
    <div class="box">Daily Profit<br><b>Rs <span id="daily">0</span></b></div>
    <div class="box">Total Profit<br><b>Rs <span id="total">0</span></b></div>
    <div class="box">Active Users<br><b><span id="users">0</span></b></div>
  </div>

  <div class="section">
    <p class="small">
      NEXA EARN is a digital earning platform designed to provide
      stable returns through structured investment plans.
      Your earnings update automatically based on active plans.
    </p>
  </div>

  <img src="https://picsum.photos/400/230?random=11">
  <img src="https://picsum.photos/400/230?random=12">
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h3>Available Plans</h3>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h3>Deposit</h3>
  <select id="depMethod" onchange="setNumber()">
    <option value="jazz">JazzCash</option>
    <option value="easy">EasyPaisa</option>
  </select>
  <input id="depNum" readonly>
  <button onclick="copyNum()">Copy Number</button>
  <input placeholder="Amount">
  <input placeholder="Transaction ID">
  <input type="file">
  <button>Submit</button>
</div>

<!-- NAV -->
<div id="nav" class="nav hidden">
  <div onclick="show('dashboard')">Home</div>
  <div onclick="show('plans')">Plans</div>
  <div onclick="show('deposit')">Deposit</div>
  <div onclick="logout()">Logout</div>
</div>

<script>
let balance=Number(localStorage.getItem('bal')||0)
let daily=Number(localStorage.getItem('daily')||0)
let total=Number(localStorage.getItem('total')||0)

function login(){
  let u=username.value.trim()
  if(!u) return alert('Enter username')
  localStorage.setItem('user',u)
  if(mode.value==='signup'){
    balance=0;daily=0;total=0
    localStorage.setItem('bal',0)
    localStorage.setItem('daily',0)
    localStorage.setItem('total',0)
  }
  load()
}

function load(){
  let u=localStorage.getItem('user')
  if(!u) return
  loginPage.classList.add('hidden')
  nav.classList.remove('hidden')
  show('dashboard')
  uName.innerText=u
  bal.innerText=balance
  daily.innerText=daily
  total.innerText=total
  users.innerText=Math.floor(1200+Math.random()*3800)
}

function logout(){
  localStorage.removeItem('user')
  location.reload()
}

function show(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'))
  document.getElementById(id).classList.remove('hidden')
}

function setNumber(){
  depNum.value = depMethod.value==='jazz'
  ? '03705519562'
  : '03379827882'
}

function copyNum(){
  navigator.clipboard.writeText(depNum.value)
}

function loadPlans(){
  let h=''
  for(let i=1;i<=50;i++){
    let inv=200*i
    let tot=Math.round(inv*2.2)
    h+=`
    <div class="plan">
      <b>Plan ${i}</b><br>
      <span class="small">Investment: Rs ${inv}</span><br>
      <span class="small">Total Return: Rs ${tot}</span><br><br>
      <button>Activate</button>
    </div>`
  }
  plansList.innerHTML=h
}

window.onload=()=>{
  load()
  loadPlans()
  setNumber()
}
</script>

</body>
</html>
