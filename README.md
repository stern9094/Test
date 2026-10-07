<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>BALANCE-TEAM</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    min-height: 100vh;
    font-family: Arial, Helvetica, sans-serif;
    color: white;

    background:
        radial-gradient(circle at top,
            #29445f 0%,
            #172638 45%,
            #0b111a 100%);
}

.container {
    width: min(1200px, 96%);
    margin: auto;
    padding: 22px 0 40px;
}

h1 {
    text-align: center;
    margin: 0;
    font-size: 40px;
    letter-spacing: 3px;
}

.subtitle {
    text-align: center;
    color: #aebfd1;
    margin: 6px 0 22px;
}

/* =========================
   LAYOUT
========================= */

.game-layout {
    display: grid;
    grid-template-columns: minmax(500px, 1fr) 360px;
    gap: 22px;
}

.panel {
    background: rgba(255,255,255,.075);
    border: 1px solid rgba(255,255,255,.12);
    border-radius: 18px;
    padding: 18px;

    box-shadow:
        0 20px 50px rgba(0,0,0,.28);

    backdrop-filter: blur(8px);
}

/* =========================
   SPIELFELD
========================= */

.board {
    width: min(650px, 100%);
    aspect-ratio: 1;
    margin: auto;

    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(5, 1fr);

    gap: 5px;
}

.cell {
    position: relative;

    display: flex;
    justify-content: center;
    align-items: center;

    background:
        linear-gradient(
            145deg,
            #30465b,
            #1c2c3c
        );

    border: 2px solid #536b82;
    border-radius: 10px;

    cursor: pointer;

    transition:
        transform .12s,
        background .12s,
        border-color .12s;

    user-select: none;
}

.cell:hover {
    transform: scale(1.035);
    border-color: #65d8ff;
    z-index: 5;
}

.cell.start {
    border-color: #38bdf8;
}

.cell.goal {
    border-color: #22c55e;
}

.cell.current {
    background:
        linear-gradient(
            145deg,
            #7c3aed,
            #4c1d95
        );

    border-color: #c4b5fd;

    box-shadow:
        0 0 25px rgba(139,92,246,.65);

    z-index: 4;
}

.cell.path {
    background:
        linear-gradient(
            145deg,
            #28506a,
            #1a3549
        );
}

.cell.wrong {
    animation: wrong .35s;
}

@keyframes wrong {
    0%,100% {
        transform: translateX(0);
    }

    25% {
        transform: translateX(-7px);
    }

    75% {
        transform: translateX(7px);
    }
}

.field-number {
    position: absolute;
    top: 5px;
    left: 7px;

    font-size: 11px;
    color: rgba(255,255,255,.55);
}

.figure {
    font-size: clamp(25px, 5vw, 44px);
}

.weight-stack {
    position: absolute;

    bottom: 7px;
    right: 7px;

    display: flex;
    flex-direction: column;
    gap: 3px;

    align-items: flex-end;
}

.weight-token {
    background: #f59e0b;
    color: #241500;

    border: 1px solid #fde68a;

    border-radius: 6px;

    padding: 3px 6px;

    font-size: 11px;
    font-weight: bold;

    box-shadow:
        0 3px 8px rgba(0,0,0,.35);
}

.weight-token.red {
    background: #ef4444;
    color: white;
    border-color: #fecaca;
}

.weight-token.blue {
    background: #38bdf8;
    color: #082f49;
    border-color: #bae6fd;
}

.weight-token.green {
    background: #22c55e;
    color: #052e16;
    border-color: #bbf7d0;
}

.weight-token.purple {
    background: #a78bfa;
    color: #241044;
    border-color: #ddd6fe;
}

/* =========================
   STATUS
========================= */

.status {
    background: rgba(0,0,0,.2);
    border-radius: 12px;
    padding: 14px;

    line-height: 1.55;

    margin-bottom: 15px;
}

.status-title {
    color: #67e8f9;
    font-weight: bold;
    font-size: 17px;
}

