<NEXA>
<html>
<head>
<meta charset="UTF-8">
<title>NEXA Earn</title>
<meta name="viewport" content="width=device-width,initial-scale=1">
<style>
body{margin:0;font-family:Arial;background:#000;color:#fff}
header{text-align:center;padding:14px;font-size:28px;font-weight:900}
.box{max-width:430px;margin:12px auto;padding:14px;background:#111;border-radius:12px}
.hidden{display:none}
input,select,button{width:100%;padding:10px;margin-top:8px;border-radius:8px;border:1px solid #333;background:#000;color:#fff}
button{background:#00ffe7;color:#000;font-weight:800;border:none}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;background:#000;padding:8px 0}
.nav div{font-size:12px;cursor:pointer}
.card{border:1px solid #333;padding:10px;border-radius:10px;margin-bottom:8px}
small{opacity:.7}
</style>
</head>

<body>
<header>NEXA Earn</header>

<div id="login" class="box">
<input id="u" placeholder="Username">
<button onclick="login()">LOGIN</button>
</div>

<div id="dash" class="box hidden">
<div class="card">
<b>User:</b> <span id="du"></span><br>
<b>Balance:</b> Rs <span id="bal">0</span><br>
<b>Daily Profit:</b> Rs <span id="dprofit">0</span><br>
<b>Total Profit:</b> Rs <span id="tprofit">0</span><br>
<b>Active Users:</b> <span id="active"></span>
</div>

<div class="card">
<b>Rules:</b><br>
Deposit / Withdrawal issue ho to <b>Administration</b> se contact karein.  
Request ke baad <b>screenshot admin ko bhejna zaroori</b>.
</div>
</div>

<div id="plans" class="box hidden"></div>

<div id="deposit" class="box hidden">
<select id="dm" onchange="setNum()">
<option value="jazz">JazzCash</option>
<option value="easy">EasyPaisa</option>
</select>
<input id="dnum" readonly>
<input id="damt" placeholder="Amount">
<input placeholder="Transaction ID">
<input type="file">
<button onclick="doDeposit()">Submit Deposit</button>
<small>15 sec baad auto approved</small>
</div>

<div id="withdraw" class="box hidden">
<input id="wamt" placeholder="Amount">
<button onclick="doWithdraw()">Request Withdrawal</button>
<small>15 sec baad auto approved</small>
</div>

<div id="history" class="box hidden"></div>

<div class="nav hidden" id="nav">
<div onclick="show('dash')">🏠</div>
<div onclick="show('plans')">📦</div>
<div onclick="show('deposit')">💰</div>
<div onclick="show('withdraw')">💵</div>
<div onclick="show('history')">📜</div>
<div onclick="logout()">🚪</div>
</div>

<script>
/* ===== STORAGE ===== */
let S = k => JSON.parse(localStorage.getItem(k)||"null");
let W = (k,v)=>localStorage.setItem(k,JSON.stringify(v));

/* ===== INIT ===== */
let user=S("u"), bal=S("bal")||0, hist=S("hist")||[];
let daily=S("daily")||10, total=S("total")||0;
let lastProfit=S("lastProfit")||Date.now();

function login(){
let x=document.getElementById("u").value;
if(!x)return;
W("u",x);start();
}
function start(){
user=S("u");
document.getElementById("login").classList.add("hidden");
document.getElementById("nav").classList.remove("hidden");
show("dash");
document.getElementById("du").innerText=user;
loadPlans();setNum();tick();renderHist();
}
function logout(){localStorage.clear();location.reload();}
function show(id){
document.querySelectorAll(".box").forEach(b=>b.classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
}

/* ===== PLANS ===== */
function loadPlans(){
let p=document.getElementById("plans");p.innerHTML="";
let a=200;
for(let i=1;i<=30;i++){
let d=25+i;
p.innerHTML+=`<div class="card">
Plan ${i}<br>Invest Rs ${a}<br>Days ${d}<br>
<button onclick="buy(${a},${d})">Buy Now</button></div>`;
a=Math.min(10000,a+300);
}
}
function buy(a,d){
W("plan",{amt:a,days:d,start:Date.now()});
show("deposit");
document.getElementById("damt").value=a;
}

/* ===== DEPOSIT / WITHDRAW ===== */
function setNum(){
document.getElementById("dnum").value=
document.getElementById("dm").value=="jazz"?"03705519562":"03379827882";
}
function doDeposit(){
let a=+document.getElementById("damt").value;
let id=Date.now();
hist.push({t:"Deposit",a,st:"Pending",id});
W("hist",hist);renderHist();
setTimeout(()=>{
let h=hist.find(x=>x.id==id);
if(h){h.st="Approved";bal+=a;W("bal",bal);W("hist",hist);renderHist();}
},15000);
alert("Deposit submitted, 15 sec baad approve");
show("dash");
}
function doWithdraw(){
let a=+document.getElementById("wamt").value;
let id=Date.now();
hist.push({t:"Withdraw",a,st:"Pending",id});
W("hist",hist);renderHist();
setTimeout(()=>{
let h=hist.find(x=>x.id==id);
if(h){h.st="Approved";bal-=a;W("bal",bal);W("hist",hist);renderHist();}
},15000);
alert("Withdrawal request sent");
show("dash");
}

/* ===== HISTORY ===== */
function renderHist(){
let h=document.getElementById("history");h.innerHTML="";
hist.forEach(x=>{
h.innerHTML+=`<div class="card">${x.t} Rs ${x.a} - ${x.st}</div>`;
});
}

/* ===== DAILY PROFIT AUTO ===== */
function tick(){
let now=Date.now();
if(now-lastProfit>=86400000){
bal+=daily; total+=daily; lastProfit=now;
W("bal",bal);W("total",total);W("lastProfit",lastProfit);
}
document.getElementById("bal").innerText=bal;
document.getElementById("dprofit").innerText=daily;
document.getElementById("tprofit").innerText=total;
document.getElementById("active").innerText= Math.floor(50+Math.random()*150);
setTimeout(tick,5000);
}

/* AUTO START */
if(user)start();
</script>
</body>
</html>
