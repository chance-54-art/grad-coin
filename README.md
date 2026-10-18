<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GRAD-COIN</title>
<style>
body{margin:0;padding:0 0 110px 0;background:#0b1e3a;color:white;font-family:Arial;text-align:center}
h1{color:#ffd700;font-size:30px;margin:15px 0 5px}
.card{background:#132c52;margin:15px;border-radius:20px;padding:20px}
.big{font-size:60px}
.balance{font-size:55px;color:#ffd700;font-weight:bold}
.timer{font-size:20px;color:#00ff88;margin:10px}
.progress{height:15px;background:#333;border-radius:10px;overflow:hidden;margin:10px 0}
.bar{height:100%;background:#ffd700;width:0%}
button{border:0;border-radius:15px;padding:15px;font-weight:bold;font-size:17px;width:100%;margin:5px 0}
.yellow{background:#ffd700;color:black}
.gray{background:#555;color:white}
.bottom{position:fixed;bottom:0;left:0;right:0;background:#0b1e3a;padding:10px 15px;box-shadow:0 -5px 20px black;z-index:99}
</style>
</head>
<body>

<h1>🚀 GRAD-COIN</h1>

<div class="card">
<div class="big">💰</div>
<div class="balance" id="solde">0</div>
<div>GRAD</div>
<div id="status">Prêt à miner</div>
<div class="timer" id="temps">24h 00m restantes</div>
<div class="progress"><div class="bar" id="barre"></div></div>
<div id="info">1 GRAD = 1 heure de minage</div>
</div>

<div class="card">
<div>🎁 Tâches</div><br>
<button class="yellow" onclick="tache(100)">✅ Rejoindre Canal +100 GRAD</button>
<button class="yellow" onclick="tache(50)">👥 INVITER +50 GRAD</button>
</div>

<div class="bottom">
<button class="yellow" id="boutonMiner" onclick="miner()">⛏️ MINER +1 GRAD / HEURE</button>
<div style="font-size:13px">Miner 1 fois par jour - 24h max - Anti-triche</div>
</div>

<script>
let solde = parseInt(localStorage.getItem("grad_solde")||"0");
let debut = parseInt(localStorage.getItem("grad_debut")||"0");
let actif = localStorage.getItem("grad_actif")==="1";

function maj(){
  document.getElementById("solde").innerText = solde;
  let maintenant = Date.now();
  if(actif && debut>0){
    // Anti-triche : si heure changée en arrière, stop
    if(maintenant < debut){
      actif=false; localStorage.setItem("grad_actif","0");
      document.getElementById("status").innerText="Tricherie détectée - Session annulée";
      document.getElementById("boutonMiner").innerText="⛏️ MINER +1 GRAD / HEURE";
      document.getElementById("boutonMiner").className="yellow";
      return;
    }
    let passe = maintenant - debut;
    let heures = Math.floor(passe / 3600000);
    if(heures >= 24){
      heures = 24;
      actif=false; localStorage.setItem("grad_actif","0");
      document.getElementById("status").innerText="✅ Session 24h terminée - Touche MINER pour recommencer";
      document.getElementById("boutonMiner").innerText="⛏️ MINER ENCORE +1 GRAD / HEURE";
      document.getElementById("boutonMiner").className="yellow";
    } else {
      let deja = parseInt(localStorage.getItem("grad_deja")||"0");
      if(heures > deja){
        let gain = heures - deja;
        solde += gain;
        localStorage.setItem("grad_solde", solde);
        localStorage.setItem("grad_deja", heures);
      }
      let reste = 24*3600000 - passe;
      let rh = Math.floor(reste/3600000);
      let rm = Math.floor((reste%3600000)/60000);
      document.getElementById("temps").innerText = rh+"h "+rm+"m restantes";
      document.getElementById("barre").style.width = (heures/24*100)+"%";
      document.getElementById("status").innerText="⛏️ En cours de minage : "+heures+"/24 GRAD gagnés";
      document.getElementById("boutonMiner").innerText="⛏️ EN COURS... "+rh+"h "+rm+"m";
      document.getElementById("boutonMiner").className="gray";
      document.getElementById("boutonMiner").disabled=true;
    }
  } else {
    document.getElementById("temps").innerText="24h 00m restantes";
    document.getElementById("barre").style.width="0%";
    document.getElementById("boutonMiner").disabled=false;
  }
}
function miner(){
  if(actif) return;
  debut = Date.now();
  actif=true;
  localStorage.setItem("grad_debut", debut);
  localStorage.setItem("grad_actif","1");
  localStorage.setItem("grad_deja","0");
  maj();
}
function tache(x){ solde+=x; localStorage.setItem("grad_solde", solde); maj(); alert("+"+x+" GRAD !"); }
setInterval(maj, 1000);
maj();
</script>
</body>
</html>
