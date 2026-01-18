<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#111;color:#fff;}
header{text-align:center;font-size:28px;padding:20px;background:linear-gradient(90deg,#4facfe,#00f2fe);-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:480px;margin:20px auto;padding:20px;background:rgba(255,255,255,0.02);border-radius:12px;border:1px solid rgba(255,255,255,0.1);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.1);background:transparent;color:#fff;}
button{background:linear-gradient(90deg,#4facfe,#00f2fe);font-weight:700;cursor:pointer;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px 0;background:rgba(0,0,0,0.8);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.plan-box{border:1px solid rgba(255,255,255,0.1);padding:10px;margin:10px 0;border-radius:10px;display:flex;justify-content:space-between;align-items:center;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;background:rgba(255,255,255,0.05);border-radius:10px;cursor:pointer;}
img.dashboard-img{width:100%;border-radius:12px;margin:8px 0;}
.countdown{font-weight:700;color:#4facfe;margin-top:6px;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">Signup</option></select>
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
<h2>Welcome <span id="dashUser">Guest</span></h2>
<div>Balance: Rs <span id="dashBalance">0</span></div>
<div>Daily Profit: Rs <span id="dashDaily">0</span></div>
<div>Active Members: <span id="activeMembers">0</span></div>

<div class="dashboard-info">
<h3>About NEXA EARN</h3>
<p>NEXA EARN is a premium digital investment platform offering fast, secure, and reliable profit growth. Our team is available 24/7 for support.</p>
<img class="dashboard-img" src="https://picsum.photos/400/150?random=1" />
<img class="dashboard-img" src="https://picsum.photos/400/150?random=2" />
<img class="dashboard-img" src="https://picsum.photos/400/150?random=3" />
<img class="dashboard-img" src="https://picsum.photos/400/150?random=4" />
</div>

<button onclick="logout()">Logout</button>
</div>

<div id="plans" class="page hidden">
<h2>Plans</h2>
<div id="plansList"></div>
</div>

<div id="deposit" class="page hidden">
<h2>Deposit</h2>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<div style="display:flex;gap:8px;align-items:center;margin-top:10px;">
<input id="depositNumber" readonly style="flex:1">
<button onclick="copyDepositNumber()">Copy</button>
</div>
<input id="depositAmount" placeholder="Enter Amount">
<input id="depositTxId" placeholder="Transaction ID">
<input type="file" id="depositProof">
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawal" class="page hidden">
<h2>Withdrawal</h2>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="withdrawAccount" placeholder="Account Number">
<input id="withdrawAmount" placeholder="Amount">
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div id="about" class="page hidden">
<h2>About & Support</h2>
<p>NEXA EARN provides secure and reliable investment services. For help, contact our support anytime.</p>
<div class="support-icon" onclick="openSupport()">
<span class="ico">🛠️</span> Support
</div>
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
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let activeMembers = parseInt(localStorage.getItem('nexa_activeMembers')||'0');
let savedTimers = JSON.parse(localStorage.getItem('nexa_offerTimers')||'{}');

// ===== DASHBOARD =====
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username');return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')) balance=0, dailyProfit=0, activeMembers=Math.floor(Math.random()*50)+1;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_activeMembers',activeMembers);
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('activeMembers').innerText=activeMembers;
  document.getElementById('bottomNav').classList.remove('hidden');
  renderPlans();
  showPage('dashboard');
}
function logout(){showPage('loginPage');} // localStorage safe
function openSupport(){window.open('https://chat.whatsapp.com/Example','_blank');}
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}
function submitWithdraw(){alert('Withdrawal requested');}

// ===== ACTIVE MEMBERS SIM =====
setInterval(()=>{
  activeMembers=parseInt(localStorage.getItem('nexa_activeMembers')||'0');
  activeMembers=Math.max(1,activeMembers + Math.floor(Math.random()*5)-2);
  document.getElementById('activeMembers').innerText=activeMembers;
  localStorage.setItem('nexa_activeMembers',activeMembers);
},5000);

// ===== PLANS =====
let plansData=[];
for(let i=1;i<=50;i++){
  let invest=200+i*200, days=25+Math.floor(i/2), multiplier=i<=5?2.4:2.2, special=i<=5;
  let endTime=savedTimers[i] || (special ? Date.now()+24*60*60*1000 : Date.now());
  if(special) savedTimers[i]=endTime;
  plansData.push({id:i,name:`Plan ${i}`,invest,days,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),special,endTime});
}
localStorage.setItem('nexa_offerTimers',savedTimers);

function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    let countdownHTML='';
    if(p.special){countdownHTML=`<div class="countdown" id="countdown${p.id}"></div>`; startCountdown(p.id);}
    div.innerHTML=`<div><b>${p.name}</b><br>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}${countdownHTML}</div>
    <button onclick="buyNow(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}

function buyNow(id){
  let plan=plansData.find(p=>p.id===id);
  if(!plan){alert('Plan not found'); return;}
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
}

// ===== SPECIAL OFFER COUNTDOWN =====
function startCountdown(id){
  const el=document.getElementById(`countdown${id}`);
  if(!el) return;
  const timerId=setInterval(()=>{
    let plan=plansData.find(p=>p.id===id);
    let now=Date.now();
    let diff=Math.max(0,plan.endTime-now);
    let h=Math.floor(diff/3600000);
    let m=Math.floor((diff%3600000)/60000);
    let s=Math.floor((diff%60000)/1000);
    el.innerText=`Ends in ${h}h ${m}m ${s}s`;
    if(diff<=0){clearInterval(timerId);el.innerText='Offer Ended';}
  },1000);
}
</script>
</body>
</html>
