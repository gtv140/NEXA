<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEXA EARN</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body{margin:0;font-family:Segoe UI,Arial;background:#f5f6fb;color:#333}
header{background:#fff;padding:16px;text-align:center;font-size:26px;font-weight:800;color:#c89b3c;box-shadow:0 6px 20px rgba(0,0,0,.08)}
.page{max-width:430px;margin:16px auto 90px;background:#fff;border-radius:18px;padding:16px;box-shadow:0 10px 30px rgba(0,0,0,.1)}
.hidden{display:none}
input,select,button{width:100%;padding:12px;margin-top:10px;border-radius:12px;border:1px solid #ddd;font-size:15px}
button{background:#c89b3c;color:#fff;font-weight:700;border:none;cursor:pointer}
.stats{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:10px}
.box{background:#f3f4f8;padding:14px;border-radius:14px;text-align:center}
.box b{display:block;margin-top:6px;font-size:18px;color:#c89b3c}
.hero{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:12px}
.hero img{width:100%;border-radius:12px;height:110px;object-fit:cover}
.notice{background:#fff3cd;padding:10px;border-radius:10px;margin-top:12px;font-size:13px}
.plan{background:#f4f5fa;padding:14px;border-radius:16px;margin-top:12px}
.plan h4{margin:0;color:#c89b3c}
.plan img{width:100%;height:120px;object-fit:cover;border-radius:12px;margin-top:8px}
.offer{border:2px dashed #c89b3c}
.timer{color:#d63031;font-weight:700;margin-top:6px}
.nav{position:fixed;bottom:0;left:0;right:0;background:#fff;display:flex;justify-content:space-around;padding:10px 0;box-shadow:0 -6px 18px rgba(0,0,0,.12)}
.nav div{font-size:12px;font-weight:700;color:#c89b3c;cursor:pointer;text-align:center}
.nav div span{display:block;font-size:20px}
.copy{background:#eee;color:#333;font-weight:600}
.small{font-size:12px;color:#666}
</style>
</head>
<body>

<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
  <h3>Login / Signup</h3>
  <input id="user" placeholder="Username">
  <input id="pass" type="password" placeholder="Password">
  <button onclick="login()">Enter Dashboard</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <h3>Welcome, <span id="uname"></span></h3>
  <div class="stats">
    <div class="box">Balance<b>Rs <span id="bal">0</span></b></div>
    <div class="box">Daily Profit<b>Rs <span id="daily">0</span></b></div>
    <div class="box">Total Profit<b>Rs <span id="total">0</span></b></div>
    <div class="box">Active Users<b><span id="users">0</span></b></div>
  </div>
  <div class="hero">
    <img src="https://images.unsplash.com/photo-1559526324-593bc073d938">
    <img src="https://images.unsplash.com/photo-1504384308090-c894fdcc538d">
    <img src="https://images.unsplash.com/photo-1556740749-887f6717d7e4">
    <img src="https://images.unsplash.com/photo-1522202176988-66273c2fd55f">
  </div>
  <div class="notice">Secure digital earning platform. Daily profits auto calculated.</div>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h3>Investment Plans</h3>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h3>Deposit</h3>
  <select id="depMethod" onchange="setNum()">
    <option value="jazz">JazzCash</option>
    <option value="easy">EasyPaisa</option>
  </select>
  <div style="display:flex;gap:8px">
    <input id="depNum" readonly>
    <button class="copy" onclick="copyNum()">Copy</button>
  </div>
  <input id="depAmt" placeholder="Amount">
  <input id="txid" placeholder="Transaction ID">
  <input type="file">
  <button onclick="submitDeposit()">Submit Deposit</button>
  <div class="small">After payment submit TX ID & proof.</div>
</div>

<!-- WITHDRAW -->
<div id="withdraw" class="page hidden">
  <h3>Withdraw</h3>
  <select id="withMethod">
    <option>JazzCash</option>
    <option>EasyPaisa</option>
  </select>
  <input id="withAcc" placeholder="Account Number">
  <input id="withAmt" placeholder="Amount">
  <button onclick="withdraw()">Request Withdrawal</button>
</div>

<!-- NAV -->
<div id="nav" class="nav hidden">
  <div onclick="show('dashboard')"><span>🏠</span>Home</div>
  <div onclick="show('plans')"><span>📦</span>Plans</div>
  <div onclick="show('deposit')"><span>💰</span>Deposit</div>
  <div onclick="show('withdraw')"><span>💵</span>Withdraw</div>
  <div onclick="logout()"><span>🚪</span>Logout</div>
</div>

<script>
let currentUser=null,balance=0,daily=0,total=0,activePlan=null;
const JAZZ="03705519562", EASY="03379827882";

function login(){
 const u=user.value.trim(); if(!u) return alert("Enter username");
 currentUser=u; localStorage.setItem("nexa_user",u);
 let d=JSON.parse(localStorage.getItem("user_"+u)||"{}");
 balance=d.balance||0; daily=d.daily||0; total=d.total||0; activePlan=d.plan||null;
 save(); loadApp();
}
function save(){localStorage.setItem("user_"+currentUser,JSON.stringify({balance,daily,total,plan:activePlan}))}
function loadApp(){
 loginPage.classList.add("hidden"); nav.classList.remove("hidden");
 uname.innerText=currentUser; users.innerText=Math.floor(1000+Math.random()*4000);
 updateUI(); show("dashboard");
}
function updateUI(){bal.innerText=balance; daily.innerText=daily; total.innerText=total}
function show(id){document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden")); document.getElementById(id).classList.remove("hidden")}
function logout(){location.reload()}
function setNum(){depNum.value=depMethod.value==="jazz"?JAZZ:EASY}
function copyNum(){navigator.clipboard.writeText(depNum.value); alert("Number copied")}
function openDepositWithAmount(a){show('deposit'); depAmt.value=a; setNum()}
function submitDeposit(){
 let a=+depAmt.value; if(a<=0) return alert("Invalid amount");
 balance+=a; save(); updateUI(); alert("Deposit submitted");
}
function withdraw(){
 let a=+withAmt.value; if(a<=0||a>balance) return alert("Invalid amount");
 balance-=a; save(); updateUI(); alert("Withdrawal requested");
}
function buyPlan(p){
 openDepositWithAmount(p.invest);
 // activate after deposit manually (frontend demo)
 activePlan={invest:p.invest, daily:p.daily, total:p.total, days:p.days, end:p.end};
 save();
}
function profitTick(){
 if(!activePlan) return;
 let now=Date.now();
 if(now>=activePlan.end){activePlan=null; daily=0; save(); return;}
 let last=activePlan.last||now; let diff=Math.floor((now-last)/86400000);
 if(diff>0){
 let earn=activePlan.daily*diff;
 balance+=earn; total+=earn; daily=activePlan.daily;
 activePlan.last=last+diff*86400000; save(); updateUI();
 }
}
setInterval(profitTick,1000);

function loadPlans(){
 let savedEnds=JSON.parse(localStorage.getItem("planEnds")||"{}");
 let html=""; const photos=[
 "https://images.unsplash.com/photo-1504384308090-c894fdcc538d",
 "https://images.unsplash.com/photo-1559526324-593bc073d938",
 "https://images.unsplash.com/photo-1522202176988-66273c2fd55f"
 ];
 for(let i=1;i<=30;i++){
   let invest=200*i, days=25+ i;
   let rate=i<=5?2.4:2.2;
   let totalP=Math.floor(invest*rate);
   let dailyP=Math.floor(totalP/days);
   let end=savedEnds[i]||Date.now()+days*86400000;
   savedEnds[i]=end;
   let left=Math.max(0,Math.floor((end-Date.now())/1000));
   html+=`
   <div class="plan ${i<=5?'offer':''}">
     <h4>${i<=5?'Special ':'Plan '} ${i}</h4>
     <img src="${photos[i%photos.length]}">
     Invest: Rs ${invest}<br>
     Daily: Rs ${dailyP}<br>
     Total: Rs ${totalP}<br>
     Days: ${days}
     <div class="timer" id="t${i}"></div>
     <button onclick='buyPlan(${JSON.stringify({invest,days,daily:dailyP,total:totalP,end})})'>Buy Now</button>
   </div>`;
   setInterval(()=>{document.getElementById("t"+i).innerText="Time left: "+Math.max(0,Math.floor((end-Date.now())/1000))+"s"},1000);
 }
 localStorage.setItem("planEnds",JSON.stringify(savedEnds));
 plansList.innerHTML=html;
}

window.onload=()=>{
 setNum(); loadPlans();
 const u=localStorage.getItem("nexa_user"); if(u){user.value=u; login()}
}
</script>
</body>
</html>
