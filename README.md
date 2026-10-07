<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BALANCE-BRÜCKE</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #dff5ff, #fef6d8);
    color: #243447;
}

.game {
    max-width: 1100px;
    margin: auto;
    padding: 20px;
}

h1 {
    text-align: center;
    color: #155e75;
    margin-bottom: 5px;
}

.subtitle {
    text-align: center;
    margin-bottom: 20px;
    color: #52616b;
}

.layout {
    display: grid;
    grid-template-columns: 1fr 320px;
    gap: 20px;
}

/* =========================
   SPIELBEREICH
========================= */

.game-area {
    background: white;
    border-radius: 24px;
    padding: 25px;
    box-shadow: 0 10px 35px rgba(0,0,0,.12);
}

/* =========================
   BRÜCKE
========================= */

.bridge {
    position: relative;
    margin-top: 30px;
    padding: 25px 10px 35px;
}

.bridge-road {
    height: 95px;

    background:
        linear-gradient(
            #a7d8e8,
            #76bdd3
        );

    border: 5px solid #397b91;

    border-radius: 25px;

    position: relative;

    display: flex;
    align-items: center;
    justify-content: center;

    box-shadow:
        inset 0 -8px 0 rgba(0,0,0,.08);
}

.bridge-center {
    position: absolute;

    left: 50%;
    top: 50%;

    transform:
        translate(-50%, -50%);

    width: 32px;
    height: 32px;

    border-radius: 50%;

    background: #ef4444;

    border: 4px solid white;

    box-shadow:
        0 0 0 5px rgba(239,68,68,.18),
        0 4px 10px rgba(0,0,0,.25);

    z-index: 20;
}

/* =========================
   SEITEN
========================= */

.sides {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 25px;
}

.side {
    text-align: center;

    background: #f8fafc;

    border-radius: 20px;

    padding: 15px;

    border: 3px solid #dbeafe;
}

.side.left {
    border-color: #93c5fd;
}

.side.right {
    border-color: #86efac;
}

.side h2 {
    margin: 0 0 8px;
}

.weight-total {
    font-size: 36px;
    font-weight: bold;
}

.weight-label {
    color: #64748b;
    font-size: 14px;
}

/* =========================
   STEINHAUFEN
========================= */

.stack-area {
    min-height: 145px;

    display: flex;
    align-items: flex-end;
    justify-content: center;
}

