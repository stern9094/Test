<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>BALANCE-TEAM – 4×4 Kugel-Version</title>

<style>

* {
    box-sizing: border-box;
}

body {
    margin: 0;
    min-height: 100vh;

    font-family:
        Arial,
        Helvetica,
        sans-serif;

    color: white;

    background:
        radial-gradient(
            circle at top,
            #29445f,
            #172638 45%,
            #0b111a 100%
        );
}

.container {
    width: min(1150px, 96%);
    margin: auto;
    padding: 22px 0 40px;
}

h1 {
    margin: 0;
    text-align: center;
    font-size: 38px;
    letter-spacing: 3px;
}

.subtitle {
    text-align: center;
    color: #aebfd1;
    margin: 6px 0 22px;
}

.game-layout {
    display: grid;
    grid-template-columns:
        minmax(450px, 1fr)
        360px;

    gap: 22px;
}

.panel {
    background: rgba(255,255,255,.075);

    border:
        1px solid
        rgba(255,255,255,.12);

    border-radius: 18px;

    padding: 18px;

    box-shadow:
        0 20px 50px
        rgba(0,0,0,.28);

    backdrop-filter: blur(8px);
}


/* =========================================================
   SPIELFELD
========================================================= */

.board {

    width: min(650px, 100%);

    aspect-ratio: 1;

    margin: auto;

    display: grid;

    grid-template-columns:
        repeat(4, 1fr);

    grid-template-rows:
        repeat(4, 1fr);

    gap: 6px;
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

    border:
        2px solid
        #536b82;

    border-radius: 10px;

    cursor: pointer;

    transition:
        .15s;

    user-select: none;
}

