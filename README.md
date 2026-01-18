<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEXA EARN</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{
  margin:0;
  font-family: Arial, sans-serif;
  background:#f4f6fb;
  color:#222;
}
.hidden{display:none;}
header{
  background:#4f46e5;
  color:#fff;
  padding:15px;
  text-align:center;
  font-size:22px;
  font-weight:bold;
}
.page{
  max-width:420px;
  margin:15px auto;
  background:#fff;
  border-radius:12px;
  padding:15px;
  box-shadow:0 6px 18px rgba(0,0,0,.08);
}
input,select,button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid #ddd;
  font-size:14px;
}
button{
  background:#4f46e5;
  color:#fff;
  font-weight:bold;
  cursor:pointer;
}
.box{
  background:#f1f5ff;
  border-radius:10px;
  padding:12px;
  margin-bottom:10px;
}
.flex{
  display:flex;
  gap:10px;
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
  font-size:12px;
  text-align:center;
  cursor:pointer;
}
.plan{
  border:1px solid #ddd;
  border-radius:10px;
  padding:10px;
  margin-bottom:10px;
}
.plan h4{margin:0 0 5px}
.ad img{
  width:100%;
  border-radius:10px;
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
  <div class="box"><b>User:</b> <span id="dUser"></span></div>
  <div class="flex">
    <div class="box" style="flex:1">Balance<br><b>Rs <span id="dBalance">0</span></b></div>
    <div class="box" style="flex:1">Daily Profit<br><b>Rs <span id="dDaily">0</span></b></div>
  </div>
  <div class="box">Total Profit: Rs <b><span id="dTotal">0</span></b></div>
  <div class="box">Active Users: <b><span id="activeUsers">0</span></b></div>

  <div class="ad">
    <img id="adImg" src="">
  </div>

  <p>
    NEXA EARN is a digital earning platform where users grow funds through
    structured plans. Earnings are added automatically based on active plans.
  </p>
</div>

<!-- PLANS -->
<div id="plansPage" class="page hidden">
  <h3>Investment Plans</h3>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositPage" class="page hidden">
  <h3>Deposit</h3>
  <select id="depMethod">
    <option>JazzCash</option>
    <option>EasyPaisa</option>
  </select>
  <input id="depAmount" readonly>
  <input id="txid" placeholder="Transaction ID">
  <button onclick="submitDeposit()">Submit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawPage" class="page hidden">
  <h3>Withdraw</h3>
  <input id="wAmount" placeholder="Amount">
  <button onclick="withdraw()">Request</button>
</div>

<!-- NAV -->
<div id="nav" class="nav hidden">
  <div onclick="show('dashboard')">🏠<br>Home</div>
  <div onclick="show('plansPage')">📦<br>Plans</div>
  <div onclick="show('depositPage')">💰<br>Deposit</div>
  <div onclick="show('withdrawPage')">💵<br>Withdraw</div>
  <div onclick="logout()">🚪<br>Logout</div>
</div>

<script>
/* ---------- AUTH ---------- */
let user = localStorage.getItem("nexa_user");
if(user){init();}

function login(){
  let u=document.getElementById("username").value.trim();
  if(!u)return alert("Enter username");
  localStorage.setItem("nexa_user",u);
  if(!localStorage.getItem("nexa_balance")){
    localStorage.setItem("nexa_balance","0");
    localStorage.setItem("nexa_total","0");
    localStorage.setItem("nexa_plans","[]");
  }
  init();
}
function logout(){
  localStorage.removeItem("nexa_user");
  location.reload();
}

/* ---------- INIT ---------- */
function init(){
  document.getElementById("loginPage").classList.add("hidden");
  document.getElementById("nav").classList.remove("hidden");
  show("dashboard");
  updateDashboard();
  loadPlans();
  startProfitEngine();
  randomAds();
}

/* ---------- NAV ---------- */
function show(id){
  document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden"));
  document.getElementById(id).classList.remove("hidden");
}

/* ---------- DASHBOARD ---------- */
function updateDashboard(){
  document.getElementById("dUser").innerText=localStorage.getItem("nexa_user");
  document.getElementById("dBalance").innerText=localStorage.getItem("nexa_balance");
  document.getElementById("dTotal").innerText=localStorage.getItem("nexa_total");
  document.getElementById("dDaily").innerText=(calcDaily()).toFixed(2);
  document.getElementById("activeUsers").innerText=
    Math.floor(1000+Math.random()*4000);
}

/* ---------- ADS ---------- */
const ads=[
 "https://picsum.photos/400/200?1",
 "https://picsum.photos/400/200?2",
 "https://picsum.photos/400/200?3",
 "https://picsum.photos/400/200?4"
];
function randomAds(){
  document.getElementById("adImg").src=ads[Math.floor(Math.random()*ads.length)];
  setInterval(()=>randomAds(),8000);
}

/* ---------- PLANS ---------- */
let PLANS=[];
for(let i=1;i<=50;i++){
  let price=200*i;
  PLANS.push({name:"Plan "+price,price,days:25});
}

function loadPlans(){
  let html="";
  PLANS.forEach(p=>{
    let total=p.price*2.2;
    let daily=(total/p.days).toFixed(2);
    html+=`
    <div class="plan">
      <h4>${p.name}</h4>
      <div>Invest: Rs ${p.price}</div>
      <div>Daily: Rs ${daily}</div>
      <div>Total: Rs ${total}</div>
      <button onclick='buy(${JSON.stringify(p)})'>Buy Now</button>
    </div>`;
  });
  document.getElementById("plansList").innerHTML=html;
}

let selectedPlan=null;
function buy(p){
  selectedPlan=p;
  document.getElementById("depAmount").value=p.price;
  show("depositPage");
}

/* ---------- DEPOSIT ---------- */
function submitDeposit(){
  if(!selectedPlan)return;
  let plans=JSON.parse(localStorage.getItem("nexa_plans"));
  plans.push({...selectedPlan,start:Date.now(),last:Date.now()});
  localStorage.setItem("nexa_plans",JSON.stringify(plans));
  alert("Deposit Submitted");
  show("dashboard");
}

/* ---------- PROFIT ENGINE ---------- */
function calcDaily(){
  let plans=JSON.parse(localStorage.getItem("nexa_plans"));
  let daily=0;
  plans.forEach(p=>{
    daily+=((p.price*2.2)/p.days);
  });
  return daily;
}

function startProfitEngine(){
  let plans=JSON.parse(localStorage.getItem("nexa_plans"));
  let bal=parseFloat(localStorage.getItem("nexa_balance"));
  let total=parseFloat(localStorage.getItem("nexa_total"));
  plans.forEach(p=>{
    let now=Date.now();
    let diff=Math.floor((now-p.last)/86400000);
    if(diff>0){
      let profit=((p.price*2.2)/p.days)*diff;
      bal+=profit;
      total+=profit;
      p.last=now;
    }
  });
  localStorage.setItem("nexa_plans",JSON.stringify(plans));
  localStorage.setItem("nexa_balance",bal.toFixed(2));
  localStorage.setItem("nexa_total",total.toFixed(2));
  updateDashboard();
}

/* ---------- WITHDRAW ---------- */
function withdraw(){
  alert("Withdrawal Request Submitted");
}
</script>

</body>
</html>
