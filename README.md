<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BALANCE-TEAM</title>
<style>
*{box-sizing:border-box}
body{
  margin:0;font-family:Arial,Helvetica,sans-serif;
  background:linear-gradient(135deg,#eef4f7,#dce8ed);
  color:#17252b;min-height:100vh;
}
.app{max-width:1050px;margin:auto;padding:18px}
.screen{
  background:white;border-radius:22px;padding:24px;
  box-shadow:0 10px 35px #0002;min-height:calc(100vh - 36px);
}
h1{font-size:clamp(34px,7vw,62px);margin:5px 0 8px;text-align:center;letter-spacing:1px}
h2{text-align:center;margin:8px 0 20px}
.subtitle{text-align:center;font-size:20px;margin-bottom:25px}
button{
  border:0;border-radius:14px;padding:14px 20px;font-size:18px;
  font-weight:bold;cursor:pointer;background:#176b87;color:white;
  box-shadow:0 4px 0 #0e4b60;transition:.12s;
}
button:active{transform:translateY(3px);box-shadow:0 1px 0 #0e4b60}
button.secondary{background:#e9eef0;color:#17252b;box-shadow:0 4px 0 #b7c5ca}
button.danger{background:#c93d3d;box-shadow:0 4px 0 #8e2828}
.levels{display:grid;grid-template-columns:repeat(4,1fr);gap:14px;margin:25px 0}
.level{
  background:#f3f7f8;border:3px solid transparent;border-radius:18px;
  padding:18px;text-align:center;cursor:pointer
}
.level:hover{border-color:#176b87}
.level strong{display:block;font-size:24px;margin-bottom:8px}
.level span{display:block;margin:5px}
.rules{background:#f5f8f9;border-radius:18px;padding:18px;line-height:1.5}
.rules h3{margin-top:0}
.center{text-align:center}
.hidden{display:none!important}

.topbar{display:flex;justify-content:space-between;gap:12px;align-items:center;flex-wrap:wrap}
.badge{background:#edf3f5;border-radius:12px;padding:9px 13px;font-weight:bold}
.timer{font-size:30px;font-weight:bold;min-width:100px;text-align:center}
.timer.warning{color:#c83b3b}
.progress{height:12px;background:#e3eaed;border-radius:10px;overflow:hidden;margin:12px 0 20px}
.progress>div{height:100%;background:#176b87;width:0;transition:.2s}

.game-layout{display:grid;grid-template-columns:minmax(300px,560px) minmax(240px,1fr);gap:24px;align-items:start}
.board-wrap{background:#e9eef0;padding:18px;border-radius:20px}
.board{
  display:grid;grid-template-columns:repeat(5,1fr);gap:8px;
  max-width:540px;margin:auto;
}
.cell{
  aspect-ratio:1;border-radius:12px;background:#d5dfe2;
  display:flex;align-items:center;justify-content:center;
  font-weight:bold;font-size:18px;position:relative;border:2px solid #b8c8cd;
}
.cell.path{background:#f8fbfc}
.cell.current{background:#176b87;color:white;border-color:#0e4b60}
.cell.start,.cell.goal{background:#d7ead8;border-color:#7caf80}
.cell.weight{background:#fff0bd;border-color:#d9b84b}
.marker{position:absolute;inset:12%;border-radius:50%;background:#f2f2f2;
  border:5px solid #333;display:flex;align-items:center;justify-content:center;
  font-size:clamp(18px,4vw,28px);z-index:3}
.panel{background:#f5f8f9;border-radius:20px;padding:20px}
.panel h3{margin-top:0}
.weights{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin:12px 0}
.weight-btn{background:#fff;border:3px solid #c4d0d4;color:#17252b;box-shadow:none;padding:12px 5px}
.weight-btn.selected{border-color:#176b87;background:#dceff5}
.positions{display:grid;grid-template-columns:repeat(5,1fr);gap:6px}
.pos-btn{padding:8px 4px;font-size:14px;background:#fff;color:#17252b;border:2px solid #c4d0d4;box-shadow:none}
.pos-btn.selected{background:#dceff5;border-color:#176b87}
.feedback{margin-top:14px;border-radius:14px;padding:14px;font-weight:bold;line-height:1.4}
.feedback.ok{background:#dcefdc;color:#235d29}
.feedback.bad{background:#fde0e0;color:#812525}
.available{font-size:14px;color:#53666d}
.weight-on-board{position:absolute;bottom:4px;right:5px;background:#fff4bd;border-radius:7px;padding:2px 5px;font-size:11px;border:1px solid #d1b24e}

.result{padding:30px;text-align:center}
.big-result{font-size:70px}
@media(max-width:760px){
 .screen{padding:16px}
 .levels{grid-template-columns:repeat(2,1fr)}
 .game-layout{grid-template-columns:1fr}
 .panel{padding:15px}
 .board{gap:5px}
 .cell{font-size:13px}
}
</style>
</head>
<body>
<div class="app">
<div class="screen">

<section id="home">
  <h1>BALANCE-TEAM</h1>
  <div class="subtitle"><b>Gemeinsam ans Ziel – bevor die Zeit abläuft!</b></div>
  <div class="rules">
    <h3>So funktioniert es</h3>
    <p>Ihr seid ein Team und steuert gemeinsam eine einzige Spielfigur.</p>
    <p>Bei jedem neuen Feld verändert sich die Belastung. Ihr müsst ein verfügbares Gewicht an die richtige Position legen.</p>
    <p><b>Nur wenn das Gleichgewicht stimmt, darf eure Figur weitergehen.</b></p>
    <p>Richtig eingesetzte Gewichte bleiben liegen. Die Zeit läuft für das gesamte Team.</p>
  </div>
  <h2>Spielerzahl wählen</h2>
  <div class="levels" id="playerLevels">
    <div class="level" onclick="setPlayers(2)" id="players2"><strong>2 Spieler</strong><span>Figur: 2 kg</span></div>
    <div class="level" onclick="setPlayers(3)" id="players3"><strong>3 Spieler</strong><span>Figur: 3 kg</span></div>
    <div class="level" onclick="setPlayers(4)" id="players4"><strong>4 Spieler</strong><span>Figur: 4 kg</span></div>
    <div class="level" onclick="setPlayers(5)" id="players5"><strong>5 Spieler</strong><span>Figur: 5 kg</span></div>
  </div>
  <div class="center" id="playerInfo">Aktuell gewählt: <b>2 Spieler – Figur 2 kg</b></div>

  <h2>Schwierigkeitsstufe wählen</h2>
  <div class="levels">
    <div class="level" onclick="startGame('Leicht')"><strong>Leicht</strong><span>80 Sekunden</span><span>6 Gewichte</span></div>
    <div class="level" onclick="startGame('Mittel')"><strong>Mittel</strong><span>70 Sekunden</span><span>7 Gewichte</span></div>
    <div class="level" onclick="startGame('Schwer')"><strong>Schwer</strong><span>60 Sekunden</span><span>8 Gewichte</span></div>
    <div class="level" onclick="startGame('Sehr schwer')"><strong>Sehr schwer</strong><span>50 Sekunden</span><span>10 Gewichte</span></div>
  </div>
  <div class="center"><button class="secondary" onclick="showRules()">Spielregeln anzeigen</button></div>
</section>

<section id="rules" class="hidden">
  <h1>Spielregeln</h1>
  <div class="rules">
    <p><b>1.</b> Ihr seid ein Team und habt nur eine Figur.</p>
    <p><b>2.</b> Die Figur bewegt sich Feld für Feld vom START zum ZIEL.</p>
    <p><b>3.</b> Nach jedem Schritt muss ein Gewicht eingesetzt werden.</p>
    <p><b>4.</b> Ihr wählt Gewicht und Position.</p>
    <p><b>5.</b> Ist die Lösung richtig, bleibt das Gewicht liegen und ihr dürft weiter.</p>
    <p><b>6.</b> Ist sie falsch, bleibt die Figur stehen. Die Zeit läuft weiter.</p>
    <p><b>7.</b> Ein eingesetztes Gewicht steht später nicht mehr zur Verfügung.</p>
    <p><b>8.</b> Das Spiel berücksichtigt Gewicht und Abstand vom Mittelpunkt.</p>
  </div>
  <p class="center"><button onclick="backHome()">Zurück</button></p>
</section>

<section id="game" class="hidden">
  <div class="topbar">
    <div class="badge" id="levelLabel"></div>
    <div class="badge">Spieler: <span id="playersLabel">2</span></div>
    <div class="badge">Figur: <span id="figureWeightLabel">2 kg</span></div>
    <div class="badge">Schritt <span id="stepLabel"></span></div>
    <div class="timer" id="timer">80</div>
    <button class="danger" onclick="backHome()">Beenden</button>
  </div>
  <div class="progress"><div id="progress"></div></div>

  <div class="game-layout">
    <div class="board-wrap">
      <div class="board" id="board"></div>
    </div>
    <div class="panel">
      <h3 id="instruction">Zum nächsten Feld!</h3>
      <p class="badge" style="display:inline-block">Eure Figur belastet das Spielfeld mit <b><span id="figureWeightInline">2 kg</span></b>.</p>
      <p id="taskText">Bewegt eure Figur auf das markierte Feld. Danach wird ein Ausgleich benötigt.</p>

      <h3>1. Gewicht wählen</h3>
      <div class="weights" id="weights"></div>
      <div class="available" id="available"></div>

      <h3>2. Position wählen</h3>
      <div class="positions" id="positions"></div>

      <button id="checkBtn" style="width:100%;margin-top:14px" onclick="checkAnswer()">Gleichgewicht prüfen</button>
      <div id="feedback"></div>
    </div>
  </div>
</section>

<section id="result" class="hidden result">
  <div class="big-result" id="resultIcon">✓</div>
  <h1 id="resultTitle">GESCHAFFT!</h1>
  <p id="resultText"></p>
  <button onclick="newGame()">Neue Runde</button>
  <button class="secondary" onclick="backHome()" style="margin-left:8px">Hauptmenü</button>
</section>

</div>
</div>

<script>
const settings={
  "Leicht":{time:80,count:6,weights:[1,2,3,4,5,6]},
  "Mittel":{time:70,count:7,weights:[1,2,3,4,5,6,7]},
  "Schwer":{time:60,count:8,weights:[1,2,3,4,5,6,7,8]},
  "Sehr schwer":{time:50,count:10,weights:[1,2,3,4,5,6,7,8,9,10]}
};

let level, timeLeft, timerId, step=0, maxSteps=8;
let available=[], placed=[], selectedWeight=null, selectedPos=null;
let solutions=[];

function show(id){
  ["home","rules","game","result"].forEach(x=>document.getElementById(x).classList.add("hidden"));
  document.getElementById(id).classList.remove("hidden");
}
function showRules(){show("rules")}
function backHome(){clearInterval(timerId);show("home")}
function newGame(){startGame(level)}

function generateSolutions(){
  // Jede Runde erhält neue, vorab berechnete Lösungen.
  // Positionen 0–24 entsprechen dem 5x5-Raster.
  solutions=[];
  let usedPositions=new Set();
  for(let i=0;i<maxSteps;i++){
    let w=available[Math.floor(Math.random()*available.length)];
    let p;
    do{p=Math.floor(Math.random()*25)}while(usedPositions.has(p));
    usedPositions.add(p);
    solutions.push({w,p});
  }
}

function startGame(lvl){
  level=lvl; step=0; placed=[]; selectedWeight=null; selectedPos=null;
  const s=settings[lvl]; timeLeft=s.time;
  available=[...s.weights];
  generateSolutions();
  document.getElementById("levelLabel").textContent=lvl;
  document.getElementById("playersLabel").textContent=players;
  document.getElementById("figureWeightLabel").textContent=figureWeight+" kg";
  document.getElementById("figureWeightInline").textContent=figureWeight+" kg";
  show("game"); render();
  clearInterval(timerId);
  timerId=setInterval(()=>{
    timeLeft--; updateTimer();
    if(timeLeft<=0) lose("Die Zeit ist abgelaufen. Ihr wart fast da!");
  },1000);
}
function updateTimer(){
  const t=document.getElementById("timer");
  t.textContent=timeLeft;
  t.classList.toggle("warning",timeLeft<=15);
}
function render(){
  updateTimer();
  document.getElementById("stepLabel").textContent=(step+1)+" / "+maxSteps;
  document.getElementById("progress").style.width=(step/maxSteps*100)+"%";
  renderBoard(); renderWeights(); renderPositions();
  document.getElementById("feedback").innerHTML="";
  document.getElementById("instruction").textContent="Gleichgewicht herstellen";
  document.getElementById("taskText").innerHTML="Wählt gemeinsam <b>ein verfügbares Gewicht</b> und anschließend die Position, an der es liegen soll.";
}
function renderBoard(){
  const board=document.getElementById("board"); board.innerHTML="";
  for(let i=0;i<25;i++){
    const c=document.createElement("div"); c.className="cell path";
    if(i===0){c.classList.add("start");c.textContent="START"}
    else if(i===24){c.classList.add("goal");c.textContent="ZIEL"}
    else c.textContent=i+1;
    if(i===step+1)c.classList.add("current");
    const pp=placed.find(x=>x.p===i);
    if(pp){
      const m=document.createElement("div");m.className="marker";m.textContent=pp.w+" kg";
      c.appendChild(m);
      const small=document.createElement("span");small.className="weight-on-board";small.textContent="gesetzt";
      c.appendChild(small);
    }
    board.appendChild(c);
  }
}
function renderWeights(){
  const box=document.getElementById("weights");box.innerHTML="";
  available.forEach(w=>{
    const b=document.createElement("button");b.className="weight-btn"+(selectedWeight===w?" selected":"");
    b.textContent=w+" kg";b.onclick=()=>{selectedWeight=w;renderWeights()};
    box.appendChild(b);
  });
  document.getElementById("available").textContent=available.length+" Gewichte verfügbar";
}
function renderPositions(){
  const box=document.getElementById("positions");box.innerHTML="";
  for(let p=0;p<25;p++){
    if(placed.some(x=>x.p===p))continue;
    const b=document.createElement("button");b.className="pos-btn"+(selectedPos===p?" selected":"");
    b.textContent=(p===0?"S":p===24?"Z":p+1);
    b.onclick=()=>{selectedPos=p;renderPositions()};
    box.appendChild(b);
  }
}
function checkAnswer(){
  if(selectedWeight===null||selectedPos===null){
    setFeedback("Bitte zuerst Gewicht und Position auswählen.","bad");return;
  }
  const sol=solutions[step];
  if(selectedWeight===sol.w && selectedPos===sol.p){
    placed.push({w:selectedWeight,p:selectedPos});
    available=available.filter(x=>x!==selectedWeight);
    step++;
    selectedWeight=null;selectedPos=null;
    if(step>=maxSteps){win();return}
    setFeedback("✓ Gleichgewicht hergestellt! Das Gewicht bleibt liegen.","ok");
    setTimeout(render,650);
  }else{
    setFeedback("✕ Noch nicht im Gleichgewicht. Probiert eine andere Kombination. Die Zeit läuft weiter.","bad");
  }
}
function setFeedback(txt,cls){
  const f=document.getElementById("feedback");f.className="feedback "+cls;f.textContent=txt;
}
function win(){
  clearInterval(timerId);
  document.getElementById("resultIcon").textContent="✓";
  document.getElementById("resultTitle").textContent="GESCHAFFT!";
  document.getElementById("resultText").innerHTML="Ihr habt das Ziel als Team erreicht.<br><b>Team: "+players+" Spieler · Figur: "+figureWeight+" kg · Restzeit: "+timeLeft+" Sekunden</b>";
  show("result");
}
function lose(msg){
  clearInterval(timerId);
  document.getElementById("resultIcon").textContent="⏱";
  document.getElementById("resultTitle").textContent="ZEIT ABGELAUFEN";
  document.getElementById("resultText").textContent=msg;
  show("result");
}
setPlayers(2);
</script>
</body>
</html>
