<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --primary:#4a90e2;
  --accent:#50e3c2;
  --bg:#f7f8fa;
  --text:#222;
  --card:#fff;
}
*{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif;}
body{background:var(--bg);color:var(--text);}
header{padding:20px;text-align:center;font-size:28px;font-weight:700;color:var(--primary);}
.page{max-width:480px;margin:20px auto;padding:20px;background:var(--card);border-radius:12px;box-shadow:0 4px 12px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin:8px 0;border-radius:8px;border:1px solid #ccc;}
button{background:linear-gradient(90deg,var(--primary),var(--accent));color:#fff;font-weight:700;cursor:pointer;transition:0.2s;}
button:hover{transform:translateY(-2px);box-shadow:0 4px 12px rgba(0,0,0,0.2);}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#fff;box-shadow:0 -2px 8px rgba(0,0,0,0.1);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;margin-bottom:2px;}
.hidden{display:none;}
.card{background:#fff;padding:12px;margin:10px 0;border-radius:12px;box-shadow:0 2px 8px rgba(0,0,0,0.1);}
.card img{width:100%;border-radius:12px;margin-bottom:10px;}
.card h3{font-size:18px;margin-bottom:6px;color:var(--primary);}
.box{background:#fff;padding:12px;margin:10px 0;border-radius:12px;box-shadow:0 2px 12px rgba(0,0,0,0.1);}
.box strong{display:block;margin-bottom:6px;}
.countdown{color:red;font-weight:700;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">New User</option></select>
<input id="username" placeholder="Username"/>
<input id="password" placeholder="Password" type="password"/>
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="box">
<strong>Username:</strong> <span id="dashUser">-</span><br>
<strong>Balance:</strong> Rs <span id="dashBalance">0</span><br>
<strong>Daily Profit:</strong> Rs <span id="dashDaily">0</span><br>
<strong>Total Profit:</strong> Rs <span id="dashTotal">0</span><br>
<strong>Active Members:</strong> <span id="activeMembers">0</span>
</div>
<div id="randomPhotos"></div>
<div id="adsBox" class="card"></div>
<button onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h2>Investment Plans</h2>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h2>Deposit</h2>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<div style="display:flex;gap:8px;align-items:center;margin-top:10px;">
<input id="depositNumber" readonly style="flex:1"/>
<button onclick="copyDepositNumber()">Copy</button>
</div>
<input id="depositAmount" placeholder="Enter Amount"/>
<input id="depositTxId" placeholder="Transaction ID"/>
<input type="file" id="depositProof"/>
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
<h2>Withdrawal</h2>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="withdrawAccount" placeholder="Account Number"/>
<input id="withdrawAmount" placeholder="Amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<!-- ABOUT -->
<div id="about" class="page hidden">
<h2>About NEXA EARN</h2>
<p>NEXA EARN is a reliable digital investment platform offering fast and safe profit growth. Our team is always ready to support you.</p>
</div>

<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
<div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
// STORAGE
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let activeMembers = parseInt(localStorage.getItem('nexa_active')||'2000');

// PHOTOS & ADS
const photos=['https://picsum.photos/400/200?random=1','https://picsum.photos/400/200?random=2','https://picsum.photos/400/200?random=3','https://picsum.photos/400/200?random=4'];
const ads=['🔥 Special Offer: 2.4x Profit!','💰 Invest Today and Earn Daily!','🚀 Limited Time Plan Available!','📈 Grow Your Balance Fast!'];

// PLANS
const plans=[];
for(let i=1;i<=50;i++){
  let invest=200+i*100;
  let days=25+Math.floor(i/5)*5;
  let multiplier = i<=5?2.4:2.2;
  plans.push({id:i,name:'Plan '+i,invest,days,multiplier,total:Math.round(invest*multiplier),daily:Math.round(invest*multiplier/days)});
}

// FUNCTIONS
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('username').value.trim();
  const p=document.getElementById('password').value.trim();
  if(!u||!p){alert('Enter username & password');return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0; totalProfit=0; userPlans=[];
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  updateDashboard();
}
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashTotal').innerText=totalProfit;
  document.getElementById('activeMembers').innerText=activeMembers;
  showPage('dashboard');
  renderPhotos(); renderAds();
}
function renderPhotos(){
  const container=document.getElementById('randomPhotos'); container.innerHTML='';
  photos.forEach(src=>{
    const img=document.createElement('img'); img.src=src; img.className='card';
    container.appendChild(img);
  });
}
function renderAds(){const box=document.getElementById('adsBox'); box.innerHTML=ads[Math.floor(Math.random()*ads.length)];}
function showPlans(){
  showPage('plans');
  const list=document.getElementById('plansList'); list.innerHTML='';
  plans.forEach(p=>{
    const div=document.createElement('div'); div.className='card';
    div.innerHTML=`<strong>${p.name}</strong><br>Invest: Rs ${p.invest}<br>Daily: Rs ${p.daily}<br>Total: Rs ${p.total}<br>Days: ${p.days}<br><button onclick="buyPlan(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}
function buyPlan(id){
  const p=plans.find(pl=>pl.id===id);
  if(balance<p.invest){alert('Insufficient Balance!'); return;}
  balance-=p.invest; dailyProfit+=p.daily; totalProfit+=p.total;
  userPlans.push({id:p.id,name:p.name,daily:p.daily,total:p.total});
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  alert(p.name+' purchased!');
  updateDashboard();
}
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied!');}
function submitDeposit(){alert('Deposit submitted!');}
function submitWithdraw(){alert('Withdrawal requested!');}

// RANDOM ACTIVE MEMBERS COUNT
setInterval(()=>{
  activeMembers=Math.floor(Math.random()*5000);
  if(currentUser) document.getElementById('activeMembers').innerText=activeMembers;
},5000);

</script>
</body>
</html>
