<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEXA</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
:root{
  --n1:#00fff0;
  --n2:#ff2fd2;
  --bg:#0b0b0b;
}
*{box-sizing:border-box}
body{
  margin:0;
  font-family:Arial,Helvetica,sans-serif;
  background:#000;
  color:#fff;
  animation:bg 20s linear infinite;
}
@keyframes bg{
  0%{background:#000}
  50%{background:#050505}
  100%{background:#000}
}
header{
  text-align:center;
  padding:16px;
  font-size:28px;
  font-weight:900;
  color:#fff;
  text-shadow:0 0 10px var(--n1),0 0 20px var(--n2);
}
.box{
  max-width:420px;
  margin:20px auto;
  padding:18px;
  border-radius:14px;
  background:rgba(255,255,255,0.05);
  box-shadow:0 0 15px var(--n1);
}
input,select,button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:none;
  outline:none;
}
input,select{
  background:#000;
  color:#fff;
  border:1px solid #333;
}
button{
  background:linear-gradient(90deg,var(--n1),var(--n2));
  font-weight:800;
  cursor:pointer;
}
.hidden{display:none}

.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  background:#000;
  padding:10px 0;
  box-shadow:0 -2px 10px #000;
}
.nav div{
  text-align:center;
  font-size:12px;
  cursor:pointer;
}
.ico{font-size:22px;display:block}

.plan{
  background:#000;
  padding:12px;
  border-radius:10px;
  margin-bottom:10px;
  border:1px solid #333;
}
.copybox{
  display:flex;
  gap:6px;
}
.copybox input{flex:1}
</style>
</head>

<body>

<header>NEXA</header>

<!-- LOGIN -->
<div id="login" class="box">
  <input id="user" placeholder="Username">
  <input id="pass" type="password" placeholder="Password">
  <button onclick="login()">LOGIN</button>
</div>

<!-- DASHBOARD (EMPTY – ONLY NAV) -->
<div id="dashboard" class="box hidden">
  <p style="text-align:center;opacity:.6">Use bottom icons</p>
</div>

<!-- PLANS -->
<div id="plans" class="box hidden"></div>

<!-- DEPOSIT -->
<div id="deposit" class="box hidden">
  <select id="dMethod" onchange="setNumber()">
    <option value="jazz">JazzCash</option>
    <option value="easy">EasyPaisa</option>
  </select>

  <div class="copybox">
    <input id="dNumber" readonly>
    <button onclick="copyNum()">Copy</button>
  </div>
</div>

<!-- REFERRAL -->
<div id="ref" class="box hidden">
  <div class="copybox">
    <input value="https://gtv140.github.io/NEXA/" readonly>
    <button onclick="copyRef()">Copy</button>
  </div>
</div>

<!-- NAV -->
<div class="nav hidden" id="nav">
  <div onclick="show('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="show('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="show('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="show('ref')"><span class="ico">🔗</span>Referral</div>
  <div onclick="logout()"><span class="ico">🚪</span>Logout</div>
</div>

<script>
let user = localStorage.getItem("nexa_user");

function login(){
  let u=user=document.getElementById("user").value;
  if(!u)return;
  localStorage.setItem("nexa_user",u);
  start();
}

function start(){
  document.getElementById("login").classList.add("hidden");
  document.getElementById("dashboard").classList.remove("hidden");
  document.getElementById("nav").classList.remove("hidden");
  loadPlans();
  setNumber();
}

function logout(){
  localStorage.removeItem("nexa_user");
  location.reload();
}

function show(id){
  document.querySelectorAll(".box").forEach(b=>b.classList.add("hidden"));
  document.getElementById(id).classList.remove("hidden");
}

function loadPlans(){
  let p=document.getElementById("plans");
  p.innerHTML="";
  let amount=200;
  for(let i=1;i<=30;i++){
    let days=25+i;
    let total=Math.round(amount*2.2);
    p.innerHTML+=`
    <div class="plan">
      <b>Plan ${i}</b><br>
      Invest: Rs ${amount}<br>
      Days: ${days}<br>
      Total: Rs ${total}
    </div>`;
    amount+=300;
    if(amount>10000)amount=10000;
  }
}

function setNumber(){
  let m=document.getElementById("dMethod").value;
  document.getElementById("dNumber").value=
    m==="jazz"?"03705519562":"03379827882";
}
function copyNum(){
  navigator.clipboard.writeText(document.getElementById("dNumber").value);
  alert("Copied");
}
function copyRef(){
  navigator.clipboard.writeText("https://gtv140.github.io/NEXA/");
  alert("Copied");
}

if(user)start();
</script>

</body>
</html>
