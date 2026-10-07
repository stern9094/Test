<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>BALANCE-TEAM</title>

<style>

/* =========================================================
   GRUNDLAYOUT
========================================================= */

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
            #29445f 0%,
            #172638 45%,
            #0b111a 100%
        );
}

.container {
    width: min(1200px, 96%);

    margin: auto;

    padding:
        22px
        0
        40px;
}

h1 {
    margin: 0;

    text-align: center;

    font-size: 40px;

    letter-spacing: 3px;
}

.subtitle {
    margin:
        6px
        0
        22px;

    text-align: center;

    color: #aebfd1;
}


/* =========================================================
   HAUPTBEREICH
========================================================= */

.game-layout {

    display: grid;

    grid-template-columns:
        minmax(500px, 1fr)
        360px;

    gap: 22px;
}

.panel {

    background:
        rgba(255,255,255,.075);

    border:
        1px solid
        rgba(255,255,255,.12);

    border-radius: 18px;

    padding: 18px;

    box-shadow:
        0 20px 50px
        rgba(0,0,0,.28);

    backdrop-filter:
        blur(8px);
}


/* =========================================================
   SPIELFELD
========================================================= */

.board {

    width:
        min(650px,100%);

    aspect-ratio: 1;

    margin: auto;

    display: grid;

    grid-template-columns:
        repeat(5,1fr);

    grid-template-rows:
        repeat(5,1fr);

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

    border:
        2px solid
        #536b82;

    border-radius: 10px;

    cursor: pointer;

    transition:
        transform .12s,
        background .15s,
        border-color .15s,
        box-shadow .15s;

    user-select: none;
}


/*
    Normales Hover
*/

.cell:hover {

    transform:
        scale(1.025);

    border-color:
        #65d8ff;

    z-index: 5;
}


/*
    Startbereich
*/

.cell.start {

    border-color:
        #38bdf8;
}


/*
    Zielbereich
*/

.cell.goal {

    border-color:
        #22c55e;
}


/*
    Aktuelles Feld der Figur
*/

.cell.current {

    background:
        linear-gradient(
            145deg,
            #7c3aed,
            #4c1d95
        );

    border-color:
        #c4b5fd;

    box-shadow:
        0 0 25px
        rgba(139,92,246,.65);

    z-index: 4;
}


/*
    Weg
*/

.cell.path {

    background:
        linear-gradient(
            145deg,
            #28506a,
            #1a3549
        );
}


/*
    ---------------------------------------------------------
    MÖGLICHE ABLAGEFELDER
    ---------------------------------------------------------

    Diese Markierung erscheint erst,
    wenn ein Gewicht ausgewählt wurde.
*/

.cell.placeable {

    border-color:
        #facc15;

    background:
        linear-gradient(
            145deg,
            #3b4b43,
            #26382f
        );

    box-shadow:
        inset 0 0 0 2px
        rgba(250,204,21,.18),

        0 0 10px
        rgba(250,204,21,.12);
}


/*
    Etwas stärker beim Darüberfahren.
*/

.cell.placeable:hover {

    border-color:
        #fde047;

    background:
        linear-gradient(
            145deg,
            #465b4e,
            #304438
        );

    box-shadow:
        0 0 18px
        rgba(250,204,21,.28);

    transform:
        scale(1.035);
}


/*
    Bereits belegtes Feld
*/

.cell.occupied {

    border-color:
        #f59e0b;
}


/*
    Falscher Versuch
*/

.cell.wrong {

    animation:
        wrong .35s;
}

@keyframes wrong {

    0%,100% {
        transform:
            translateX(0);
    }

    25% {
        transform:
            translateX(-7px);
    }

    75% {
        transform:
            translateX(7px);
    }
}


/* =========================================================
   SPIELFIGUR
========================================================= */

.figure {

    font-size:
        clamp(
            25px,
            5vw,
            44px
        );

    filter:
        drop-shadow(
            0 4px 5px
            rgba(0,0,0,.4)
        );

    z-index: 5;
}


/* =========================================================
   GEWICHT AUF DEM SPIELFELD
========================================================= */