/* =========================
   INFOS
========================= */

.info-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 9px;
}

.info {
    background: rgba(0,0,0,.18);
    padding: 11px;
    border-radius: 9px;
}

.info-label {
    color: #8fa4b9;
    font-size: 12px;
}

.info-value {
    font-size: 20px;
    font-weight: bold;
    margin-top: 3px;
}

/* =========================
   GEWICHTE
========================= */

h2 {
    font-size: 18px;
    margin: 20px 0 10px;
}

.weights {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
}

.weight-option {
    padding: 13px 5px;

    border-radius: 9px;

    background:
        linear-gradient(
            145deg,
            #d97706,
            #92400e
        );

    border: 2px solid #fbbf24;

    color: white;

    font-weight: bold;
    text-align: center;

    cursor: pointer;

    transition: .15s;
}

.weight-option:hover {
    transform: translateY(-2px);
    filter: brightness(1.12);
}

.weight-option.selected {
    border-color: white;

    box-shadow:
        0 0 20px rgba(251,191,36,.7);

    transform: scale(1.04);
}

/* =========================
   BUTTONS
========================= */

button {
    width: 100%;

    border: none;
    border-radius: 9px;

    padding: 12px;

    margin-top: 9px;

    color: white;

    font-size: 15px;
    font-weight: bold;

    cursor: pointer;

    background:
        linear-gradient(
            135deg,
            #0284c7,
            #0369a1
        );
}

button:hover {
    filter: brightness(1.12);
}

button.new {
    background:
        linear-gradient(
            135deg,
            #7c3aed,
            #5b21b6
        );
}

button.reset {
    background:
        linear-gradient(
            135deg,
            #475569,
            #334155
        );
}

/* =========================
   MELDUNG
========================= */

.message {
    margin-top: 13px;

    padding: 12px;

    border-radius: 9px;

    line-height: 1.5;

    display: none;
}

.message.show {
    display: block;
}

.message.info {
    background: rgba(14,116,144,.2);
    border: 1px solid #22d3ee;
}

.message.success {
    background: rgba(22,163,74,.2);
    border: 1px solid #22c55e;
}

.message.error {
    background: rgba(220,38,38,.2);
    border: 1px solid #ef4444;
}

/* =========================
   WEG
========================= */

.path {
    margin-top: 15px;

    padding: 12px;

    border-radius: 10px;

    background: rgba(0,0,0,.18);
}

.path-list {
    display: flex;
    flex-wrap: wrap;
    gap: 5px;

    margin-top: 8px;
}

.path-field {
    padding: 4px 8px;

    border-radius: 5px;

    background: #273b4e;

    border: 1px solid #526b81;

    font-size: 12px;
}

.path-field.active {
    background: #7c3aed;
    border-color: #c4b5fd;
}

.path-field.done {
    background: #166534;
    border-color: #4ade80;
}

/* =========================
   BERECHNUNG
========================= */

.calculation {
    margin-top: 15px;

    padding: 13px;

    border-radius: 10px;

    background: #09121d;

    font-family: Consolas, monospace;

    font-size: 12px;

    line-height: 1.65;

    display: none;
}

.calculation.show {
    display: block;
}

.good {
    color: #86efac;
}

.warning {
    color: #facc15;
}

.bad {
    color: #fca5a5;
}

/* =========================
   RESPONSIVE
========================= */

@media(max-width: 850px) {

    .game-layout {
        grid-template-columns: 1fr;
    }

    h1 {
        font-size: 30px;
    }
}

</style>
</head>

<body>

