<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Spinnen-Spiel</title>

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
        margin: 0 0 8px 0;
        font-size: 28px;
    }

    #status {
        min-height: 32px;
        margin-bottom: 15px;
        font-size: 20px;
        font-weight: bold;
    }

    /* 3 x 3 Spielfeld */

    .board {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 10px;
        width: 100%;
        max-width: 360px;
        margin: auto;
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

        transition: transform 0.08s;
    }

    .field:active {
        transform: scale(0.94);
    }

    /* Falsches Feld */

    .wrong {
        background: #e53935 !important;
        color: white;
        animation: shake 0.12s linear 8;
    }

    /* Richtiges Feld */

    .correct {
        background: #43a047 !important;
        color: white;
    }

    @keyframes shake {

        0% {
            transform: translateX(0);
        }

        25% {
            transform: translateX(-8px);
        }

        50% {
            transform: translateX(8px);
        }

        75% {
            transform: translateX(-8px);
        }

        100% {
            transform: translateX(0);
        }
    }

    /* Spinne */

    .spider {
        font-size: 65px;
        margin: 18px auto 5px auto;
        width: fit-content;

        user-select: none;
        pointer-events: none;
    }

    .spider.shake {
        animation: spiderShake 0.1s linear infinite;
    }

    @keyframes spiderShake {

        0% {
            transform: translate(0, 0) rotate(0deg);
        }

        25% {
            transform: translate(-3px, 2px) rotate(-3deg);
        }

        50% {
            transform: translate(3px, -2px) rotate(3deg);
        }

        75% {
            transform: translate(-2px, -1px) rotate(-2deg);
        }

        100% {
            transform: translate(0, 0) rotate(0deg);
        }
    }

    /* Testknopf */

    .test {
        margin-top: 20px;
        padding: 14px 22px;

        border: none;
        border-radius: 12px;

        background: #333;
        color: white;

        font-size: 17px;
        font-weight: bold;

        cursor: pointer;
        touch-action: manipulation;
    }

    .test:active {
        transform: scale(0.96);
    }

    .info {
        margin-top: 12px;
        color: #555;
        font-size: 14px;
        line-height: 1.4;
    }
</style>
</head>


<body>

<div class="game">

    <h1>🕷️ Spinnenspiel</h1>

    <div id="status">
        Drücke ein Feld
    </div>


    <!-- Spinne -->

    <div id="spider" class="spider">
        🕷️
    </div>


    <!-- 3 x 3 Spielfeld -->

    <div id="board" class="board"></div>


    <!-- Vibrations-Test -->

    <button id="testButton" class="test">
        Vibration 5 Sekunden testen
    </button>


    <div class="info">
        Bei einem falschen Feld ertönt ein Signal
        und das Handy vibriert ungefähr 5 Sekunden.
    </div>

</div>


<script>

/* =====================================================
   EINSTELLUNGEN
===================================================== */

const VIBRATIONSDAUER = 5000;


/* =====================================================
   ELEMENTE
===================================================== */

const board =
    document.getElementById("board");

const statusText =
    document.getElementById("status");

const spider =
    document.getElementById("spider");

const testButton =
    document.getElementById("testButton");


/* =====================================================
   RICHTIGES FELD
===================================================== */

let richtigesFeld =
    Math.floor(Math.random() * 9);


/* =====================================================
   VIBRATION
===================================================== */

function starkeVibration() {

    if (!("vibrate" in navigator)) {

        statusText.textContent =
            "Vibration wird von diesem Browser nicht unterstützt.";

        return;
    }


    /*
       Statt einer einzigen 5-Sekunden-Vibration
       verwenden wir viele kräftige Impulse.

       400 ms Vibration
       100 ms Pause

       Das wiederholt sich ungefähr 5 Sekunden.
    */

    const muster = [];

    const vibration = 400;
    const pause = 100;

    const anzahl =
        Math.floor(
            VIBRATIONSDAUER /
            (vibration + pause)
        );


    for (let i = 0; i < anzahl; i++) {

        muster.push(vibration);
        muster.push(pause);

    }


    navigator.vibrate(muster);
}


/* =====================================================
   SPINNE BEWEGEN
===================================================== */

function spinneStart() {

    spider.classList.add("shake");

}


function spinneStop() {

    spider.classList.remove("shake");

}


/* =====================================================
   FEHLERTON
===================================================== */

function fehlerTon() {

    try {

        const AudioContext =
            window.AudioContext ||
            window.webkitAudioContext;


        if (!AudioContext) {
            return;
        }


        const audio =
            new AudioContext();


        const oscillator =
            audio.createOscillator();


        const gain =
            audio.createGain();


        oscillator.type = "square";


        oscillator.frequency.setValueAtTime(
            130,
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
            audio.currentTime + 0.4
        );


        oscillator.connect(gain);

        gain.connect(audio.destination);


        oscillator.start();


        oscillator.stop(
            audio.currentTime + 0.42
        );


    } catch (error) {

        console.log(
            "Ton konnte nicht abgespielt werden."
        );

    }

}


/* =====================================================
   FALSCHES FELD
===================================================== */

function falschesFeld(feld) {

    statusText.textContent =
        "FALSCH!";


    feld.classList.add("wrong");


    /* Fehlerton */

    fehlerTon();


    /* echte Handy-Vibration */

    starkeVibration();


    /* Spinne optisch bewegen */

    spinneStart();


    /* Nach 5 Sekunden stoppen */

    setTimeout(function() {

        spinneStop();

    }, VIBRATIONSDAUER);


    /* Feld nach kurzer Zeit zurücksetzen */

    setTimeout(function() {

        feld.classList.remove("wrong");

        statusText.textContent =
            "Drücke ein Feld";

    }, 900);

}


/* =====================================================
   RICHTIGES FELD
===================================================== */

function richtig(feld) {

    statusText.textContent =
        "RICHTIG!";


    feld.classList.add("correct");


    feld.textContent =
        "✓";


    setTimeout(function() {

        spielfeldErstellen();

        statusText.textContent =
            "Neue Runde";

    }, 1200);

}


/* =====================================================
   SPIELFELD ERSTELLEN
===================================================== */

function spielfeldErstellen() {

    board.innerHTML = "";


    richtigesFeld =
        Math.floor(Math.random() * 9);


    for (
        let i = 0;
        i < 9;
        i++
    ) {

        const feld =
            document.createElement("button");


        feld.type =
            "button";


        feld.className =
            "field";


        feld.dataset.number =
            i + 1;


        feld.textContent =
            i + 1;


        feld.setAttribute(
            "aria-label",
            "Feld " + (i + 1)
        );


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


/* =====================================================
   VIBRATIONSTEST
===================================================== */

testButton.addEventListener(
    "click",
    function() {

        statusText.textContent =
            "VIBRATION!";


        /* Fehlerton */

        fehlerTon();


        /* Vibration */

        starkeVibration();


        /* Spinne bewegen */

        spinneStart();


        setTimeout(function() {

            spinneStop();

            statusText.textContent =
                "Drücke ein Feld";

        }, VIBRATIONSDAUER);

    }
);


/* =====================================================
   START
===================================================== */

spinneStop();

spielfeldErstellen();

</script>

</body>
</html>
