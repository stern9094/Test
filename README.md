<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Total Verschnurrt – Raum 4</title>

<style>

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  min-height: 100vh;

  font-family: Arial, sans-serif;

  background: #f2f0e8;

  color: #222;
}

.app {
  width: 100%;
  max-width: 500px;

  margin: auto;

  padding: 16px;
}


/* =========================================
   TITEL
========================================= */

.header {
  text-align: center;

  margin-bottom: 16px;
}

.title {
  font-size: 30px;

  font-weight: 900;

  margin-bottom: 4px;
}

.subtitle {
  font-size: 16px;

  color: #555;
}


/* =========================================
   MENÜ
========================================= */

.menu {
  background: white;

  padding: 16px;

  border-radius: 15px;

  box-shadow: 0 2px 9px #0002;
}

label {
  display: block;

  font-weight: bold;

  margin: 10px 0 5px;
}

select,
button {
  width: 100%;

  font-size: 18px;

  padding: 12px;

  border-radius: 10px;
}

select {
  border: 1px solid #aaa;

  background: white;
}

button {
  margin-top: 14px;

  border: 0;

  background: #222;

  color: white;

  font-weight: bold;

  cursor: pointer;

  -webkit-tap-highlight-color: transparent;
}

button:active {
  transform: scale(.98);
}


/* =========================================
   INFO
========================================= */

.info {
  margin-top: 15px;

  padding: 13px;

  border-radius: 11px;

  background: #f0f0f0;

  text-align: center;

  font-size: 15px;

  line-height: 1.5;
}


/* =========================================
   SPIEL
========================================= */

.game {
  display: none;

  text-align: center;
}

.room {
  margin-top: 5px;

  font-size: 18px;

  font-weight: bold;
}

.difficulty {
  margin-top: 4px;

  font-size: 16px;

  color: #555;
}


/* =========================================
   AUFGABE
========================================= */

.task {
  background: white;

  padding: 15px;

  margin-top: 15px;

  border-radius: 15px;

  box-shadow: 0 2px 9px #0002;
}

.task-title {
  font-size: 18px;

  font-weight: bold;

  margin-bottom: 8px;
}

.task-text {
  font-size: 16px;

  line-height: 1.45;
}


/* =========================================
   TIMER
========================================= */

.timer-box {
  margin-top: 18px;

  padding: 20px;

  border-radius: 18px;

  background: #222;

  color: white;
}

.timer-label {
  font-size: 16px;

  opacity: .8;

  margin-bottom: 5px;
}

.timer {
  font-size: 76px;

  line-height: 1;

  font-weight: 900;

  font-variant-numeric: tabular-nums;
}

.timer.warning {
  color: #ffd400;
}

.timer.danger {
  color: #ff4b4b;
}


/* =========================================
   STATUS
========================================= */

.status {
  margin-top: 18px;

  padding: 18px;

  border-radius: 15px;

  background: white;

  font-size: 23px;

  font-weight: 900;

  line-height: 1.25;
}

.status.observe {
  color: #0645ad;
}

.status.transition {
  color: #a00000;
  background: #fff1f1;
}

.status.think {
  color: #c00000;
}

.status.finished {
  color: #16832c;
}


/* =========================================
   NUMMERN
========================================= */

.answer {
  display: none;

  margin-top: 18px;

  padding: 16px;

  background: white;

  border-radius: 15px;

  box-shadow: 0 2px 9px #0002;
}

.answer-title {
  font-size: 18px;

  font-weight: bold;

  margin-bottom: 12px;
}

.number-grid {
  display: grid;

  grid-template-columns:
    repeat(4, 1fr);

  gap: 8px;
}

.number-button {
  margin: 0;

  padding: 15px;

  font-size: 28px;

  background: #eee;

  color: #222;

  border: 2px solid #bbb;
}

.number-button:hover {
  background: #ddd;
}


/* =========================================
   AUFLÖSUNG
========================================= */

.result {
  display: none;

  margin-top: 18px;

  padding: 18px;

  background: white;

  border-radius: 15px;

  box-shadow: 0 2px 9px #0002;
}

.result-title {
  font-size: 30px;

  font-weight: 900;

  margin-bottom: 10px;
}

.result-text {
  font-size: 16px;

  line-height: 1.5;
}


/* =========================================
   NEUE RUNDE
========================================= */

.secondary {
  background: white;

  color: #222;

  border: 1px solid #888;
}


/* =========================================
   VOLLBILD-ÜBERGANG
========================================= */

.transition-screen {
  display: none;

  position: fixed;

  inset: 0;

  z-index: 1000;

  background: #a00000;

  color: white;

  align-items: center;

  justify-content: center;

  text-align: center;

  padding: 25px;
}

.transition-content {
  width: 100%;
}