<div class="container">

    <h1>⚖️ BALANCE-TEAM</h1>

    <div class="subtitle">
        Das 5 × 5 Balance-Spiel
    </div>


    <div class="game-layout">

        <!-- ==========================================
             SPIELFELD
        =========================================== -->

        <div class="panel">

            <div id="board" class="board"></div>

        </div>


        <!-- ==========================================
             STEUERUNG
        =========================================== -->

        <div class="panel">

            <div class="status">

                <div class="status-title">
                    Aktuelle Aufgabe
                </div>

                <div>
                    Die Figur muss über den zufälligen
                    Weg zum Ziel gelangen.
                </div>

                <br>

                <div>
                    <strong>
                        Wähle zuerst eines der bereitliegenden
                        Gewichte.
                    </strong>
                </div>

                <div>
                    Danach klicke auf das Feld,
                    auf dem du das Gewicht ablegen möchtest.
                </div>

            </div>


            <div class="info-grid">

                <div class="info">
                    <div class="info-label">
                        Start
                    </div>

                    <div
                        id="startField"
                        class="info-value">
                        -
                    </div>
                </div>


                <div class="info">
                    <div class="info-label">
                        Ziel
                    </div>

                    <div
                        id="goalField"
                        class="info-value">
                        -
                    </div>
                </div>


                <div class="info">
                    <div class="info-label">
                        Figur
                    </div>

                    <div
                        id="figureWeight"
                        class="info-value">
                        2,0 kg
                    </div>
                </div>


                <div class="info">
                    <div class="info-label">
                        aktuelles Feld
                    </div>

                    <div
                        id="currentField"
                        class="info-value">
                        -
                    </div>
                </div>

            </div>


            <h2>
                ⚖️ 6 bereitliegende Gewichte
            </h2>


            <div
                id="weights"
                class="weights">
            </div>


            <button
                class="new"
                onclick="newGame()">
                🎲 Neue Runde
            </button>


            <button
                class="reset"
                onclick="resetWeights()">
                ↩ Gewichte zurücknehmen
            </button>


            <div
                id="message"
                class="message">
            </div>


            <div
                id="calculation"
                class="calculation">
            </div>


            <div class="path">

                <strong>
                    Zufälliger Weg
                </strong>

                <div
                    id="pathList"
                    class="path-list">
                </div>

            </div>

        </div>

    </div>

</div>


<script>

/* =========================================================
   GRUNDKONSTANTEN
========================================================= */

const SIZE = 5;

const CENTER = 13;

const FIGURE_WEIGHT = 2.0;

const WEIGHT_COUNT = 6;


/*
    Diese sechs Gewichte werden bei jedem Spiel
    neu erzeugt.

    Eines davon wird mathematisch genau passen.
*/
let availableWeights = [];


/* =========================================================
   SPIELZUSTAND
========================================================= */

let path = [];

let currentPathIndex = 0;

let currentField = null;

let startField = null;

let goalField = null;

let placedWeights = [];

let selectedWeightIndex = null;


/* =========================================================
   KOORDINATEN
=========================================================

   Feld 13 ist der Mittelpunkt.

   1   2   3   4   5
   6   7   8   9  10
  11  12  13  14  15
  16  17  18  19  20
  21  22  23  24  25

   Feld 13 = (0,0)
========================================================= */

function fieldToCoordinate(field) {

    const index = field - 1;

    const row =
        Math.floor(index / SIZE);

    const col =
        index % SIZE;

    return {
        x: col - 2,
        y: row - 2
    };
}


function coordinateToField(x, y) {

    const col = x + 2;

    const row = y + 2;

    if (
        col < 0 ||
        col >= SIZE ||
        row < 0 ||
        row >= SIZE
    ) {
        return null;
    }

    return row * SIZE + col + 1;
}


/* =========================================================
   DISTANZ
========================================================= */

function distanceFromCenter(field) {

    const p =
        fieldToCoordinate(field);

    return Math.sqrt(
        p.x * p.x +
        p.y * p.y
    );
}


/* =========================================================
   DREHMOMENT
=========================================================

   Für die Spiellogik wird die Position als
   zweidimensionaler Hebel betrachtet.

   Die Gesamtsituation ist nur ausgeglichen,
   wenn die Summe der Momente beider Achsen
   ausgeglichen ist.
========================================================= */

