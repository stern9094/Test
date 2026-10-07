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
    background: linear-gradient(135deg, #10253a, #173b55 50%, #0b1b2a);
    color: white;
}

.container {
    width: min(1100px, 96%);
    margin: auto;
    padding: 20px 0 40px;
}

h1 {
    text-align: center;
    margin: 0;
    font-size: 38px;
}

.subtitle {
    text-align: center;
    color: #b9cad8;
    margin: 5px 0 20px;
}

.game {
    display: grid;
    grid-template-columns: minmax(500px, 1fr) 340px;
    gap: 20px;
}

/* =========================
   PANEL
========================= */

.panel {
    background: rgba(255,255,255,.07);
    border: 1px solid rgba(255,255,255,.13);
    border-radius: 20px;
    padding: 18px;
    box-shadow: 0 20px 50px rgba(0,0,0,.3);
}

/* =========================
   SPIELBRETT
========================= */

.board-wrapper {
    position: relative;
    width: min(650px, 100%);
    margin: auto;
}

.board {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(4, 1fr);
    gap: 7px;

    width: 100%;
    aspect-ratio: 1;
}

.cell {
    position: relative;

    display: flex;
    align-items: center;
    justify-content: center;

    min-width: 0;
    min-height: 0;

    border-radius: 13px;

    background: linear-gradient(
        145deg,
        #344c60,
        #1e3040
    );

    border: 2px solid #50687b;

    cursor: pointer;

    transition:
        transform .15s,
        border-color .15s,
        box-shadow .15s,
        background .15s;

    overflow: visible;
}

.cell:hover {
    transform: scale(1.025);
    border-color: #8ddcff;
    z-index: 10;
}

.cell.start {
    border-color: #38bdf8;
}

.cell.goal {
    border-color: #4ade80;
}

.cell.current {
    background: linear-gradient(
        145deg,
        #7043bb,
        #4c1d95
    );

    border-color: #c4b5fd;

    box-shadow:
        0 0 24px rgba(139,92,246,.65);
}

.cell.path {
    background: linear-gradient(
        145deg,
        #29485e,
        #1b3346
    );
}

.cell.placeable {
    border-color: #facc15;

    box-shadow:
        inset 0 0 0 2px rgba(250,204,21,.15),
        0 0 18px rgba(250,204,21,.25);
}

/* =========================
   ROTER MITTELPUNKT
========================= */

.center-point {
    position: absolute;

    left: 50%;
    top: 50%;

    width: 30px;
    height: 30px;

    transform: translate(-50%, -50%);

    border-radius: 50%;

    background:
        radial-gradient(
            circle at 35% 30%,
            #ffb0b0,
            #ef4444 45%,
            #991b1b
        );

    border: 3px solid #fecaca;

    box-shadow:
        0 0 0 4px rgba(239,68,68,.16),
        0 0 22px rgba(239,68,68,.8);

    z-index: 50;

    pointer-events: none;
}

/* =========================
   FIGUR
========================= */

.figure {
    position: relative;
    z-index: 20;

    display: flex;
    flex-direction: column;
    align-items: center;

    font-size: 38px;

    filter:
        drop-shadow(0 4px 4px rgba(0,0,0,.45));
}

.figure-weight {
    margin-top: -5px;

    padding: 3px 7px;

    border-radius: 7px;

    background: rgba(0,0,0,.4);

    font-size: 10px;
    font-weight: bold;
}

/* =========================
   GEWICHTE AUF FELD
========================= */

.weight-stack {
    position: absolute;

    left: 50%;
    bottom: 5px;

    transform: translateX(-50%);

    display: flex;
    flex-direction: column;
    align-items: center;

    z-index: 15;

    pointer-events: none;
}

.weight-row {
    display: flex;
    justify-content: center;
}

