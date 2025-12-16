<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEXA</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
:root{--n1:#00fff0;--n2:#ff2fd2}
*{box-sizing:border-box}
body{
margin:0;font-family:Arial;background:#000;color:#fff;
animation:bg 18s linear infinite
}
@keyframes bg{
0%{background:#000}
50%{background:#070707}
100%{background:#000}
}
header{
text-align:center;padding:15px;font-size:28px;font-weight:900;
text-shadow:0 0 10px var(--n1),0 0 20px var(--n2)
}
.box{
max-width:420px;margin:20px auto;padding:16px;
background:rgba(255,255,255,.05);
border-radius:14px;box-shadow:0 0 15px var(--n1)
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
.plan{
border:1px solid #333;padding:10px;border-radius:10px;margin-bottom:10px
}
.nav{
position:fixed;bottom:0;left:0;right:0;
display:flex;justify-content:space-around;
background:#000;padding:10px 0
}
.nav div{text-align:center;font-size:12px;cursor:pointer}
.ico{font-size:22px;display:block}
.copybox{display:flex;gap:6px}
.copybox input{flex:1}
small{opacity:.7}
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

<!-- DASHBOARD -->
<div id="dashboard" class="box hidden">
<p style="text-align:center">Welcome <b id="dUser"></b></p>
<p>Balance: Rs <b id="bal">0</b></p>
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
<button onclick="copyText('dNumber')">Copy</button>
</div>

<input id="dAmount" placeholder="Amount">
<input placeholder="TX ID">
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdraw" class="box hidden">
<select>
<option>JazzCash</option>
<option>EasyPaisa</option>
</select>
<input placeholder="Account Number">
<input id="wAmount" placeholder="Amount">
<button onclick="withdraw()">Request Withdraw</button>
</div>

<!-- REF -->
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
<div onclick="show('withdraw')"><span class="ico">💵</span>Withdraw</div>
<div onclick="show('ref')"><span class="ico">🔗</span>Referral</div>
<div onclick="logout()"><span class="ico">🚪</span>Logout</div>
</div>

<script>
let user=localStorage.getItem("nexa_user");
let bal=parseInt(localStorage.getItem("nexa_bal")||0);

function login(){
let u=document.getElementById("user").value;
if(!u)return;
localStorage.setItem("nexa_user",u);
start();
}

function start(){
document.getElementById("login").classList.add("hidden");
document.getElementById("nav").classList.remove("hidden");
document.getElementById("dashboard").classList.remove("hidden");
document.getElementById("dUser").innerText=localStorage.getItem("nexa_user");
updateBal();
loadPlans();
setNumber();
}

function logout(){
localStorage.clear();
location.reload();
}

function show(id){
document.querySelectorAll(".box").forEach(b=>b.classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
}

function updateBal(){
document.getElementById("bal").innerText=bal;
localStorage.setItem("nexa_bal",bal);
}

function loadPlans(){
let p=document.getElementById("plans");
p.innerHTML="";
let amt=200;
for(let i=1;i<=30;i++){
let days=25+i;
let total=Math.round(amt*2.2);
p.innerHTML+=`
<div class="plan">
<b>Plan ${i}</b><br>
Invest: Rs ${amt}<br>
Days: ${days}<br>
Total: Rs ${total}<br>
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
let m=document.getElementById("dMethod").value;
document.getElementById("dNumber").value=
m==="jazz"?"03705519562":"03379827882";
}

function submitDeposit(){
let a=parseInt(document.getElementById("dAmount").value||0);
if(a<=0)return alert("Invalid amount");
bal+=a;
updateBal();
alert("Deposit Submitted");
show("dashboard");
}

function withdraw(){
let a=parseInt(document.getElementById("wAmount").value||0);
if(a<=0||a>bal)return alert("Invalid");
bal-=a;
updateBal();
alert("Withdraw Requested");
show("dashboard");
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