.cell:hover {
    transform: scale(1.025);
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
        0 0 25px
        rgba(139,92,246,.65);

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


/* mögliche Ablagefelder */

.cell.placeable {

    border-color: #facc15;

    background:
        linear-gradient(
            145deg,
            #3b4b43,
            #26382f
        );

    box-shadow:
        inset 0 0 0 2px
        rgba(250,204,21,.18),

        0 0 12px
        rgba(250,204,21,.15);
}

.cell.placeable:hover {

    border-color: #fde047;

    box-shadow:
        0 0 20px
        rgba(250,204,21,.35);

    transform: scale(1.035);
}


/* richtiges Lösungsfeld */

.cell.solution-field {

    border-color: #22c55e;

    box-shadow:
        0 0 20px
        rgba(34,197,94,.55);

}


/* falsch */

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


/* =========================================================
   FIGUR
========================================================= */

.figure {

    display: flex;

    flex-direction: column;

    align-items: center;

    justify-content: center;

    gap: 2px;

    font-size: 30px;

    filter:
        drop-shadow(
            0 4px 5px
            rgba(0,0,0,.4)
        );

    z-index: 5;
}

.figure-label {

    font-size: 12px;

    font-weight: bold;

    color: #ffffff;
}


/* =========================================================
   KUGELN
========================================================= */

.balls {

    display: flex;

    justify-content: center;

    flex-wrap: wrap;

    gap: 3px;

    max-width: 80px;
}

.ball {

    width: 14px;
    height: 14px;

    border-radius: 50%;

    background:
        radial-gradient(
            circle at 30% 25%,
            #ffffff,
            #dbeafe 35%,
            #60a5fa 70%,
            #1d4ed8
        );

    border:
        1px solid
        #bfdbfe;

    box-shadow:
        0 2px 4px
        rgba(0,0,0,.4);
}


/* Kugeln auf Spielfeld */

.weight-stack {

    position: absolute;

    bottom: 6px;
    right: 6px;

    display: flex;

    flex-direction: column;

    align-items: flex-end;

    gap: 2px;

    z-index: 6;
}

.weight-token {

    display: flex;

    align-items: center;

    gap: 3px;

    background:
        rgba(15,23,42,.92);

    border:
        1px solid
        #60a5fa;

    border-radius: 6px;

    padding:
        3px 5px;

    font-size: 10px;

    font-weight: bold;
}


/* eingefrorenes Gewicht */

.weight-token.frozen {

    background:
        #166534;

    border-color:
        #4ade80;

    box-shadow:
        0 0 9px
        rgba(34,197,94,.45);
}


/* =========================================================
   HAKEN
========================================================= */

.correct-check {

    position: absolute;

    top: 50%;
    left: 50%;

    transform:
        translate(-50%, -50%);

    width: 52px;
    height: 52px;

    display: flex;

    align-items: center;
    justify-content: center;

    border-radius: 50%;

    background: #16a34a;

    border:
        3px solid
        #bbf7d0;

    color: white;

    font-size: 34px;

    font-weight: bold;

    z-index: 20;

    box-shadow:
        0 0 25px
        rgba(34,197,94,.9);
}


/* =========================================================
   STATUS
========================================================= */

.status {

    background:
        rgba(0,0,0,.2);

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


/* =========================================================
   INFO
========================================================= */

.info-grid {

    display: grid;

    grid-template-columns:
        1fr 1fr;

    gap: 9px;
}

.info {

    background:
        rgba(0,0,0,.18);

    padding: 11px;

    border-radius: 9px;
}

.info-label {

    color: #8fa4b9;

    font-size: 12px;
}

.info-value {

    font-size: 18px;

    font-weight: bold;

    margin-top: 3px;
}


/* =========================================================
   GEWICHTSAUSWAHL
========================================================= */

h2 {

    font-size: 18px;

    margin:
        20px 0 10px;
}

.weights {

    display: grid;

    grid-template-columns:
        repeat(3,1fr);

    gap: 8px;
}

.weight-option {

    padding: 12px 5px;

    border-radius: 9px;

    background:
        linear-gradient(
            145deg,
            #334155,
            #1e293b
        );

    border:
        2px solid
        #64748b;

    color: white;

    font-weight: bold;

    text-align: center;

    cursor: pointer;

    transition: .15s;
}

.weight-option:hover {

    transform:
        translateY(-2px);

    border-color:
        #60a5fa;
}

.weight-option.selected {

    border-color:
        #facc15;

    box-shadow:
        0 0 20px
        rgba(250,204,21,.55);

    transform:
        scale(1.04);
}


/* richtige Auswahl in Probe */

.weight-option.solution-weight {

    border-color:
        #22c55e;

    box-shadow:
        0 0 18px
        rgba(34,197,94,.6);
}


/* =========================================================
   BUTTONS
========================================================= */

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

button:hover {
    filter: brightness(1.12);
}


/* =========================================================
   MELDUNGEN
========================================================= */

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

    background:
        rgba(14,116,144,.2);

    border:
        1px solid
        #22d3ee;
}

.message.success {

    background:
        rgba(22,163,74,.2);

    border:
        1px solid
        #22c55e;
}

.message.error {

    background:
        rgba(220,38,38,.2);

    border:
        1px solid
        #ef4444;
}


/* =========================================================
   LÖSUNGSANZEIGE
========================================================= */

.solution-box {

    margin-top: 15px;

    padding: 13px;

    border-radius: 10px;

    background:
        rgba(22,163,74,.12);

    border:
        1px solid
        #22c55e;

    color:
        #bbf7d0;
}

.solution-title {

    color:
        #4ade80;

    font-weight:
        bold;

    margin-bottom:
        6px;
}


/* =========================================================
   BERECHNUNG
========================================================= */

.calculation {

    margin-top: 15px;

    padding: 13px;

    border-radius: 10px;

    background:
        #09121d;

    font-family:
        Consolas,
        monospace;

    font-size: 12px;

    line-height: 1.65;
}


/* =========================================================
   RESPONSIVE
========================================================= */

@media(max-width:850px) {

    .game-layout {
        grid-template-columns: 1fr;
    }

    h1 {
        font-size: 30px;
    }

    .board {
        width: 100%;
    }
}

</style>
</head>


<body>

<div class="container">

    <h1>⚖️ BALANCE-TEAM</h1>

    <div class="subtitle">
        Probeversion – 4 × 4 Kugel-Balance
    </div>


    <div class="game-layout">


        <!-- =================================================
             SPIELFELD
        ================================================== -->

        <div class="panel">

            <div
                id="board"
                class="board">
            </div>

        </div>


        <!-- =================================================
             STEUERUNG
        ================================================== -->

        <div class="panel">


            <div class="status">

                <div class="status-title">
                    Aufgabe
                </div>

                <div>
                    Wähle eine Kugelanzahl und
                    lege sie auf ein hervorgehobenes Feld.
                </div>

                <br>

                <div>
                    <strong>Probe:</strong>
                    Die aktuelle Lösung wird rechts angezeigt.
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
                        Waage
                    </div>

                    <div
                        id="figureWeight"
                        class="info-value">
                    </div>

                </div>


                <div class="info">

                    <div class="info-label">
                        Position
                    </div>

                    <div
                        id="currentField"
                        class="info-value">
                        -
                    </div>

                </div>

            </div>


            <h2>
                ⚪ Kugeln auswählen
            </h2>


            <div
                id="weights"
                class="weights">
            </div>


            <!-- =================================================
                 PROBE-LÖSUNG
            ================================================== -->

            <div
                id="solutionBox"
                class="solution-box">

                <div class="solution-title">
                    🔎 PROBE-LÖSUNG
                </div>

                <div
                    id="solutionText">
                </div>

            </div>


            <button
                class="new"
                onclick="newGame()">

                🎲 Neue Runde

            </button>


            <button
                class="reset"
                onclick="resetNonFrozenWeights()">

                ↩ Falsche Gewichte zurücknehmen

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
   EINSTELLUNG
========================================================= */

const SHOW_SOLUTION = true;


/* =========================================================
   SPIELKONSTANTEN
========================================================= */

const SIZE = 4;

/*
    4×4 Spielfeld.

    Die geometrische Mitte liegt zwischen
    den Feldern 6, 7, 10 und 11.

    Wir rechnen daher mit Koordinaten:

    -1.5  -0.5   0.5   1.5
    -1.5  -0.5   0.5   1.5
    -1.5  -0.5   0.5   1.5
    -1.5  -0.5   0.5   1.5
*/

const FIGURE_BALLS = 2;


/*
    Nur ganze Kugelzahlen.
*/

const POSSIBLE_WEIGHTS = [

    1,
    2,
    3,
    4,
    5,
    6

];


/* =========================================================
   SPIELZUSTAND
========================================================= */

let path = [];

let currentPathIndex = 0;

let currentField = null;

let startField = null;

let goalField = null;


/*
    Gewichte, die bereits liegen.

    frozen = true bedeutet:
    Dieses Gewicht ist fest eingefroren.
*/

let placedWeights = [];


/*
    Die drei Auswahlmöglichkeiten.
*/

let availableWeights = [];


/*
    Aktuell ausgewähltes Gewicht.
*/

let selectedWeightIndex = null;


/*
    Aktuelle Lösung.
*/

let currentSolution = null;


/*
    Während der 1-Sekunden-Animation
    wird das Spiel gesperrt.
*/

let gameLocked = false;


/* =========================================================
   FELD → KOORDINATE
========================================================= */

function fieldToCoordinate(field) {

    const index =
        field - 1;

    const row =
        Math.floor(
            index / SIZE
        );

    const col =
        index % SIZE;


    return {

        x: col - 1.5,

        y: row - 1.5

    };
}


/* =========================================================
   KOORDINATE → FELD
========================================================= */

function coordinateToField(x,y) {

    const col =
        Math.round(
            x + 1.5
        );

    const row =
        Math.round(
            y + 1.5
        );


    if (
        col < 0 ||
        col >= SIZE ||
        row < 0 ||
        row >= SIZE
    ) {

        return null;
    }


    return (
        row * SIZE +
        col +
        1
    );
}


/* =========================================================
   NACHBARN
========================================================= */

function getNeighbors(field) {

    const p =
        fieldToCoordinate(
            field
        );


    /*
        Da die Koordinaten bei einem 4×4
        Feld Halbwerte besitzen, verwenden
        wir einfach Zeile/Spalte.
    */

    const index =
        field - 1;

    const row =
        Math.floor(
            index / SIZE
        );

    const col =
        index % SIZE;


    const result = [];


    const directions = [

        [1,0],

        [-1,0],

        [0,1],

        [0,-1]

    ];


    directions.forEach(

        ([dr,dc]) => {

            const r =
                row + dr;

            const c =
                col + dc;


            if (
                r >= 0 &&
                r < SIZE &&
                c >= 0 &&
                c < SIZE
            ) {

                result.push(
                    r * SIZE +
                    c +
                    1
                );

            }

        }

    );


    return result;
}


/* =========================================================
   ZUFÄLLIGER WEG
========================================================= */

function generatePath() {

    startField =
        randomInt(
            1,
            4
        );


    goalField =
        randomInt(
            13,
            16
        );


    const result = [

        startField

    ];


    let current =
        startField;


    const used =
        new Set(result);


    let safety = 0;


    while (

        current !== goalField &&

        safety < 300

    ) {

        safety++;


        let neighbors =
            getNeighbors(
                current
            )
            .filter(
                field =>
                    !used.has(field)
            );


        if (
            neighbors.length === 0
        ) {

            return generatePath();

        }


        /*
            Ziel bevorzugen, wenn es
            direkt erreichbar ist.
        */

        if (
            neighbors.includes(
                goalField
            )
        ) {

            current =
                goalField;

        } else {

            /*
                Bewegung nach unten bevorzugen.
            */

            const currentRow =
                Math.floor(
                    (current - 1) / SIZE
                );


            const down =
                neighbors.filter(

                    field =>
                        Math.floor(
                            (field - 1) /
                            SIZE
                        ) >= currentRow

                );


            if (
                down.length > 0 &&
                Math.random() < 0.75
            ) {

                current =
                    down[
                        randomInt(
                            0,
                            down.length - 1
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

        }


        result.push(
            current
        );

        used.add(
            current
        );
    }


    if (
        current !== goalField
    ) {

        return generatePath();

    }


    return result;
}


/* =========================================================
   BALANCE-ACHSE
========================================================= */

/*
    Die Waage arbeitet zweidimensional.

    Die aktuelle Figur bestimmt,
    ob die horizontale oder vertikale
    Richtung stärker berücksichtigt wird.

    So bleibt die Bedienung einfach,
    die Berechnung ist aber tatsächlich
    räumlich.
*/

function getBalanceAxis() {

    const p =
        fieldToCoordinate(
            currentField
        );


    if (
        Math.abs(p.x) >=
        Math.abs(p.y)
    ) {

        return "x";

    }


    return "y";
}


/* =========================================================
   HEBELARM
========================================================= */

function getArm(
    field,
    axis
) {

    const p =
        fieldToCoordinate(
            field
        );


    return p[axis];
}


/* =========================================================
   BALANCE BERECHNEN
========================================================= */

function calculateBalance(
    extraWeight = null,
    extraField = null
) {

    const axis =
        getBalanceAxis();


    let total = 0;


    /*
        Figur.
    */

    total +=

        FIGURE_BALLS *

        getArm(
            currentField,
            axis
        );


    /*
        Bereits liegende Gewichte.
    */

    placedWeights.forEach(

        item => {

            total +=

                item.weight *

                getArm(
                    item.field,
                    axis
                );

        }

    );


    /*
        Testgewicht.
    */

    if (
        extraWeight !== null &&
        extraField !== null
    ) {

        total +=

            extraWeight *

            getArm(
                extraField,
                axis
            );

    }


    return {

        value:
            total,

        axis:
            axis

    };
}


/* =========================================================
   FREIE FELDER
========================================================= */

function getFreeFields() {

    const result = [];


    for (
        let field = 1;
        field <= 16;
        field++
    ) {

        /*
            Figurfeld darf nicht
            belegt werden.
        */

        if (
            field === currentField
        ) {

            continue;
        }


        /*
            Bereits gefrorene oder
            normale Gewichte.
        */

        if (
            placedWeights.some(
                item =>
                    item.field === field
            )
        ) {

            continue;
        }


        result.push(
            field
        );

    }


    return result;
}


/* =========================================================
   MÖGLICHE ABLAGEFELDER
========================================================= */

function getPlaceableFields() {

    const balance =
        calculateBalance();


    const fields = [];


    for (
        const field of getFreeFields()
    ) {

        const arm =
            getArm(
                field,
                balance.axis
            );


        if (
            Math.abs(arm) < 0.0001
        ) {

            continue;

        }


        /*
            Gegenseite der Balance.
        */

        if (
            balance.value > 0 &&
            arm < 0
        ) {

            fields.push(
                field
            );

        }


        if (
            balance.value < 0 &&
            arm > 0
        ) {

            fields.push(
                field
            );

        }

    }


    return fields;
}


/* =========================================================
   LÖSUNG SUCHEN
========================================================= */

function findSolution() {

    const balance =
        calculateBalance();


    const solutions = [];


    /*
        Alle freien Felder testen.
    */

    for (
        const field of getFreeFields()
    ) {

        const arm =
            getArm(
                field,
                balance.axis
            );


        if (
            Math.abs(arm) < 0.0001
        ) {

            continue;
        }


        /*
            benötigte Kugelanzahl
        */

        const needed =
            -balance.value /
            arm;


        /*
            Nur ganze Kugeln.
        */

        const rounded =
            Math.round(
                needed
            );


        if (
            rounded < 1 ||
            rounded > 6
        ) {

            continue;
        }


        const result =
            calculateBalance(
                rounded,
                field
            );


        if (
            Math.abs(
                result.value
            ) < 0.0001
        ) {

            solutions.push({

                field:
                    field,

                weight:
                    rounded

            });

        }

    }


    if (
        solutions.length === 0
    ) {

        return null;

    }


    /*
        Zufällige Lösung.
    */

    return solutions[
        randomInt(
            0,
            solutions.length - 1
        )
    ];
}


/* =========================================================
   DREI GEWICHTE ERZEUGEN
========================================================= */

function createWeights() {

    let solution = null;

    let tries = 0;


    /*
        Eine echte Aufgabe suchen.
    */

    while (
        solution === null &&
        tries < 200
    ) {

        solution =
            findSolution();

        tries++;

    }


    /*
        Falls keine Lösung gefunden wurde,
        wird das Spielfeld neu aufgebaut.
    */

    if (
        solution === null
    ) {

        /*
            Fallback:
            neues Spiel.
        */

        path =
            generatePath();

        currentPathIndex =
            0;

        currentField =
            path[0];

        placedWeights =
            [];

        return createWeights();

    }


    currentSolution =
        solution;


    /*
        Richtiges Gewicht.
    */

    const weights = [

        solution.weight

    ];


    /*
        Zwei falsche Kugelzahlen.
    */

    while (
        weights.length < 3
    ) {

        const candidate =
            POSSIBLE_WEIGHTS[
                randomInt(
                    0,
                    POSSIBLE_WEIGHTS.length - 1
                )
            ];


        if (
            !weights.includes(
                candidate
            )
        ) {

            weights.push(
                candidate
            );

        }

    }


    availableWeights =
        shuffle(
            weights
        );


    selectedWeightIndex =
        null;
}


/* =========================================================
   GEWICHT AUSWÄHLEN
========================================================= */

function selectWeight(index) {

    if (
        gameLocked
    ) {

        return;
    }


    selectedWeightIndex =
        index;


    renderWeights();

    renderBoard();


    showMessage(

        `${ballsText(
            availableWeights[index]
        )} ausgewählt.
        Jetzt ein hervorgehobenes Feld anklicken.`,

        "info"

    );
}


/* =========================================================
   FELD KLICK
========================================================= */

function boardClicked(field) {

    if (
        gameLocked
    ) {

        return;
    }


    /*
        Bereits liegendes Gewicht?
    */

    const existing =
        placedWeights.find(
            item =>
                item.field === field
        );


    if (
        existing
    ) {

        /*
            Eingefrorene Gewichte
            können NICHT entfernt werden.
        */

        if (
            existing.frozen
        ) {

            showMessage(

                "🔒 Dieses Gewicht ist eingefroren und kann nicht mehr entfernt werden.",

                "info"

            );

            return;
        }


        /*
            Normales falsches Gewicht
            darf zurückgenommen werden.
        */

        const index =
            placedWeights.indexOf(
                existing
            );


        placedWeights.splice(
            index,
            1
        );


        availableWeights.push(
            existing.weight
        );


        selectedWeightIndex =
            null;


        createWeights();

        renderWeights();

        renderBoard();

        showCalculation();


        showMessage(

            "↩ Gewicht wurde zurückgenommen.",

            "info"

        );


        return;
    }


    /*
        Noch kein Gewicht ausgewählt?
    */

    if (
        selectedWeightIndex === null
    ) {

        showMessage(

            "Bitte zuerst eine Kugelanzahl auswählen.",

            "error"

        );

        return;
    }


    /*
        Gewicht holen.
    */

    const weight =
        availableWeights[
            selectedWeightIndex
        ];


    /*
        Gewicht ablegen.
    */

    placedWeights.push({

        field:
            field,

        weight:
            weight,

        frozen:
            false

    });


    availableWeights.splice(

        selectedWeightIndex,

        1

    );


    selectedWeightIndex =
        null;


    renderWeights();

    renderBoard();


    /*
        Balance prüfen.
    */

    evaluatePlacement(
        field,
        weight
    );
}


/* =========================================================
   PLATZIERUNG AUSWERTEN
========================================================= */

function evaluatePlacement(
    field,
    weight
) {

    const balance =
        calculateBalance();


    /*
        RICHTIG
    */

    if (
        Math.abs(
            balance.value
        ) < 0.0001
    ) {

        gameLocked =
            true;


        /*
            Das gerade platzierte Gewicht
            wird eingefroren.
        */

        const placed =
            placedWeights.find(
                item =>
                    item.field === field
            );


        if (
            placed
        ) {

            placed.frozen =
                true;

        }


        showCorrectMark(
            field
        );


        showMessage(

            "✓ Richtig! Das Gewicht wird eingefroren.",

            "success"

        );


        showCalculation();


        setTimeout(

            () => {

                removeCorrectMark();

                advanceFigure();

            },

            1000

        );


        return;
    }


    /*
        FALSCH
    */

    showMessage(

        "❌ Noch nicht richtig. Die Figur bleibt stehen.",

        "error"

    );


    showWrongAnimation();


    showCalculation();


    /*
        Das falsche Gewicht bleibt liegen.
        Es kann später wieder entfernt werden.
    */

    createWeights();

    renderWeights();

    renderBoard();
}


/* =========================================================
   NÄCHSTES FELD
========================================================= */

function advanceFigure() {

    /*
        Ziel erreicht?
    */

    if (
        currentPathIndex >=
        path.length - 1
    ) {

        finishGame();

        return;
    }


    currentPathIndex++;

    currentField =
        path[
            currentPathIndex
        ];


    gameLocked =
        false;


    /*
        Die gefrorenen Gewichte bleiben.

        Normale falsche Gewichte bleiben
        ebenfalls liegen.
    */

    createWeights();

    renderWeights();

    renderBoard();

    updateInformation();

    hideCalculation();


    showMessage(

        "➡️ Die Figur ist ein Feld weiter.",

        "info"

    );
}


/* =========================================================
   RICHTIG-HÄKCHEN
========================================================= */

function showCorrectMark(field) {

    const cells =
        document.querySelectorAll(
            ".cell"
        );


    cells.forEach(

        (cell,index) => {

            if (
                index + 1 === field
            ) {

                const check =
                    document.createElement(
                        "div"
                    );


                check.className =
                    "correct-check";


                check.textContent =
                    "✓";


                cell.appendChild(
                    check
                );

            }

        }

    );
}


/* =========================================================
   HÄKCHEN ENTFERNEN
========================================================= */

function removeCorrectMark() {

    document
        .querySelectorAll(
            ".correct-check"
        )
        .forEach(
            element =>
                element.remove()
        );
}


/* =========================================================
   FALSCHE ANIMATION
========================================================= */

function showWrongAnimation() {

    const cell =
        document.querySelector(
            ".cell.current"
        );


    if (
        !cell
    ) {

        return;
    }


    cell.classList.add(
        "wrong"
    );


    setTimeout(

        () => {

            cell.classList.remove(
                "wrong"
            );

        },

        400

    );
}


/* =========================================================
   SPIEL BEENDET
========================================================= */

function finishGame() {

    gameLocked =
        true;


    showMessage(

        "🏆 ZIEL ERREICHT! Sehr gut gemacht!",

        "success"

    );


    showCalculation();
}


/* =========================================================
   SPIELBRETT ZEICHNEN
========================================================= */

function renderBoard() {

    const board =
        document.getElementById(
            "board"
        );


    board.innerHTML =
        "";


    const placeable =
        selectedWeightIndex !== null

            ? getPlaceableFields()

            : [];


    for (
        let field = 1;
        field <= 16;
        field++
    ) {

        const cell =
            document.createElement(
                "div"
            );


        cell.className =
            "cell";


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
            Figur
        */

        if (
            field === currentField
        ) {

            cell.classList.add(
                "current"
            );

        }


        /*
            Lösung hervorheben
        */

        if (
            SHOW_SOLUTION &&
            currentSolution &&
            field === currentSolution.field
        ) {

            cell.classList.add(
                "solution-field"
            );

        }


        /*
            Belegtes Feld
        */

        const weights =
            placedWeights.filter(
                item =>
                    item.field === field
            );


        /*
            Mögliche Ablage
        */

        if (
            placeable.includes(field) &&
            weights.length === 0 &&
            field !== currentField
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


            figure.innerHTML =
                `
                <div>⚖️</div>
                <div class="figure-label">
                    ${ballsText(FIGURE_BALLS)}
                </div>
                `;


            cell.appendChild(
                figure
            );

        }


        /*
            Gewichte darstellen
        */

        if (
            weights.length > 0
        ) {

            const stack =
                document.createElement(
                    "div"
                );


            stack.className =
                "weight-stack";


            weights.forEach(

                item => {

                    const token =
                        document.createElement(
                            "div"
                        );


                    token.className =
                        "weight-token";


                    if (
                        item.frozen
                    ) {

                        token.classList.add(
                            "frozen"
                        );

                    }


                    token.innerHTML =

                        ballsText(
                            item.weight
                        );


                    stack.appendChild(
                        token
                    );

                }

            );


            cell.appendChild(
                stack
            );

        }


        cell.addEventListener(

            "click",

            () =>
                boardClicked(field)

        );


        board.appendChild(
            cell
        );

    }
}


/* =========================================================
   GEWICHTE RENDERN
========================================================= */

function renderWeights() {

    const container =
        document.getElementById(
            "weights"
        );


    container.innerHTML =
        "";


    availableWeights.forEach(

        (weight,index) => {

            const option =
                document.createElement(
                    "div"
                );


            option.className =
                "weight-option";


            if (
                index ===
                selectedWeightIndex
            ) {

                option.classList.add(
                    "selected"
                );

            }


            /*
                Probe: richtige Auswahl
                grün markieren.
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


            option.innerHTML =
                ballsText(
                    weight
                );


            option.addEventListener(

                "click",

                () =>
                    selectWeight(index)

            );


            container.appendChild(
                option
            );

        }

    );
}


/* =========================================================
   INFORMATIONEN
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
    ).innerHTML =
        ballsText(
            FIGURE_BALLS
        );


    document.getElementById(
        "currentField"
    ).textContent =
        currentField;
}


/* =========================================================
   LÖSUNG ANZEIGEN
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

        box.textContent =
            "";

        return;
    }


    box.innerHTML =

        `${ballsText(
            currentSolution.weight
        )}
        → Feld ${currentSolution.field}
        <br>
        Das grüne Feld ist das richtige
        Ablagefeld.`;
}


/* =========================================================
   BERECHNUNG
========================================================= */

function showCalculation() {

    const box =
        document.getElementById(
            "calculation"
        );


    const balance =
        calculateBalance();


    let html =

        "<strong>MATHEMATISCHE BERECHNUNG</strong><br><br>";


    html +=

        `Waage:
        ${FIGURE_BALLS} Kugeln<br>`;


    html +=

        `Aktuelles Feld:
        ${currentField}<br>`;


    html +=

        `Balance-Achse:
        ${balance.axis === "x"
            ? "X"
            : "Y"}<br><br>`;


    if (
        placedWeights.length === 0
    ) {

        html +=
            "Keine abgelegten Gewichte.<br>";

    } else {

        html +=
            "Abgelegte Gewichte:<br>";


        placedWeights.forEach(

            item => {

                html +=

                    `${item.frozen ? "🔒" : "⚪"}
                    ${item.weight} Kugeln
                    → Feld ${item.field}<br>`;

            }

        );

    }


    html +=
        "<br>";


    html +=

        `Gesamtbalance:
        ${balance.value.toFixed(2)}<br>`;


    if (
        Math.abs(
            balance.value
        ) < 0.0001
    ) {

        html +=

            `<span style="color:#4ade80">
            ✓ Gleichgewicht
            </span>`;

    } else {

        html +=

            `<span style="color:#facc15">
            Noch nicht im Gleichgewicht
            </span>`;

    }


    box.innerHTML =
        html;
}


/* =========================================================
   FALSCHE GEWICHTE ZURÜCKNEHMEN
========================================================= */

function resetNonFrozenWeights() {

    if (
        gameLocked
    ) {

        return;
    }


    const removable =
        placedWeights.filter(
            item =>
                !item.frozen
        );


    removable.forEach(

        item => {

            availableWeights.push(
                item.weight
            );

        }

    );


    placedWeights =
        placedWeights.filter(
            item =>
                item.frozen
        );


    selectedWeightIndex =
        null;


    createWeights();

    renderWeights();

    renderBoard();

    renderSolution();

    showCalculation();


    showMessage(

        "↩ Alle nicht eingefrorenen Gewichte wurden zurückgenommen.",

        "info"

    );
}


/* =========================================================
   NEUES SPIEL
========================================================= */

function newGame() {

    gameLocked =
        false;


    placedWeights =
        [];


    selectedWeightIndex =
        null;


    currentPathIndex =
        0;


    path =
        generatePath();


    currentField =
        path[0];


    createWeights();


    renderWeights();

    renderBoard();

    updateInformation();

    renderSolution();

    showCalculation();


    showMessage(

        "🎲 Neue Runde gestartet.",

        "info"

    );
}


/* =========================================================
   KUGELN DARSTELLEN
========================================================= */

function ballsHTML(count) {

    let html = "";


    for (
        let i = 0;
        i < count;
        i++
    ) {

        html +=
            `<span class="ball"></span>`;

    }


    return html;
}


function ballsText(count) {

    return `

        <span
            style="
                display:inline-flex;
                align-items:center;
                gap:3px;
            "
        >

            ${ballsHTML(count)}

            <span>
                ${count}
                ${count === 1
                    ? "Kugel"
                    : "Kugeln"}
            </span>

        </span>

    `;
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

        ] = [

            result[j],
            result[i]

        ];

    }


    return result;
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
   START
========================================================= */

newGame();

</script>

</body>
</html>