.weight-block {
    width: 18px;
    height: 18px;

    margin: 1px;

    border-radius: 4px;

    background: linear-gradient(
        145deg,
        #bfdbfe,
        #60a5fa 50%,
        #2563eb
    );

    border: 1px solid #dbeafe;

    box-shadow:
        inset 1px 1px 2px rgba(255,255,255,.55),
        0 2px 3px rgba(0,0,0,.4);
}

.weight-stack.frozen .weight-block {
    background: linear-gradient(
        145deg,
        #bbf7d0,
        #4ade80 50%,
        #15803d
    );

    border-color: #dcfce7;

    box-shadow:
        0 0 8px rgba(34,197,94,.8);
}

/* =========================
   HAKEN
========================= */

.correct-check {
    position: absolute;

    right: 5px;
    top: 5px;

    width: 31px;
    height: 31px;

    display: flex;
    align-items: center;
    justify-content: center;

    border-radius: 50%;

    background: #16a34a;

    border: 2px solid #bbf7d0;

    font-size: 20px;
    font-weight: bold;

    z-index: 40;

    box-shadow:
        0 0 16px rgba(34,197,94,.9);
}

/* =========================
   RECHTE SEITE
========================= */

.status {
    padding: 13px;

    background: rgba(0,0,0,.2);

    border-radius: 12px;

    line-height: 1.5;
}

.status-title {
    color: #67e8f9;
    font-weight: bold;
    margin-bottom: 5px;
}

.info-grid {
    display: grid;

    grid-template-columns: 1fr 1fr;

    gap: 8px;

    margin-top: 13px;
}

.info {
    padding: 10px;

    background: rgba(0,0,0,.2);

    border-radius: 9px;
}

.info-label {
    font-size: 11px;
    color: #91a7ba;
}

.info-value {
    margin-top: 3px;

    font-size: 17px;
    font-weight: bold;
}

h2 {
    margin: 20px 0 10px;

    font-size: 18px;
}

/* =========================
   GEWICHT-AUSWAHL
========================= */

.weights {
    display: grid;

    grid-template-columns: repeat(3, 1fr);

    gap: 8px;
}

.weight-option {
    min-height: 110px;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    background:
        linear-gradient(
            145deg,
            #334155,
            #1e293b
        );

    border: 2px solid #64748b;

    border-radius: 11px;

    cursor: pointer;

    transition: .15s;
}

.weight-option:hover {
    transform: translateY(-2px);

    border-color: #60a5fa;
}

.weight-option.selected {
    border-color: #facc15;

    box-shadow:
        0 0 20px rgba(250,204,21,.55);
}

.weight-option.solution-weight {
    border-color: #22c55e;
}

.weight-label {
    margin-top: 6px;

    font-size: 11px;

    color: #cbd5e1;
}

/* =========================
   LÖSUNG
========================= */

.solution {
    margin-top: 14px;

    padding: 12px;

    border-radius: 10px;

    background: rgba(22,163,74,.12);

    border: 1px solid #22c55e;

    color: #bbf7d0;
}

.solution-title {
    color: #4ade80;
    font-weight: bold;
    margin-bottom: 5px;
}

/* =========================
   MELDUNGEN
========================= */

