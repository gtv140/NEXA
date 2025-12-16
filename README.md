<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEXA – Manual Investment Platform</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
:root{--n1:#00ffe7;--n2:#ff3cac}
*{box-sizing:border-box}
body{
margin:0;font-family:Arial;background:#000;color:#fff;
animation:bg 20s linear infinite
}
@keyframes bg{0%{background:#000}50%{background:#080808}100%{background:#000}}
header{
text-align:center;padding:16px;font-size:30px;font-weight:900;
text-shadow:0 0 12px var(--n1),0 0 25px var(--n2)
}
.box{
max-width:440px;margin:18px auto;padding:16px;
background:rgba(255,255,255,.05);
border-radius:14px;box-shadow:0 0 18px var(--n1)
}
input,select,button{
width:100%;padding:10px;margin-top:10px;
border-radius:8px;border:none;outline:none
}
input,select{background:#000;color:#fff;border:1px solid #333}
button{
background:linear-gradient(90deg,var(--n1),var(--n2));
font-weight:800;cursor:pointer
}
.hidden{display:none}
.plan,.card{
border:1px solid #333;padding:12px;border-radius:10px;margin-bottom:10px
}
.nav{
position:fixed;bottom:0;left:0;right:0;
display:flex;justify-content:space-around;
background:#000;padding:10px 0
}
.nav div{text-align:center;font-size:12px;cursor:pointer}
.ico{font-size:22px;display:block}
.copy{display:flex;gap:6px}
.copy input{flex:1}
.badge{font-size:12px;padding:4px 8px;border-radius:6px;background:#333}
.pending{color:#ffcc00}
.approved{color:#00ff99}
</style>
</head>

<body>

<header>NEXA</header>

<!-- LOGIN -->
<div id="login" class="box">
<input id="user" placeholder="Username">
<input id="pass" type="password" placeholder="Password">
<button onclick="login()">Login / Signup</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="box hidden">
<div class="card">
<b>User:</b> <span id="dUser"></span><br>
<b>Balance:</b> Rs <span id="balance">0</span>
</div>
<div class="card">
<b>Status:</b> Manual System (Admin Approval)
</div>
</div>

<!-- PLANS -->
<div id="plans" class="box hidden"></div>

<!-- DEPOSIT -->
<div id="deposit" class="box hidden">
<select id="dMethod" onchange="setNumber()">
<option value="jazz">JazzCash</option>
<option value="easy">EasyPaisa</option>
</select>
<div class="copy">
<input id="dNumber" readonly>
<button onclick="copyText('dNumber')">Copy</button>
</div>
<input id="dAmount" placeholder="Amount">
<input id="dTx" placeholder="Transaction ID">
<button onclick="submitDeposit()">Submit Deposit</button>
<small>Deposit will be approved manually</small>
</div>

<!-- WITHDRAW -->
<div id="withdraw" class="box hidden">
<input id="wAmount" placeholder="Amount">
<select>
<option>JazzCash</option>
<option>EasyPaisa</option>
</select>
<input placeholder="Account Number">
<button onclick="submitWithdraw()">Request Withdrawal</button>
<small>Withdrawal processed manually</small>
</div>

<!-- HISTORY -->
<div id="history" class="box hidden"></div>

<!-- REFERRAL -->
<div id="ref" class="box hidden">
<div class="copy">
<input value="https://gtv140.github.io/NEXA/" readonly>
<button onclick="copyRef()">Copy</button>
</div>
</div>

<!-- NAV -->
<div class="nav hidden" id="nav">
<div onclick="show('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="show('plans')"><span class="ico">📦</span>Plans</div>
<div onclick="show('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="show('withdraw')"><span class="ico">💵</span>Withdraw</div>
<div onclick="show('history')"><span class="ico">📜</span>History</div>
<div onclick="show('ref')"><span class="ico">🔗</span>Referral</div>
<div onclick="logout()"><span class="ico">🚪</span>Logout</div>
</div>

<script>
let user=localStorage.getItem("nexa_user");
let bal=parseInt(localStorage.getItem("nexa_bal")||0);
let history=JSON.parse(localStorage.getItem("nexa_hist")||"[]");

function login(){
let u=document.getElementById("user").value;
if(!u)return;
localStorage.setItem("nexa_user",u);
start();
}
function start(){
loginHide();
document.getElementById("dUser").innerText=localStorage.getItem("nexa_user");
updateBal();
loadPlans();
renderHistory();
setNumber();
}
function loginHide(){
document.getElementById("login").classList.add("hidden");
document.getElementById("dashboard").classList.remove("hidden");
document.getElementById("nav").classList.remove("hidden");
}
function logout(){
localStorage.clear();location.reload();
}
function show(id){
document.querySelectorAll(".box").forEach(b=>b.classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
}
function updateBal(){
document.getElementById("balance").innerText=bal;
localStorage.setItem("nexa_bal",bal);
}
function loadPlans(){
let p=document.getElementById("plans");p.innerHTML="";
let amt=200;
for(let i=1;i<=30;i++){
let days=25+i;
p.innerHTML+=`
<div class="plan">
<b>Plan ${i}</b><br>
Investment: Rs ${amt}<br>
Duration: ${days} days<br>
<button onclick="buy(${amt})">Buy Now</button>
</div>`;
amt+=300;if(amt>10000)amt=10000;
}
}
function buy(a){
show("deposit");
document.getElementById("dAmount").value=a;
}
function setNumber(){
document.getElementById("dNumber").value=
document.getElementById("dMethod").value==="jazz"
?"03705519562":"03379827882";
}
function submitDeposit(){
let a=document.getElementById("dAmount").value;
history.push(`Deposit Rs ${a} <span class="pending">Pending</span>`);
saveHist();
alert("Deposit submitted for manual approval");
show("dashboard");
}
function submitWithdraw(){
let a=document.getElementById("wAmount").value;
history.push(`Withdraw Rs ${a} <span class="pending">Pending</span>`);
saveHist();
alert("Withdrawal request submitted");
show("dashboard");
}
function renderHistory(){
let h=document.getElementById("history");h.innerHTML="";
history.forEach(i=>h.innerHTML+=`<div class="card">${i}</div>`);
}
function saveHist(){
localStorage.setItem("nexa_hist",JSON.stringify(history));
renderHistory();
}
function copyText(id){
navigator.clipboard.writeText(document.getElementById(id).value);
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
