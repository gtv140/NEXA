<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>

<style>
body{
  margin:0;
  font-family:Arial, sans-serif;
  background:#f4f6f8;
}
header{
  text-align:center;
  font-size:30px;
  padding:18px;
  font-weight:bold;
  color:#2e7d32;
  background:#ffffff;
  box-shadow:0 2px 8px rgba(0,0,0,0.1);
}
.page{
  max-width:520px;
  margin:20px auto;
  padding:20px;
  background:#ffffff;
  border-radius:14px;
  box-shadow:0 4px 20px rgba(0,0,0,0.12);
}
.hidden{display:none}
input,select,button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid #ccc;
}
button{
  background:#2e7d32;
  color:#fff;
  font-weight:bold;
  cursor:pointer;
}
button:hover{opacity:0.9}

.info-box{
  background:#e8f5e9;
  padding:14px;
  border-radius:12px;
  margin-bottom:12px;
  font-weight:bold;
}
.photos{
  display:flex;
  gap:10px;
  margin:12px 0;
}
.photos img{
  width:48%;
  border-radius:10px;
}
.ad-box{
  background:#fff3cd;
  padding:10px;
  border-radius:10px;
  font-weight:bold;
  margin-top:10px;
}

.plan{
  border:1px solid #ddd;
  border-radius:10px;
  padding:12px;
  margin-top:10px;
}
.plan h4{margin:4px 0}

.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  background:#ffffff;
  display:flex;
  justify-content:space-around;
  padding:10px 0;
  box-shadow:0 -2px 10px rgba(0,0,0,0.15);
}
.nav div{text-align:center;cursor:pointer;font-size:13px}
.nav span{display:block;font-size:20px}
</style>
</head>

<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
<h3>Login / Signup</h3>
<select id="mode">
<option value="login">Login</option>
<option value="signup">Signup</option>
</select>
<input id="username" placeholder="Username">
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="info-box">
<div>User: <span id="u"></span></div>
<div>Balance: Rs <span id="bal">0</span></div>
<div>Daily Profit: Rs <span id="daily">0</span></div>
<div>Total Profit: Rs <span id="total">0</span></div>
<div>Active Users: <span id="active">0</span></div>
</div>

<div class="photos">
<img id="p1">
<img id="p2">
</div>

<div class="ad-box" id="adText"></div>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h3>Investment Plans</h3>
<div id="planList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h3>Deposit</h3>
<input id="depAmount" readonly>
<select>
<option>JazzCash 03705519562</option>
<option>EasyPaisa 03379827882</option>
</select>
<button onclick="confirmDeposit()">Confirm Deposit</button>
</div>

<!-- NAV -->
<div class="nav hidden" id="nav">
<div onclick="show('dashboard')"><span>🏠</span>Home</div>
<div onclick="show('plans')"><span>📦</span>Plans</div>
<div onclick="show('deposit')"><span>💰</span>Deposit</div>
<div onclick="logout()"><span>🚪</span>Logout</div>
</div>

<script>
let user = localStorage.getItem("user");
let balance = Number(localStorage.getItem("bal")||0);
let daily = Number(localStorage.getItem("daily")||0);
let total = Number(localStorage.getItem("total")||0);

function show(id){
document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
document.getElementById(id).classList.remove('hidden');
}

function login(){
let u=document.getElementById("username").value;
if(!u)return alert("Enter username");
user=u;
localStorage.setItem("user",u);
update();
}

function update(){
document.getElementById("u").innerText=user;
document.getElementById("bal").innerText=balance;
document.getElementById("daily").innerText=daily;
document.getElementById("total").innerText=total;
document.getElementById("nav").classList.remove("hidden");
show("dashboard");
}

function logout(){
localStorage.clear();
location.reload();
}

function randomPhotos(){
document.getElementById("p1").src="https://picsum.photos/200?"+Math.random();
document.getElementById("p2").src="https://picsum.photos/201?"+Math.random();
setTimeout(randomPhotos,5000);
}

function randomAds(){
let ads=[
"🔥 Special Offer: 2.4x Profit Today",
"🎁 Limited Time Bonus Active",
"🚀 High Return Plans Available",
"💎 Trusted by Thousands"
];
document.getElementById("adText").innerText=ads[Math.floor(Math.random()*ads.length)];
setTimeout(randomAds,6000);
}

function activeUsers(){
document.getElementById("active").innerText=Math.floor(Math.random()*5000)+1;
setTimeout(activeUsers,4000);
}

function buildPlans(){
let list=document.getElementById("planList");
for(let i=1;i<=50;i++){
let amt=200*i;
list.innerHTML+=`
<div class="plan">
<h4>Plan ${i}</h4>
<p>Investment: Rs ${amt}</p>
<p>Total Return: Rs ${Math.floor(amt*2.2)}</p>
<button onclick="buy(${amt})">Buy Now</button>
</div>`;
}
}

function buy(a){
document.getElementById("depAmount").value=a;
show("deposit");
}

function confirmDeposit(){
let a=Number(document.getElementById("depAmount").value);
balance+=a;
daily+=a*0.02;
total+=a*0.2;
localStorage.setItem("bal",balance);
localStorage.setItem("daily",daily);
localStorage.setItem("total",total);
update();
}

if(user){update()}
randomPhotos(); randomAds(); activeUsers(); buildPlans();
</script>
</body>
</html>