.message {
    display: none;

    margin-top: 12px;

    padding: 12px;

    border-radius: 10px;
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
   PROBE-BERECHNUNG
========================= */

.calculation {
    margin-top: 12px;

    padding: 12px;

    border-radius: 10px;

    background: #08111b;

    font-family: Consolas, monospace;

    font-size: 11px;

    line-height: 1.6;

    color: #cbd5e1;
}

/* =========================
   BUTTONS
========================= */

button {
    width: 100%;

    margin-top: 9px;

    padding: 12px;

    border: none;

    border-radius: 10px;

    color: white;

    font-size: 15px;
    font-weight: bold;

    cursor: pointer;
}

button:hover {
    filter: brightness(1.12);
}

.new-game {
    background:
        linear-gradient(
            135deg,
            #7c3aed,
            #5b21b6
        );
}

.reset {
    background:
        linear-gradient(
            135deg,
            #475569,
            #334155
        );
}

/* =========================
   RESPONSIVE
========================= */

@media(max-width:850px) {

    .game {
        grid-template-columns: 1fr;
    }

    h1 {
        font-size: 30px;
    }

    .board-wrapper {
        width: 100%;
    }
}

@media(max-width:500px) {

    .container {
        padding-top: 10px;
    }

    .panel {
        padding: 10px;
    }

    .weight-block {
        width: 14px;
        height: 14px;
    }

    .figure {
        font-size: 30px;
    }

    .center-point {
        width: 25px;
        height: 25px;
    }
}
</style>
</head>

<body>

<div class="container">

    <h1>⚖️ BALANCE-TEAM</h1>

    <div class="subtitle">
        Hilf der Figur, sicher zum Ziel zu kommen!
    </div>

    <div class="game">

        <!-- =========================
             SPIELBRETT
        ========================== -->

        <div class="panel">

            <div class="board-wrapper">

                <div
                    id="board"
                    class="board">
                </div>

                <!--
                    Der rote Punkt liegt genau
                    zwischen den vier mittleren
                    Feldern.
                -->

                <div
                    class="center-point"
                    title="Mittelpunkt">
                </div>

            </div>

        </div>


        <!-- =========================
             STEUERUNG
        ========================== -->

        <div class="panel">

            <div class="status">

                <div class="status-title">
                    🧒 Deine Aufgabe
                </div>

                Wähle einen Gewichtshaufen und
                lege ihn auf ein freies Feld.

                Die Gewichte bleiben liegen.

                Wenn die Balance stimmt,
                geht die Figur ein Feld weiter.
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
                        class="info-value">
                        2
                    </div>

                </div>


                <div class="info">

                    <div class="info-label">
                        Figur steht
                    </div>

                    <div
                        id="currentField"
                        class="info-value">
                        -
                    </div>

                </div>

            </div>


            <h2>
                🧱 Gewicht auswählen
            </h2>

            <div
                id="weights"
                class="weights">
            </div>


            <!-- =====================
                 PROBE-LÖSUNG
            ====================== -->

            <div
                id="solutionBox"
                class="solution">

                <div class="solution-title">
                    🔎 PROBE-LÖSUNG
                </div>

                <div
                    id="solutionText">
                </div>

            </div>


            <button
                class="new-game"
                onclick="newGame()">

                🎲 Neue Runde

            </button>


            <button
                class="reset"
                onclick="removeWrongWeights()">

                ↩ Falsche Gewichte entfernen

            </button>


            <div
                id="message"
                class="message">
            </div>


            <div
                id="calculation"
                class="calculation">
            </div>

        </div>

    </div>

</div>


<script>

/* =========================================================
   SPIEL-EINSTELLUNGEN
========================================================= */

const FIGURE_WEIGHT = 2;

const ALL_WEIGHTS = [1,2,3,4,5,6];

const BOARD_SIZE = 4;

const SHOW_SOLUTION = true;


/* =========================================================
   SPIELZUSTAND
========================================================= */

let path = [];

let currentIndex = 0;

let currentField = null;

let startField = null;

let goalField = null;

let placedWeights = [];

let availableWeights = [];

let selectedWeight = null;

let currentSolution = null;

let locked = false;


/* =========================================================
   FELD → KOORDINATE
=========================================================

   Das Koordinatensystem sieht intern so aus:

       -1.5  -0.5   0.5   1.5

   -1.5
   -0.5
    0.5
    1.5

   Der rote Mittelpunkt ist:

       X = 0
       Y = 0

   Für das Kind wird das NICHT angezeigt.
========================================================= */

function getCoordinate(field) {

    const index = field - 1;

    const row = Math.floor(index / 4);

    const col = index % 4;

    return {
        x: col - 1.5,
        y: row - 1.5
    };
}


/* =========================================================
   GEGENÜBERLIEGENDES FELD
=========================================================

   1 ↔ 16
   2 ↔ 15
   3 ↔ 14
   4 ↔ 13

   5 ↔ 12
   6 ↔ 11
   7 ↔ 10
   8 ↔  9
========================================================= */

function oppositeField(field) {

    return 17 - field;
}


/* =========================================================
   NACHBARFELDER
========================================================= */

function getNeighbors(field) {

    const index = field - 1;

    const row = Math.floor(index / 4);

    const col = index % 4;

    const directions = [
        [1,0],
        [-1,0],
        [0,1],
        [0,-1]
    ];

    const result = [];

    for (const [dr,dc] of directions) {

        const r = row + dr;

        const c = col + dc;

        if (
            r >= 0 &&
            r < 4 &&
            c >= 0 &&
            c < 4
        ) {

            result.push(
                r * 4 + c + 1
            );
        }
    }

    return result;
}


/* =========================================================
   ZUFÄLLIGER WEG
========================================================= */

function generatePath() {

    const start =
        randomInt(1,4);

    const goal =
        randomInt(13,16);

    const result = [start];

    let current = start;

    const visited =
        new Set([start]);

    let safety = 0;

    while (
        current !== goal &&
        safety < 100
    ) {

        safety++;

        let neighbors =
            getNeighbors(current)
            .filter(
                f => !visited.has(f)
            );

        if (
            neighbors.length === 0
        ) {
            return generatePath();
        }


        /*
           Wenn das Zielfeld direkt
           erreichbar ist, nehmen wir es.
        */

        if (
            neighbors.includes(goal)
        ) {

            current = goal;

        } else {

            /*
               Nach unten gerichtete
               Schritte werden bevorzugt.
            */

            const currentRow =
                Math.floor(
                    (current - 1) / 4
                );

            const down =
                neighbors.filter(
                    f =>
                        Math.floor(
                            (f - 1) / 4
                        ) >= currentRow
                );

            if (
                down.length &&
                Math.random() < .75
            ) {

                current =
                    randomChoice(down);

            } else {

                current =
                    randomChoice(neighbors);
            }
        }

        result.push(current);

        visited.add(current);
    }


    if (
        current !== goal
    ) {

        return generatePath();
    }


    return result;
}


/* =========================================================
   BALANCE-BERECHNUNG
=========================================================

   WICHTIG:

   Hier wird KEINE komplizierte Hebelphysik
   verwendet.

   Jedes Gewicht wirkt vom Mittelpunkt
   aus in Richtung seines Feldes.

   Die Figur zählt als 2 Gewichtseinheiten.

   Die Summe aller Wirkungen wird berechnet.

   Ein neues Gegengewicht muss auf der
   gegenüberliegenden Seite liegen.

========================================================= */

function calculateBalance(
    additionalWeight = 0,
    additionalField = null
) {

    let x = 0;

    let y = 0;


    /*
       Wirkung der Figur
    */

    const figure =
        getCoordinate(currentField);

    x +=
        figure.x *
        FIGURE_WEIGHT;

    y +=
        figure.y *
        FIGURE_WEIGHT;


    /*
       Alle bereits liegenden Gewichte.
    */

    for (
        const item of placedWeights
    ) {

        const position =
            getCoordinate(item.field);

        x +=
            position.x *
            item.weight;

        y +=
            position.y *
            item.weight;
    }


    /*
       Testgewicht.
    */

    if (
        additionalField !== null &&
        additionalWeight > 0
    ) {

        const position =
            getCoordinate(
                additionalField
            );

        x +=
            position.x *
            additionalWeight;

        y +=
            position.y *
            additionalWeight;
    }


    return {
        x,
        y
    };
}


/* =========================================================
   FEHLER DER BALANCE
========================================================= */

function balanceError(balance) {

    return Math.sqrt(
        balance.x * balance.x +
        balance.y * balance.y
    );
}


/* =========================================================
   LÖSUNG SUCHEN
========================================================= */

function findSolution() {

    const candidates = [];


    /*
       Alle freien Felder ausprobieren.
    */

    for (
        let field = 1;
        field <= 16;
        field++
    ) {

        if (
            field === currentField
        ) {
            continue;
        }


        /*
           Auf einem Feld darf nur
           ein Gewichtshaufen liegen.
        */

        if (
            placedWeights.some(
                item =>
                    item.field === field
            )
        ) {
            continue;
        }


        /*
           Nur Felder auf der
           gegenüberliegenden Seite
           sind sinnvoll.
        */

        const figurePosition =
            getCoordinate(currentField);

        const fieldPosition =
            getCoordinate(field);


        /*
           Das Testgewicht muss
           grundsätzlich in die
           entgegengesetzte Richtung
           zeigen.
        */

        const directionDot =
            figurePosition.x *
            fieldPosition.x +

            figurePosition.y *
            fieldPosition.y;


        if (
            directionDot > 0
        ) {
            continue;
        }


        for (
            const weight of ALL_WEIGHTS
        ) {

            const balance =
                calculateBalance(
                    weight,
                    field
                );

            const error =
                balanceError(balance);


            candidates.push({
                field,
                weight,
                error
            });
        }
    }


    if (
        candidates.length === 0
    ) {

        return null;
    }


    /*
       Beste Lösung zuerst.
    */

    candidates.sort(
        (a,b) =>
            a.error - b.error
    );


    /*
       Wir nehmen die mathematisch
       beste Gegenbalance.
    */

    return candidates[0];
}


/* =========================================================
   DREI GEWICHTE ERZEUGEN
========================================================= */

function createWeightChoices() {

    currentSolution =
        findSolution();


    /*
       Falls keine sinnvolle Lösung
       gefunden wurde, verwenden wir
       eine sichere Gegenseite.
    */

    if (
        !currentSolution
    ) {

        currentSolution =
            createFallbackSolution();
    }


    if (
        !currentSolution
    ) {

        return;
    }


    const choices = [
        currentSolution.weight
    ];


    /*
       Zwei falsche Gewichte erzeugen.
    */

    while (
        choices.length < 3
    ) {

        const candidate =
            randomChoice(
                ALL_WEIGHTS
            );

        if (
            !choices.includes(candidate)
        ) {

            choices.push(candidate);
        }
    }


    availableWeights =
        shuffle(choices);

    selectedWeight = null;
}


/* =========================================================
   FALLBACK-LÖSUNG
========================================================= */

function createFallbackSolution() {

    const possibleFields = [];


    for (
        let field = 1;
        field <= 16;
        field++
    ) {

        if (
            field === currentField
        ) {
            continue;
        }

        if (
            placedWeights.some(
                item =>
                    item.field === field
            )
        ) {
            continue;
        }

        possibleFields.push(field);
    }


    if (
        possibleFields.length === 0
    ) {
        return null;
    }


    /*
       Gegenüber der Figur bevorzugen.
    */

    const opposite =
        oppositeField(currentField);


    if (
        possibleFields.includes(opposite)
    ) {

        return {
            field: opposite,
            weight: FIGURE_WEIGHT,
            error: 0
        };
    }


    return {
        field:
            randomChoice(
                possibleFields
            ),

        weight:
            FIGURE_WEIGHT,

        error: 0
    };
}


/* =========================================================
   FELD ANKLICKEN
========================================================= */

function clickField(field) {

    if (locked) {
        return;
    }


    /*
       Liegt bereits ein Gewicht dort?
    */

    const existing =
        placedWeights.find(
            item =>
                item.field === field
        );


    if (existing) {

        /*
           Eingefrorene Gewichte
           können nicht mehr entfernt
           werden.
        */

        if (
            existing.frozen
        ) {

            showMessage(
                "🔒 Dieses Gewicht ist richtig und bleibt liegen!",
                "info"
            );

            return;
        }


        /*
           Falsches Gewicht entfernen.
        */

        placedWeights =
            placedWeights.filter(
                item =>
                    item !== existing
            );

        selectedWeight = null;

        createWeightChoices();

        render();

        showMessage(
            "↩ Das Gewicht wurde zurückgenommen.",
            "info"
        );

        return;
    }


    if (
        selectedWeight === null
    ) {

        showMessage(
            "🧱 Wähle zuerst einen Gewichtshaufen.",
            "error"
        );

        return;
    }


    /*
       Gewicht ablegen.
    */

    const newWeight = {

        field: field,

        weight: selectedWeight,

        frozen: false
    };


    placedWeights.push(
        newWeight
    );


    /*
       Richtig?
    */

    const correct =
        field ===
            currentSolution.field &&

        selectedWeight ===
            currentSolution.weight;


    selectedWeight = null;


    if (correct) {

        newWeight.frozen = true;

        locked = true;

        render();

        showCorrectMark(field);

        showMessage(
            "🎉 Richtig! Die Balance stimmt!",
            "success"
        );


        setTimeout(
            () => {

                removeCorrectMark();

                moveFigure();

            },
            1000
        );

    } else {

        render();

        showMessage(
            "🤔 Noch nicht. Die Figur bleibt stehen.",
            "error"
        );
    }
}


/* =========================================================
   FIGUR BEWEGT SICH
========================================================= */

function moveFigure() {

    if (
        currentIndex >=
        path.length - 1
    ) {

        finishGame();

        return;
    }


    /*
       NUR die Figur wird bewegt.

       Die Gewichte bleiben exakt
       dort liegen, wo sie waren.
    */

    currentIndex++;

    currentField =
        path[currentIndex];


    locked = false;


    /*
       Jetzt wird die komplette
       Balance mit der neuen
       Figurenposition neu berechnet.
    */

    createWeightChoices();

    render();


    if (
        currentField === goalField
    ) {

        finishGame();

        return;
    }


    showMessage(
        "➡️ Die Figur ist weitergegangen. Die Gewichte bleiben liegen!",
        "info"
    );
}


/* =========================================================
   RICHTIGES GEWICHT EINFRIEREN
========================================================= */

function showCorrectMark(field) {

    const cells =
        document.querySelectorAll(".cell");


    cells.forEach(
        (cell,index) => {

            if (
                index + 1 === field
            ) {

                const mark =
                    document.createElement(
                        "div"
                    );

                mark.className =
                    "correct-check";

                mark.textContent = "✓";

                cell.appendChild(mark);
            }
        }
    );
}


function removeCorrectMark() {

    document
        .querySelectorAll(
            ".correct-check"
        )
        .forEach(
            item =>
                item.remove()
        );
}


/* =========================================================
   FALSCHE GEWICHTE ENTFERNEN
========================================================= */

function removeWrongWeights() {

    if (locked) {
        return;
    }


    placedWeights =
        placedWeights.filter(
            item =>
                item.frozen
        );


    createWeightChoices();

    render();

    showMessage(
        "↩ Falsche Gewichte wurden entfernt.",
        "info"
    );
}


/* =========================================================
   SPIEL FERTIG
========================================================= */

function finishGame() {

    locked = true;

    showMessage(
        "🏆 SUPER! Die Figur hat das Ziel erreicht!",
        "success"
    );
}


/* =========================================================
   GEWICHTSSTAPEL DARSTELLEN
=========================================================

   1:
       ■

   2:
       ■
       ■

   3:
        ■
       ■ ■

   4:
       ■ ■
       ■ ■

   5:
        ■
       ■ ■
       ■ ■

   6:
       ■ ■ ■
       ■ ■ ■
========================================================= */

function createWeightStack(
    count,
    frozen = false
) {

    const stack =
        document.createElement(
            "div"
        );

    stack.className =
        "weight-stack";


    if (frozen) {
        stack.classList.add(
            "frozen"
        );
    }


    let rows;


    switch(count) {

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
            rows = [1,2,2];
            break;

        case 6:
            rows = [3,3];
            break;

        default:
            rows = [1];
    }


    rows.forEach(
        rowCount => {

            const row =
                document.createElement(
                    "div"
                );

            row.className =
                "weight-row";


            for (
                let i = 0;
                i < rowCount;
                i++
            ) {

                const block =
                    document.createElement(
                        "div"
                    );

                block.className =
                    "weight-block";

                row.appendChild(
                    block
                );
            }


            stack.appendChild(
                row
            );
        }
    );


    return stack;
}


/* =========================================================
   SPIELBRETT DARSTELLEN
========================================================= */

function render() {

    const board =
        document.getElementById(
            "board"
        );

    board.innerHTML = "";


    /*
       Felder hervorheben,
       auf denen das Kind ein
       Gewicht ablegen kann.
    */

    const placeable = [];


    if (
        selectedWeight !== null
    ) {

        for (
            let field = 1;
            field <= 16;
            field++
        ) {

            if (
                field === currentField
            ) {
                continue;
            }

            if (
                placedWeights.some(
                    item =>
                        item.field === field
                )
            ) {
                continue;
            }

            placeable.push(field);
        }
    }


    for (
        let field = 1;
        field <= 16;
        field++
    ) {

        const cell =
            document.createElement(
                "div"
            );

        cell.className = "cell";


        /*
           Start
        */

        if (
            field === startField
        ) {

            cell.classList.add(
                "start"
            );
        }


        /*
           Ziel
        */

        if (
            field === goalField
        ) {

            cell.classList.add(
                "goal"
            );
        }


        /*
           Aktuelle Figur
        */

        if (
            field === currentField
        ) {

            cell.classList.add(
                "current"
            );
        }


        /*
           Weg
        */

        if (
            path.includes(field)
        ) {

            cell.classList.add(
                "path"
            );
        }


        /*
           Mögliche Ablagefelder
        */

        if (
            placeable.includes(field)
        ) {

            cell.classList.add(
                "placeable"
            );
        }


        /*
           Figur darstellen
        */

        if (
            field === currentField
        ) {

            const figure =
                document.createElement(
                    "div"
                );

            figure.className =
                "figure";

            figure.innerHTML = `
                🧒
                <div class="figure-weight">
                    2
                </div>
            `;

            cell.appendChild(
                figure
            );
        }


        /*
           Gewichte darstellen
        */

        const fieldWeights =
            placedWeights.filter(
                item =>
                    item.field === field
            );


        for (
            const item of fieldWeights
        ) {

            const stack =
                createWeightStack(
                    item.weight,
                    item.frozen
                );

            cell.appendChild(
                stack
            );
        }


        cell.addEventListener(
            "click",
            () =>
                clickField(field)
        );


        board.appendChild(
            cell
        );
    }


    updateInfo();

    renderChoices();

    renderSolution();

    showCalculation();
}


/* =========================================================
   AUSWAHL DER 3 GEWICHTE
========================================================= */

function renderChoices() {

    const container =
        document.getElementById(
            "weights"
        );

    container.innerHTML = "";


    for (
        const weight of availableWeights
    ) {

        const option =
            document.createElement(
                "div"
            );

        option.className =
            "weight-option";


        if (
            selectedWeight === weight
        ) {

            option.classList.add(
                "selected"
            );
        }


        /*
           Probeversion:
           richtige Auswahl grün
        */

        if (
            SHOW_SOLUTION &&
            currentSolution &&
            weight ===
                currentSolution.weight
        ) {

            option.classList.add(
                "solution-weight"
            );
        }


        const stack =
            createWeightStack(
                weight
            );


        option.appendChild(
            stack
        );


        const label =
            document.createElement(
                "div"
            );

        label.className =
            "weight-label";

        label.textContent =
            weight === 1
                ? "1 Gewicht"
                : `${weight} Gewichte`;


        option.appendChild(
            label
        );


        option.addEventListener(
            "click",
            () => {

                if (locked) {
                    return;
                }


                selectedWeight =
                    weight;


                render();


                showMessage(
                    "👆 Jetzt ein Feld auswählen.",
                    "info"
                );
            }
        );


        container.appendChild(
            option
        );
    }
}


/* =========================================================
   INFORMATION
========================================================= */

function updateInfo() {

    document.getElementById(
        "startField"
    ).textContent =
        startField;

    document.getElementById(
        "goalField"
    ).textContent =
        goalField;

    document.getElementById(
        "currentField"
    ).textContent =
        currentField;
}


/* =========================================================
   LÖSUNG
========================================================= */

function renderSolution() {

    const box =
        document.getElementById(
            "solutionText"
        );


    if (
        !SHOW_SOLUTION ||
        !currentSolution
    ) {

        box.textContent = "";

        return;
    }


    const opposite =
        oppositeField(
            currentField
        );


    box.innerHTML = `
        Figur steht auf Feld
        <strong>${currentField}</strong>.<br><br>

        Lösung:
        <strong>
            ${currentSolution.weight} Gewichte
        </strong>
        auf Feld
        <strong>
            ${currentSolution.field}
        </strong>.

        <br><br>

        Gegenüberliegend zu Feld
        ${currentField} ist Feld
        ${opposite}.
    `;
}


/* =========================================================
   PROBE-BERECHNUNG
========================================================= */

function showCalculation() {

    const box =
        document.getElementById(
            "calculation"
        );


    if (
        !currentField
    ) {
        return;
    }


    const balance =
        calculateBalance();


    let html =
        "<strong>PROBE-BERECHNUNG</strong><br><br>";


    html +=
        `Figur = 2 Gewichte<br>`;

    html +=
        `Figur auf Feld ${currentField}<br><br>`;


    if (
        placedWeights.length === 0
    ) {

        html +=
            "Noch keine Gewichte liegen.<br>";

    } else {

        html +=
            "<strong>Bereits liegende Gewichte:</strong><br>";


        for (
            const item of placedWeights
        ) {

            html +=
                `${item.frozen ? "✓" : "○"}
                 ${item.weight}
                 → Feld ${item.field}<br>`;
        }
    }


    html += "<br>";

    html +=
        `Balance X = ${balance.x.toFixed(2)}<br>`;

    html +=
        `Balance Y = ${balance.y.toFixed(2)}<br>`;

    html +=
        `Abweichung = ${balanceError(balance).toFixed(2)}<br>`;


    if (
        currentSolution
    ) {

        html += "<br>";

        html +=
            `<strong>
                Richtige Lösung:
            </strong><br>`;

        html +=
            `${currentSolution.weight}
             Gewichte →
             Feld ${currentSolution.field}`;
    }


    box.innerHTML =
        html;
}


/* =========================================================
   MELDUNG
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
        `message show ${type}`;

    box.textContent =
        text;
}


/* =========================================================
   NEUES SPIEL
========================================================= */

function newGame() {

    locked = false;

    placedWeights = [];

    selectedWeight = null;

    currentIndex = 0;


    path =
        generatePath();


    startField =
        path[0];


    goalField =
        path[path.length - 1];


    currentField =
        startField;


    createWeightChoices();

    render();


    showMessage(
        "🎲 Neue Runde! Wähle einen Gewichtshaufen.",
        "info"
    );
}


/* =========================================================
   ZUFALL
========================================================= */

function randomInt(min,max) {

    return Math.floor(
        Math.random() *
        (max - min + 1)
    ) + min;
}


function randomChoice(array) {

    return array[
        randomInt(
            0,
            array.length - 1
        )
    ];
}


/* =========================================================
   MISCHEN
========================================================= */

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