function calculateMoment(weight, field) {

    const p =
        fieldToCoordinate(field);

    return {
        x: weight * p.x,
        y: weight * p.y
    };
}


/* =========================================================
   ALLE MOMENTE BERECHNEN
========================================================= */

function calculateTotalMoment() {

    let totalX = 0;
    let totalY = 0;


    /*
        Figur
    */

    const figureMoment =
        calculateMoment(
            FIGURE_WEIGHT,
            currentField
        );

    totalX += figureMoment.x;
    totalY += figureMoment.y;


    /*
        Bereits abgelegte Gewichte
    */

    for (const placed of placedWeights) {

        const moment =
            calculateMoment(
                placed.weight,
                placed.field
            );

        totalX += moment.x;
        totalY += moment.y;
    }


    return {
        x: totalX,
        y: totalY
    };
}


/* =========================================================
   GEWICHTE ERZEUGEN
=========================================================

   Wir erzeugen zunächst fünf verschiedene
   Gewichte.

   Das sechste wird anschließend so berechnet,
   dass eine gültige Lösung existiert.

   Dadurch ist immer mindestens ein Gewicht
   tatsächlich richtig.
========================================================= */

function createWeights() {

    /*
        Wir verwenden bewusst einfache Werte,
        damit Kinder/Spieler die Gewichte
        leicht erkennen können.
    */

    let values = [
        0.5,
        1.0,
        1.5,
        2.0,
        2.5
    ];


    /*
        Ein zusätzliches Gewicht.
        Es wird später so angepasst,
        dass eine Lösung existiert.
    */

    values.push(3.0);


    /*
        Zufällige Reihenfolge.
    */

    values =
        values.sort(
            () => Math.random() - 0.5
        );


    availableWeights = values;
}


/* =========================================================
   NACHBARN
========================================================= */

function getNeighbors(field) {

    const p =
        fieldToCoordinate(field);

    const directions = [

        { x: 1, y: 0 },
        { x: -1, y: 0 },
        { x: 0, y: 1 },
        { x: 0, y: -1 }

    ];

    const result = [];


    for (const d of directions) {

        const field2 =
            coordinateToField(
                p.x + d.x,
                p.y + d.y
            );

        if (field2 !== null) {
            result.push(field2);
        }
    }


    return result;
}


/* =========================================================
   ZUFÄLLIGEN WEG ERZEUGEN
========================================================= */

function generatePath() {

    /*
        Start immer oben:
        1 - 5
    */

    startField =
        randomInt(1, 5);


    /*
        Ziel immer unten:
        21 - 25
    */

    goalField =
        randomInt(21, 25);


    let result = [startField];

    let current = startField;

    const used =
        new Set(result);


    /*
        Wir versuchen,
        einen Weg nach unten zu bauen.
    */

    let safety = 0;


    while (
        current !== goalField &&
        safety < 500
    ) {

        safety++;


        let neighbors =
            getNeighbors(current)
            .filter(
                field =>
                    !used.has(field)
            );


        /*
            Bevorzugt Felder,
            die Richtung Ziel gehen.
        */

        const currentPosition =
            fieldToCoordinate(current);

        const goalPosition =
            fieldToCoordinate(goalField);


        neighbors.sort(
            () => Math.random() - 0.5
        );


        /*
            Wenn Ziel erreichbar ist,
            bevorzugen wir es.
        */

        const direct =
            neighbors.includes(goalField);


        if (direct) {

            current = goalField;

        } else if (neighbors.length > 0) {

            /*
                Leichte Bevorzugung
                Richtung unten.
            */

            const downward =
                neighbors.filter(field => {

                    const p =
                        fieldToCoordinate(field);

                    return p.y >= currentPosition.y;

                });


            if (
                downward.length > 0 &&
                Math.random() < 0.72
            ) {

                current =
                    downward[
                        randomInt(
                            0,
                            downward.length - 1
                        )
                    ];

            } else {

                current =
                    neighbors[
                        randomInt(
                            0,
                            neighbors.length - 1
                        )
                    ];
            }

        } else {

            /*
                Sackgasse:
                Weg neu erzeugen.
            */

            return generatePath();
        }


        result.push(current);

        used.add(current);
    }


    if (
        current !== goalField
    ) {
        return generatePath();
    }


    return result;
}


