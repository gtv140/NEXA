# NEXA<!DOCTYPE html>
<html lang="ur">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA Investment</title>

<style>
:root{
--main:#00ffd5;
--accent:#ff4fd8;
--bg:#050505;
}
*{box-sizing:border-box;font-family:Arial}
body{margin:0;background:var(--bg);color:#fff}

header{
text-align:center;
padding:18px;
font-size:26px;
font-weight:900;
background:linear-gradient(90deg,var(--main),var(--accent));
-webkit-background-clip:text;
-webkit-text-fill-color:transparent;
}

.page{
max-width:420px;
margin:14px auto;
padding:16px;
border-radius:14px;
background:linear-gradient(180deg,#0c0c0c,#050505);
border:1px solid rgba(0,255,213,0.15);
}

.hidden{display:none}

input,select,button{
width:100%;
padding:10px;
margin-top:10px;
border-radius:8px;
border:1px solid rgba(0,255,213,0.2);
background:#000;
color:#fff;
font-size:14px;
}

button{
background:linear-gradient(90deg,var(--main),var(--accent));
color:#000;
font-weight:800;
border:none;
cursor:pointer;
}

.nav{
position:fixed;
bottom:0;
left:0;
right:0;
display:flex;
justify-content:space-around;
background:#000;
border-top:1px solid rgba(255,255,255,0.1);
padding:10px 0;
}

.nav div{
text-align:center;
font-size:13px;
cursor:pointer;
}

.ico{font-size:20px}

.card{
padding:12px;
border-radius:10px;
margin:10px 0;
background:#0b0b0b;
border:1px solid rgba(0,255,213,0.15);
}

.plan{
display:flex;
justify-content:space-between;
gap:10px;
}

.badge{
padding:6px 10px;
border-radius:999px;
background:rgba(0,255,213,0.1);
color:var(--main);
font-weight:800;
font-size:13px;
}

.support{
display:flex;
gap:10px;
margin-bottom:10px;
cursor:pointer;
font-weight:700;
}

.support span{font-size:20px}
</style>
</head>

<body>

<header>NEXA Investment</header>

<!-- LOGIN -->
<div id="login" class="page">
<h3>لاگ ان / نیا اکاؤنٹ</h3>
<input id="user" placeholder="یوزر نیم">
<input id="pass" type="password" placeholder="پاس ورڈ">
<button onclick="login()">جاری رکھیں</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="card">
<b>خوش آمدید:</b> <span id="uName"></span><br>
بیلنس: Rs <span id="balance">0</span><br>
<span class="badge">روزانہ منافع: Rs <span id="daily">0</span></span>
</div>

<div class="card">
<b>ریفرل لنک</b>
<input id="ref" readonly>
<button onclick="copyRef()">کاپی کریں</button>
</div>

<button onclick="logout()">لاگ آؤٹ</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h3>انویسٹمنٹ پلانز</h3>
<div id="plansBox"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h3>ڈیپازٹ</h3>

<select id="method" onchange="updateNum()">
<option value="jazz">JazzCash</option>
<option value="easy">EasyPaisa</option>
</select>

<input id="number" readonly>
<input id="amount" placeholder="رقم">
<button onclick="submitDeposit()">ڈیپازٹ جمع کریں</button>
</div>

<!-- HISTORY -->
<div id="history" class="page hidden">
<h3>ہسٹری / سپورٹ</h3>

<div class="support" onclick="openWA()">
<span>💬</span> WhatsApp سپورٹ
</div>

<div class="support" onclick="openMail()">
<span>📧</span> Email سپورٹ
</div>

<div id="historyBox"></div>
</div>

<!-- NAV -->
<div id="nav" class="nav hidden">
<div onclick="show('dashboard')"><div class="ico">🏠</div>ہوم</div>
<div onclick="show('plans')"><div class="ico">📦</div>پلانز</div>
<div onclick="show('deposit')"><div class="ico">💰</div>ڈیپازٹ</div>
<div onclick="show('history')"><div class="ico">📜</div>ہسٹری</div>
</div>

<script>
let user=null;
let balance=5000;
let daily=0;
let history=[];

const plans=[
{id:1,price:500,days:20,profit:40},
{id:2,price:1000,days:25,profit:90},
{id:3,price:3000,days:30,profit:300},
{id:4,price:5000,days:40,profit:600}
];

function login(){
user=document.getElementById("user").value;
if(!user)return alert("نام درج کریں");
document.getElementById("login").classList.add("hidden");
document.getElementById("dashboard").classList.remove("hidden");
document.getElementById("nav").classList.remove("hidden");
document.getElementById("uName").innerText=user;
updateUI();
renderPlans();
}

function logout(){location.reload()}

function show(p){
document.querySelectorAll(".page").forEach(x=>x.classList.add("hidden"));
document.getElementById(p).classList.remove("hidden");
}

function updateUI(){
document.getElementById("balance").innerText=balance;
document.getElementById("daily").innerText=daily;
document.getElementById("ref").value="https://nexa-invest.com/?ref="+user;
}

function renderPlans(){
let box=document.getElementById("plansBox");
box.innerHTML="";
plans.forEach(p=>{
let d=document.createElement("div");
d.className="card plan";
d.innerHTML=`
<div>
<b>Rs ${p.price}</b><br>
${p.days} دن<br>
روزانہ: Rs ${p.profit}
</div>
<button onclick="buy(${p.id})">خریدیں</button>
`;
box.appendChild(d);
});
}

function buy(id){
let p=plans.find(x=>x.id===id);
if(balance<p.price)return alert("بیلنس ناکافی");
balance-=p.price;
daily+=p.profit;
history.push(`پلان خریدا: Rs ${p.price}`);
updateUI();
renderHistory();
show("dashboard");
}

function renderHistory(){
let h=document.getElementById("historyBox");
h.innerHTML="";
history.forEach(x=>{
let d=document.createElement("div");
d.className="card";
d.innerText=x;
h.appendChild(d);
});
}

function updateNum(){
document.getElementById("number").value=
method.value==="jazz"?"03705519562":"03379827882";
}

function submitDeposit(){
let amt=document.getElementById("amount").value;
history.push("ڈیپازٹ ریکویسٹ: Rs "+amt);
renderHistory();
alert("ڈیپازٹ سبمٹ ہو گیا");
}

function copyRef(){
navigator.clipboard.writeText(ref.value);
alert("کاپی ہو گیا");
}

function openWA(){
window.open("https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu");
}
function openMail(){
window.location="mailto:rock.earn92@gmail.com";
}
</script>

</body>
</html>
