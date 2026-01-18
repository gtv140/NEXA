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
  background:#ffffff;
  padding:15px;
  text-align:center;
  font-size:26px;
  font-weight:700;
  border-bottom:1px solid #ddd;
}
.page{
  max-width:420px;
  margin:20px auto;
  background:#fff;
  border-radius:12px;
  padding:15px;
  box-shadow:0 6px 18px rgba(0,0,0,0.08);
}
.hidden{display:none;}

input,select,button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid #ccc;
  font-size:14px;
}
button{
  background:#2563eb;
  color:#fff;
  border:none;
  cursor:pointer;
}
button:hover{opacity:0.9;}

.box{
  background:#f1f5f9;
  padding:12px;
  border-radius:10px;
  margin-bottom:10px;
}
.flex{display:flex;gap:10px;}
.badge{
  background:#2563eb;
  color:#fff;
  padding:4px 10px;
  border-radius:20px;
  font-size:12px;
}

img{
  width:100%;
  border-radius:10px;
  margin-top:8px;
}

.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  background:#ffffff;
  border-top:1px solid #ddd;
  display:flex;
  justify-content:space-around;
  padding:8px 0;
}
.nav div{
  text-align:center;
  font-size:13px;
  cursor:pointer;
}
.nav span{display:block;font-size:18px;}

.plan{
  border:1px solid #e5e7eb;
  padding:10px;
  border-radius:10px;
  margin-top:10px;
}
.plan h4{margin:0 0 6px 0;}
.special{
  border:2px solid #16a34a;
}
.timer{
  font-size:12px;
  color:#16a34a;
  font-weight:bold;
}
.ad{
  background:#fff7ed;
  border:1px dashed #fb923c;
  padding:10px;
  border-radius:10px;
  margin-top:10px;
}
</style>
</head>

<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page hidden">
  <h3>Login / Signup</h3>
  <input id="username" placeholder="Enter Username">
  <button onclick="login()">Continue</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="box flex">
    <div>
      <b id="uName">User</b><br>
      <small>Welcome to NEXA EARN</small>
    </div>
    <div style="margin-left:auto;text-align:right">
      <div class="badge">Balance</div>
      Rs <span id="bal">0</span>
    </div>
  </div>

  <div class="box flex">
    <div>Daily Profit<br><b>Rs <span id="daily">0</span></b></div>
    <div style="margin-left:auto">
      Active Users<br><b id="activeUsers">0</b>
    </div>
  </div>

  <div class="box">
    <b>About NEXA EARN</b>
    <p style="font-size:13px">
      NEXA EARN is a digital earning platform designed to provide simple,
      transparent and user‑friendly investment plans. Your data and progress
      are stored securely on your device.
    </p>
  </div>

  <!-- PHOTOS -->
  <div class="box">
    <b>Platform Highlights</b>
    <img src="https://picsum.photos/400/200?1">
    <img src="https://picsum.photos/400/200?2">
    <img src="https://picsum.photos/400/200?3">
  </div>

  <!-- ADS -->
  <div id="ads" class="ad"></div>
</div>

<!-- PLANS -->
<div id="plansPage" class="page hidden">
  <h3>Investment Plans</h3>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositPage" class="page hidden">
  <h3>Deposit</h3>
  <div class="box">
    Selected Plan Amount: Rs <b id="depAmount">0</b>
  </div>
  <select id="depMethod" onchange="updateNumber()">
    <option value="jazz">JazzCash</option>
    <option value="easy">EasyPaisa</option>
  </select>
  <div class="flex">
    <input id="depNumber" readonly>
    <button onclick="copyNum()">Copy</button>
  </div>
  <input placeholder="Transaction ID">
  <input type="file">
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- ABOUT -->
<div id="aboutPage" class="page hidden">
  <h3>About & Support</h3>
  <p style="font-size:14px">
    For any assistance, deposit confirmation or withdrawal support,
    contact our official support team.
  </p>
  <button onclick="openSupport()">WhatsApp Support</button>
</div>

<!-- NAV -->
<div class="nav hidden" id="nav">
  <div onclick="show('dashboard')"><span>🏠</span>Home</div>
  <div onclick="show('plansPage')"><span>📦</span>Plans</div>
  <div onclick="show('depositPage')"><span>💰</span>Deposit</div>
  <div onclick="show('aboutPage')"><span>ℹ️</span>About</div>
</div>

<script>
/* ---------- STORAGE ---------- */
let user = localStorage.getItem("nexa_user");
let balance = Number(localStorage.getItem("nexa_balance")) || 0;
let daily = Number(localStorage.getItem("nexa_daily")) || 0;
let selectedAmount = 0;

/* ---------- INIT ---------- */
if(user){
  openApp();
}else{
  document.getElementById("loginPage").classList.remove("hidden");
}

function login(){
  const u = document.getElementById("username").value.trim();
  if(!u){alert("Enter username");return;}
  localStorage.setItem("nexa_user",u);
  localStorage.setItem("nexa_balance",0);
  localStorage.setItem("nexa_daily",0);
  openApp();
}

function openApp(){
  document.getElementById("loginPage").classList.add("hidden");
  document.getElementById("nav").classList.remove("hidden");
  show("dashboard");
  document.getElementById("uName").innerText = localStorage.getItem("nexa_user");
  document.getElementById("bal").innerText = balance;
  document.getElementById("daily").innerText = daily;
  randomUsers();
  randomAd();
  loadPlans();
}

/* ---------- NAV ---------- */
function show(id){
  document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden"));
  document.getElementById(id).classList.remove("hidden");
}

/* ---------- ACTIVE USERS ---------- */
function randomUsers(){
  document.getElementById("activeUsers").innerText =
    Math.floor(Math.random()*5000)+1000;
}

/* ---------- ADS ---------- */
function randomAd(){
  const ads=[
    "🔥 Special Offer: 2.4x return for limited time!",
    "🎁 New users get faster approval today!",
    "⚡ High demand plans filling fast!"
  ];
  document.getElementById("ads").innerText =
    ads[Math.floor(Math.random()*ads.length)];
}

/* ---------- PLANS ---------- */
function loadPlans(){
  const list=document.getElementById("plansList");
  list.innerHTML="";
  for(let i=1;i<=50;i++){
    let invest=200*i;
    let total = Math.round(invest*2.2);
    let special = i<=5;
    if(special) total=Math.round(invest*2.4);
    let div=document.createElement("div");
    div.className="plan"+(special?" special":"");
    div.innerHTML=`
      <h4>Plan ${i}</h4>
      Invest: Rs ${invest}<br>
      Total Return: Rs ${total}<br>
      ${special?'<span class="timer">Special 24h Offer</span><br>':''}
      <button onclick="buy(${invest})">Buy Now</button>
    `;
    list.appendChild(div);
  }
}

function buy(amount){
  selectedAmount=amount;
  document.getElementById("depAmount").innerText=amount;
  updateNumber();
  show("depositPage");
}

/* ---------- DEPOSIT ---------- */
function updateNumber(){
  const m=document.getElementById("depMethod").value;
  document.getElementById("depNumber").value =
    m==="jazz"?"03705519562":"03379827882";
}
function copyNum(){
  navigator.clipboard.writeText(document.getElementById("depNumber").value);
  alert("Number copied");
}
function submitDeposit(){
  alert("Deposit submitted. Admin will verify.");
}

/* ---------- SUPPORT ---------- */
function openSupport(){
  window.open("https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu","_blank");
}
</script>
</body>
</html>