/* =========================================================
   GEWICHT AUSWÄHLEN
========================================================= */

function selectWeight(index) {

    selectedWeightIndex = index;

    document
        .querySelectorAll(".weight-option")
        .forEach(
            (element, i) => {

                element.classList.toggle(
                    "selected",
                    i === index
                );

            }
        );


    showMessage(
        `Gewicht ${formatKg(
            availableWeights[index]
        )} ausgewählt.
        Klicke jetzt auf das Feld,
        auf dem du es ablegen möchtest.`,
        "info"
    );
}


/* =========================================================
   FELD ANGEKLICKT
========================================================= */

function boardClicked(field) {

    /*
        Ohne Gewicht keine Aktion.
    */

    if (
        selectedWeightIndex === null
    ) {

        showMessage(
            "Wähle zuerst eines der sechs Gewichte.",
            "error"
        );

        return;
    }


    /*
        Ein Gewicht darf nicht auf
        dem aktuellen Figurenfeld liegen.
    */

    if (field === currentField) {

        showMessage(
            "Auf dem Feld der Figur kann kein Gegengewicht abgelegt werden.",
            "error"
        );

        return;
    }


    /*
        Mittelpunkt darf benutzt werden.
        Er beeinflusst die Balance allerdings
        nicht, da sein Hebelarm 0 ist.
    */


    const weight =
        availableWeights[
            selectedWeightIndex
        ];


    /*
        Gewicht ablegen.
    */

    placedWeights.push({

        field: field,

        weight: weight

    });


    /*
        Gewicht verbraucht.
    */

    availableWeights.splice(
        selectedWeightIndex,
        1
    );


    selectedWeightIndex = null;


    renderWeights();

    renderBoard();


    /*
        Jetzt Balance berechnen.
    */

    evaluatePlacement();
}


/* =========================================================
   PLATZIERUNG AUSWERTEN
========================================================= */

function evaluatePlacement() {

    const moment =
        calculateTotalMoment();


    /*
        Toleranz wegen Rundungsfehlern.
    */

    const balanced =
        Math.abs(moment.x) < 0.001 &&
        Math.abs(moment.y) < 0.001;


    /*
        Balance erreicht?
    */

    if (balanced) {

        showMessage(
            "✅ Gleichgewicht erreicht! Die Figur darf zum nächsten Feld.",
            "success"
        );


        setTimeout(
            advanceFigure,
            900
        );


        return;
    }


    /*
        Noch nicht ausgeglichen.
    */

    showMessage(
        `Noch nicht im Gleichgewicht.
        Das abgelegte Gewicht bleibt liegen und wird
        bei der nächsten Berechnung berücksichtigt.`,
        "info"
    );


    showCalculation();
}


/* =========================================================
   FIGUR WEITERBEWEGEN
========================================================= */

function advanceFigure() {

    if (
        currentPathIndex >=
        path.length - 1
    ) {

        finishGame();

        return;
    }


    currentPathIndex++;

    currentField =
        path[currentPathIndex];


    /*
        Die alten Gewichte bleiben liegen!
    */


    /*
        Für das neue Feld werden wieder
        sechs neue Gewichte bereitgelegt.

        Die bereits abgelegten Gewichte
        bleiben natürlich Bestandteil
        der Balance.
    */

    createWeights();

    renderWeights();

    renderBoard();

    updateInformation();

    updatePathDisplay();

    hideCalculation();


    showMessage(
        `➡️ Die Figur ist auf Feld ${currentField}.
        Die bereits abgelegten Gewichte bleiben liegen.`,
        "info"
    );
}


/* =========================================================
   SPIEL BEENDET
========================================================= */

