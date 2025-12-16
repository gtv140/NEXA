<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root{
  --primary:#00ffff;
  --secondary:#ff00ff;
  --accent:#ff0000;
  --dark:#0b0b0b;
  --text:#ffffff;
}

/* GLOBAL */
*{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif;}
body{
  background:#0b0b0b;
  color:var(--text);
  overflow-x:hidden;
  animation:bgNeon 20s linear infinite alternate;
}
@keyframes bgNeon{
  0%{background:radial-gradient(circle at top left, #0b0b0b, #1a1a1a);}
  25%{background:radial-gradient(circle at top right, #0b0b0b, #111111);}
  50%{background:radial-gradient(circle at bottom left, #0b0b0b, #1c1c1c);}
  75%{background:radial-gradient(circle at bottom right, #0b0b0b, #121212);}
  100%{background:radial-gradient(circle at top left, #0b0b0b, #1a1a1a);}
}

/* HEADER NEON */
header{
  text-align:center;
  font-size:34px;
  font-weight:900;
  background:linear-gradient(90deg,var(--primary),var(--secondary),var(--accent));
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
  text-shadow:0 0 10px var(--primary),0 0 20px var(--secondary),0 0 30px var(--accent);
  letter-spacing:2px;
  animation: neonGlow 2s ease-in-out infinite alternate;
}
@keyframes neonGlow{
  0%{text-shadow:0 0 5px var(--primary),0 0 10px var(--secondary),0 0 15px var(--accent);}
  50%{text-shadow:0 0 15px var(--primary),0 0 25px var(--secondary),0 0 35px var(--accent);}
  100%{text-shadow:0 0 5px var(--primary),0 0 10px var(--secondary),0 0 15px var(--accent);}
}

/* PAGE STYLING */
.page{
  max-width:480px;
  margin:20px auto;
  background:rgba(0,0,0,0.7);
  padding:20px;
  border-radius:15px;
  border:1px solid rgba(255,255,255,0.1);
  box-shadow:0 0 20px var(--primary),0 0 40px var(--secondary);
  transition:0.3s all;
}
.page:hover{
  box-shadow:0 0 25px var(--accent),0 0 50px var(--secondary);
}

/* BUTTONS */
button,input,select{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:none;
  background:transparent;
  color:#fff;
  outline:none;
  font-size:14px;
}
button{
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  color:#000;
  font-weight:700;
  cursor:pointer;
  transition:0.3s all;
  box-shadow:0 0 10px var(--primary),0 0 20px var(--secondary);
}
button:hover{
  transform:translateY(-3px);
  box-shadow:0 0 25px var(--accent),0 0 40px var(--secondary);
}

/* BOXES */
.plan-box,.alert-box,.user-box,.referral-box,.support-icon{
  border-radius:12px;
  padding:14px;
  margin-bottom:12px;
  transition:0.3s all;
  background:rgba(255,255,255,0.05);
  box-shadow:0 0 15px var(--primary),0 0 30px var(--secondary) inset;
}
.plan-box:hover{
  transform:translateY(-3px);
  box-shadow:0 0 25px var(--accent),0 0 40px var(--secondary) inset;
}
.alert-box{
  background:rgba(0,255,255,0.1);
  color:#fff;
  box-shadow:0 0 15px var(--primary) inset;
}

/* NAVIGATION */
.nav{
  position:fixed;
  bottom:10px;
  left:50%;
  transform:translateX(-50%);
  display:flex;
  justify-content:space-around;
  background:rgba(20,20,20,0.95);
  padding:10px 14px;
  border-radius:40px;
  max-width:420px;
  box-shadow:0 5px 15px rgba(0,255,255,0.3);
  z-index:999;
}
.nav div{
  text-align:center;
  cursor:pointer;
  width:60px;
  height:60px;
  border-radius:50%;
  display:flex;
  flex-direction:column;
  justify-content:center;
  align-items:center;
  transition:0.3s all;
  background:rgba(255,255,255,0.05);
}
.nav div:hover{
  transform:translateY(-5px);
  box-shadow:0 0 15px var(--accent);
}
.nav div.active{
  background:linear-gradient(135deg,var(--primary),var(--secondary));
  color:#000;
  box-shadow:0 4px 12px var(--primary),0 0 15px var(--secondary);
}
.nav div .ico{font-size:24px;display:block;margin-bottom:4px;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN PAGE -->
<div id="loginPage" class="page">
  <h2>Login to NEXA EARN</h2>
  <label>Username</label>
  <input type="text" id="loginUsername" placeholder="Enter Username"/>
  <label>Password</label>
  <input type="password" id="loginPassword" placeholder="Enter Password"/>
  <button onclick="login()">Login</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div id="dashboardMsg" class="alert-box">Welcome to NEXA EARN! 🚀 Grow your earnings with daily automated profits.</div>
  <div class="user-box">
    <div style="display:flex;justify-content:space-between;align-items:center;">
      <div>
        <div style="font-weight:900;font-size:16px;">Welcome <span id="dashUser">User</span></div>
        <div class="small">Member since: <span id="dashSince">—</span></div>
      </div>
      <div>
        <div class="small">Balance</div>
        <div style="font-size:18px;font-weight:900">Rs <span id="dashBalance">0</span></div>
        <div class="small">Daily Profit: Rs <span id="dashDaily">0</span></div>
      </div>
    </div>
  </div>
  <div class="alert-box" style="background:linear-gradient(135deg,#00ffff,#ff00ff,#ff0000);text-align:center;font-weight:900;letter-spacing:1px;">Active Members: <span id="activeMembers">0</span></div>
  <div class="alert-box" id="countdownContainer">Plan Countdown: <span id="planCountdown">—</span></div>
</div>

<!-- Add your plans, deposit, withdrawal, history pages here same as previous structure -->

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div id="homeIcon" onclick="setActive(this);showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div id="plansIcon" onclick="setActive(this);showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div id="depositIcon" onclick="setActive(this);showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div id="withdrawalIcon" onclick="setActive(this);showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div id="historyIcon" onclick="setActive(this);showPage('history')"><span class="ico">📜</span>History</div>
  <div id="logoutIcon" onclick="logout()"><span class="ico">🚪</span>Logout</div>
</div>

<script>
// ===== STORAGE =====
let balance=parseFloat(localStorage.getItem('balance')||'0');
let loggedUser=localStorage.getItem('loggedUser')||'';
let activePlan=JSON.parse(localStorage.getItem('activePlan')||'null');
let planEndTime=localStorage.getItem('planEndTime')||'';

// ===== LOGIN PERSISTENCE =====
window.onload=function(){
  if(loggedUser){
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    document.getElementById('dashUser').innerText=loggedUser;
  }
};

// ===== LOGIN FUNCTION =====
function login(){
  let u=document.getElementById('loginUsername').value.trim();
  let p=document.getElementById('loginPassword').value.trim();
  if(!u||!p){alert('Enter username and password!'); return;}
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  document.getElementById('bottomNav').classList.remove('hidden');
  document.getElementById('dashUser').innerText=u;
  localStorage.setItem('loggedUser',u);
}
</script>
</body>
</html>
