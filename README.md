<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Haus der Rätsel – Vorschau</title>
<style>
*{box-sizing:border-box}
body{
  margin:0; min-height:100vh; display:flex; align-items:center; justify-content:center;
  background:#202124; font-family:Arial,sans-serif; color:#fff;
}
.phone{
  width:390px; max-width:100vw; min-height:780px; background:#111;
  border:10px solid #333; border-radius:35px; overflow:hidden;
  box-shadow:0 15px 50px #000;
}
.screen{
  min-height:760px; padding:28px 20px; display:flex; flex-direction:column;
  align-items:center; justify-content:center; text-align:center;
  background:linear-gradient(160deg,#172033,#0d111b 65%,#24152a);
}
h1{font-size:36px;margin:0 0 22px}
h2{font-size:25px;margin:0 0 18px}
p{line-height:1.5;color:#ddd}
button,select{
  width:100%; padding:14px; margin:7px 0; border:0; border-radius:12px;
  font-size:17px;
}
button{background:#d6a84f;color:#111;font-weight:bold;cursor:pointer}
button.locked{background:#333;color:#777;cursor:default}
.card{
  width:100%; background:#1d2635; border:1px solid #3a465a;
  border-radius:16px; padding:18px; margin:8px 0;
}
label{display:block;text-align:left;margin-top:10px;color:#ccc}
.keypad{
  width:100%; max-width:300px; display:grid; grid-template-columns:repeat(3,1fr);
  gap:9px; margin:15px auto;
}
.key{
  height:58px; margin:0; background:#293447;color:#fff;
  border:1px solid #536078;font-size:22px;
  transition: background 0.15s, box-shadow 0.15s, color 0.15s;
}
.key.active{background:#d6a84f;color:#111;box-shadow:0 0 18px #d6a84f}
.key.wrong{background:#c0392b;color:#fff;box-shadow:0 0 18px #c0392b}
.key.zero{grid-column:2}
.message{min-height:52px;color:#ddd;font-size:16px}
.code{font-size:30px;letter-spacing:8px;min-height:40px}
.back{background:#555;color:#fff}
.small{font-size:13px;color:#999;margin-top:18px}
</style>
</head>
<body>
<div class="phone"><div id="app" class="screen"></div></div>
<script>
const app=document.getElementById("app");
let difficulty="Leicht", players=2, level=0, attempt=0, code="", input="";
let timer, pos=0;
const levels=[
  "Der Eingang","Raum 1 – Stühle","Raum 2 – Total verschnurrt",
  "Raum 3","Raum 4 – Spinnen","Raum 5 – Die Flucht über den geheimen Weg"
];
function page1(){
 app.innerHTML=`<h1>Haus der Rätsel</h1><p>Ein geheimnisvolles Haus wartet auf euch.<br>Findet den Weg hinein und löst Raum für Raum die Rätsel.</p><div class="small">Das Spiel startet gleich …</div>`;
 setTimeout(page2,5000);
}
function page2(){
 app.innerHTML=`<h2>Einstellungen</h2>
 <div class="card"><label>Schwierigkeit</label>
 <select id="diff">
 <option>Leicht</option><option>Mittel</option><option>Schwer</option><option>Sehr schwer</option>
 </select>
 <label>Spieleranzahl</label>
 <select id="players"><option>2</option><option>3</option><option>4</option><option>5</option></select>
 </div>
 <button onclick="saveSettings()">Weiter</button>`;
}
function saveSettings(){
 difficulty=document.getElementById("diff").value;
 players=+document.getElementById("players").value;
 page3();
}
function page3(){
 app.innerHTML=`<h2>Haus der Rätsel</h2><p>Wählt euren nächsten Raum.</p>
 ${levels.map((x,i)=>i===0||i<=level?
 `<button onclick="startLevel(${i})">${x}</button>`:
 `<button class="locked">🔒 ${x}</button>`).join("")}`;
}
function startLevel(i){
 if(i===0){page4();return;}
 alert("Dieser Raum ist in der Vorschau noch nicht programmiert.");
}
function page4(){
 app.innerHTML=`<h1>KNACK DEN CODE!</h1><div class="small">Der Eingang</div>`;
 setTimeout(page5,5000);
}
function page5(){
 app.innerHTML=`<h2>Der erste Schritt ins Haus</h2>
 <p>Vor euch steht eine verschlossene Tür. Ein Zahlen-Schloss versperrt den Eingang.</p>
 <p>Merkt euch die Zahlenfolge und gebt sie anschließend richtig ein.</p>
 <button onclick="startMemory()">Seid ihr bereit? Los geht's!</button>`;
}
function settings(){
 if(difficulty==="Leicht") return [5,800,false];
 if(difficulty==="Mittel") return [6,700,false];
 if(difficulty==="Schwer") return [7,600,true];
 return [8,500,true];
}
function makeCode(){
 const [n,ms,dup]=settings(); let a=[];
 while(a.length<n){
   let d=Math.floor(Math.random()*10);
   if(dup||!a.includes(d)) a.push(d);
 }
 return a.join("");
}
function keypad(disabled=false){
 return `<div class="keypad">
 ${[7,8,9,4,5,6,1,2,3,0].map(n=>
 `<button class="key ${n===0?'zero':''}" data-n="${n}" ${disabled?'disabled':''} onclick="press(${n})">${n}</button>`).join("")}
 </div>`;
}
function highlightKey(n, className="active", duration=450){
  const btn=document.querySelector(`.key[data-n="${n}"]`);
  if(!btn) return;
  btn.classList.add(className);
  setTimeout(()=>btn.classList.remove(className), duration);
}
function startMemory(){
 code=makeCode(); input=""; attempt=0; pos=0;
 showMemory(0);
}
function showMemory(i){
 const [n,ms]=settings();
 app.innerHTML=`<h2>Zahlen merken</h2>
 <p>Merkt euch die Zahlenfolge.</p>
 <div class="code"> </div>
 ${keypad(true)}
 <div class="message">Zahl ${i+1} von ${n}</div>`;
 // kurz aufleuchten lassen
 setTimeout(()=>highlightKey(code[i], "active", ms-80), 30);
 if(i<n-1) timer=setTimeout(()=>showMemory(i+1), ms);
 else timer=setTimeout(showInput, ms);
}
function showInput(){
 input=""; pos=0;
 app.innerHTML=`<h2>Zahlen eingeben</h2>
 <div class="code">${input||"–"}</div>
 ${keypad()}
 <div class="message">Tippe jetzt die richtige Kombination ein.<br>Du hast 2 Versuche.</div>
 <button class="back" onclick="page2()">Zurück</button>`;
}
function press(n){
  if(pos>=code.length) return;

  // richtige Taste?
  if(String(n)===code[pos]){
    input+=n;
    pos++;
    highlightKey(n, "active", 280);
    // Anzeige aktualisieren
    document.querySelector(".code").textContent=input;
    if(pos===code.length){
      // komplett richtig
      level=1;
      setTimeout(()=>{
        app.innerHTML=`<h2>Du hast es geschafft!</h2><p>Die Tür geht auf.</p>
        <button onclick="page3()">Weiter</button>`;
      }, 350);
    }
  } else {
    // falsche Taste → rot aufleuchten
    highlightKey(n, "wrong", 600);
    attempt++;
    if(attempt===1){
      // noch ein Versuch
      setTimeout(()=>{
        app.innerHTML=`<h2>Falsch!</h2><p>Du hast nur noch einen Versuch.</p>
        <button onclick="showInput()">Noch einmal</button>`;
      }, 650);
    } else {
      // zwei Fehler → Code anzeigen
      setTimeout(()=>{
        app.innerHTML=`<h2>Leider falsch.</h2>
        <p>Die richtige Kombination war:</p>
        <div class="code">${code}</div>
        <p>Ihr müsst von vorne beginnen.</p>
        <button onclick="page2()">Zurück zu den Einstellungen</button>`;
      }, 650);
    }
  }
}
page1();
</script>
</body>
</html>