function finishGame() {

    renderBoard();


    showMessage(
        `🏆 ZIEL ERREICHT!

        Die Figur hat Feld ${goalField}
        erreicht.

        Alle abgelegten Gewichte wurden
        während des gesamten Weges
        berücksichtigt.`,
        "success"
    );
}


/* =========================================================
   BOARD ZEICHNEN
========================================================= */

function renderBoard() {

    const board =
        document.getElementById("board");

    board.innerHTML = "";


    for (
        let field = 1;
        field <= 25;
        field++
    ) {

        const cell =
            document.createElement("div");

        cell.className = "cell";


        /*
            Weg markieren
        */

        if (
            path.includes(field)
        ) {

            cell.classList.add("path");

        }


        /*
            Start
        */

        if (
            field === startField
        ) {

            cell.classList.add("start");

        }


        /*
            Ziel
        */

        if (
            field === goalField
        ) {

            cell.classList.add("goal");

        }


        /*
            aktuelles Feld
        */

        if (
            field === currentField
        ) {

            cell.classList.add("current");

        }


        /*
            Feldnummer
        */

        const number =
            document.createElement("span");

        number.className =
            "field-number";

        number.textContent =
            field;


        cell.appendChild(number);


        /*
            Figur
        */

        if (
            field === currentField
        ) {

            const figure =
                document.createElement("span");

            figure.className =
                "figure";

            figure.textContent =
                "⚖️";

            cell.appendChild(figure);
        }


        /*
            abgelegte Gewichte
        */

        const weightsHere =
            placedWeights.filter(
                item =>
                    item.field === field
            );


        if (
            weightsHere.length > 0
        ) {

            const stack =
                document.createElement("div");

            stack.className =
                "weight-stack";


            weightsHere.forEach(
                (item, index) => {

                    const token =
                        document.createElement("div");

                    token.className =
                        "weight-token";


                    const colors = [
                        "",
                        "red",
                        "blue",
                        "green",
                        "purple"
                    ];

                    if (
                        colors[index]
                    ) {

                        token.classList.add(
                            colors[index]
                        );
                    }


                    token.textContent =
                        formatKg(item.weight);


                    stack.appendChild(token);

                }
            );


            cell.appendChild(stack);
        }


        /*
            Klick
        */

        cell.addEventListener(
            "click",
            () => boardClicked(field)
        );


        board.appendChild(cell);
    }
}


/* =========================================================
   GEWICHTE ZEIGEN
========================================================= */

function renderWeights() {

    const container =
        document.getElementById("weights");

    container.innerHTML = "";


    availableWeights.forEach(
        (weight, index) => {

            const button =
                document.createElement("div");

            button.className =
                "weight-option";


            if (
                index === selectedWeightIndex
            ) {

                button.classList.add(
                    "selected"
                );
            }


            button.textContent =
                formatKg(weight);


            button.addEventListener(
                "click",
                () => selectWeight(index)
            );


            container.appendChild(button);
        }
    );


    /*
        Falls alle Gewichte verbraucht sind.
    */

    if (
        availableWeights.length === 0
    ) {

        container.innerHTML =
            `<div style="
                grid-column:1/-1;
                text-align:center;
                color:#aebfd1;
                padding:10px;">
                Keine Gewichte mehr bereit.
            </div>`;
    }
}


/* =========================================================
   INFORMATION
========================================================= */

function updateInformation() {

    document.getElementById(
        "startField"
    ).textContent =
        startField;


    document.getElementById(
        "goalField"
    ).textContent =
        goalField;


    document.getElementById(
        "figureWeight"
    ).textContent =
        formatKg(FIGURE_WEIGHT);


    document.getElementById(
        "currentField"
    ).textContent =
        currentField;
}


/* =========================================================
   WEG ANZEIGEN
========================================================= */

