<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>

<style>
:root{
 --bg:#0b0f1a;
 --card:#111827;
 --blue:#00e5ff;
 --red:#ff1744;
 --green:#00e676;
 --yellow:#ffea00;
 --border:rgba(255,255,255,0.08);
}

*{box-sizing:border-box}

body{
 margin:0;
 font-family:Arial,Helvetica,sans-serif;
 background:linear-gradient(180deg,#05070d,#0b0f1a);
 color:#fff;
}

header{
 text-align:center;
 padding:18px;
 font-size:28px;
 font-weight:800;
 letter-spacing:2px;
 background:linear-gradient(90deg,var(--blue),var(--red));
 -webkit-background-clip:text;
 -webkit-text-fill-color:transparent;
}

.page{
 max-width:520px;
 margin:20px auto;
 padding:18px;
}

.hidden{display:none}

.card{
 background:linear-gradient(145deg,#0f172a,#020617);
 border:1px solid var(--border);
 border-radius:16px;
 padding:15px;
 margin-bottom:15px;
 box-shadow:0 0 25px rgba(0,229,255,0.08);
}

input,select,button{
 width:100%;
 padding:12px;
 margin-top:10px;
 border-radius:12px;
 border:none;
 background:#020617;
 color:#fff;
 outline:none;
}

button{
 background:linear-gradient(90deg,var(--blue),var(--red));
 font-weight:700;
 cursor:pointer;
}

.stats{
 display:grid;
 grid-template-columns:1fr 1fr;
 gap:12px;
}

.stat{
 background:linear-gradient(145deg,#020617,#020617);
 border:1px solid var(--border);
 border-radius:14px;
 padding:14px;
 text-align:center;
}

.stat span{
 display:block;
 margin-top:6px;
 font-size:18px;
 font-weight:700;
}

.blue{color:var(--blue)}
.red{color:var(--red)}
.green{color:var(--green)}
.yellow{color:var(--yellow)}

.grid{
 display:grid;
 grid-template-columns:repeat(3,1fr);
 gap:12px;
 margin-top:15px;
}

.grid div{
 background:#020617;
 border:1px solid var(--border);
 border-radius:14px;
 padding:16px 10px;
 text-align:center;
 cursor:pointer;
 font-size:14px;
}

.grid div:hover{
 box-shadow:0 0 18px rgba(255,23,68,0.4);
}

img{
 width:100%;
 border-radius:14px;
 margin-top:10px;
}

.nav{
 position:fixed;
 bottom:0;
 left:0;
 right:0;
 background:#020617;
 border-top:1px solid var(--border);
 display:flex;
 justify-content:space-around;
 padding:10px 0;
}

.nav div{font-size:13px;cursor:pointer}
</style>
</head>

<body>

<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="login" class="page">
 <div class="card">
  <h3>Login / Signup</h3>
  <input id="username" placeholder="Username">
  <input id="password" type="password" placeholder="Password">
  <button onclick="login()">Continue</button>
 </div>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">

 <div class="stats">
  <div class="stat blue">Username<span id="dUser">-</span></div>
  <div class="stat green">Balance<span>Rs <span id="dBal">0</span></span></div>
  <div class="stat yellow">Daily Profit<span>Rs <span id="dDaily">0</span></span></div>
  <div class="stat red">Total Profit<span>Rs <span id="dTotal">0</span></span></div>
 </div>

 <div class="card">
  <h4>About NEXA EARN</h4>
  <p>Premium digital earning platform with ads tasks and investment plans.</p>
  <img src="https://picsum.photos/400/200?random=10">
 </div>

 <div class="grid">
  <div onclick="show('plans')">📦<br>Plans</div>
  <div onclick="show('ads')">🎬<br>Ads</div>
  <div onclick="show('deposit')">💰<br>Deposit</div>
  <div onclick="show('withdraw')">💵<br>Withdraw</div>
  <div onclick="show('history')">🕒<br>History</div>
  <div onclick="logout()">🚪<br>Logout</div>
 </div>

</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
 <div class="card"><h3>Investment Plans</h3><div id="planList"></div></div>
</div>

<!-- ADS -->
<div id="ads" class="page hidden">
 <div class="card">
  <h3>Watch Ads</h3>
  <p>Daily ads watch → automatic profit add</p>
  <button onclick="watchAd()">Watch Ad</button>
 </div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
 <div class="card">
  <h3>Deposit</h3>
  <select id="method">
   <option>JazzCash</option>
   <option>EasyPaisa</option>
  </select>
  <input id="amount" placeholder="Amount">
  <button onclick="deposit()">Submit</button>
 </div>
</div>

<!-- WITHDRAW -->
<div id="withdraw" class="page hidden">
 <div class="card">
  <h3>Withdraw</h3>
  <input id="wAmount" placeholder="Amount">
  <button onclick="withdraw()">Request</button>
 </div>
</div>

<!-- HISTORY -->
<div id="history" class="page hidden">
 <div class="card">
  <h3>History</h3>
  <div id="hist"></div>
 </div>
</div>

<script>
let user=localStorage.getItem("user");
let bal=parseFloat(localStorage.getItem("bal")||0);
let daily=parseFloat(localStorage.getItem("daily")||0);
let total=parseFloat(localStorage.getItem("total")||0);

function save(){
 localStorage.setItem("bal",bal);
 localStorage.setItem("daily",daily);
 localStorage.setItem("total",total);
}

function login(){
 let u=username.value.trim();
 let p=password.value.trim();
 if(!u||!p) return alert("Fill all fields");
 localStorage.setItem("user",u);
 user=u;
 load();
}

function load(){
 login.classList.add("hidden");
 dashboard.classList.remove("hidden");
 dUser.innerText=user;
 dBal.innerText=bal.toFixed(0);
 dDaily.innerText=daily.toFixed(0);
 dTotal.innerText=total.toFixed(0);
}

function show(id){
 document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden"));
 document.getElementById(id).classList.remove("hidden");
}

function logout(){
 localStorage.clear();
 location.reload();
}

function deposit(){
 let a=parseFloat(amount.value);
 if(!a) return;
 bal+=a;
 save();
 load();
}

function withdraw(){
 let a=parseFloat(wAmount.value);
 if(a>bal) return alert("Low balance");
 bal-=a;
 save();
 load();
}

function watchAd(){
 daily+=50;
 total+=50;
 bal+=50;
 save();
 alert("Ad watched! Rs 50 added");
 load();
}

let plansHTML="";
for(let i=1;i<=6;i++){
 plansHTML+=`
 <div class="card">
  <b>Plan ${i}</b><br>
  Invest: Rs ${i*500}<br>
  Days: ${20+i*5}<br>
  <button onclick="amount.value=${i*500};show('deposit')">Buy</button>
 </div>`;
}
planList.innerHTML=plansHTML;

if(user) load();
</script>

</body>
</html>