.transition-title {
  font-size: 34px;

  font-weight: 900;

  line-height: 1.15;

  margin-bottom: 15px;
}

.transition-timer {
  font-size: 100px;

  font-weight: 900;

  line-height: 1;
}

.transition-subtitle {
  margin-top: 15px;

  font-size: 20px;
}


/* =========================================
   BERATUNG
========================================= */

.think-screen {
  display: none;

  position: fixed;

  inset: 0;

  z-index: 999;

  background: #222;

  color: white;

  align-items: center;

  justify-content: center;

  text-align: center;

  padding: 25px;
}

.think-content {
  width: 100%;
}

.think-title {
  font-size: 35px;

  font-weight: 900;

  line-height: 1.15;

  margin-bottom: 20px;
}

.think-timer {
  font-size: 110px;

  font-weight: 900;

  line-height: 1;
}

.think-subtitle {
  font-size: 20px;

  margin-top: 18px;
}

</style>
</head>


<body>


<div class="app">


<!-- =====================================
     HEADER
===================================== -->

<div class="header">

  <div class="title">
    TOTAL VERSCHNURRT
  </div>

  <div class="subtitle">
    Raum 4 – Farbschnüre
  </div>

</div>


<!-- =====================================
     MENÜ
===================================== -->

<div
  class="menu"
  id="menu">

  <label for="difficulty">
    Schwierigkeitsstufe
  </label>

  <select id="difficulty">

    <option value="easy">
      🟢 Leicht
    </option>

    <option value="medium">
      🟡 Mittel
    </option>

    <option value="hard">
      🟠 Schwer
    </option>

    <option value="veryhard">
      🔴 Sehr schwer
    </option>

  </select>


  <div
    class="info"
    id="info">
  </div>


  <button
    type="button"
    id="startButton">

    Runde starten

  </button>

</div>


<!-- =====================================
     SPIEL
===================================== -->

<div
  class="game"
  id="game">


  <div class="room">
    RAUM 4
  </div>


  <div
    class="difficulty"
    id="gameDifficulty">
  </div>


  <div class="task">

    <div class="task-title">
      Eure Aufgabe
    </div>

    <div
      class="task-text"
      id="taskText">
    </div>

  </div>


  <!-- TIMER -->

  <div class="timer-box">

    <div
      class="timer-label"
      id="timerLabel">

      Schnüre verfolgen

    </div>

    <div
      class="timer"
      id="timer">

      0

    </div>

  </div>


  <div
    class="status observe"
    id="status">

    Bereit

  </div>


  <!-- ANTWORT -->

  <div
    class="answer"
    id="answer">

    <div class="answer-title">

      Welche Nummer wählt ihr gemeinsam?

    </div>


    <div class="number-grid">

      <button
        class="number-button"
        data-number="1">

        1

      </button>


      <button
        class="number-button"
        data-number="2">

        2

      </button>


      <button
        class="number-button"
        data-number="3">

        3

      </button>


      <button
        class="number-button"
        data-number="4">

        4

      </button>

    </div>

  </div>


  <!-- ERGEBNIS -->

  <div
    class="result"
    id="result">

    <div
      class="result-title"
      id="resultTitle">

    </div>


    <div
      class="result-text"
      id="resultText">

    </div>


    <button
      type="button"
      class="secondary"
      id="newRoundButton">

      Neue Runde

    </button>

  </div>

</div>


</div>


<!-- =====================================
     5 SEKUNDEN ÜBERGANG
===================================== -->

<div
  class="transition-screen"
  id="transitionScreen">

  <div class="transition-content">

    <div class="transition-title">

      GEMEINSAMES<br>
      ÜBERLEGEN &amp; EINSPRUCH

    </div>


    <div
      class="transition-timer"
      id="transitionTimer">

      5

    </div>


    <div class="transition-subtitle">

      Die Verfolgungszeit ist abgelaufen.

    </div>

  </div>

</div>


<!-- =====================================
     BERATUNG
===================================== -->

<div
  class="think-screen"
  id="thinkScreen">

  <div class="think-content">

    <div class="think-title">

      GEMEINSAM<br>
      ÜBERLEGEN &amp; EINSPRUCH

    </div>


    <div
      class="think-timer"
      id="thinkTimer">

      25

    </div>


    <div class="think-subtitle">

      Einigt euch auf eure gemeinsame Antwort.

      <br><br>

      Ein Einspruch muss jetzt erfolgen.

    </div>

  </div>

</div>


<script>


/* =========================================
   SCHWIERIGKEITEN
========================================= */