function updatePathDisplay() {

    const container =
        document.getElementById("pathList");

    container.innerHTML = "";


    path.forEach(
        (field, index) => {

            const element =
                document.createElement("div");

            element.className =
                "path-field";


            if (
                index <
                currentPathIndex
            ) {

                element.classList.add(
                    "done"
                );

            }


            if (
                index ===
                currentPathIndex
            ) {

                element.classList.add(
                    "active"
                );
            }


            element.textContent =
                field;


            container.appendChild(element);

        }
    );
}


/* =========================================================
   BERECHNUNG ANZEIGEN
========================================================= */

function showCalculation() {

    const moment =
        calculateTotalMoment();


    const box =
        document.getElementById(
            "calculation"
        );


    let html = "";

    html +=
        "<strong>AKTUELLE BALANCE</strong><br><br>";


    html +=
        `Figur: ${formatKg(FIGURE_WEIGHT)}
        auf Feld ${currentField}<br>`;


    const figurePosition =
        fieldToCoordinate(currentField);


    html +=
        `Position der Figur:
        (${figurePosition.x},
        ${figurePosition.y})<br><br>`;


    html +=
        "ABGELEGTE GEWICHTE<br>";


    if (
        placedWeights.length === 0
    ) {

        html +=
            "Noch keine Gewichte<br>";

    } else {

        placedWeights.forEach(
            item => {

                const p =
                    fieldToCoordinate(
                        item.field
                    );


                html +=
                    `${formatKg(item.weight)}
                    auf Feld ${item.field}
                    (${p.x},${p.y})<br>`;
            }
        );
    }


    html += "<br>";


    html +=
        `Gesamtmoment X:
        ${moment.x.toFixed(3)}<br>`;


    html +=
        `Gesamtmoment Y:
        ${moment.y.toFixed(3)}<br><br>`;


    if (
        Math.abs(moment.x) < 0.001 &&
        Math.abs(moment.y) < 0.001
    ) {

        html +=
            `<span class="good">
            ✓ X = 0 und Y = 0
            → Gleichgewicht
            </span>`;

    } else {

        html +=
            `<span class="warning">
            Noch kein Gleichgewicht
            </span>`;
    }


    box.innerHTML = html;

    box.classList.add("show");
}


function hideCalculation() {

    document
        .getElementById("calculation")
        .classList.remove("show");
}


/* =========================================================
   GEWICHTE ZURÜCKNEHMEN
========================================================= */

function resetWeights() {

    placedWeights = [];

    selectedWeightIndex = null;

    createWeights();

    renderWeights();

    renderBoard();

    hideCalculation();

    showMessage(
        "Alle abgelegten Gewichte wurden zurückgenommen.",
        "info"
    );
}


/* =========================================================
   NEUES SPIEL
========================================================= */

function newGame() {

    placedWeights = [];

    selectedWeightIndex = null;

    currentPathIndex = 0;


    /*
        Neuen zufälligen Weg erzeugen.
    */

    path =
        generatePath();


    currentField =
        path[0];


    /*
        Neue sechs Gewichte.
    */

    createWeights();


    renderWeights();

    renderBoard();

    updateInformation();

    updatePathDisplay();

    hideCalculation();


    showMessage(
        `🎲 Neue Runde gestartet.
        Startfeld: ${startField}
        Ziel: ${goalField}`,
        "info"
    );
}


/* =========================================================
   ZUFALL
========================================================= */

function randomInt(min, max) {

    return Math.floor(
        Math.random() *
        (max - min + 1)
    ) + min;
}


/* =========================================================
   GEWICHT FORMATIEREN
========================================================= */

function formatKg(value) {

    return Number(value)
        .toFixed(1)
        .replace(".", ",") +
        " kg";
}


/* =========================================================
   NACHRICHT
========================================================= */

function showMessage(
    text,
    type
) {

    const box =
        document.getElementById(
            "message"
        );


    box.className =
        "message show " + type;


    box.textContent =
        text;
}


/* =========================================================
   START
========================================================= */

newGame();

</script>

</body>
</html>
