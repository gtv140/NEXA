<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEXA EARN</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{
  margin:0;
  font-family:Arial,Helvetica,sans-serif;
  background:#f4f6f8;
  color:#222;
}
header{
  text-align:center;
  padding:18px;
  font-size:26px;
  font-weight:700;
  background:#ffffff;
  box-shadow:0 2px 8px rgba(0,0,0,.08);
}
.page{
  max-width:420px;
  margin:15px auto 90px;
  background:#fff;
  padding:15px;
  border-radius:12px;
  box-shadow:0 4px 15px rgba(0,0,0,.08);
}
.hidden{display:none;}
input,select,button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid #ccc;
}
button{
  background:#2563eb;
  color:#fff;
  border:none;
  font-weight:600;
}
.stat{
  background:#f1f5f9;
  padding:10px;
  border-radius:10px;
  margin-top:10px;
}
.photos img{
  width:100%;
  border-radius:10px;
  margin-top:10px;
}
.ad{
  background:#fff7ed;
  border:1px solid #fed7aa;
  padding:10px;
  border-radius:10px;
  margin-top:10px;
  font-size:14px;
}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  background:#ffffff;
  border-top:1px solid #ddd;
}
.nav div{
  padding:10px;
  text-align:center;
  font-size:13px;
  cursor:pointer;
}
.plan{
  border:1px solid #ddd;
  padding:10px;
  border-radius:10px;
  margin-top:10px;
}
</style>
</head>

<body>

<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="login" class="page">
  <h3>Login / Signup</h3>
  <input id="username" placeholder="Username">
  <button onclick="login()">Continue</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="stat"><b>User:</b> <span id="uName"></span></div>
  <div class="stat"><b>Balance:</b> Rs <span id="bal"></span></div>
  <div class="stat"><b>Daily Profit:</b> Rs <span id="daily"></span></div>
  <div class="stat"><b>Total Profit:</b> Rs <span id="total"></span></div>
  <div class="stat"><b>Active Users:</b> <span id="users"></span></div>

  <div class="photos">
    <img id="ph1">
    <img id="ph2">
  </div>

  <div id="adBox" class="ad"></div>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h3>Investment Plans</h3>
  <div id="planList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h3>Deposit</h3>
  <input id="depAmount" readonly>
  <select id="method" onchange="setNumber()">
    <option>JazzCash</option>
    <option>EasyPaisa</option>
  </select>
  <input id="number" readonly>
  <button onclick="copyNum()">Copy Number</button>
  <input placeholder="Transaction ID">
  <input type="file">
  <button>Submit</button>
</div>

<!-- NAV -->
<div class="nav hidden" id="nav">
  <div onclick="show('dashboard')">Home</div>
  <div onclick="show('plans')">Plans</div>
  <div onclick="show('deposit')">Deposit</div>
</div>

<script>
let user = localStorage.getItem("nexa_user");
let bal = Number(localStorage.getItem("nexa_bal")||0);
let daily = Number(localStorage.getItem("nexa_daily")||0);
let total = Number(localStorage.getItem("nexa_total")||0);

if(user) start();

function login(){
  user=document.getElementById("username").value;
  if(!user) return;
  localStorage.setItem("nexa_user",user);
  start();
}

function start(){
  document.getElementById("login").classList.add("hidden");
  document.getElementById("dashboard").classList.remove("hidden");
  document.getElementById("nav").classList.remove("hidden");
  document.getElementById("uName").innerText=user;
  updateStats();
  loadPlans();
  photos();
  ads();
}

function updateStats(){
  document.getElementById("bal").innerText=bal;
  document.getElementById("daily").innerText=daily;
  document.getElementById("total").innerText=total;
  document.getElementById("users").innerText=Math.floor(Math.random()*5000)+500;
}

function photos(){
  ph1.src="https://picsum.photos/400/200?"+Math.random();
  ph2.src="https://picsum.photos/401/200?"+Math.random();
  setTimeout(photos,6000);
}

function ads(){
  let a=[
    "🎉 Special offer active for limited users",
    "💰 Withdrawals processed within 24 hours",
    "🔥 New plans launching soon",
    "📢 Invite friends & earn more"
  ];
  adBox.innerText=a[Math.floor(Math.random()*a.length)];
  setTimeout(ads,5000);
}

function loadPlans(){
  let html="";
  for(let i=1;i<=50;i++){
    let price=200*i;
    html+=`<div class="plan">
      <b>Plan ${i}</b><br>
      Invest: Rs ${price}<br>
      <button onclick="buy(${price})">Buy Now</button>
    </div>`;
  }
  planList.innerHTML=html;
}

function buy(a){
  document.getElementById("depAmount").value=a;
  show("deposit");
  setNumber();
}

function setNumber(){
  number.value = method.value=="JazzCash"?"03705519562":"03379827882";
}
function copyNum(){
  navigator.clipboard.writeText(number.value);
  alert("Copied");
}

function show(id){
  document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden"));
  document.getElementById(id).classList.remove("hidden");
}
</script>

</body>
</html>