.stack {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.row {
    display: flex;
    justify-content: center;
}

.stone {
    width: 34px;
    height: 30px;

    margin: 2px;

    border-radius: 7px;

    background:
        linear-gradient(
            145deg,
            #e5e7eb,
            #9ca3af
        );

    border: 2px solid #6b7280;

    box-shadow:
        inset 2px 2px 3px rgba(255,255,255,.7),
        0 3px 4px rgba(0,0,0,.2);
}

/* =========================
   FIGUR
========================= */

.figure {
    position: absolute;

    bottom: 8px;

    font-size: 42px;

    transition:
        left .7s ease;

    z-index: 30;

    transform:
        translateX(-50%);
}

.figure-weight {
    position: absolute;

    left: 50%;
    bottom: -2px;

    transform: translateX(-50%);

    background: #334155;

    color: white;

    font-size: 10px;

    padding: 2px 5px;

    border-radius: 5px;
}

/* =========================
   STATUS
========================= */

.status {
    text-align: center;

    padding: 15px;

    margin-bottom: 20px;

    border-radius: 15px;

    background: #ecfeff;

    border: 2px solid #a5f3fc;

    font-size: 18px;

    font-weight: bold;
}

.status.good {
    background: #dcfce7;
    border-color: #86efac;
    color: #166534;
}

.status.bad {
    background: #fee2e2;
    border-color: #fca5a5;
    color: #991b1b;
}

/* =========================
   AUSWAHL
========================= */

.controls {
    background: white;

    border-radius: 24px;

    padding: 20px;

    box-shadow:
        0 10px 35px rgba(0,0,0,.12);
}

.controls h2 {
    margin-top: 0;
}

.direction {
    display: grid;

    grid-template-columns: 1fr 1fr;

    gap: 10px;

    margin-bottom: 15px;
}

.direction button {
    padding: 12px;

    border: 2px solid #cbd5e1;

    border-radius: 12px;

    background: #f8fafc;

    cursor: pointer;

    font-size: 15px;
}

.direction button.selected {
    background: #dbeafe;
    border-color: #3b82f6;
}

.weights {
    display: grid;

    grid-template-columns:
        repeat(3, 1fr);

    gap: 10px;
}

.weight-choice {
    min-height: 110px;

    border: 3px solid #cbd5e1;

    border-radius: 15px;

    background: #f8fafc;

    cursor: pointer;

    display: flex;

    flex-direction: column;

    align-items: center;

    justify-content: center;

    transition: .15s;
}

.weight-choice:hover {
    transform: translateY(-3px);
    border-color: #38bdf8;
}

.weight-choice.selected {
    border-color: #f59e0b;

    background: #fff7ed;

    box-shadow:
        0 0 15px rgba(245,158,11,.3);
}

.choice-number {
    margin-top: 7px;

    font-weight: bold;

    color: #475569;
}

/* =========================
   BUTTONS
========================= */

button.main {
    width: 100%;

    margin-top: 12px;

    padding: 13px;

    border: none;

    border-radius: 12px;

    font-size: 16px;

    font-weight: bold;

    cursor: pointer;

    color: white;

    background:
        linear-gradient(
            135deg,
            #0891b2,
            #0e7490
        );
}

button.new {
    background:
        linear-gradient(
            135deg,
            #7c3aed,
            #5b21b6
        );
}

/* =========================
   MELDUNG
========================= */

.message {
    margin-top: 15px;

    padding: 13px;

    border-radius: 12px;

    display: none;

    text-align: center;

    font-weight: bold;
}

.message.show {
    display: block;
}

.message.good {
    background: #dcfce7;
    color: #166534;
}

.message.bad {
    background: #fee2e2;
    color: #991b1b;
}

.message.info {
    background: #e0f2fe;
    color: #075985;
}

/* =========================
   ZIEL
========================= */

.goal {
    margin-top: 20px;

    text-align: center;

    font-size: 20px;

    font-weight: bold;

    color: #15803d;
}

/* =========================
   RESPONSIVE
========================= */

@media(max-width:850px) {

    .layout {
        grid-template-columns: 1fr;
    }
}

@media(max-width:550px) {

    .sides {
        grid-template-columns: 1fr;
    }

    .weights {
        grid-template-columns: repeat(3,1fr);
    }

    .stone {
        width: 27px;
        height: 24px;
    }
}
</style>
</head>

<body>

<div class="game">

    <h1>⚖️ BALANCE-BRÜCKE</h1>

    <div class="subtitle">
        Bringe die Brücke ins Gleichgewicht und hilf der Figur zum Ziel!
    </div>


    <div class="layout">

        <!-- =========================
             SPIEL
        ========================== -->

        <div class="game-area">

            <div
                id="status"
                class="status">

                ⚖️ Die Brücke wartet auf dich!

            </div>


            <div class="sides">

                <div class="side left">

                    <h2>🔵 LINKS</h2>

                    <div
                        id="leftTotal"
                        class="weight-total">
                        0
                    </div>

                    <div class="weight-label">
                        Gewichte
                    </div>

                    <div
                        id="leftStack"
                        class="stack-area">
                    </div>

                </div>


                <div class="side right">

                    <h2>🟢 RECHTS</h2>

                    <div
                        id="rightTotal"
                        class="weight-total">
                        0
                    </div>

                    <div class="weight-label">
                        Gewichte
                    </div>

                    <div
                        id="rightStack"
                        class="stack-area">
                    </div>

                </div>

            </div>


            <div class="bridge">

                <div class="bridge-road">

                    <div
                        id="figure"
                        class="figure">

                        🧒

                        <div
                            class="figure-weight">
                            2
                        </div>

                    </div>

                    <div class="bridge-center"></div>

                </div>

            </div>


            <div
                id="goal"
                class="goal">

                🏁 Ziel: Noch nicht erreicht

            </div>

        </div>


        <!-- =========================
             STEUERUNG
        ========================== -->

        <div class="controls">

            <h2>🧱 Gewicht wählen</h2>

            <p>
                Wähle zuerst die Seite und danach
                einen Gewichtshaufen.
            </p>


            <div class="direction">

                <button
                    id="leftButton"
                    onclick="selectSide('left')">

                    🔵 Links

                </button>

                <button
                    id="rightButton"
                    onclick="selectSide('right')">

                    🟢 Rechts

                </button>

            </div>


            <div
                id="weights"
                class="weights">
            </div>


            <button
                class="main"
                onclick="placeWeight()">

                🧱 Gewicht ablegen

            </button>


            <button
                class="main new"
                onclick="newGame()">

                🎲 Neue Runde

            </button>


            <div
                id="message"
                class="message">
            </div>

        </div>

    </div>

</div>


<script>

/* =========================================================
   SPIELREGELN
========================================================= */

const FIGURE_WEIGHT = 2;

const AVAILABLE_WEIGHTS = [1, 2, 3, 4, 5, 6];

const MAX_STEPS = 7;


/* =========================================================
   SPIELZUSTAND
========================================================= */

let leftWeights = [];

let rightWeights = [];

let selectedSide = null;

let selectedWeight = null;

let step = 0;

let goalStep = MAX_STEPS;

let locked = false;


/* =========================================================
   STEINHAUFEN
========================================================= */

function createStack(amount) {

    const stack =
        document.createElement("div");

    stack.className = "stack";


    let rows = [];


    /*
       1 = 1
       2 = 1 + 1
       3 = 1 + 2
       4 = 2 + 2
       5 = 2 + 3
       6 = 3 + 3
    */

    switch(amount) {

        case 1:
            rows = [1];
            break;

        case 2:
            rows = [1,1];
            break;

        case 3:
            rows = [1,2];
            break;

        case 4:
            rows = [2,2];
            break;

        case 5:
            rows = [2,3];
            break;

        case 6:
            rows = [3,3];
            break;
    }


    rows.forEach(count => {

        const row =
            document.createElement("div");

        row.className = "row";


        for(let i=0; i<count; i++) {

            const stone =
                document.createElement("div");

            stone.className = "stone";

            row.appendChild(stone);
        }


        stack.appendChild(row);
    });


    return stack;
}


/* =========================================================
   GESAMTGEWICHT
========================================================= */

function getTotal(weights) {

    return weights.reduce(
        (sum, value) =>
            sum + value,
        0
    );
}


/* =========================================================
   GEWICHTSAUSWAHL
========================================================= */

function renderWeightChoices() {

    const container =
        document.getElementById("weights");

    container.innerHTML = "";


    const choices =
        generateChoices();


    choices.forEach(weight => {

        const button =
            document.createElement("div");

        button.className =
            "weight-choice";


        if (
            selectedWeight === weight
        ) {

            button.classList.add(
                "selected"
            );
        }


        button.appendChild(
            createStack(weight)
        );


        const label =
            document.createElement("div");

        label.className =
            "choice-number";

        label.textContent =
            weight === 1
                ? "1 Gewicht"
                : `${weight} Gewichte`;


        button.appendChild(label);


        button.onclick = () => {

            if (locked) return;

            selectedWeight = weight;

            renderWeightChoices();

            showMessage(
                "🧱 Gewicht ausgewählt. Jetzt ablegen.",
                "info"
            );
        };


        container.appendChild(button);
    });
}


/* =========================================================
   3 AUSWAHLEN
========================================================= */

function generateChoices() {

    /*
       Für die aktuelle Aufgabe wird
       immer eine passende Lösung erzeugt.

       Die beiden anderen Werte sind
       bewusst falsch.
    */

    const difference =
        Math.abs(
            getTotal(leftWeights)
            -
            getTotal(rightWeights)
        );


    let correct =
        difference;


    /*
       Wenn beide Seiten gleich sind,
       muss 1 gewählt werden, damit
       die nächste Aufgabe entsteht.
    */

    if (correct === 0) {

        correct = 1;
    }


    /*
       Maximal 6.
    */

    correct =
        Math.min(
            correct,
            6
        );


    const choices = [correct];


    while (
        choices.length < 3
    ) {

        const value =
            randomInt(1,6);


        if (
            !choices.includes(value)
        ) {

            choices.push(value);
        }
    }


    return shuffle(choices);
}


/* =========================================================
   SEITE AUSWÄHLEN
========================================================= */

function selectSide(side) {

    if (locked) return;


    selectedSide = side;


    document
        .getElementById("leftButton")
        .classList.toggle(
            "selected",
            side === "left"
        );


    document
        .getElementById("rightButton")
        .classList.toggle(
            "selected",
            side === "right"
        );


    showMessage(
        side === "left"
            ? "🔵 Linke Seite ausgewählt."
            : "🟢 Rechte Seite ausgewählt.",
        "info"
    );
}


/* =========================================================
   GEWICHT ABLEGEN
========================================================= */

function placeWeight() {

    if (locked) return;


    if (!selectedSide) {

        showMessage(
            "👆 Wähle zuerst links oder rechts.",
            "bad"
        );

        return;
    }


    if (!selectedWeight) {

        showMessage(
            "👆 Wähle zuerst ein Gewicht.",
            "bad"
        );

        return;
    }


    const beforeLeft =
        getTotal(leftWeights);

    const beforeRight =
        getTotal(rightWeights);


    /*
       Das gewählte Gewicht wird
       dauerhaft abgelegt.
    */

    if (
        selectedSide === "left"
    ) {

        leftWeights.push(
            selectedWeight
        );

    } else {

        rightWeights.push(
            selectedWeight
        );
    }


    const left =
        getTotal(leftWeights);

    const right =
        getTotal(rightWeights);


    selectedSide = null;

    selectedWeight = null;


    document
        .getElementById("leftButton")
        .classList.remove("selected");

    document
        .getElementById("rightButton")
        .classList.remove("selected");


    renderBoard();


    /*
       Gleichgewicht?
    */

    if (left === right) {

        locked = true;


        showMessage(
            "🎉 PERFEKT! Die Brücke ist im Gleichgewicht!",
            "good"
        );


        setTimeout(() => {

            locked = false;

            moveFigure();

        }, 1000);


    } else {

        showMessage(
            left > right
                ? "⚖️ Links ist noch schwerer."
                : "⚖️ Rechts ist noch schwerer.",
            "bad"
        );
    }
}


/* =========================================================
   FIGUR BEWEGEN
========================================================= */

function moveFigure() {

    step++;


    if (
        step >= goalStep
    ) {

        finishGame();

        return;
    }


    renderBoard();


    showMessage(
        "➡️ Die Figur ist ein Feld weiter!",
        "good"
    );


    /*
       Neue Auswahl erzeugen.
    */

    renderWeightChoices();
}


/* =========================================================
   FIGUR POSITION
========================================================= */

function updateFigure() {

    const figure =
        document.getElementById(
            "figure"
        );


    /*
       Die Figur bewegt sich
       von links nach rechts
       über die Brücke.
    */

    const positions = [
        8,
        20,
        32,
        44,
        56,
        68,
        80
    ];


    const position =
        positions[
            Math.min(
                step,
                positions.length - 1
            )
        ];


    figure.style.left =
        position + "%";
}


/* =========================================================
   BRETT DARSTELLEN
========================================================= */

function renderBoard() {

    const left =
        getTotal(leftWeights);

    const right =
        getTotal(rightWeights);


    document
        .getElementById("leftTotal")
        .textContent =
        left;


    document
        .getElementById("rightTotal")
        .textContent =
        right;


    const leftStack =
        document.getElementById(
            "leftStack"
        );

    const rightStack =
        document.getElementById(
            "rightStack"
        );


    leftStack.innerHTML = "";

    rightStack.innerHTML = "";


    /*
       Alle einzelnen Gewichtshaufen
       bleiben sichtbar.
    */

    leftWeights.forEach(weight => {

        leftStack.appendChild(
            createStack(weight)
        );
    });


    rightWeights.forEach(weight => {

        rightStack.appendChild(
            createStack(weight)
        );
    });


    updateFigure();


    /*
       Statusanzeige
    */

    const status =
        document.getElementById(
            "status"
        );


    if (left === right) {

        status.className =
            "status good";

        status.textContent =
            "⚖️ GLEICHGEWICHT!";

    } else {

        status.className =
            "status";

        status.textContent =
            "⚖️ Die Brücke muss ausgeglichen werden.";
    }
}


/* =========================================================
   SPIEL BEENDET
========================================================= */

function finishGame() {

    locked = true;


    document
        .getElementById("goal")
        .textContent =
        "🏆 ZIEL ERREICHT! SUPER GEMACHT!";


    showMessage(
        "🎉 Du hast die BALANCE-BRÜCKE geschafft!",
        "good"
    );
}


/* =========================================================
   NEUES SPIEL
========================================================= */

function newGame() {

    leftWeights = [];

    rightWeights = [];

    selectedSide = null;

    selectedWeight = null;

    step = 0;

    goalStep =
        randomInt(5,7);

    locked = false;


    document
        .getElementById("goal")
        .textContent =
        "🏁 Ziel: Noch nicht erreicht";


    document
        .getElementById("leftButton")
        .classList.remove("selected");

    document
        .getElementById("rightButton")
        .classList.remove("selected");


    renderBoard();

    renderWeightChoices();


    showMessage(
        "🎲 Neue Runde! Bringe beide Seiten ins Gleichgewicht.",
        "info"
    );
}


/* =========================================================
   HILFSFUNKTIONEN
========================================================= */

function randomInt(min,max) {

    return Math.floor(
        Math.random() *
        (max - min + 1)
    ) + min;
}


function shuffle(array) {

    const result =
        [...array];


    for (
        let i =
            result.length - 1;

        i > 0;

        i--
    ) {

        const j =
            Math.floor(
                Math.random() *
                (i + 1)
            );


        [
            result[i],
            result[j]
        ] =
        [
            result[j],
            result[i]
        ];
    }


    return result;
}


/* =========================================================
   START
========================================================= */

newGame();

</script>

</body>
</html>
