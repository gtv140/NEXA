<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>

<style>
:root{
  --red:#ff2d2d;
  --blue:#2563ff;
  --gold:#f5c542;
  --green:#4ade80;
  --purple:#9333ea;
  --dark:#0f1117;
  --card:#181b24;
}
body{
  margin:0;
  font-family:Arial,Helvetica,sans-serif;
  background:linear-gradient(135deg,#0f1117,#14182a);
  color:#fff;
  overflow-x:hidden;
}
header{
  text-align:center;
  padding:18px;
  font-size:26px;
  font-weight:700;
  color:var(--gold);
  text-shadow:0 0 12px #f5c542;
}
.page{
  max-width:480px;
  margin:15px auto 90px;
  padding:15px;
  display:none;
}
.show{display:block;}
.card{
  background:var(--card);
  border-radius:14px;
  padding:14px;
  margin-bottom:12px;
  box-shadow:0 0 20px rgba(255,255,255,.2);
  transition:all 0.3s ease;
}
.card:hover{
  box-shadow:0 0 20px var(--blue),0 0 30px var(--purple);
  transform:translateY(-2px);
}
input,select,button{
  width:100%;
  padding:11px;
  margin-top:10px;
  border-radius:10px;
  border:none;
  outline:none;
  transition:0.2s;
}
input,select{
  background:#0f1320;
  color:#fff;
}
input:focus,select:focus{ box-shadow:0 0 8px var(--blue); }
button{
  background:linear-gradient(90deg,var(--red),var(--gold));
  color:#000;
  font-weight:700;
  cursor:pointer;
  box-shadow:0 0 10px var(--red);
}
button:hover{
  box-shadow:0 0 15px var(--gold),0 0 25px var(--red);
  transform:scale(1.02);
}
button:active{transform:scale(.98)}

.stats{
  display:grid;
  grid-template-columns:repeat(3,1fr);
  gap:10px;
}
.stat{
  padding:10px;
  border-radius:12px;
  text-align:center;
  font-size:13px;
  background:linear-gradient(135deg,#0f1320,#181b24);
  box-shadow:0 0 8px #000 inset;
  transition:all 0.3s;
}
.stat:hover{
  box-shadow:0 0 12px #16a34a,0 0 15px #2563ff;
}
.stat b{display:block;font-size:15px;margin-top:3px; text-shadow:0 0 5px #fff;}
.s-user{background:linear-gradient(135deg,#6366f1,#9333ea); animation:pulse 2s infinite;}
.s-bal{background:linear-gradient(135deg,#16a34a,#4ade80); animation:pulse 2s infinite;}
.s-day{background:linear-gradient(135deg,#ef4444,#f97316); animation:pulse 2s infinite;}
.s-total{background:linear-gradient(135deg,#facc15,#fde047);color:#000; animation:pulse 2s infinite;}
.s-mem{background:linear-gradient(135deg,#334155,#475569); animation:pulse 2s infinite;}

@keyframes pulse{
  0%,100%{ box-shadow:0 0 8px rgba(255,255,255,0.1); }
  50%{ box-shadow:0 0 18px rgba(255,255,255,0.6); transform:scale(1.02);}
}

.icons{
  display:grid;
  grid-template-columns:repeat(3,1fr);
  gap:10px;
  margin-top:15px;
}
.icon{
  background:#0f1320;
  padding:14px 5px;
  border-radius:14px;
  text-align:center;
  cursor:pointer;
  border:1px solid rgba(255,255,255,.06);
  transition:0.3s;
}
.icon:hover{
  box-shadow:0 0 10px var(--blue),0 0 15px var(--purple);
  transform:translateY(-2px);
}
.icon span{font-size:22px;display:block;margin-bottom:4px}

img.banner{
  width:100%;
  border-radius:12px;
  margin-top:10px;
  transition:transform 0.4s ease;
}
img.banner:hover{ transform:scale(1.02); }

.nav{
  position:fixed;
  bottom:0;left:0;right:0;
  display:flex;
  justify-content:space-around;
  background:#0b0e18;
  padding:8px 0;
  box-shadow:0 0 10px #2563ff inset;
}
.nav div{
  text-align:center;
  font-size:12px;
  cursor:pointer;
  transition:0.3s;
}
.nav div:hover{ color:var(--gold); transform:scale(1.1); }
.nav span{font-size:20px;display:block}

.small{font-size:12px;opacity:.8}
.badge{
  display:inline-block;
  padding:4px 8px;
  border-radius:8px;
  background:linear-gradient(135deg,#2563ff,#9333ea);
  font-size:12px;
  margin-top:5px;
}
</style>
</head>

<body>

<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="login" class="page show">
  <div class="card">
    <h3>Login / Signup</h3>
    <input id="lUser" placeholder="Username">
    <input id="lPass" type="password" placeholder="Password">
    <button onclick="login()">Continue</button>
  </div>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page">
  <div class="stats">
    <div class="stat s-user"><small>User</small><b id="dUser">-</b></div>
    <div class="stat s-bal"><small>Balance</small><b>Rs <span id="dBal">0</span></b></div>
    <div class="stat s-day"><small>Daily</small><b>Rs <span id="dDay">0</span></b></div>
    <div class="stat s-total"><small>Total</small><b>Rs <span id="dTotal">0</span></b></div>
    <div class="stat s-mem"><small>Active</small><b id="dMem">0</b></div>
  </div>

  <div class="card">
    <h3>About NEXA EARN</h3>
    <p class="small">
      Since 2022, NEXA EARN provides digital earning opportunities through
      investment plans & ad-based tasks. Secure system, fast processing,
      user-friendly dashboard.
    </p>
    <img class="banner" src="https://picsum.photos/500/250?random=11">
    <img class="banner" src="https://picsum.photos/500/250?random=12">
  </div>

  <div class="icons">
    <div class="icon" onclick="openPage('plans')"><span>📦</span>Plans</div>
    <div class="icon" onclick="openPage('ads')"><span>🎬</span>Watch Ads</div>
    <div class="icon" onclick="openPage('deposit')"><span>💰</span>Deposit</div>
    <div class="icon" onclick="openPage('withdraw')"><span>💵</span>Withdraw</div>
    <div class="icon" onclick="openPage('support')"><span>🛠️</span>Support</div>
    <div class="icon" onclick="logout()"><span>🚪</span>Logout</div>
  </div>
</div>

<!-- Other pages like plans, ads, deposit, withdraw, support same as before -->

<script>
// All previous JS logic for login, plans, ads, tasks, countdown, notifications remains
// Just add glow animation to dashboard update for a real app-feel
function update(){
  dUser.innerText=user;
  dBal.innerText=bal.toFixed(0);
  dDay.innerText=day.toFixed(0);
  dTotal.innerText=total.toFixed(0);
  dMem.innerText=Math.floor(Math.random()*4000)+800;

  // Neon pulse effect on balance
  dBal.parentElement.style.boxShadow='0 0 15px #16a34a,0 0 25px #4ade80';
  setTimeout(()=>dBal.parentElement.style.boxShadow='',500);

  localStorage.setItem('nx_bal',bal);
  localStorage.setItem('nx_day',day);
  localStorage.setItem('nx_total',total);
}
</script>

</body>
</html>
