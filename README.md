<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --primary:#1E90FF;
  --accent:#FF69B4;
  --bg:#121212;
  --text:#fff;
}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:var(--text);}
header{font-size:28px;text-align:center;padding:20px;background:linear-gradient(90deg,var(--primary),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:480px;margin:20px auto;padding:20px;background:rgba(255,255,255,0.02);border-radius:12px;border:1px solid rgba(255,255,255,0.05);}
input, select, button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.2);background:transparent;color:#fff;}
button{background:linear-gradient(90deg,var(--primary),var(--accent));font-weight:700;cursor:pointer;transition:0.3s;}
button:hover{opacity:0.8;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px 0;background:rgba(0,0,0,0.9);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.plan-box{border-radius:12px;padding:12px;margin:10px 0;display:flex;justify-content:space-between;align-items:center;box-shadow:0 4px 12px rgba(0,0,0,0.5);transition:0.3s;}
.plan-box.special{background:linear-gradient(90deg,#FF69B4,#1E90FF);}
.plan-box.normal{background:rgba(255,255,255,0.05);}
.plan-box:hover{transform:translateY(-3px);}
.countdown{color:#FFD700;font-weight:700;}
.user-box{border-radius:12px;padding:15px;margin-bottom:12px;display:grid;grid-template-columns:1fr 1fr;gap:10px;background:rgba(255,255,255,0.03);box-shadow:0 4px 12px rgba(0,0,0,0.5);}
.user-box div{padding:8px;border-radius:8px;background:rgba(255,255,255,0.05);}
.photos{display:flex;gap:6px;overflow-x:auto;margin-top:10px;}
.photos img{width:120px;height:80px;border-radius:8px;object-fit:cover;transition:0.3s;}
.photos img:hover{transform:scale(1.05);}
.ads{background:rgba(255,255,255,0.05);padding:10px;border-radius:10px;margin-top:10px;font-size:14px;transition:0.3s;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;background:rgba(255,255,255,0.05);border-radius:10px;cursor:pointer;transition:0.3s;}
.support-icon:hover{box-shadow:0 6px 15px rgba(255,105,180,0.3);}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
<input id="username" placeholder="Enter Username"/>
<div style="position:relative;">
<input id="password" placeholder="Enter Password" type="password"/>
<span id="togglePass" style="position:absolute;right:10px;top:10px;cursor:pointer;">👁️</span>
</div>
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="user-box">
<div>Username: <span id="dashUser">-</span></div>
<div>Balance: Rs <span id="dashBalance">0</span></div>
<div>Daily Profit: Rs <span id="dashDaily">0</span></div>
<div>Total Profit: Rs <span id="dashTotal">0</span></div>
<div>Active Members: <span id="dashActive">0</span></div>
</div>

<div class="photos" id="photosBox"></div>
<div class="ads" id="adsBox"></div>

<h3>Plans</h3>
<div id="plansList"></div>
<button onclick="logout()">Logout</button>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h2>Deposit</h2>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<div style="display:flex;gap:8px;align-items:center;margin-top:10px">
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
<p>NEXA EARN is a digital platform providing easy investment plans with auto profit calculation. Trusted by thousands of members worldwide. Our goal is safe & fast profit growth.</p>
<div class="support-icon" onclick="openSupport()"><span class="ico">🛠️</span>Support</div>
</div>

<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
<div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
// ===== STORAGE =====
let users = JSON.parse(localStorage.getItem('nexa_users')||'{}');
let currentUser = localStorage.getItem('nexa_currentUser')||null;

// ===== PHOTOS & ADS =====
const photoUrls = ['https://picsum.photos/200/120?1','https://picsum.photos/200/120?2','https://picsum.photos/200/120?3','https://picsum.photos/200/120?4','https://picsum.photos/200/120?5'];
const adTexts = ['Special 2.4x Profit Today!','Limited Offer Plans!','Earn Daily Rs 500+','Invest Now & Get Bonus','New Users Get Extra Profit'];

// ===== PLANS =====
let plansData=[];
for(let i=1;i<=50;i++){
  let invest = 200 + (i-1)*100;
  let days = i<=5?25:30+Math.floor(i/5)*2;
  let multiplier = i<=5?2.4:2.2;
  let total = Math.round(invest*multiplier);
  let daily = Math.round(total/days);
  plansData.push({id:i,name:`Plan ${i}`,invest,days,multiplier,total,daily,special:i<=5});
}

// ===== SHOW PAGE =====
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

// ===== LOGIN =====
document.getElementById('togglePass').addEventListener('click',function(){
  const pass = document.getElementById('password');
  if(pass.type==='password'){ pass.type='text'; this.textContent='🙈'; }
  else{ pass.type='password'; this.textContent='👁️'; }
});

function login(){
  const option=document.getElementById('userOption').value;
  const user=document.getElementById('username').value.trim();
  const pass=document.getElementById('password').value.trim();
  if(!user||!pass){ alert('Enter Username & Password'); return; }

  if(option==='signup'){
    if(users[user]){ alert('Username exists'); return; }
    users[user]={password:pass,balance:0,daily:0,totalProfit:0,plans:[]};
    alert('Signup successful');
  } else {
    if(!users[user] || users[user].password!==pass){ alert('Invalid login'); return; }
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
  showPhotosAds();
  showPlans();
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
}

// ===== PHOTOS & ADS =====
function showPhotosAds(){
  const box=document.getElementById('photosBox');
  const adsBox=document.getElementById('adsBox');
  box.innerHTML=''; adsBox.innerHTML='';
  photoUrls.forEach(url=>{ const img=document.createElement('img'); img.src=url; box.appendChild(img); });
  adTexts.forEach(text=>{ const div=document.createElement('div'); div.textContent=text; adsBox.appendChild(div); });
}

// ===== PLANS RENDER =====
function showPlans(){
  const list=document.getElementById('plansList');
  list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='plan-box '+(p.special?'special':'normal');
    let countdown=p.special?`<div class="countdown" id="cd${p.id}"></div>`:'';
    div.innerHTML=`<div><b>${p.name}</b><br>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}<br>${countdown}</div>
    <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    list.appendChild(div);
    if(p.special) startCountdown(p.id,24*60*60);
  });
}

// ===== BUY PLAN =====
function buyPlan(id){
  const plan = plansData.find(p=>p.id===id);
  const data = users[currentUser];
  if(data.balance < plan.invest){ alert('Insufficient balance'); return; }
  data.balance -= plan.invest;
  data.daily += plan.daily;
  data.totalProfit += plan.total;
  data.plans.push({id:plan.id,start:Date.now()});
  users[currentUser]=data;
  localStorage.setItem('nexa_users',JSON.stringify(users));
  updateDashboard();
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
}

// ===== COUNTDOWN =====
function startCountdown(id,sec){
  const el = document.getElementById('cd'+id);
  let t=sec;
  setInterval(()=>{
    if(!el) return;
    const h=Math.floor(t/3600); const m=Math.floor(t%3600/60); const s=t%60;
    el.innerText=`Offer ends in ${h}h ${m}m ${s}s`; t--; if(t<0) t=0;
  },1000);
}

// ===== DEPOSIT =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}

// ===== WITHDRAW =====
function submitWithdraw(){alert('Withdrawal requested');}

// ===== SUPPORT =====
function openSupport(){window.open('https://chat.whatsapp.com/Example','_blank');}

// ===== LOGOUT =====
function logout(){currentUser=null; localStorage.removeItem('nexa_currentUser'); showPage('loginPage');}

// ===== AUTO LOGIN =====
if(currentUser) updateDashboard();
</script>
</body>
</html>
