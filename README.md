<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEXA EARN</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
:root{
  --bg:#f9f7f4;
  --card:#ffffff;
  --accent:#c9a24d;
  --accent2:#e6d3a3;
  --text:#3b3b3b;
  --soft:#f1eee8;
}

*{box-sizing:border-box}
body{
  margin:0;
  font-family: 'Segoe UI', sans-serif;
  background:var(--bg);
  color:var(--text);
}

header{
  padding:18px;
  text-align:center;
  font-size:26px;
  font-weight:700;
  background:linear-gradient(135deg,var(--accent),var(--accent2));
  color:#fff;
}

.page{
  max-width:420px;
  margin:20px auto 90px;
  background:var(--card);
  border-radius:18px;
  padding:18px;
  box-shadow:0 10px 30px rgba(0,0,0,.08);
}

.hidden{display:none}

h3{margin-top:0}

input,select,button{
  width:100%;
  padding:12px;
  margin-top:12px;
  border-radius:12px;
  border:1px solid #ddd;
  font-size:15px;
}

button{
  background:linear-gradient(135deg,var(--accent),var(--accent2));
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
  margin-top:12px;
}

.box{
  background:var(--soft);
  padding:14px;
  border-radius:14px;
  text-align:center;
}

.box b{
  font-size:18px;
  color:#8b6b2f;
}

img{
  width:100%;
  border-radius:16px;
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
  box-shadow:0 -4px 18px rgba(0,0,0,.1);
}

.nav div{
  font-size:13px;
  cursor:pointer;
  color:#8b6b2f;
  font-weight:600;
}

.plan{
  background:var(--soft);
  padding:14px;
  border-radius:14px;
  margin-top:12px;
}
.plan b{color:#8b6b2f}

.small{font-size:13px;color:#666}
</style>
</head>

<body>

<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
  <h3>Secure Login</h3>
  <select id="mode">
    <option value="login">Login</option>
    <option value="signup">Signup</option>
  </select>
  <input id="username" placeholder="Username">
  <input id="password" type="password" placeholder="Password">
  <button onclick="login()">Continue</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <h3>Hello, <span id="uName"></span></h3>

  <div class="stats">
    <div class="box">Balance<br><b>Rs <span id="bal">0</span></b></div>
    <div class="box">Daily Profit<br><b>Rs <span id="daily">0</span></b></div>
    <div class="box">Total Profit<br><b>Rs <span id="total">0</span></b></div>
    <div class="box">Active Users<br><b><span id="users">0</span></b></div>
  </div>

  <img src="https://picsum.photos/400/220?random=1">
  <img src="https://picsum.photos/400/220?random=2">
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h3>Investment Plans</h3>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h3>Deposit Funds</h3>
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
  if(!u) return alert('Username required')
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
  users.innerText=Math.floor(1500+Math.random()*3500)
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
    let tot=Math.round(inv*2.3)
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
