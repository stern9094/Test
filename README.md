<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>3 x 3 Vibrationsspiel</title>

<style>
    * {
        box-sizing: border-box;
    }

    body {
        margin: 0;
        min-height: 100vh;
        font-family: Arial, sans-serif;
        background: #eeeeee;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .game {
        width: min(92vw, 420px);
        text-align: center;
    }

    h1 {
        margin-bottom: 10px;
    }

    #status {
        min-height: 30px;
        font-size: 20px;
        font-weight: bold;
        margin-bottom: 15px;
    }

    .board {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 10px;
    }

    .field {
        aspect-ratio: 1 / 1;
        border: 3px solid #333;
        border-radius: 15px;
        background: white;
        font-size: 30px;
        font-weight: bold;
        cursor: pointer;
        touch-action: manipulation;
        -webkit-tap-highlight-color: transparent;
    }

    .field:active {
        transform: scale(0.94);
    }

    .wrong {
        background: #e53935 !important;
        animation: shake 0.12s linear 5;
    }

    .correct {
        background: #43a047 !important;
        color: white;
    }

    @keyframes shake {
        0%   { transform: translateX(0); }
        25%  { transform: translateX(-8px); }
        50%  { transform: translateX(8px); }
        75%  { transform: translateX(-8px); }
        100% { transform: translateX(0); }
    }

    button#test {
        margin-top: 20px;
        padding: 14px 20px;
        border: none;
        border-radius: 10px;
        background: #333;
        color: white;
        font-size: 17px;
        cursor: pointer;
        touch-action: manipulation;
    }

    #info {
        margin-top: 12px;
        font-size: 14px;
        color: #555;
    }
</style>
</head>

<body>

<div class="game">

    <h1>3 × 3 Spielfeld</h1>

    <div id="status">Drücke ein Feld</div>

    <div class="board" id="board"></div>

    <button id="test">
        Vibration testen
    </button>

    <div id="info">
        Bei einem falschen Feld ertönt ein Signal und das Handy vibriert.
    </div>

</div>

<script>

let richtigesFeld = Math.floor(Math.random() * 9);

const board = document.getElementById("board");
const statusText = document.getElementById("status");
const testButton = document.getElementById("test");


/* --------------------------------
   VIBRATION
-------------------------------- */

function starkeVibration() {

    if ("vibrate" in navigator) {

        // Sehr deutlicher Vibrationsimpuls
        navigator.vibrate([
            300,
            80,
            300,
            80,
            500
        ]);

    } else {

        alert(
            "Dein Browser unterstützt keine Vibration über diese Webseite."
        );
    }
}


/* --------------------------------
   FEHLERTON
-------------------------------- */

function fehlerTon() {

    try {

        const AudioContext =
            window.AudioContext ||
            window.webkitAudioContext;

        if (!AudioContext) return;

        const audio =
            new AudioContext();

        const oscillator =
            audio.createOscillator();

        const gain =
            audio.createGain();

        oscillator.type = "square";

        oscillator.frequency.setValueAtTime(
            140,
            audio.currentTime
        );

        gain.gain.setValueAtTime(
            0.0001,
            audio.currentTime
        );

        gain.gain.exponentialRampToValueAtTime(
            0.5,
            audio.currentTime + 0.02
        );

        gain.gain.exponentialRampToValueAtTime(
            0.0001,
            audio.currentTime + 0.35
        );

        oscillator.connect(gain);
        gain.connect(audio.destination);

        oscillator.start();

        oscillator.stop(
            audio.currentTime + 0.36
        );

    } catch (e) {

        console.log("Ton konnte nicht abgespielt werden.");

    }
}


/* --------------------------------
   FALSCHES FELD
-------------------------------- */

function falschesFeld(feld) {

    statusText.textContent = "FALSCH!";

    feld.classList.add("wrong");

    // Ton
    fehlerTon();

    // starke Vibration
    starkeVibration();

    setTimeout(function() {

        feld.classList.remove("wrong");

        statusText.textContent =
            "Drücke ein Feld";

    }, 900);
}


/* --------------------------------
   RICHTIGES FELD
-------------------------------- */

function richtig(feld) {

    statusText.textContent = "RICHTIG!";

    feld.classList.add("correct");

    feld.textContent = "✓";

    // Neues richtiges Feld auswählen
    setTimeout(function() {

        document.querySelectorAll(".field")
            .forEach(function(f) {

                f.classList.remove("correct");
                f.textContent =
                    f.dataset.number;

            });

        richtigesFeld =
            Math.floor(Math.random() * 9);

        statusText.textContent =
            "Neue Runde";

    }, 1200);
}


/* --------------------------------
   SPIELFELD ERSTELLEN
-------------------------------- */

function spielfeldErstellen() {

    board.innerHTML = "";

    for (let i = 0; i < 9; i++) {

        const feld =
            document.createElement("button");

        feld.className = "field";

        feld.dataset.number = i + 1;

        feld.textContent = i + 1;

        feld.addEventListener(
            "click",
            function() {

                if (i === richtigesFeld) {

                    richtig(feld);

                } else {

                    falschesFeld(feld);

                }

            }
        );

        board.appendChild(feld);
    }
}


/* --------------------------------
   VIBRATION TEST
-------------------------------- */

testButton.addEventListener(
    "click",
    function() {

        statusText.textContent =
            "Vibrationstest";

        fehlerTon();

        starkeVibration();

        setTimeout(function() {

            statusText.textContent =
                "Drücke ein Feld";

        }, 1000);
    }
);


/* Start */

spielfeldErstellen();

</script>

</body>
</html>
