<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEXA EARN</title>
<meta name="viewport" content="width=device-width, initial-scale=1">

<style>
body{
  margin:0;
  font-family:Arial,Helvetica,sans-serif;
  background:#f4f6f8;
  color:#222;
}
header{
  background:#111;
  color:#fff;
  padding:15px;
  text-align:center;
  font-size:24px;
  font-weight:bold;
}
.page{
  max-width:430px;
  margin:20px auto;
  background:#fff;
  padding:16px;
  border-radius:10px;
  box-shadow:0 4px 20px rgba(0,0,0,.08);
}
.hidden{display:none}
input,select,button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:6px;
  border:1px solid #ccc;
  font-size:14px;
}
button{
  background:#111;
  color:#fff;
  border:none;
  cursor:pointer;
}
button:hover{opacity:.9}
.box{
  background:#f1f3f5;
  padding:12px;
  border-radius:8px;
  margin-bottom:10px;
}
.flex{
  display:flex;
  justify-content:space-between;
  gap:10px;
}
.photos img{
  width:100%;
  border-radius:8px;
  margin-top:8px;
}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  background:#fff;
  border-top:1px solid #ddd;
  display:flex;
  justify-content:space-around;
  padding:8px 0;
}
.nav div{
  text-align:center;
  font-size:12px;
  cursor:pointer;
}
.nav span{font-size:18px}
.plan{
  border:1px solid #ddd;
  border-radius:8px;
  padding:10px;
  margin-top:10px;
}
.small{font-size:12px;color:#666}
.ad{
  background:#fff3cd;
  border:1px solid #ffeeba;
  padding:10px;
  border-radius:8px;
  margin-top:10px;
}
</style>
</head>

<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
  <h3>Login / Signup</h3>
  <input id="username" placeholder="Username">
  <button onclick="login()">Continue</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="box">
    <b>User:</b> <span id="uName"></span>
  </div>

  <div class="flex">
    <div class="box">Balance<br><b>Rs <span id="bal">0</span></b></div>
    <div class="box">Daily<br><b>Rs <span id="daily">0</span></b></div>
  </div>

  <div class="box">
    Total Profit: <b>Rs <span id="total">0</span></b>
  </div>

  <div class="box">
    Active Users: <b id="activeUsers">0</b>
  </div>

  <div class="ad" id="adBox"></div>

  <div class="photos">
    <img id="img1">
    <img id="img2">
  </div>

  <button onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h3>Investment Plans</h3>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h3>Deposit</h3>
  <select id="depMethod" onchange="updateNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>

  <div class="flex">
    <input id="depNumber" readonly>
    <button onclick="copyNum()">Copy</button>
  </div>

  <input id="depAmount" placeholder="Amount">
  <input placeholder="Transaction ID">
  <input type="file">
  <button onclick="submitDeposit()">Submit</button>
</div>

<!-- WITHDRAW -->
<div id="withdraw" class="page hidden">
  <h3>Withdraw</h3>
  <select>
    <option>JazzCash</option>
    <option>EasyPaisa</option>
  </select>
  <input placeholder="Account Number">
  <input placeholder="Amount">
  <button onclick="alert('Withdrawal request sent')">Submit</button>
</div>

<!-- NAV -->
<div class="nav hidden" id="nav">
  <div onclick="show('dashboard')"><span>🏠</span><br>Home</div>
  <div onclick="show('plans')"><span>📦</span><br>Plans</div>
  <div onclick="show('deposit')"><span>💰</span><br>Deposit</div>
  <div onclick="show('withdraw')"><span>💵</span><br>Withdraw</div>
</div>

<script>
let user = localStorage.getItem("nexa_user");
let balance = Number(localStorage.getItem("nexa_balance")||0);
let daily = Number(localStorage.getItem("nexa_daily")||0);
let total = Number(localStorage.getItem("nexa_total")||0);

function login(){
  const u=document.getElementById("username").value.trim();
  if(!u) return alert("Enter username");
  user=u;
  localStorage.setItem("nexa_user",u);
  showDashboard();
}

function logout(){
  localStorage.removeItem("nexa_user");
  location.reload();
}

function showDashboard(){
  document.getElementById("loginPage").classList.add("hidden");
  document.getElementById("dashboard").classList.remove("hidden");
  document.getElementById("nav").classList.remove("hidden");
  document.getElementById("uName").innerText=user;
  updateUI();
  randomStuff();
}

function show(id){
  document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden"));
  document.getElementById(id).classList.remove("hidden");
}

function updateUI(){
  document.getElementById("bal").innerText=balance;
  document.getElementById("daily").innerText=daily;
  document.getElementById("total").innerText=total;
}

function randomStuff(){
  document.getElementById("activeUsers").innerText =
    Math.floor(500+Math.random()*4500);

  document.getElementById("img1").src="https://picsum.photos/400?"+Math.random();
  document.getElementById("img2").src="https://picsum.photos/401?"+Math.random();

  const ads=[
    "🔥 Limited offer – Earn daily!",
    "💰 Special bonus for new users",
    "⚡ Fast deposit & withdrawal"
  ];
  document.getElementById("adBox").innerText=
    ads[Math.floor(Math.random()*ads.length)];

  setTimeout(randomStuff,6000);
}

function updateNumber(){
  document.getElementById("depNumber").value =
    document.getElementById("depMethod").value==="jazzcash"
    ?"03705519562":"03379827882";
}
updateNumber();

function copyNum(){
  navigator.clipboard.writeText(document.getElementById("depNumber").value);
  alert("Number copied");
}

function submitDeposit(){
  alert("Deposit submitted (demo)");
}

function buildPlans(){
  let html="";
  for(let i=1;i<=50;i++){
    let amt=200+(i-1)*200;
    html+=`
      <div class="plan">
        <b>Plan ${i}</b><br>
        Invest: Rs ${amt}<br>
        Duration: 25 Days<br>
        Profit: 2.2x
        <button onclick="buy(${amt})">Buy Now</button>
      </div>`;
  }
  document.getElementById("plansList").innerHTML=html;
}
buildPlans();

function buy(a){
  show("deposit");
  document.getElementById("depAmount").value=a;
}

if(user){showDashboard();}
</script>
</body>
</html>