const difficultyData = {

  easy: {

    name: "Leicht",

    icon: "🟢",

    strings: 1,

    observe: 15,

    think: 25

  },

  medium: {

    name: "Mittel",

    icon: "🟡",

    strings: 2,

    observe: 20,

    think: 25

  },

  hard: {

    name: "Schwer",

    icon: "🟠",

    strings: 3,

    observe: 30,

    think: 25

  },

  veryhard: {

    name: "Sehr schwer",

    icon: "🔴",

    strings: 4,

    observe: 25,

    think: 30

  }

};


/* =========================================
   ELEMENTE
========================================= */

const menu =
  document.getElementById(
    "menu"
  );

const game =
  document.getElementById(
    "game"
  );

const difficulty =
  document.getElementById(
    "difficulty"
  );

const info =
  document.getElementById(
    "info"
  );

const startButton =
  document.getElementById(
    "startButton"
  );

const gameDifficulty =
  document.getElementById(
    "gameDifficulty"
  );

const taskText =
  document.getElementById(
    "taskText"
  );

const timer =
  document.getElementById(
    "timer"
  );

const timerLabel =
  document.getElementById(
    "timerLabel"
  );

const status =
  document.getElementById(
    "status"
  );

const answer =
  document.getElementById(
    "answer"
  );

const result =
  document.getElementById(
    "result"
  );

const resultTitle =
  document.getElementById(
    "resultTitle"
  );

const resultText =
  document.getElementById(
    "resultText"
  );

const newRoundButton =
  document.getElementById(
    "newRoundButton"
  );

const transitionScreen =
  document.getElementById(
    "transitionScreen"
  );

const transitionTimer =
  document.getElementById(
    "transitionTimer"
  );

const thinkScreen =
  document.getElementById(
    "thinkScreen"
  );

const thinkTimer =
  document.getElementById(
    "thinkTimer"
  );


/* =========================================
   VARIABLEN
========================================= */

let interval = null;

let transitionInterval = null;

let currentPhase = "idle";

let selectedNumber = null;


/* =========================================
   INFO
========================================= */

function updateInfo() {

  const data =
    difficultyData[
      difficulty.value
    ];


  info.innerHTML =

    "<strong>" +

    data.icon +
    " " +
    data.name +

    "</strong><br><br>" +

    "<strong>" +
    data.strings +
    "</strong> " +

    (
      data.strings === 1
        ? "Schnur"
        : "Schnüre"
    ) +

    " verfolgen: <strong>" +

    data.observe +

    " Sekunden</strong><br>" +

    "Danach: <strong>5 Sekunden</strong> Übergang<br>" +

    "Überlegen &amp; Einspruch: <strong>" +

    data.think +

    " Sekunden</strong>";

}


difficulty.addEventListener(
  "change",
  updateInfo
);


/* =========================================
   TON
========================================= */

function beep(
  frequency = 700,
  duration = 300
) {

  /*
     Der Ton wird direkt über den
     Browser erzeugt.

     Dadurch wird keine Audiodatei
     benötigt.
  */

  try {

    const AudioContext =
      window.AudioContext ||
      window.webkitAudioContext;

    const context =
      new AudioContext();

    const oscillator =
      context.createOscillator();

    const gain =
      context.createGain();


    oscillator.type =
      "sine";

    oscillator.frequency.value =
      frequency;


    gain.gain.setValueAtTime(
      0.0001,
      context.currentTime
    );

    gain.gain.exponentialRampToValueAtTime(
      0.35,
      context.currentTime + 0.02
    );

    gain.gain.exponentialRampToValueAtTime(
      0.0001,
      context.currentTime +
      duration / 1000
    );


    oscillator.connect(
      gain
    );

    gain.connect(
      context.destination
    );


    oscillator.start();

    oscillator.stop(
      context.currentTime +
      duration / 1000
    );

  }

  catch (error) {

    console.log(
      "Ton konnte nicht abgespielt werden."
    );

  }

}


/* =========================================
   TIMER STOPPEN
========================================= */

function stopTimer() {

  clearInterval(
    interval
  );

  interval =
    null;

}


/* =========================================
   BEOBACHTUNGS-TIMER
========================================= */

function startObservationTimer(
  seconds
) {

  stopTimer();

  let time =
    seconds;


  timerLabel.textContent =
    "SCHNÜRE VERFOLGEN";

  timer.textContent =
    time;

  timer.className =
    "timer";


  currentPhase =
    "observation";


  status.className =
    "status observe";

  status.textContent =
    "Nur mit den Augen verfolgen!";


  interval =
    setInterval(
      function() {

        time--;

        timer.textContent =
          time;


        if (
          time <= 5
        ) {

          timer.classList.add(
            "danger"
          );

        }


        if (
          time <= 0
        ) {

          stopTimer();

          beep(
            500,
            250
          );

          startTransition();

        }

      },
      1000
    );

}


/* =========================================
   5 SEKUNDEN ÜBERGANG
========================================= */

