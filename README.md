<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#111;color:#fff;}
header{font-size:28px;text-align:center;padding:20px;background:linear-gradient(90deg,#1E90FF,#FF69B4);-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:480px;margin:20px auto;padding:20px;background:rgba(255,255,255,0.02);border-radius:12px;border:1px solid rgba(255,255,255,0.05);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.2);background:transparent;color:#fff;}
button{background:linear-gradient(90deg,#1E90FF,#FF69B4);font-weight:700;cursor:pointer;transition:0.3s;}
button:hover{opacity:0.8;}
.plan-box{border:1px solid rgba(255,255,255,0.1);padding:10px;margin:10px 0;border-radius:10px;display:flex;justify-content:space-between;align-items:center;}
img{width:100%;border-radius:10px;margin:10px 0;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN / SIGNUP -->
<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption">
  <option value="login">Login</option>
  <option value="signup">Signup</option>
</select>
<input id="username" placeholder="Enter Username"/>
<input id="password" type="password" placeholder="Enter Password"/>
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<h2>Welcome, <span id="dashUser">-</span></h2>
<p>Balance: Rs <span id="dashBalance">0</span></p>
<p>Daily Profit: Rs <span id="dashDaily">0</span></p>
<p>Total Profit: Rs <span id="dashTotal">0</span></p>
<p>Active Members: <span id="dashActive">0</span></p>

<div id="companyInfo">
<h3>About NEXA EARN</h3>
<p>NEXA EARN is a digital investment platform providing fast, secure, and reliable profit opportunities.</p>
<img src="https://source.unsplash.com/400x200/?finance,money" alt="Company">
<img src="https://source.unsplash.com/400x200/?technology,crypto" alt="Company">
<img src="https://source.unsplash.com/400x200/?investment" alt="Company">
</div>

<h3>Plans</h3>
<div id="plansList"></div>

<h3>Deposit</h3>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<div style="display:flex; gap:8px; align-items:center; margin-top:10px">
<input id="depositNumber" readonly style="flex:1" />
<button onclick="copyDepositNumber()">Copy</button>
</div>
<input id="depositAmount" placeholder="Enter Amount"/>
<input id="depositTxId" placeholder="Transaction ID"/>
<input type="file" id="depositProof"/>
<button onclick="submitDeposit()">Submit Deposit</button>

<h3>Withdrawal</h3>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="withdrawAccount" placeholder="Account Number"/>
<input id="withdrawAmount" placeholder="Amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>

<button onclick="logout()">Logout</button>
</div>

<script>
// ===== STORAGE =====
let users = JSON.parse(localStorage.getItem('nexa_users')||'{}');
let currentUser = localStorage.getItem('nexa_currentUser')||null;

// ===== PLANS =====
let plansData = [];
for(let i=1;i<=50;i++){
  let invest = 200 + (i-1)*200;
  let days = 25 + Math.floor((i-1)/5)*5;
  let multiplier = i<=5 ? 2.4 : 2.2;
  plansData.push({id:i,name:`Plan ${i}`,invest,days,multiplier,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),special:i<=5});
}

// ===== SHOW PAGE =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}

// ===== LOGIN / SIGNUP =====
function login(){
  const option=document.getElementById('userOption').value;
  const user=document.getElementById('username').value.trim();
  const pass=document.getElementById('password').value.trim();
  if(!user||!pass){alert('Enter Username & Password');return;}
  if(option==='signup'){
    if(users[user]){alert('Username exists'); return;}
    users[user]={password:pass,balance:0,daily:0,totalProfit:0,plans:[]};
    alert('Signup Successful');
  } else {
    if(!users[user]||users[user].password!==pass){alert('Invalid login');return;}
  }
  currentUser=user;
  localStorage.setItem('nexa_currentUser',currentUser);
  localStorage.setItem('nexa_users',JSON.stringify(users));
  updateDashboard();
}

// ===== DASHBOARD =====
function updateDashboard(){
  if(!currentUser) return;
  const data = users[currentUser];
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=data.balance.toFixed(2);
  document.getElementById('dashDaily').innerText=data.daily.toFixed(2);
  document.getElementById('dashTotal').innerText=data.totalProfit.toFixed(2);
  document.getElementById('dashActive').innerText=Math.floor(Math.random()*5000+1);
  renderPlans();
  showPage('dashboard');
}

// ===== PLANS =====
function renderPlans(){
  const container=document.getElementById('plansList');
  container.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<div><b>${p.name}</b><br>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div>
    <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    container.appendChild(div);
  });
}

// ===== BUY PLAN =====
function buyPlan(id){
  const plan=plansData.find(p=>p.id===id);
  if(!plan) return;
  const data=users[currentUser];
  if(data.balance<plan.invest){alert('Insufficient balance'); return;}
  data.balance-=plan.invest;
  data.daily+=plan.daily;
  data.totalProfit+=plan.total;
  data.plans.push({id:plan.id,name:plan.name,daily:plan.daily,total:plan.total,start:Date.now(),days:plan.days});
  users[currentUser]=data;
  localStorage.setItem('nexa_users',JSON.stringify(users));
  document.getElementById('depositAmount').value=plan.invest;
  updateDashboard();
  alert(`Plan ${plan.name} purchased! Please deposit the amount.`);
}

// ===== DEPOSIT =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}
function submitWithdraw(){alert('Withdrawal requested');}

// ===== LOGOUT =====
function logout(){currentUser=null; localStorage.removeItem('nexa_currentUser');showPage('loginPage');}

// ===== AUTO LOGIN =====
if(currentUser) updateDashboard();
</script>
</body>
</html>