.weight-stack {

    position: absolute;

    bottom: 7px;
    right: 7px;

    display: flex;

    flex-direction: column;

    gap: 3px;

    align-items: flex-end;

    z-index: 6;
}

.weight-token {

    background:
        linear-gradient(
            145deg,
            #f59e0b,
            #b45309
        );

    color:
        white;

    border:
        1px solid
        #fde68a;

    border-radius: 6px;

    padding:
        4px 7px;

    font-size:
        11px;

    font-weight:
        bold;

    box-shadow:
        0 3px 8px
        rgba(0,0,0,.35);
}


/* =========================================================
   GRÜNER HAKEN
========================================================= */

.correct-check {

    position: absolute;

    top: 50%;
    left: 50%;

    transform:
        translate(
            -50%,
            -50%
        );

    width: 52px;
    height: 52px;

    display: flex;

    justify-content: center;
    align-items: center;

    border-radius: 50%;

    background:
        #16a34a;

    border:
        3px solid
        #bbf7d0;

    color:
        white;

    font-size:
        34px;

    font-weight:
        bold;

    z-index: 20;

    box-shadow:
        0 0 25px
        rgba(34,197,94,.9),

        0 0 50px
        rgba(34,197,94,.45);

    animation:
        correctPop .2s
        ease-out;
}

@keyframes correctPop {

    0% {

        transform:
            translate(
                -50%,
                -50%
            )
            scale(.3);

        opacity: 0;
    }

    70% {

        transform:
            translate(
                -50%,
                -50%
            )
            scale(1.15);

        opacity: 1;
    }

    100% {

        transform:
            translate(
                -50%,
                -50%
            )
            scale(1);

        opacity: 1;
    }
}


/* =========================================================
   STATUS
========================================================= */

.status {

    background:
        rgba(0,0,0,.2);

    border-radius:
        12px;

    padding:
        14px;

    line-height:
        1.55;

    margin-bottom:
        15px;
}

.status-title {

    color:
        #67e8f9;

    font-weight:
        bold;

    font-size:
        17px;
}


/* =========================================================
   INFOS
========================================================= */

.info-grid {

    display: grid;

    grid-template-columns:
        1fr 1fr;

    gap:
        9px;
}

.info {

    background:
        rgba(0,0,0,.18);

    padding:
        11px;

    border-radius:
        9px;
}

.info-label {

    color:
        #8fa4b9;

    font-size:
        12px;
}

.info-value {

    font-size:
        20px;

    font-weight:
        bold;

    margin-top:
        3px;
}


/* =========================================================
   GEWICHTE
========================================================= */

h2 {

    font-size:
        18px;

    margin:
        20px 0 10px;
}

.weights {

    display: grid;

    grid-template-columns:
        repeat(3,1fr);

    gap:
        8px;
}

.weight-option {

    padding:
        15px 5px;

    border-radius:
        9px;

    background:
        linear-gradient(
            145deg,
            #d97706,
            #92400e
        );

    border:
        2px solid
        #fbbf24;

    color:
        white;

    font-weight:
        bold;

    text-align:
        center;

    cursor:
        pointer;

    transition:
        .15s;
}

.weight-option:hover {

    transform:
        translateY(-2px);

    filter:
        brightness(1.12);
}

.weight-option.selected {

    border-color:
        white;

    box-shadow:
        0 0 20px
        rgba(251,191,36,.7);

    transform:
        scale(1.04);
}


/* =========================================================
   BUTTONS
========================================================= */

button {

    width:
        100%;

    border:
        none;

    border-radius:
        9px;

    padding:
        12px;

    margin-top:
        9px;

    color:
        white;

    font-size:
        15px;

    font-weight:
        bold;

    cursor:
        pointer;

    background:
        linear-gradient(
            135deg,
            #0284c7,
            #0369a1
        );
}

