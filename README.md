<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>NEXA EARN</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body{margin:0;font-family:Segoe UI,Arial;background:#f6f7fb;color:#333}
header{background:#ffffff;padding:16px;text-align:center;
font-size:26px;font-weight:800;color:#c89b3c;box-shadow:0 6px 20px rgba(0,0,0,.08)}
.page{max-width:430px;margin:18px auto 90px;background:#fff;border-radius:18px;
padding:18px;box-shadow:0 10px 30px rgba(0,0,0,.1)}
.hidden{display:none}
input,select,button{width:100%;padding:12px;margin-top:10px;
border-radius:12px;border:1px solid #ddd;font-size:15px}
button{background:#c89b3c;color:#fff;font-weight:700;border:none;cursor:pointer}
.stats{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:14px}
.box{background:#f3f4f8;padding:14px;border-radius:14px;text-align:center}
.box b{display:block;margin-top:6px;font-size:18px;color:#c89b3c}
.hero{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:12px}
.hero img{width:100%;border-radius:12px;height:110px;object-fit:cover}
.notice{background:#fff3cd;padding:10px;border-radius:10px;margin-top:12px;
font-size:13px}
.plan{background:#f4f5fa;padding:14px;border-radius:14px;margin-top:12px}
.plan h4{margin:0;color:#c89b3c}
.offer{border:2px dashed #c89b3c}
.timer{color:#d63031;font-weight:700;margin-top:6px}
.nav{position:fixed;bottom:0;left:0;right:0;background:#fff;
display:flex;justify-content:space-around;padding:10px 0;
box-shadow:0 -6px 18px rgba(0,0,0,.12)}
.nav div{font-size:12px;font-weight:700;color:#c89b3c;cursor:pointer;text-align:center}
.nav div span{display:block;font-size:20px}
.ad{background:#e8f0ff;padding:12px;border-radius:12px;margin-top:12px;font-size:14px}
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
    <img src="https://images.unsplash.com/photo-1522202176988-66273c2fd55f">
    <img src="https://images.unsplash.com/photo-1504384308090-c894fdcc538d">
    <img src="https://images.unsplash.com/photo-1559526324-593bc073d938">
    <img src="https://images.unsplash.com/photo-1556740749-887f6717d7e4">
  </div>

  <div class="notice">
    Secure digital earning platform. Funds are protected & profits calculated daily.
  </div>

  <div class="ad" id="adBox"></div>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h3>Investment Plans</h3>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h3>Deposit Funds</h3>
  <select id="depMethod" onchange="setNum()">
    <option value="jazz">JazzCash</option>
    <option value="easy">EasyPaisa</option>
  </select>
  <input id="depNum" readonly>
  <input id="depAmt" placeholder="Amount">
  <button onclick="deposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdraw" class="page hidden">
  <h3>Withdraw Funds</h3>
  <input id="wAmt" placeholder="Amount">
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
let currentUser=null,balance=0,daily=0,total=0,activePlan=null
const ads=[
"🔥 Limited Offer: Get 2.4x profit today!",
"💰 5,000+ users earning daily profits",
"⚡ Fast withdrawals within 24 hours",
"🎁 Special plans available for limited time"
]

function login(){
 const u=user.value.trim()
 if(!u)return alert("Enter username")
 currentUser=u
 localStorage.setItem("nexa_user",u)
 let data=JSON.parse(localStorage.getItem("user_"+u)||"{}")
 balance=data.balance||0
 daily=data.daily||0
 total=data.total||0
 activePlan=data.plan||null
 save(); loadApp()
}

function save(){
 localStorage.setItem("user_"+currentUser,
 JSON.stringify({balance,daily,total,plan:activePlan}))
}

function loadApp(){
 loginPage.classList.add("hidden")
 nav.classList.remove("hidden")
 uname.innerText=currentUser
 users.innerText=Math.floor(1000+Math.random()*4000)
 adBox.innerText=ads[Math.floor(Math.random()*ads.length)]
 updateUI(); show("dashboard")
}

function updateUI(){
 bal.innerText=balance
 daily.innerText=daily
 total.innerText=total
}

function show(id){
 document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden"))
 document.getElementById(id).classList.remove("hidden")
}

function logout(){location.reload()}

function setNum(){
 depNum.value=depMethod.value==="jazz"?"03705519562":"03379827882"
}

function deposit(){
 let a=+depAmt.value
 if(a<=0)return alert("Invalid amount")
 balance+=a; save(); updateUI()
 alert("Deposit successful")
}

function withdraw(){
 let a=+wAmt.value
 if(a<=0||a>balance)return alert("Invalid amount")
 balance-=a; save(); updateUI()
 alert("Withdrawal requested")
}

function buyPlan(inv,rate){
 if(balance<inv)return alert("Insufficient balance")
 balance-=inv
 activePlan={inv,rate,last:Date.now()}
 save(); updateUI()
 alert("Plan activated")
}

function profitTick(){
 if(!activePlan)return
 let days=Math.floor((Date.now()-activePlan.last)/86400000)
 if(days>0){
 let earn=Math.floor(activePlan.inv*activePlan.rate)*days
 balance+=earn; total+=earn; daily=Math.floor(activePlan.inv*activePlan.rate)
 activePlan.last+=days*86400000
 save(); updateUI()
 }
}

function loadPlans(){
 let h=""
 for(let i=1;i<=50;i++){
 let inv=200*i
 let rate=i<=5?0.024:0.022
 h+=`
 <div class="plan ${i<=5?'offer':''}">
 <h4>Plan ${i}</h4>
 Invest: Rs ${inv}<br>
 Daily Profit: Rs ${Math.floor(inv*rate)}<br>
 <button onclick="buyPlan(${inv},${rate})">Buy Now</button>
 ${i<=5?'<div class="timer">Special Offer</div>':''}
 </div>`
 }
 plansList.innerHTML=h
}

setInterval(profitTick,1000)

window.onload=()=>{
 setNum(); loadPlans()
 const u=localStorage.getItem("nexa_user")
 if(u){user.value=u; login()}
}
</script>
</body>
</html>
