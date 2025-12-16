<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA Earn</title>
<style>
:root{
  --primary:#ff00ff;
  --secondary:#00ffff;
  --accent:#ff9900;
  --dark:#0b0b0b;
  --text:#ffffff;
}
*{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif;}
body{
  background:#0b0b0b;color:var(--text);overflow-x:hidden;
  animation:bgAnim 20s ease-in-out infinite alternate;
}
@keyframes bgAnim{
  0%{background:linear-gradient(120deg,#ff00ff,#00ffff);}
  25%{background:linear-gradient(120deg,#ff9900,#ff00ff);}
  50%{background:linear-gradient(120deg,#00ffff,#ff9900);}
  75%{background:linear-gradient(120deg,#ff00ff,#ff9900);}
  100%{background:linear-gradient(120deg,#00ffff,#ff00ff);}
}
header{text-align:center;font-size:28px;font-weight:900;padding:20px;text-shadow:0 0 15px var(--primary),0 0 25px var(--secondary);}
.page{max-width:480px;margin:20px auto;background:rgba(0,0,0,0.5);padding:20px;border-radius:15px;border:1px solid rgba(255,255,255,0.1);box-shadow:0 0 20px var(--primary),0 0 40px var(--secondary);transition:0.3s all;}
.page:hover{box-shadow:0 0 25px var(--accent),0 0 50px var(--secondary);}
button,input,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:none;background:transparent;color:#fff;outline:none;font-size:14px;}
button{background:linear-gradient(90deg,var(--primary),var(--secondary));color:#000;font-weight:700;cursor:pointer;transition:0.3s all;box-shadow:0 0 10px var(--primary),0 0 20px var(--secondary);}
button:hover{transform:translateY(-3px);box-shadow:0 0 25px var(--accent),0 0 40px var(--secondary);}
.top-bar{display:flex;align-items:center;padding:10px 16px;background:rgba(0,0,0,0.5);border-radius:12px;max-width:480px;margin:20px auto 10px auto;}
.back-button{background:#ff4d4d;color:#fff;border:none;padding:8px 16px;border-radius:8px;cursor:pointer;font-weight:bold;font-size:16px;}
.hidden{display:none;}
.small{font-size:13px;color:rgba(255,255,255,0.7);}
.plan-box,.alert-box,.user-box,.referral-box,.support-icon{border-radius:12px;padding:14px;margin-bottom:12px;transition:0.3s all;}
.plan-box,.user-box,.referral-box,.support-icon{background:rgba(255,255,255,0.05);box-shadow:0 0 15px var(--primary),0 0 30px var(--secondary) inset;}
.plan-box:hover{transform:translateY(-3px);box-shadow:0 0 25px var(--accent),0 0 40px var(--secondary) inset;}
.alert-box{background:rgba(255,0,136,0.15);color:#fff;box-shadow:0 0 15px #ff00ff inset;}
.support-icon{display:flex;align-items:center;gap:8px;padding:12px;cursor:pointer;width:fit-content;font-weight:700;}
.support-icon:hover{transform:translateY(-2px);box-shadow:0 0 20px var(--accent),0 0 30px var(--secondary);}
.countdown{font-weight:700;color:var(--secondary);}
.more-menu{display:none;position:absolute;bottom:60px;left:50%;transform:translateX(-50%);background:rgba(0,0,0,0.9);border-radius:12px;padding:10px;min-width:140px;z-index:999;}
.more-menu div{padding:10px;border-radius:8px;cursor:pointer;}
.more-menu div:hover{background:rgba(255,255,255,0.1);}
#welcomePopup{position:fixed;top:20%;left:50%;transform:translateX(-50%);background:linear-gradient(90deg,#ff00ff,#00ffff);padding:20px;border-radius:15px;color:#000;font-weight:800;z-index:9999;text-align:center;display:none;}
#welcomePopup button{margin-top:10px;padding:10px;border:none;border-radius:10px;background:#000;color:#fff;cursor:pointer;}
/* ===== MOBILE APP STYLE NAV ===== */
.nav{
  position:fixed;bottom:10px;left:50%;transform:translateX(-50%);
  display:flex;justify-content:space-around;
  background:rgba(20,20,20,0.95);
  padding:10px 14px;border-radius:40px;
  max-width:420px;box-shadow:0 5px 15px rgba(0,0,0,0.5);z-index:999;
}
.nav div{
  text-align:center;cursor:pointer;width:60px;height:60px;border-radius:50%;
  display:flex;flex-direction:column;justify-content:center;align-items:center;transition:0.3s all;background:rgba(255,255,255,0.05);
}
.nav div:hover{transform:translateY(-5px);box-shadow:0 4px 12px var(--accent);}
.nav div.active{background:linear-gradient(135deg,var(--primary),var(--secondary));color:#000;box-shadow:0 4px 12px var(--primary),0 0 15px var(--secondary);}
.nav div .ico{font-size:24px;display:block;margin-bottom:4px;}
@media(max-width:480px){.page{margin:12px;padding:14px}.nav div{width:50px;height:50px}header{font-size:22px}}
</style>
</head>
<body>
<header>NEXA Earn</header>

<!-- WELCOME POPUP -->
<div id="welcomePopup">
  Welcome, <span id="welcomeUser">User</span>! To NEXA Earn 🚀
  <button onclick="closeWelcome()">Close</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page">
  <div class="alert-box">Deposit/Withdrawal issues? Contact Admin.</div>
  <div class="user-box">
    <div style="display:flex;justify-content:space-between;align-items:center;">
      <div>
        <div style="font-weight:900;font-size:16px;">Welcome User</div>
        <div class="small">Member since: <span id="dashSince">—</span></div>
      </div>
      <div>
        <div class="small">Balance</div>
        <div style="font-size:18px;font-weight:900">Rs <span id="dashBalance">0</span></div>
        <div class="small">Daily Profit: Rs <span id="dashDaily">0</span></div>
      </div>
    </div>
  </div>
  <div class="alert-box">Active Members: <span id="activeMembers">0</span></div>

  <div id="bottomNav" class="nav">
    <div id="homeIcon" onclick="setActive(this);showPage('dashboard')"><span class="ico">🏠</span>Home</div>
    <div id="plansIcon" onclick="setActive(this);showPage('plans')"><span class="ico">📦</span>Plans</div>
    <div id="depositIcon" onclick="setActive(this);showPage('deposit')"><span class="ico">💰</span>Deposit</div>
    <div id="withdrawalIcon" onclick="setActive(this);showPage('withdrawal')"><span class="ico">💵</span>Withdrawal</div>
    <div id="moreIcon"><span class="ico">⋯</span>More
      <div id="moreMenu" class="more-menu">
        <div onclick="setActive(this);showPage('history')">📜 History</div>
        <div onclick="setActive(this);showPage('about')">ℹ️ About</div>
        <div onclick="logout()">🚪 Logout</div>
      </div>
    </div>
  </div>
</div>

<!-- PAGES (PLANS, DEPOSIT, WITHDRAWAL, HISTORY, ABOUT) -->
<div id="plans" class="page hidden"><div class="top-bar"><button class="back-button" onclick="goHome()">← Back</button></div><h2>Plans</h2><div id="plansList"></div></div>
<div id="deposit" class="page hidden"><div class="top-bar"><button class="back-button" onclick="goHome()">← Back</button></div><h2>Deposit</h2>
<label>Method</label><select id="depositMethod" onchange="updateDepositNumber()"><option value="jazzcash">JazzCash</option><option value="easypaisa">EasyPaisa</option></select>
<div style="display:flex;gap:8px;align-items:center;margin-top:10px;"><input id="depositNumber" readonly style="flex:1"/><button onclick="copyDepositNumber()">Copy</button></div>
<label>Amount</label><input id="depositAmount" placeholder="Enter Amount"/><label>TX ID</label><input id="depositTxId" placeholder="TX ID"/>
<label>Upload Proof</label><input type="file" id="depositProof"/><button onclick="submitDeposit()">Submit Deposit</button></div>
<div id="withdrawal" class="page hidden"><div class="top-bar"><button class="back-button" onclick="goHome()">← Back</button></div><h2>Withdrawal</h2>
<label>Method</label><select id="withdrawMethod"><option value="jazzcash">JazzCash</option><option value="easypaisa">EasyPaisa</option></select>
<input id="withdrawAccount" placeholder="Account Number"/>
<input id="withdrawAmount" placeholder="Amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button></div>
<div id="history" class="page hidden"><div class="top-bar"><button class="back-button" onclick="goHome()">← Back</button></div><h2>History</h2><div id="historyList"></div></div>
<div id="about" class="page hidden"><div class="top-bar"><button class="back-button" onclick="goHome()">← Back</button></div>
<h2>About NEXA Earn</h2><p>Safe, professional, and modern investment platform. Automatic daily profits & full history tracking.</p>
<div class="support-icon" onclick="window.open('mailto:rock.earn92@gmail.com')">📧 Email Support</div>
<div class="support-icon" onclick="window.open('https://chat.whatsapp.com')">💬 WhatsApp Group</div>
</div>

<script>
// ===== STORAGE =====
let balance=parseFloat(localStorage.getItem('balance')||'0');
let dailyProfit=parseFloat(localStorage.getItem('dailyProfit')||'10');
let historyData=JSON.parse(localStorage.getItem('historyData')||'[]');

// ===== ACTIVE MEMBERS =====
function updateActiveMembers(){document.getElementById('activeMembers').innerText=Math.floor(Math.random()*500+50);setTimeout(updateActiveMembers,5000);}updateActiveMembers();

// ===== DASHBOARD =====
document.getElementById('dashBalance').innerText=balance.toFixed(2);
document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
document.getElementById('dashSince').innerText=new Date().toLocaleDateString();

// ===== NAVIGATION =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function goHome(){showPage('dashboard');}

// ===== ACTIVE ICON =====
function setActive(elem){document.querySelectorAll('.nav div').forEach(d=>d.classList.remove('active'));elem.classList.add('active');}

// ===== MORE MENU =====
function toggleMoreMenu(){const menu=document.getElementById('moreMenu');menu.style.display=menu.style.display==='block'?'none':'block';}
document.getElementById('moreIcon').addEventListener('click',toggleMoreMenu);
document.addEventListener('click',function(e){const more=document.getElementById('moreIcon');if(!more.contains(e.target)){document.getElementById('moreMenu').style.display='none';}});

// ===== PLANS =====
let plansData=[];for(let i=1;i<=50;i++){let invest=200*i;let days=25+i*2;let total=Math.round(invest*2.2);let daily=Math.round(total/days);plansData.push({id:i,name:'Plan '+i,invest,total,daily,days});}
function renderPlans(){const list=document.getElementById('plansList');list.innerHTML='';plansData.forEach(p=>{const div=document.createElement('div');div.className='plan-box';div.innerHTML=`<b>${p.name}</b> | Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days} <button onclick="buyPlan(${p.id})">Buy Now</button>`;list.appendChild(div);});}renderPlans();
function buyPlan(id){let plan=plansData.find(p=>p.id===id);if(!plan) return;document.getElementById('depositAmount').value=plan.invest;document.getElementById('depositMethod').value='jazzcash';updateDepositNumber();showPage('deposit');}

// ===== DEPOSIT =====
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value);alert('Deposit number copied!');}
function submitDeposit(){let amt=document.getElementById('depositAmount').value;let tx=document.getElementById('depositTxId').value;historyData.push(`Deposit Rs ${amt} TX:${tx} Approved`);localStorage.setItem('historyData',JSON.stringify(historyData));renderHistory();alert('Deposit submitted & approved!');}

// ===== WITHDRAWAL =====
function submitWithdraw(){let amt=document.getElementById('withdrawAmount').value;let acct=document.getElementById('withdrawAccount').value;historyData.push(`Withdrawal Rs ${amt} Account:${acct} Approved`);localStorage.setItem('historyData',JSON.stringify(historyData));renderHistory();alert('Withdrawal submitted & approved!');}

// ===== HISTORY =====
function renderHistory(){const list=document.getElementById('historyList');list.innerHTML='';historyData.forEach(h=>{const div=document.createElement('div');div.className='plan-box';div.innerText=h;list.appendChild(div);});}renderHistory();

// ===== DAILY PROFIT =====
setInterval(()=>{balance+=dailyProfit;localStorage.setItem('balance',balance);document.getElementById('dashBalance').innerText=balance.toFixed(2);},1000*60*60*24);

// ===== LOGOUT =====
function logout(){alert('Logged out!');location.reload();}

// ===== WELCOME POPUP =====
function showWelcome(username){const popup=document.getElementById('welcomePopup');document.getElementById('welcomeUser').innerText=username;popup.style.display='block';setTimeout(()=>{popup.style.display='none';},4000);}
function closeWelcome(){document.getElementById('welcomePopup').style.display='none';}

// ===== EXAMPLE LOGIN/ SIGNUP =====
let username=prompt("Enter your username");if(username) showWelcome(username);
</script>
</body>
</html>