function startTransition() {

  currentPhase =
    "transition";


  transitionScreen.style.display =
    "flex";


  let time =
    5;


  transitionTimer.textContent =
    time;


  clearInterval(
    transitionInterval
  );


  transitionInterval =
    setInterval(
      function() {

        time--;

        transitionTimer.textContent =
          time;


        if (
          time <= 0
        ) {

          clearInterval(
            transitionInterval
          );

          transitionInterval =
            null;


          beep(
            900,
            200
          );


          transitionScreen.style.display =
            "none";


          startThinkingPhase();

        }

      },
      1000
    );

}


/* =========================================
   DENKPHASE
========================================= */

function startThinkingPhase() {

  const data =
    difficultyData[
      difficulty.value
    ];


  currentPhase =
    "thinking";


  thinkScreen.style.display =
    "flex";


  let time =
    data.think;


  thinkTimer.textContent =
    time;


  status.className =
    "status think";

  status.textContent =
    "Gemeinsam überlegen und Einspruch";


  clearInterval(
    interval
  );


  interval =
    setInterval(
      function() {

        time--;

        thinkTimer.textContent =
          time;


        if (
          time <= 5
        ) {

          thinkTimer.style.color =
            "#ff4b4b";

        }


        if (
          time <= 0
        ) {

          stopTimer();

          thinkTimer.style.color =
            "white";


          beep(
            350,
            500
          );


          thinkScreen.style.display =
            "none";


          endThinkingPhase();

        }

      },
      1000
    );

}


/* =========================================
   DENKPHASE ENDE
========================================= */

function endThinkingPhase() {

  currentPhase =
    "finished";


  timerLabel.textContent =
    "ZEIT ABGELAUFEN";


  timer.textContent =
    "0";


  status.className =
    "status finished";


  status.textContent =
    "Entscheidung steht fest!";


  answer.style.display =
    "block";


  result.style.display =
    "block";


  resultTitle.textContent =
    "JETZT ENTSCHEIDEN";


  resultText.innerHTML =

    "Nennt jetzt eure gemeinsame " +

    "Nummer und führt anschließend " +

    "die Auflösung durch.";

}


/* =========================================
   NUMMER AUSWÄHLEN
========================================= */

document
  .querySelectorAll(
    ".number-button"
  )
  .forEach(
    function(button) {

      button.addEventListener(
        "click",
        function() {

          if (
            currentPhase !==
            "finished"
          ) {

            return;

          }


          selectedNumber =
            button.dataset.number;


          answer.style.display =
            "none";


          resultTitle.textContent =
            "GEMEINSAME ANTWORT";


          resultText.innerHTML =

            "Die Gruppe hat sich " +

            "für Öffnung <strong>" +

            selectedNumber +

            "</strong> entschieden." +

            "<br><br>" +

            "Jetzt darf die Box geöffnet " +

            "und die tatsächliche Position " +

            "der Kugel überprüft werden.";


          status.textContent =
            "Antwort: Öffnung " +
            selectedNumber;


          beep(
            800,
            180
          );

        }
      );

    }
  );


/* =========================================
   RUNDE STARTEN
========================================= */

startButton.addEventListener(
  "click",
  function() {

    const data =
      difficultyData[
        difficulty.value
      ];


    menu.style.display =
      "none";

    game.style.display =
      "block";

    result.style.display =
      "none";

    answer.style.display =
      "none";


    gameDifficulty.textContent =

      data.icon +
      " " +
      data.name;


    taskText.innerHTML =

      "Verfolgt " +

      "<strong>" +

      data.strings +

      "</strong> " +

      (
        data.strings === 1
          ? "Schnur"
          : "Schnüre"
      ) +

      " von der farbigen Leiste " +

      "bis zur Box.<br><br>" +

      "Merkt euch die Nummer der " +

      "Öffnung, an der die gesuchte " +

      "Kugel endet.";


    selectedNumber =
      null;


    transitionScreen.style.display =
      "none";

    thinkScreen.style.display =
      "none";


    startObservationTimer(
      data.observe
    );

  }
);


/* =========================================
   NEUE RUNDE
========================================= */

newRoundButton.addEventListener(
  "click",
  function() {

    stopTimer();

    clearInterval(
      transitionInterval
    );


    transitionInterval =
      null;


    transitionScreen.style.display =
      "none";

    thinkScreen.style.display =
      "none";


    currentPhase =
      "idle";


    game.style.display =
      "none";

    menu.style.display =
      "block";


    result.style.display =
      "none";

    answer.style.display =
      "none";


    timer.textContent =
      "0";

    status.textContent =
      "Bereit";

  }
);


/* =========================================
   STARTZUSTAND
========================================= */

updateInfo();

</script>

</body>
</html>