button:hover {

    filter:
        brightness(1.12);
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


/* =========================================================
   MELDUNG
========================================================= */

.message {

    margin-top:
        13px;

    padding:
        12px;

    border-radius:
        9px;

    line-height:
        1.5;

    display:
        none;
}

.message.show {

    display:
        block;
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
   WEG
========================================================= */

.path {

    margin-top:
        15px;

    padding:
        12px;

    border-radius:
        10px;

    background:
        rgba(0,0,0,.18);
}

.path-list {

    display:
        flex;

    flex-wrap:
        wrap;

    gap:
        5px;

    margin-top:
        8px;
}

.path-field {

    padding:
        4px 8px;

    border-radius:
        5px;

    background:
        #273b4e;

    border:
        1px solid
        #526b81;

    font-size:
        12px;
}

.path-field.active {

    background:
        #7c3aed;

    border-color:
        #c4b5fd;
}

.path-field.done {

    background:
        #166534;

    border-color:
        #4ade80;
}


/* =========================================================
   BERECHNUNG
========================================================= */

.calculation {

    margin-top:
        15px;

    padding:
        13px;

    border-radius:
        10px;

    background:
        #09121d;

    font-family:
        Consolas,
        monospace;

    font-size:
        12px;

    line-height:
        1.65;

    display:
        none;
}

.calculation.show {

    display:
        block;
}

.good {

    color:
        #86efac;
}

.warning {

    color:
        #facc15;
}

.bad {

    color:
        #fca5a5;
}


/* =========================================================
   RESPONSIVE
========================================================= */

@media(max-width:850px) {

    .game-layout {

        grid-template-columns:
            1fr;
    }

    h1 {

        font-size:
            30px;
    }

    .board {

        width:
            100%;
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
             SPIELSTEUERUNG
        ================================================== -->

        <div class="panel">


            <div class="status">

                <div class="status-title">
                    Aktuelle Aufgabe
                </div>

                <div>
                    Wähle eines der drei Gewichte.
                </div>

                <div>
                    Danach kannst du auf ein
                    hervorgehobenes Feld klicken.
                </div>

                <br>

                <div>
                    Ist die Balance richtig,
                    geht die Figur genau
                    <strong>ein Feld weiter</strong>.
                </div>

                <div>
                    Ist sie falsch,
                    bleibt die Figur stehen.
                </div>

            </div>


            <!-- =================================================
                 INFORMATIONEN
            ================================================== -->

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


            <!-- =================================================
                 GEWICHTE
            ================================================== -->

            <h2>
                ⚖️ 3 bereitliegende Gewichte
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

                ↩ Alle Gewichte zurücknehmen

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

const BALANCE_TOLERANCE = 0.001;


/*
    Drei Gewichte.
    Das Programm berechnet bei jedem neuen
    Schritt ein passendes Gewicht.

    Die beiden anderen Gewichte sind
    bewusst Ablenkungen.
*/

const POSSIBLE_WEIGHTS = [

    0.5,
    1.0,
    1.5,
    2.0,
    2.5,
    3.0,
    3.5,
    4.0

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
    Alle dauerhaft abgelegten Gewichte.
*/

let placedWeights = [];


/*
    Die drei momentan verfügbaren Gewichte.
*/

let availableWeights = [];


/*
    Ausgewähltes Gewicht.
*/

let selectedWeightIndex = null;


/*
    Intern bekannte Lösung.
    Sie wird dem Spieler natürlich
    NICHT angezeigt.
*/

let currentSolution = null;


/*
    Verhindert Klicks während
    der Erfolgsanimation.
*/

let gameLocked = false;


/* =========================================================
   FELD → KOORDINATE
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


/* =========================================================
   KOORDINATE → FELD
========================================================= */

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


    return (
        row * SIZE +
        col +
        1
    );
}


/* =========================================================
   NACHBARFELDER
========================================================= */

function getNeighbors(field) {

    const position =
        fieldToCoordinate(field);


    const directions = [

        { x: 1, y: 0 },

        { x: -1, y: 0 },

        { x: 0, y: 1 },

        { x: 0, y: -1 }

    ];


    const result = [];


    for (
        const direction of directions
    ) {

        const neighbor =
            coordinateToField(

                position.x +
                direction.x,

                position.y +
                direction.y

            );


        if (
            neighbor !== null
        ) {

            result.push(
                neighbor
            );

        }
    }


    return result;
}


/* =========================================================
   ZUFÄLLIGEN WEG ERZEUGEN
========================================================= */

function generatePath() {

    startField =
        randomInt(1,5);

    goalField =
        randomInt(21,25);


    let result = [

        startField

    ];


    let current =
        startField;


    const used =
        new Set(result);


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
            Ziel direkt erreichen,
            wenn möglich.
        */

        if (
            neighbors.includes(
                goalField
            )
        ) {

            current =
                goalField;

        } else {

            const currentPosition =
                fieldToCoordinate(
                    current
                );


            /*
                Bewegung nach unten
                bevorzugen.
            */

            const downward =
                neighbors.filter(

                    field => {

                        const position =
                            fieldToCoordinate(
                                field
                            );


                        return (
                            position.y >=
                            currentPosition.y
                        );

                    }

                );


            if (

                downward.length > 0 &&

                Math.random() < 0.75

            ) {

                current =
                    downward[
                        randomInt(
                            0,
                            downward.length - 1
                        )
                    ];

            } else if (
                neighbors.length > 0
            ) {

                current =
                    neighbors[
                        randomInt(
                            0,
                            neighbors.length - 1
                        )
                    ];

            } else {

                return generatePath();

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
   VEREINFACHTE BALANCE-BERECHNUNG
========================================================= */

/*
    Für das vereinfachte Spiel verwenden wir
    eine einzige Balance-Achse.

    Die Achse richtet sich nach der Position
    der Figur:

    - Figur links/rechts vom Mittelpunkt:
      horizontale Balance

    - Figur oberhalb/unterhalb:
      vertikale Balance

    - Figur genau in der Mitte:
      horizontale Achse

    Dadurch muss der Spieler keine
    zweidimensionale Mathematik verstehen.
*/


function getBalanceAxis(field) {

    const position =
        fieldToCoordinate(field);


    if (
        Math.abs(position.x) >=
        Math.abs(position.y)
    ) {

        return "x";

    }


    return "y";
}


/* =========================================================
   SIGNEDER HEBELARM
========================================================= */

function getSignedArm(
    field,
    axis
) {

    const position =
        fieldToCoordinate(field);


    return position[axis];
}


/* =========================================================
   GESAMTBALANCE
========================================================= */

function calculateBalance(
    testWeight = null,
    testField = null
) {

    const axis =
        getBalanceAxis(
            currentField
        );


    let total = 0;


    /*
        Figur
    */

    total +=

        FIGURE_WEIGHT *

        getSignedArm(
            currentField,
            axis
        );


    /*
        Bereits abgelegte Gewichte
    */

    for (
        const item of placedWeights
    ) {

        total +=

            item.weight *

            getSignedArm(
                item.field,
                axis
            );
    }


    /*
        Optional ein neues Gewicht
    */

    if (

        testWeight !== null &&

        testField !== null

    ) {

        total +=

            testWeight *

            getSignedArm(
                testField,
                axis
            );
    }


    return {

        value: total,

        axis: axis

    };
}


/* =========================================================
   GÜLTIGE LÖSUNGSFELDER
========================================================= */

/*
    Nur Felder auf der entgegengesetzten
    Seite der aktuellen Balance werden
    als sinnvolle Ablagefelder hervorgehoben.

    Dadurch wird das Spiel für den Spieler
    deutlich übersichtlicher.
*/

function getPlaceableFields() {

    const balance =
        calculateBalance();


    const fields = [];


    for (
        let field = 1;
        field <= 25;
        field++
    ) {

        /*
            Figurfeld nicht benutzen.
        */

        if (
            field === currentField
        ) {

            continue;
        }


        /*
            Mittelpunkt ist kein
            sinnvolles Gegengewicht.
        */

        if (
            field === CENTER
        ) {

            continue;
        }


        /*
            Bereits belegte Felder
            können nicht erneut belegt
            werden.
        */

        const occupied =
            placedWeights.some(

                item =>
                    item.field === field

            );


        if (
            occupied
        ) {

            continue;
        }


        const arm =
            getSignedArm(
                field,
                balance.axis
            );


        /*
            Ein Gegengewicht muss auf
            der entgegengesetzten Seite liegen.
        */

        if (
            balance.value > 0 &&
            arm < 0
        ) {

            fields.push(field);

        } else if (
            balance.value < 0 &&
            arm > 0
        ) {

            fields.push(field);

        }

    }


    return fields;
}


/* =========================================================
   PASSENDES GEWICHT FINDEN
========================================================= */

function findSolution() {

    const balance =
        calculateBalance();


    /*
        Wenn die aktuelle Balance bereits
        exakt 0 wäre, erzeugen wir eine
        kleine neue Aufgabe.
    */

    if (
        Math.abs(balance.value)
        < BALANCE_TOLERANCE
    ) {

        /*
            Ein Feld suchen, das die
            Aufgabe verändert.
        */

        const candidates =
            getAllFreeFields();


        const field =
            candidates[
                randomInt(
                    0,
                    candidates.length - 1
                )
            ];


        const arm =
            getSignedArm(
                field,
                balance.axis
            );


        if (
            arm !== 0
        ) {

            return {
                field: field,
                weight: 1.0
            };
        }
    }


    /*
        Wir suchen ein freies Feld,
        auf dem ein gut spielbares Gewicht
        die Balance exakt auf 0 bringt.
    */

    const candidates =
        getAllFreeFields();


    const possibleSolutions = [];


    for (
        const field of candidates
    ) {

        const arm =
            getSignedArm(
                field,
                balance.axis
            );


        if (
            arm === 0
        ) {

            continue;
        }


        /*
            Benötigtes Gewicht:

            Gewicht =
            -Balance / Hebelarm
        */

        const needed =
            -balance.value / arm;


        /*
            Auf ein sinnvolles Spielgewicht
            runden.
        */

        const rounded =
            Math.round(
                needed * 2
            ) / 2;


        if (
            rounded >= 0.5 &&
            rounded <= 4.0
        ) {

            const check =
                calculateBalance(
                    rounded,
                    field
                );


            if (
                Math.abs(
                    check.value
                ) < BALANCE_TOLERANCE
            ) {

                possibleSolutions.push({

                    field: field,

                    weight: rounded

                });

            }

        }

    }


    /*
        Wenn mehrere Lösungen möglich sind,
        eine zufällige auswählen.
    */

    if (
        possibleSolutions.length > 0
    ) {

        return possibleSolutions[
            randomInt(
                0,
                possibleSolutions.length - 1
            )
        ];
    }


    /*
        Falls keine passende Lösung mit
        0,5-Schritten existiert, erzeugen
        wir eine neue Aufgabe.
    */

    return null;
}


/* =========================================================
   FREIE FELDER
========================================================= */

function getAllFreeFields() {

    const result = [];


    for (
        let field = 1;
        field <= 25;
        field++
    ) {

        if (
            field === currentField ||
            field === CENTER
        ) {

            continue;
        }


        const occupied =
            placedWeights.some(

                item =>
                    item.field === field

            );


        if (
            !occupied
        ) {

            result.push(field);

        }

    }


    return result;
}


/* =========================================================
   DREI GEWICHTE ERZEUGEN
========================================================= */

function createWeights() {

    /*
        Wir suchen eine Aufgabe,
        bei der eine exakte Lösung existiert.
    */

    let solution = null;

    let attempts = 0;


    while (
        solution === null &&
        attempts < 100
    ) {

        solution =
            findSolution();

        attempts++;

    }


    /*
        Sollte es wegen einer ungünstigen
        Kombination keine Lösung geben,
        erzeugen wir eine neue Runde.
    */

    if (
        solution === null
    ) {

        solution = {

            field:
                getRandomOppositeField(),

            weight:
                1.0

        };

    }


    currentSolution =
        solution;


    /*
        Das richtige Gewicht kommt
        in die drei Auswahlmöglichkeiten.
    */

    const weights = [

        solution.weight

    ];


    /*
        Zwei falsche Gewichte auswählen.
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


        /*
            Nicht doppelt.
        */

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


    /*
        Mischen.
    */

    availableWeights =
        shuffle(weights);


    selectedWeightIndex =
        null;
}


/* =========================================================
   ZUFÄLLIGES GEGENFELD
========================================================= */

function getRandomOppositeField() {

    const balance =
        calculateBalance();


    const candidates =
        getPlaceableFields();


    if (
        candidates.length > 0
    ) {

        return candidates[
            randomInt(
                0,
                candidates.length - 1
            )
        ];

    }


    return 1;
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


    document
        .querySelectorAll(
            ".weight-option"
        )
        .forEach(

            (element, i) => {

                element.classList.toggle(

                    "selected",

                    i === index

                );

            }

        );


    renderBoard();


    showMessage(

        `Gewicht ${
            formatKg(
                availableWeights[index]
            )
        } ausgewählt.
        Die möglichen Ablagefelder
        sind jetzt hervorgehoben.`,

        "info"

    );
}


/* =========================================================
   FELD ANGEKLICKT
========================================================= */

function boardClicked(field) {

    if (
        gameLocked
    ) {

        return;
    }


    /*
        -----------------------------------------------------
        EXISTIERENDES GEWICHT
        -----------------------------------------------------
    */

    const existingIndex =
        placedWeights.findIndex(

            item =>
                item.field === field

        );


    if (
        existingIndex !== -1
    ) {

        const removed =
            placedWeights[
                existingIndex
            ];


        availableWeights.push(
            removed.weight
        );


        placedWeights.splice(
            existingIndex,
            1
        );


        selectedWeightIndex =
            null;


        renderWeights();

        renderBoard();


        showMessage(

            `↩ ${
                formatKg(
                    removed.weight
                )
            } wurde zurückgenommen.`,

            "info"

        );


        showCalculation();


        /*
            Da sich die Balance durch das
            Entfernen verändert hat,
            wird eine neue Aufgabe berechnet.
        */

        createWeights();

        renderWeights();

        renderBoard();


        return;
    }


    /*
        -----------------------------------------------------
        KEIN GEWICHT AUSGEWÄHLT
        -----------------------------------------------------
    */

    if (
        selectedWeightIndex === null
    ) {

        showMessage(

            "Bitte zuerst eines der drei Gewichte auswählen.",

            "error"

        );

        return;
    }


    /*
        -----------------------------------------------------
        FIGURENFELD
        -----------------------------------------------------
    */

    if (
        field === currentField
    ) {

        showMessage(

            "Auf dem Feld der Spielfigur kann kein Gewicht liegen.",

            "error"

        );

        return;
    }


    /*
        -----------------------------------------------------
        MITTELPUNKT
        -----------------------------------------------------
    */

    if (
        field === CENTER
    ) {

        showMessage(

            "Auf dem Mittelpunkt kann kein Gegengewicht abgelegt werden.",

            "error"

        );

        return;
    }


    /*
        -----------------------------------------------------
        GEWICHT ABLEGEN
        -----------------------------------------------------
    */

    const weight =
        availableWeights[
            selectedWeightIndex
        ];


    placedWeights.push({

        field:
            field,

        weight:
            weight

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
        Jetzt echte Balance prüfen.
    */

    evaluatePlacement();
}


/* =========================================================
   BALANCE AUSWERTEN
========================================================= */

function evaluatePlacement() {

    const balance =
        calculateBalance();


    const balanced =

        Math.abs(
            balance.value
        ) < BALANCE_TOLERANCE;


    /*
        -----------------------------------------------------
        RICHTIG
        -----------------------------------------------------
    */

    if (
        balanced
    ) {

        gameLocked =
            true;


        const lastWeight =
            placedWeights[
                placedWeights.length - 1
            ];


        /*
            Grüner Haken auf dem
            gerade gelegten Gewicht.
        */

        showCorrectMark(
            lastWeight.field
        );


        showMessage(

            "✓ Richtig! Die Balance stimmt.",

            "success"

        );


        showCalculation();


        /*
            Eine Sekunde warten.
        */

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
        -----------------------------------------------------
        FALSCH
        -----------------------------------------------------
    */

    showMessage(

        "❌ Noch nicht im Gleichgewicht. Die Figur bleibt stehen.",

        "error"

    );


    showCalculation();


    /*
        Das Gewicht bleibt liegen.
    */

    const cells =
        document.querySelectorAll(
            ".cell"
        );


    cells.forEach(

        cell => {

            /*
                Wir suchen das aktuelle
                Figurenfeld.
            */

            if (
                cell.classList.contains(
                    "current"
                )
            ) {

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

        }

    );


    /*
        Für die nächste Aufgabe wird
        die neue Balance berücksichtigt.

        Dadurch bleiben die Gewichte
        dauerhaft relevant.
    */

    createWeights();

    renderWeights();

    renderBoard();
}


/* =========================================================
   FIGUR EIN FELD WEITER
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


    /*
        Genau ein Feld weiter.
    */

    currentPathIndex++;


    currentField =
        path[
            currentPathIndex
        ];


    /*
        Alle alten Gewichte bleiben liegen.
    */

    gameLocked =
        false;


    /*
        Neue drei Gewichte
        für die neue Aufgabe.
    */

    createWeights();


    renderWeights();

    renderBoard();

    updateInformation();

    updatePathDisplay();

    hideCalculation();


    showMessage(

        `➡️ Die Figur geht ein Feld weiter.`,

        "info"

    );
}


/* =========================================================
   RICHTIG-HÄKCHEN
========================================================= */

function showCorrectMark(field) {

    removeCorrectMark();


    const cells =
        document.querySelectorAll(
            ".cell"
        );


    cells.forEach(

        cell => {

            /*
                Das Feld anhand seiner Position
                im Board bestimmen.
            */

            const fieldIndex =
                Array.from(
                    cell.parentNode.children
                ).indexOf(cell) + 1;


            if (
                fieldIndex === field
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
   SPIEL BEENDET
========================================================= */

function finishGame() {

    gameLocked =
        true;


    renderBoard();


    showMessage(

        "🏆 ZIEL ERREICHT! Die Balance wurde über den gesamten Weg gehalten.",

        "success"

    );


    showCalculation();
}


/* =========================================================
   SPIELFELD ZEICHNEN
========================================================= */

function renderBoard() {

    const board =
        document.getElementById(
            "board"
        );


    board.innerHTML =
        "";


    /*
        Mögliche Ablagefelder
        nur anzeigen, wenn ein
        Gewicht ausgewählt wurde.
    */

    const placeableFields =

        selectedWeightIndex !== null

            ? getPlaceableFields()

            : [];


    for (
        let field = 1;
        field <= 25;
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
            Ist dort schon ein Gewicht?
        */

        const weightsHere =
            placedWeights.filter(

                item =>
                    item.field === field

            );


        if (
            weightsHere.length > 0
        ) {

            cell.classList.add(
                "occupied"
            );

        }


        /*
            Mögliche Ablagefelder
        */

        if (
            placeableFields.includes(
                field
            ) &&

            field !== currentField &&

            weightsHere.length === 0

        ) {

            cell.classList.add(
                "placeable"
            );

        }


        /*
            Figur
        */

        if (
            field === currentField
        ) {

            const figure =
                document.createElement(
                    "span"
                );


            figure.className =
                "figure";


            figure.textContent =
                "⚖️";


            cell.appendChild(
                figure
            );

        }


        /*
            Gewichte anzeigen
        */

        if (
            weightsHere.length > 0
        ) {

            const stack =
                document.createElement(
                    "div"
                );


            stack.className =
                "weight-stack";


            weightsHere.forEach(

                item => {

                    const token =
                        document.createElement(
                            "div"
                        );


                    token.className =
                        "weight-token";


                    token.textContent =
                        formatKg(
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


        /*
            Klick
        */

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
   GEWICHTE ZEIGEN
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

            const button =
                document.createElement(
                    "div"
                );


            button.className =
                "weight-option";


            if (
                index ===
                selectedWeightIndex
            ) {

                button.classList.add(
                    "selected"
                );

            }


            button.textContent =
                formatKg(
                    weight
                );


            button.addEventListener(

                "click",

                () =>
                    selectWeight(index)

            );


            container.appendChild(
                button
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
    ).textContent =
        formatKg(
            FIGURE_WEIGHT
        );


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
        document.getElementById(
            "pathList"
        );


    container.innerHTML =
        "";


    path.forEach(

        (field,index) => {

            const element =
                document.createElement(
                    "div"
                );


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


            /*
                Der Weg wird momentan
                rechts noch als Kontrollanzeige
                gezeigt.
            */

            element.textContent =
                field;


            container.appendChild(
                element
            );

        }

    );
}


/* =========================================================
   BERECHNUNG ANZEIGEN
========================================================= */

function showCalculation() {

    const balance =
        calculateBalance();


    const box =
        document.getElementById(
            "calculation"
        );


    let html =

        "<strong>MATHEMATISCHE BERECHNUNG</strong><br><br>";


    html +=

        `Balance-Achse:
        ${balance.axis === "x"
            ? "horizontal"
            : "vertikal"}<br>`;


    html +=

        `Figur:
        ${formatKg(FIGURE_WEIGHT)}
        auf Feld ${currentField}<br>`;


    html +=
        "<br>Abgelegte Gewichte:<br>";


    if (
        placedWeights.length === 0
    ) {

        html +=
            "Keine Gewichte<br>";

    } else {

        placedWeights.forEach(

            item => {

                html +=

                    `${formatKg(item.weight)}
                    → Feld ${item.field}<br>`;

            }

        );

    }


    html +=
        "<br>";


    html +=

        `Gesamtbalance:
        ${balance.value.toFixed(3)}<br><br>`;


    if (

        Math.abs(
            balance.value
        ) < BALANCE_TOLERANCE

    ) {

        html +=

            `<span class="good">
            ✓ BALANCE = 0
            → GLEICHGEWICHT
            </span>`;

    } else {

        html +=

            `<span class="warning">
            Noch nicht im Gleichgewicht
            </span>`;

    }


    box.innerHTML =
        html;


    box.classList.add(
        "show"
    );
}


/* =========================================================
   BERECHNUNG AUSBLENDEN
========================================================= */

function hideCalculation() {

    document
        .getElementById(
            "calculation"
        )
        .classList.remove(
            "show"
        );
}


/* =========================================================
   ALLE GEWICHTE ZURÜCKNEHMEN
========================================================= */

function resetWeights() {

    if (
        gameLocked
    ) {

        return;
    }


    /*
        Gewicht zurück in die Auswahl.
    */

    placedWeights.forEach(

        item => {

            availableWeights.push(
                item.weight
            );

        }

    );


    placedWeights =
        [];


    selectedWeightIndex =
        null;


    createWeights();


    renderWeights();

    renderBoard();

    hideCalculation();


    showMessage(

        "↩ Alle abgelegten Gewichte wurden zurückgenommen.",

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


    /*
        Zufälligen Weg erzeugen.
    */

    path =
        generatePath();


    /*
        Startposition.
    */

    currentField =
        path[0];


    /*
        Drei Gewichte erzeugen.
    */

    createWeights();


    renderWeights();

    renderBoard();

    updateInformation();

    updatePathDisplay();

    hideCalculation();


    showMessage(

        "🎲 Neue Runde gestartet. Wähle eines der drei Gewichte.",

        "info"

    );
}


/* =========================================================
   ZUFALLSZAHL
========================================================= */

function randomInt(min,max) {

    return Math.floor(

        Math.random() *
        (max - min + 1)

    ) + min;
}


/* =========================================================
   ARRAY MISCHEN
========================================================= */

function shuffle(array) {

    const copy =
        [...array];


    for (
        let i =
            copy.length - 1;

        i > 0;

        i--
    ) {

        const j =
            Math.floor(

                Math.random() *
                (i + 1)

            );


        [
            copy[i],
            copy[j]

        ] = [

            copy[j],
            copy[i]

        ];

    }


    return copy;
}


/* =========================================================
   GEWICHT FORMATIEREN
========================================================= */

function formatKg(value) {

    return Number(value)

        .toFixed(1)

        .replace(
            ".",
            ","
        )

        + " kg";
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
        "message show " +
        type;


    box.textContent =
        text;
}


/* =========================================================
   SPIEL STARTEN
========================================================= */

newGame();

</script>

</body>
</html>
