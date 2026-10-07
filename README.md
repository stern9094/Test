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
    font-family: Arial, sans-serif;
    background:
        radial-gradient(circle at top, #294765, #142333 55%, #09111a);
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
    letter-spacing: 2px;
}

.subtitle {
    text-align: center;
    color: #b8c8d8;
    margin: 5px 0 20px;
}

/* ================================
   LAYOUT
================================ */

.game {
    display: grid;
    grid-template-columns: minmax(500px, 1fr) 350px;
    gap: 20px;
}

.panel {
    background: rgba(255,255,255,.07);
    border: 1px solid rgba(255,255,255,.12);
    border-radius: 18px;
    padding: 18px;
    box-shadow: 0 20px 45px rgba(0,0,0,.3);
}

/* ================================
   SPIELBRETT
================================ */

.board-wrapper {
    position: relative;
    width: min(650px, 100%);
    margin: auto;
}

.board {
    position: relative;

    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(4, 1fr);

    gap: 7px;

    aspect-ratio: 1;
}

.cell {
    position: relative;

    display: flex;
    align-items: center;
    justify-content: center;

    border-radius: 11px;

    background:
        linear-gradient(145deg, #344b60, #1d2e3e);

    border: 2px solid #536b80;

    cursor: pointer;

    transition: .15s;

    overflow: visible;
}

.cell:hover {
    transform: scale(1.025);
    border-color: #7dd3fc;
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
        linear-gradient(145deg, #7040c0, #4c1d95);

    border-color: #c4b5fd;

    box-shadow: 0 0 24px rgba(139,92,246,.65);
}

.cell.path {
    background:
        linear-gradient(145deg, #294b61, #1b3447);
}

.cell.placeable {
    border-color: #facc15;

    box-shadow:
        inset 0 0 0 2px rgba(250,204,21,.15),
        0 0 15px rgba(250,204,21,.25);
}

.cell.solution {
    border-color: #22c55e;

    box-shadow:
        0 0 25px rgba(34,197,94,.65);
}

/* ================================
   ROTER MITTELPUNKT
================================ */

/*
   Der Mittelpunkt liegt NICHT in einem Feld.

   Er liegt genau zwischen:

   Feld 6 | Feld 7
   -------●-------
   Feld10 | Feld11

   Deshalb wird der Kreis absolut
   über der Brettmitte positioniert.
*/

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
            #ffb4b4,
            #ef4444 45%,
            #991b1b 100%
        );

    border: 3px solid #fecaca;

    box-shadow:
        0 0 0 4px rgba(239,68,68,.15),
        0 0 20px rgba(239,68,68,.8);

    z-index: 30;

    pointer-events: none;
}

.center-point::after {
    content: "";

    position: absolute;

    inset: 6px;

    border-radius: 50%;

    border: 1px solid rgba(255,255,255,.6);
}

/* ================================
   FIGUR
================================ */

.figure {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    font-size: 32px;

    filter:
        drop-shadow(0 4px 4px rgba(0,0,0,.45));

    z-index: 10;
}

.figure-small {
    font-size: 11px;
    font-weight: bold;
}

/* ================================
   GEWICHTSHÄUFEN
================================ */

.weight-stack {
    position: absolute;

    left: 50%;
    bottom: 4px;

    transform: translateX(-50%);

    display: flex;
    flex-direction: column;
    align-items: center;

    z-index: 12;

    pointer-events: none;
}

.weight-row {
    display: flex;
    justify-content: center;
}

.weight-block {
    width: 17px;
    height: 17px;

    margin: 1px;

    border-radius: 4px;

    background:
        linear-gradient(
            145deg,
            #93c5fd,
            #3b82f6 55%,
            #1d4ed8
        );

    border: 1px solid #bfdbfe;

    box-shadow:
        inset 1px 1px 2px rgba(255,255,255,.5),
        0 2px 3px rgba(0,0,0,.35);
}

.weight-stack.frozen .weight-block {
    background:
        linear-gradient(
            145deg,
            #86efac,
            #22c55e 55%,
            #15803d
        );

    border-color: #bbf7d0;

    box-shadow:
        0 0 7px rgba(34,197,94,.7);
}

/* ================================
   HAKEN
================================ */

.correct-check {
    position: absolute;

    top: 50%;
    left: 50%;

    transform: translate(-50%, -50%);

    width: 48px;
    height: 48px;

    border-radius: 50%;

    display: flex;
    align-items: center;
    justify-content: center;

    background: #16a34a;

    border: 3px solid #bbf7d0;

    font-size: 30px;
    font-weight: bold;

    z-index: 40;

    box-shadow:
        0 0 25px rgba(34,197,94,.9);
}

/* ================================
   RECHTE SEITE
================================ */

.status {
    padding: 13px;

    background: rgba(0,0,0,.2);

    border-radius: 11px;

    line-height: 1.5;
}

.status-title {
    color: #67e8f9;
    font-weight: bold;
    margin-bottom: 5px;
}

.info-grid {
    margin-top: 13px;

    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
}

.info {
    background: rgba(0,0,0,.2);
    border-radius: 9px;
    padding: 10px;
}

.info-label {
    font-size: 11px;
    color: #8fa4b9;
}

.info-value {
    font-size: 17px;
    font-weight: bold;
    margin-top: 3px;
}

h2 {
    font-size: 18px;
    margin: 20px 0 10px;
}

/* ================================
   GEWICHT AUSWAHL
================================ */

.weights {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
}

.weight-option {
    min-height: 105px;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    background:
        linear-gradient(145deg, #334155, #1e293b);

    border: 2px solid #64748b;

    border-radius: 10px;

    cursor: pointer;

    transition: .15s;
}

.weight-option:hover {
    border-color: #60a5fa;
    transform: translateY(-2px);
}

.weight-option.selected {
    border-color: #facc15;

    box-shadow:
        0 0 20px rgba(250,204,21,.5);
}

.weight-option.solution-weight {
    border-color: #22c55e;

    box-shadow:
        0 0 18px rgba(34,197,94,.55);
}

.weight-number {
    margin-top: 6px;
    font-size: 12px;
    color: #cbd5e1;
}

/* ================================
   AUSWAHL-HÄUFEN
================================ */

.choice-stack {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.choice-stack .weight-block {
    width: 19px;
    height: 19px;
}

/* ================================
   LÖSUNG
================================ */

.solution {
    margin-top: 14px;

    padding: 12px;

    border-radius: 10px;

    background: rgba(22,163,74,.12);

    border: 1px solid #22c55e;

    color: #bbf7d0;
}

.solution-title {
    font-weight: bold;
    color: #4ade80;
    margin-bottom: 5px;
}

/* ================================
   MELDUNG
================================ */

.message {
    display: none;

    margin-top: 12px;

    padding: 12px;

    border-radius: 9px;
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

/* ================================
   BERECHNUNG
================================ */

.calculation {
    margin-top: 12px;

    padding: 12px;

    border-radius: 9px;

    background: #08111b;

    font-family: Consolas, monospace;

    font-size: 11px;

    line-height: 1.6;
}

/* ================================
   BUTTONS
================================ */

button {
    width: 100%;

    padding: 12px;

    margin-top: 9px;

    border: none;

    border-radius: 9px;

    color: white;

    font-size: 15px;
    font-weight: bold;

    cursor: pointer;
}

.new-game {
    background:
        linear-gradient(135deg, #7c3aed, #5b21b6);
}

.reset {
    background:
        linear-gradient(135deg, #475569, #334155);
}

button:hover {
    filter: brightness(1.12);
}

/* ================================
   RESPONSIVE
================================ */

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
</style>
</head>

<body>

<div class="container">

    <h1>⚖️ BALANCE-TEAM</h1>

    <div class="subtitle">
        Das Balance-Abenteuer
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

                <!-- Roter Mittelpunkt -->
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
                    🧒 Hilf der Figur!
                </div>

                Lege das richtige Gewicht auf ein
                hervorgehobenes Feld.

                Wenn die Balance stimmt,
                geht die Figur einen Schritt weiter.

                Die Gewichte bleiben liegen!
            </div>


            <div class="info-grid">

                <div class="info">
                    <div class="info-label">
                        Startfeld
                    </div>

                    <div
                        id="startField"
                        class="info-value">
                        -
                    </div>
                </div>

                <div class="info">
                    <div class="info-label">
                        Zielfeld
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
                        -
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
   EINSTELLUNG
========================================================= */

const SHOW_SOLUTION = true;


/* =========================================================
   GRUNDWERTE
========================================================= */

const SIZE = 4;

/*
   Gewicht der Figur.
*/

const FIGURE_WEIGHT = 2;


/*
   Erlaubte Gewichte.

   Das Spiel stellt daraus immer
   drei unterschiedliche Gewichte bereit.
*/

const WEIGHTS = [1,2,3,4,5,6];


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
   KOORDINATEN
========================================================= */

/*
   Die vier mittleren Felder sind:

   6 | 7
   -------
   10|11

   Der rote Punkt liegt genau zwischen diesen Feldern.

   Koordinaten:

   -1.5   -0.5    0.5    1.5
*/

function getCoordinate(field) {

    const index = field - 1;

    const row =
        Math.floor(index / 4);

    const col =
        index % 4;

    return {
        x: col - 1.5,
        y: row - 1.5
    };
}


/* =========================================================
   NACHBARN
========================================================= */

function getNeighbors(field) {

    const index = field - 1;

    const row =
        Math.floor(index / 4);

    const col =
        index % 4;

    const result = [];

    const directions = [
        [1,0],
        [-1,0],
        [0,1],
        [0,-1]
    ];

    directions.forEach(([dr,dc]) => {

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
    });

    return result;
}


/* =========================================================
   ZUFÄLLIGER WEG
========================================================= */

function generatePath() {

    startField =
        randomInt(1,4);

    goalField =
        randomInt(13,16);

    const result = [startField];

    let current = startField;

    const visited =
        new Set(result);

    let safety = 0;

    while (
        current !== goalField &&
        safety < 300
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
           Ziel bevorzugen, wenn
           es direkt erreichbar ist.
        */

        if (
            neighbors.includes(goalField)
        ) {

            current = goalField;

        } else {

            /*
               Nach unten gerichtete
               Bewegung bevorzugen.
            */

            const row =
                Math.floor(
                    (current - 1) / 4
                );

            const down =
                neighbors.filter(
                    f =>
                        Math.floor(
                            (f - 1) / 4
                        ) >= row
                );

            if (
                down.length &&
                Math.random() < .75
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

        result.push(current);

        visited.add(current);
    }

    if (
        current !== goalField
    ) {
        return generatePath();
    }

    return result;
}


/* =========================================================
   BALANCE BERECHNEN
========================================================= */

function calculateBalance(
    testWeight = null,
    testField = null
) {

    /*
       Wir betrachten die Richtung
       vom roten Mittelpunkt zur Figur.

       Das macht die Balance für Kinder
       räumlich verständlich.
    */

    const figure =
        getCoordinate(
            currentField
        );

    /*
       Figur erzeugt ein Drehmoment.

       Je weiter die Figur vom roten
       Mittelpunkt entfernt ist,
       desto größer ihr Einfluss.
    */

    let totalX =
        FIGURE_WEIGHT *
        figure.x;

    let totalY =
        FIGURE_WEIGHT *
        figure.y;


    /*
       Alle bereits liegenden Gewichte
       bleiben aktiv!
    */

    placedWeights.forEach(item => {

        const p =
            getCoordinate(
                item.field
            );

        totalX +=
            item.weight * p.x;

        totalY +=
            item.weight * p.y;
    });


    /*
       Testgewicht hinzufügen.
    */

    if (
        testWeight !== null &&
        testField !== null
    ) {

        const p =
            getCoordinate(
                testField
            );

        totalX +=
            testWeight * p.x;

        totalY +=
            testWeight * p.y;
    }


    return {
        x: totalX,
        y: totalY
    };
}


/* =========================================================
   LÖSUNG SUCHEN
========================================================= */

function findSolution() {

    const freeFields = [];

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

        freeFields.push(field);
    }


    const solutions = [];


    /*
       Alle drei-dimensionalen Kombinationen
       werden mathematisch geprüft.

       Für eine kindgerechte Lösung reicht
       eine sehr kleine Toleranz.
    */

    for (
        const field of freeFields
    ) {

        for (
            const weight of WEIGHTS
        ) {

            const result =
                calculateBalance(
                    weight,
                    field
                );

            /*
               Wir suchen eine möglichst
               ausgewogene Position.

               Da das 4×4-Brett nicht immer
               eine exakte Null zulässt,
               wird die kleinste Abweichung
               ermittelt.
            */

            const error =
                Math.sqrt(
                    result.x * result.x +
                    result.y * result.y
                );

            solutions.push({
                field,
                weight,
                error
            });
        }
    }


    if (
        solutions.length === 0
    ) {
        return null;
    }


    /*
       Kleinste Abweichung zuerst.
    */

    solutions.sort(
        (a,b) =>
            a.error - b.error
    );


    /*
       Nur Lösungen mit sinnvoller
       Balance akzeptieren.

       Falls keine perfekte Lösung
       existiert, nehmen wir die beste.
    */

    const best =
        solutions[0];

    return best;
}


/* =========================================================
   DREI AUSWAHLGEWICHTE
========================================================= */

function createWeightChoices() {

    currentSolution =
        findSolution();

    if (
        !currentSolution
    ) {
        return;
    }


    const values = [
        currentSolution.weight
    ];


    while (
        values.length < 3
    ) {

        const candidate =
            randomChoice(WEIGHTS);

        if (
            !values.includes(candidate)
        ) {
            values.push(candidate);
        }
    }


    availableWeights =
        shuffle(values);

    selectedWeight = null;
}


/* =========================================================
   FELD KLICK
========================================================= */

function clickField(field) {

    if (locked) {
        return;
    }


    /*
       Liegt hier schon ein Gewicht?
    */

    const existing =
        placedWeights.find(
            item =>
                item.field === field
        );


    if (existing) {

        /*
           Richtiges Gewicht wurde
           eingefroren.
        */

        if (
            existing.frozen
        ) {

            showMessage(
                "🔒 Dieses Gewicht bleibt liegen!",
                "info"
            );

            return;
        }


        /*
           Falsches Gewicht darf
           entfernt werden.
        */

        placedWeights =
            placedWeights.filter(
                item =>
                    item !== existing
            );

        availableWeights.push(
            existing.weight
        );

        selectedWeight = null;

        createWeightChoices();

        render();

        showMessage(
            "↩ Gewicht wurde entfernt.",
            "info"
        );

        return;
    }


    if (
        selectedWeight === null
    ) {

        showMessage(
            "Wähle zuerst ein Gewicht aus.",
            "error"
        );

        return;
    }


    /*
       Gewicht ablegen.
    */

    placedWeights.push({

        field: field,

        weight: selectedWeight,

        frozen: false
    });


    /*
       Prüfen.
    */

    const result =
        calculateBalance();


    /*
       Erlaubte Toleranz.

       Für das Spiel wird die
       bestmögliche Balance verwendet.
    */

    const isCorrect =
        field === currentSolution.field &&
        selectedWeight ===
            currentSolution.weight;


    selectedWeight = null;


    if (
        isCorrect
    ) {

        /*
           Gewicht einfrieren.
        */

        const placed =
            placedWeights.find(
                item =>
                    item.field === field
            );

        placed.frozen = true;

        locked = true;

        render();

        showCorrectMark(field);

        showMessage(
            "🎉 Richtig! Die Balance stimmt. Die Figur geht weiter!",
            "success"
        );


        setTimeout(() => {

            removeCorrectMark();

            moveFigure();

        },1000);

    } else {

        render();

        showMessage(
            "🤔 Noch nicht richtig. Die Figur bleibt stehen.",
            "error"
        );
    }
}


/* =========================================================
   FIGUR WEITERBEWEGEN
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
       Nur die Figur bewegt sich.

       ALLE GEWICHTE BLEIBEN LIEGEN.
    */

    currentIndex++;

    currentField =
        path[currentIndex];


    locked = false;


    /*
       Neue Balance!

       Die alten Gewichte werden
       automatisch erneut berücksichtigt.
    */

    createWeightChoices();

    render();

    showMessage(
        "➡️ Die Figur ist ein Feld weiter. Die Balance hat sich verändert!",
        "info"
    );
}


/* =========================================================
   GEWICHT ENTFERNEN
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
        "↩ Alle nicht eingefrorenen Gewichte wurden entfernt.",
        "info"
    );
}


/* =========================================================
   SPIEL BEENDET
========================================================= */

function finishGame() {

    locked = true;

    showMessage(
        "🏆 SUPER! Die Figur hat das Ziel erreicht!",
        "success"
    );
}


/* =========================================================
   RICHTIG-HÄKCHEN
========================================================= */

function showCorrectMark(field) {

    const cells =
        document.querySelectorAll(".cell");

    cells.forEach((cell,index) => {

        if (
            index + 1 === field
        ) {

            const mark =
                document.createElement("div");

            mark.className =
                "correct-check";

            mark.textContent = "✓";

            cell.appendChild(mark);
        }
    });
}


function removeCorrectMark() {

    document
        .querySelectorAll(".correct-check")
        .forEach(
            item =>
                item.remove()
        );
}


/* =========================================================
   SPIELBRETT RENDERN
========================================================= */

function render() {

    const board =
        document.getElementById("board");

    board.innerHTML = "";


    /*
       Mögliche Felder hervorheben.
    */

    let placeable = [];

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
            document.createElement("div");

        cell.className = "cell";


        if (
            path.includes(field)
        ) {
            cell.classList.add("path");
        }


        if (
            field === startField
        ) {
            cell.classList.add("start");
        }


        if (
            field === goalField
        ) {
            cell.classList.add("goal");
        }


        if (
            field === currentField
        ) {
            cell.classList.add("current");
        }


        /*
           Lösung sichtbar.
        */

        if (
            SHOW_SOLUTION &&
            currentSolution &&
            field === currentSolution.field
        ) {

            cell.classList.add("solution");
        }


        if (
            placeable.includes(field)
        ) {

            cell.classList.add("placeable");
        }


        /*
           Figur.
        */

        if (
            field === currentField
        ) {

            const figure =
                document.createElement("div");

            figure.className = "figure";

            figure.innerHTML = `
                🧒
                <div class="figure-small">
                    ${FIGURE_WEIGHT} Gewicht
                </div>
            `;

            cell.appendChild(figure);
        }


        /*
           Gewichte.
        */

        const weights =
            placedWeights.filter(
                item =>
                    item.field === field
            );


        if (
            weights.length
        ) {

            weights.forEach(item => {

                const stack =
                    createWeightStack(
                        item.weight,
                        item.frozen
                    );

                cell.appendChild(stack);
            });
        }


        cell.addEventListener(
            "click",
            () => clickField(field)
        );


        board.appendChild(cell);
    }


    updateInfo();

    renderChoices();

    renderSolution();

    showCalculation();
}


/* =========================================================
   GEWICHTSSTAPEL
========================================================= */

function createWeightStack(
    count,
    frozen = false
) {

    const stack =
        document.createElement("div");

    stack.className =
        "weight-stack";


    if (
        frozen
    ) {
        stack.classList.add("frozen");
    }


    /*
       Die gewünschten Formen:

       1 = 1

       2 = 1 oben + 1 unten

       3 = 1 + 2

       4 = 2 + 2

       5 = 1 + 2 + 2

       6 = 3 + 3
    */

    let rows = [];

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


    rows.forEach(rowCount => {

        const row =
            document.createElement("div");

        row.className =
            "weight-row";


        for (
            let i = 0;
            i < rowCount;
            i++
        ) {

            const block =
                document.createElement("div");

            block.className =
                "weight-block";

            row.appendChild(block);
        }


        stack.appendChild(row);
    });


    return stack;
}


/* =========================================================
   AUSWAHL RENDERN
========================================================= */

function renderChoices() {

    const container =
        document.getElementById("weights");

    container.innerHTML = "";


    availableWeights.forEach(weight => {

        const option =
            document.createElement("div");

        option.className =
            "weight-option";


        if (
            selectedWeight === weight
        ) {

            option.classList.add("selected");
        }


        if (
            SHOW_SOLUTION &&
            currentSolution &&
            weight === currentSolution.weight
        ) {

            option.classList.add(
                "solution-weight"
            );
        }


        const stack =
            createWeightStack(weight);


        stack.classList.add(
            "choice-stack"
        );


        option.appendChild(stack);


        const label =
            document.createElement("div");

        label.className =
            "weight-number";

        label.textContent =
            `${weight} Gewicht${
                weight === 1 ? "" : "e"
            }`;


        option.appendChild(label);


        option.addEventListener(
            "click",
            () => {

                if (locked) {
                    return;
                }

                selectedWeight = weight;

                render();

                showMessage(
                    "Jetzt ein hervorgehobenes Feld anklicken.",
                    "info"
                );
            }
        );


        container.appendChild(option);
    });
}


/* =========================================================
   INFORMATIONEN
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
        "figureWeight"
    ).textContent =
        `${FIGURE_WEIGHT} Gewichte`;

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


    box.innerHTML = `
        Richtig ist:
        <strong>
            ${currentSolution.weight} Gewichte
        </strong>
        auf
        <strong>
            Feld ${currentSolution.field}
        </strong>.
    `;
}


/* =========================================================
   BERECHNUNG ANZEIGEN
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
        `Figur: ${FIGURE_WEIGHT} Gewichte<br>`;

    html +=
        `Figur steht auf Feld: ${currentField}<br>`;

    html +=
        `Balance X: ${balance.x.toFixed(2)}<br>`;

    html +=
        `Balance Y: ${balance.y.toFixed(2)}<br><br>`;


    if (
        placedWeights.length
    ) {

        html +=
            "<strong>Liegende Gewichte:</strong><br>";


        placedWeights.forEach(item => {

            html +=
                `${item.frozen ? "✓" : "○"}
                 ${item.weight}
                 → Feld ${item.field}<br>`;
        });

    } else {

        html +=
            "Noch keine Gewichte auf dem Brett.<br>";
    }


    box.innerHTML = html;
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

    currentField =
        path[0];

    createWeightChoices();

    render();

    showMessage(
        "🎲 Neue Runde! Wähle ein Gewicht.",
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
