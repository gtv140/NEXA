<NEXA>
<html lang="ur">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA Investment</title>
<style>
body{margin:0;background:#050505;color:#fff;font-family:Arial}
.hidden{display:none}
header{text-align:center;padding:18px;font-size:26px;font-weight:900;color:#00ffd5}
.page{max-width:420px;margin:14px auto;padding:16px;border-radius:14px;background:#0b0b0b}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:none}
button{background:#00ffd5;color:#000;font-weight:800;cursor:pointer}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;background:#000;padding:10px 0}
.nav div{text-align:center;font-size:13px;cursor:pointer}
.card{background:#111;padding:12px;border-radius:10px;margin:10px 0}
.plan{display:flex;justify-content:space-between;gap:10px;align-items:center}
.copy{background:#222;color:#00ffd5}
.countdown{font-size:13px;color:#0ff;font-weight:700;margin-top:4px}
</style>
</head>
<body>

<header>NEXA Investment</header>

<!-- LOGIN -->
<div id="login" class="page">
<h3>خوش آمدید</h3>
<input id="user" placeholder="یوزر نیم">
<button onclick="login()">جاری رکھیں</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="card">
<b>یوزر:</b> <span id="uName"></span><br>
بیلنس: Rs <span id="balance">0</span><br>
روزانہ منافع: Rs <span id="daily">0</span>
</div>

<div class="card">
<b>ممبر آئی ڈی</b>
<input id="memberID" readonly>
<button class="copy" onclick="copyID()">کاپی کریں</button>
</div>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h3>سمال پلانز</h3>
<div id="plansBox"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h3>ڈیپازٹ</h3>
<input id="amount" readonly>
<select id="method" onchange="updateNum()">
<option value="jazz">JazzCash</option>
<option value="easy">EasyPaisa</option>
</select>
<input id="number" readonly>
<button onclick="submitDeposit()">سبمٹ</button>
</div>

<!-- NAV -->
<div id="nav" class="nav hidden">
<div onclick="show('dashboard')">🏠 ہوم</div>
<div onclick="show('plans')">📦 پلانز</div>
<div onclick="show('deposit')">💰 ڈیپازٹ</div>
</div>

<script>
// ===== VARIABLES =====
let user="", balance=0, daily=0, selectedAmount=0;
const plans=[];
const planCount=50;
for(let i=0;i<planCount;i++){
  let price=200 + i*200;
  let days=25 + i;
  let dailyProfit=Math.floor(price*0.07);
  let endDate=new Date().getTime() + days*24*60*60*1000;
  plans.push({price,days,dailyProfit,endDate});
}

// ===== LOGIN =====
function login(){
user=document.getElementById("user").value;
if(!user)return alert("نام درج کریں");
document.getElementById("login").classList.add("hidden");
document.getElementById("dashboard").classList.remove("hidden");
document.getElementById("nav").classList.remove("hidden");
document.getElementById("uName").innerText=user;
document.getElementById("memberID").value="NEXA-"+Math.floor(100000+Math.random()*900000);
renderPlans();
updateDashboard();
}

// ===== DASHBOARD =====
function updateDashboard(){
document.getElementById("balance").innerText=balance.toFixed(2);
document.getElementById("daily").innerText=daily.toFixed(2);
}

// ===== SHOW PAGE =====
function show(p){
document.querySelectorAll(".page").forEach(x=>x.classList.add("hidden"));
document.getElementById(p).classList.remove("hidden");
}

// ===== RENDER PLANS =====
function renderPlans(){
let box=document.getElementById("plansBox");
box.innerHTML="";
plans.forEach((p,i)=>{
  let d=document.createElement("div");
  d.className="card plan";
  d.innerHTML=`
  <div>
  <b>Rs ${p.price}</b><br>
  ${p.days} دن<br>
  روزانہ Rs ${p.dailyProfit}
  <div class="countdown" id="cd${i}"></div>
  </div>
  <button onclick="buyNow(${i})">Buy Now</button>
  `;
  box.appendChild(d);
  startCountdown(i);
});
}

// ===== BUY NOW =====
function buyNow(index){
let plan=plans[index];
selectedAmount=plan.price;
document.getElementById("amount").value="Rs "+plan.price;
show("deposit");
updateNum();
}

// ===== DEPOSIT =====
function updateNum(){
document.getElementById("number").value=
document.getElementById("method").value==="jazz"?"03705519562":"03379827882";
}
function submitDeposit(){
balance += selectedAmount;
daily += Math.floor(selectedAmount*0.07);
updateDashboard();
alert("ڈیپازٹ کامیاب ہو گیا");
}

// ===== COPY MEMBER ID =====
function copyID(){
navigator.clipboard.writeText(memberID.value);
alert("ممبر آئی ڈی کاپی ہو گئی");
}

// ===== COUNTDOWN =====
function startCountdown(i){
function updateCD(){
let remaining=Math.floor((plans[i].endDate - new Date().getTime())/1000);
if(remaining<0){document.getElementById("cd"+i).innerText="ختم ہو گیا";return;}
let d=Math.floor(remaining/86400);
let h=Math.floor((remaining%86400)/3600);
let m=Math.floor((remaining%3600)/60);
let s=remaining%60;
document.getElementById("cd"+i).innerText=`باقی: ${d} دن ${h}:${m}:${s}`;
setTimeout(updateCD,1000);
}
updateCD();
}
</script>

</body>
</html>
