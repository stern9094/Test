<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BALANCE-TEAM – 5×5 Balance-Spiel</title>

<style>
    * {
        box-sizing: border-box;
    }

    body {
        margin: 0;
        font-family: Arial, Helvetica, sans-serif;
        background:
            radial-gradient(circle at top, #243b55 0%, #141e30 55%, #0d1320 100%);
        color: #fff;
        min-height: 100vh;
    }

    .container {
        width: min(1100px, 95%);
        margin: 0 auto;
        padding: 25px 0 40px;
    }

    h1 {
        text-align: center;
        margin: 5px 0 8px;
        font-size: 38px;
        letter-spacing: 2px;
    }

    .subtitle {
        text-align: center;
        color: #b8c7d9;
        margin-bottom: 25px;
    }

    .layout {
        display: grid;
        grid-template-columns: 1fr 350px;
        gap: 25px;
        align-items: start;
    }

    .card {
        background: rgba(255,255,255,0.08);
        border: 1px solid rgba(255,255,255,0.12);
        border-radius: 18px;
        padding: 20px;
        box-shadow: 0 15px 40px rgba(0,0,0,0.25);
        backdrop-filter: blur(8px);
    }

    /* Spielfeld */
    .board {
        width: min(650px, 100%);
        aspect-ratio: 1;
        margin: auto;
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        grid-template-rows: repeat(5, 1fr);
        gap: 6px;
    }

    .cell {
        position: relative;
        border: 2px solid #52677f;
        border-radius: 10px;
        background: linear-gradient(145deg, #263b52, #1a2a3c);
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: clamp(17px, 3vw, 28px);
        font-weight: bold;
        cursor: pointer;
        transition: 0.15s ease;
        user-select: none;
    }

    .cell:hover {
        transform: scale(1.03);
        border-color: #71d7ff;
        z-index: 2;
    }

    .cell.center {
        background:
            radial-gradient(circle, #ffe066 0%, #f5a623 55%, #9b5b00 100%);
        color: #151515;
        border-color: #fff0a6;
        box-shadow: 0 0 25px rgba(255, 203, 70, 0.55);
    }

    .cell.path {
        background: linear-gradient(145deg, #285b7a, #17384e);
        border-color: #4fc3f7;
    }

    .cell.current {
        background: linear-gradient(145deg, #7c3aed, #4c1d95);
        border-color: #c4b5fd;
        box-shadow: 0 0 25px rgba(139,92,246,0.6);
        transform: scale(1.04);
        z-index: 3;
    }

    .cell.figure {
        background: linear-gradient(145deg, #ef4444, #991b1b);
        border-color: #fecaca;
        box-shadow: 0 0 30px rgba(239,68,68,0.6);
    }

    .cell.solution {
        background: linear-gradient(145deg, #16a34a, #166534);
        border-color: #86efac;
        box-shadow: 0 0 25px rgba(34,197,94,0.5);
    }

    .cell-number {
        position: absolute;
        top: 5px;
        left: 7px;
        font-size: 11px;
        opacity: 0.65;
        font-weight: normal;
    }

    .cell-coordinate {
        position: absolute;
        bottom: 4px;
        right: 6px;
        font-size: 9px;
        opacity: 0.45;
        font-weight: normal;
    }

    .icon {
        font-size: clamp(25px, 4vw, 42px);
    }

    /* Rechte Seite */
    .status {
        padding: 15px;
        border-radius: 12px;
        background: rgba(0,0,0,0.18);
        margin-bottom: 15px;
        line-height: 1.5;
    }

    .status strong {
        color: #71d7ff;
    }

    .stat-grid {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 10px;
        margin: 15px 0;
    }

    .stat {
        background: rgba(0,0,0,0.18);
        border-radius: 10px;
        padding: 12px;
    }

    .stat-label {
        font-size: 12px;
        color: #9fb0c3;
    }

    .stat-value {
        font-size: 22px;
        font-weight: bold;
        margin-top: 4px;
    }

    label {
        display: block;
        margin: 12px 0 6px;
        color: #cbd5e1;
        font-size: 14px;
    }

    input,
    select {
        width: 100%;
        padding: 12px;
        border-radius: 9px;
        border: 1px solid #52677f;
        background: #111c29;
        color: white;
        font-size: 16px;
        outline: none;
    }

    input:focus,
    select:focus {
        border-color: #4fc3f7;
        box-shadow: 0 0 0 2px rgba(79,195,247,0.15);
    }

    button {
        width: 100%;
        padding: 13px 16px;
        margin-top: 10px;
        border: none;
        border-radius: 10px;
        color: white;
        background: linear-gradient(135deg, #0284c7, #0369a1);
        font-size: 16px;
        font-weight: bold;
        cursor: pointer;
        transition: 0.15s;
    }

    button:hover {
        transform: translateY(-1px);
        filter: brightness(1.12);
    }

    button.secondary {
        background: linear-gradient(135deg, #475569, #334155);
    }

    button.success {
        background: linear-gradient(135deg, #16a34a, #15803d);
    }

    button.warning {
        background: linear-gradient(135deg, #d97706, #b45309);
    }

    .message {
        margin-top: 15px;
        padding: 14px;
        border-radius: 10px;
        display: none;
        line-height: 1.5;
    }

    .message.success {
        display: block;
        background: rgba(22,163,74,0.2);
        border: 1px solid #22c55e;
        color: #bbf7d0;
    }

    .message.error {
        display: block;
        background: rgba(220,38,38,0.2);
        border: 1px solid #ef4444;
        color: #fecaca;
    }

    .message.info {
        display: block;
        background: rgba(14,116,144,0.2);
        border: 1px solid #22d3ee;
        color: #cffafe;
    }

    .calculation {
        margin-top: 15px;
        background: #0b1420;
        border-radius: 10px;
        padding: 14px;
        font-family: Consolas, monospace;
        font-size: 13px;
        line-height: 1.7;
        overflow-x: auto;
    }

    .formula {
        color: #67e8f9;
    }

    .good {
        color: #86efac;
    }

    .bad {
        color: #fca5a5;
    }

    .path-info {
        margin-top: 18px;
        padding: 13px;
        background: rgba(0,0,0,0.18);
        border-radius: 10px;
    }

    .path-list {
        display: flex;
        flex-wrap: wrap;
        gap: 5px;
        margin-top: 8px;
    }

    .path-item {
        background: #263b52;
        border: 1px solid #52677f;
        border-radius: 6px;
        padding: 4px 8px;
        font-size: 13px;
    }

    .path-item.active {
        background: #7c3aed;
        border-color: #c4b5fd;
    }

    .legend {
        display: flex;
        flex-wrap: wrap;
        gap: 12px;
        margin-top: 15px;
        justify-content: center;
    }

    .legend-item {
        display: flex;
        align-items: center;
        gap: 6px;
        font-size: 12px;
        color: #cbd5e1;
    }

    .legend-color {
        width: 16px;
        height: 16px;
        border-radius: 4px;
    }

    .footer {
        text-align: center;
        margin-top: 25px;
        color: #718096;
        font-size: 12px;
    }

    .hidden {
        display: none;
    }

    @media (max-width: 850px) {
        .layout {
            grid-template-columns: 1fr;
        }

        .board {
            max-width: 600px;
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
        5 × 5 Balance-Spiel · Mittelpunkt = Feld 13 · mathematische Berechnung
    </div>

    <div class="layout">

        <!-- SPIELFELD -->
        <div class="card">

            <div id="board" class="board"></div>

            <div class="legend">
                <div class="legend-item">
                    <span class="legend-color" style="background:#f5a623"></span>
                    Drehpunkt
                </div>

                <div class="legend-item">
                    <span class="legend-color" style="background:#7c3aed"></span>
                    aktuelles Feld
                </div>

                <div class="legend-item">
                    <span class="legend-color" style="background:#ef4444"></span>
                    Figur
                </div>

                <div class="legend-item">
                    <span class="legend-color" style="background:#16a34a"></span>
                    Gegengewicht
                </div>
            </div>

        </div>

        <!-- SPIELSTEUERUNG -->
        <div class="card">

            <div class="status">
                <strong>Aufgabe</strong><br>
                Bringe das Spielfeld ins Gleichgewicht.
                Die rote Figur steht auf dem aktuellen Feld.
            </div>

            <div class="stat-grid">

                <div class="stat">
                    <div class="stat-label">Figur</div>
                    <div id="figureWeight" class="stat-value">2,0 kg</div>
                </div>

                <div class="stat">
                    <div class="stat-label">Feld</div>
                    <div id="currentField" class="stat-value">–</div>
                </div>

                <div class="stat">
                    <div class="stat-label">X-Koordinate</div>
                    <div id="currentX" class="stat-value">–</div>
                </div>

                <div class="stat">
                    <div class="stat-label">Y-Koordinate</div>
                    <div id="currentY" class="stat-value">–</div>
                </div>

            </div>

            <label for="counterField">
                Gegengewicht auf Feld
            </label>

            <select id="counterField">
                <option value="">Bitte Feld auswählen</option>
            </select>

            <label for="counterWeight">
                Gewicht des Gegengewichts (kg)
            </label>

            <input
                id="counterWeight"
                type="number"
                min="0.1"
                max="20"
                step="0.1"
                value="2.0"
            >

            <button onclick="checkAnswer()">
                ⚖️ Gleichgewicht prüfen
            </button>

            <button class="secondary" onclick="showCalculation()">
                🧮 Berechnung anzeigen
            </button>

            <button class="success" onclick="newChallenge()">
                🎲 Neue Aufgabe
            </button>

            <button class="warning" onclick="showSolution()">
                💡 Lösung anzeigen
            </button>

            <div id="message" class="message"></div>

            <div id="calculation" class="calculation hidden"></div>

            <div class="path-info">
                <strong>Zufälliger Weg</strong>

                <div id="pathList" class="path-list"></div>
            </div>

        </div>

    </div>

    <div class="footer">
        BALANCE-TEAM · 5×5 mathematisches Balance-System
    </div>

</div>

<script>

/* ============================================================
   GRUNDKONSTANTEN
   ============================================================ */

const SIZE = 5;

// Feld 13 ist der Mittelpunkt.
const CENTER_FIELD = 13;

// Gewicht der Figur
const FIGURE_WEIGHT = 2.0;

// Anzahl Felder für einen zufälligen Weg
const MIN_PATH_LENGTH = 8;
const MAX_PATH_LENGTH = 15;

// Genauigkeit für Vergleiche
const EPSILON = 0.000001;


/* ============================================================
   SPIELZUSTAND
   ============================================================ */

let path = [];
let currentPathIndex = 0;
let currentField = null;
let solution = null;


/* ============================================================
   FELD-KOORDINATEN
   ============================================================

   Feld 13 = (0,0)

             Y
             ↑
       -2    -1    0    1    2

   1    (-2,-2) ...       5
   6    (-2,-1) ...      10
   11   (-2, 0) ...      15
   16   (-2, 1) ...      20
   21   (-2, 2) ...      25

   X läuft von links nach rechts.
   Y läuft von oben nach unten.

   Für die Physik verwenden wir:
   oben = -Y
   unten = +Y
   ============================================================ */

function fieldToCoordinate(field) {

    const index = field - 1;

    const row = Math.floor(index / SIZE);
    const col = index % SIZE;

    const x = col - 2;
    const y = row - 2;

    return { x, y };
}


/* ============================================================
   KOORDINATE -> FELD
   ============================================================ */

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


/* ============================================================
   DISTANZ VOM DREHPUNKT
   ============================================================ */

function distanceFromCenter(field) {

    const p = fieldToCoordinate(field);

    return Math.sqrt(
        p.x * p.x +
        p.y * p.y
    );
}


/* ============================================================
   PRÜFEN, OB ZWEI VEKTOREN GEGENÜBERLIEGEN
   ============================================================

   Für eine echte 2D-Balance müssen die beiden Vektoren
   auf derselben Linie liegen und entgegengesetzte Richtung
   besitzen.

   Beispiel:

       Figur
         ↘
          \
           ● Mittelpunkt
          /
         ↗
       Gegengewicht

   Cross Product = 0
   Dot Product < 0
   ============================================================ */

function areOppositeVectors(fieldA, fieldB) {

    const a = fieldToCoordinate(fieldA);
    const b = fieldToCoordinate(fieldB);

    const cross =
        a.x * b.y -
        a.y * b.x;

    const dot =
        a.x * b.x +
        a.y * b.y;

    return (
        Math.abs(cross) < EPSILON &&
        dot < 0
    );
}


/* ============================================================
   BENÖTIGTES GEGENGEWICHT
   ============================================================

   Drehmoment:

       M = Gewicht × Abstand

   Für Gleichgewicht:

       M1 = M2

   Also:

       W1 × r1 = W2 × r2

   Daraus:

       W2 = W1 × r1 / r2
   ============================================================ */

function calculateRequiredWeight(figureField, counterField) {

    const r1 = distanceFromCenter(figureField);
    const r2 = distanceFromCenter(counterField);

    if (r1 === 0 || r2 === 0) {
        return null;
    }

    return FIGURE_WEIGHT * r1 / r2;
}


/* ============================================================
   GÜLTIGE GEGENGEWICHTE FINDEN
   ============================================================ */

function findSolutions(figureField) {

    const solutions = [];

    for (let field = 1; field <= 25; field++) {

        // Mittelpunkt kann kein Gegengewicht sein.
        if (field === CENTER_FIELD) {
            continue;
        }

        // Feld muss genau entgegengesetzt liegen.
        if (!areOppositeVectors(figureField, field)) {
            continue;
        }

        const weight =
            calculateRequiredWeight(
                figureField,
                field
            );

        if (weight === null) {
            continue;
        }

        solutions.push({
            field: field,
            weight: weight,
            distance: distanceFromCenter(field)
        });
    }

    // Zufällige Reihenfolge
    solutions.sort(() => Math.random() - 0.5);

    return solutions;
}


/* ============================================================
   ZUFÄLLIGER NACHBAR
   ============================================================ */

function getNeighbors(field) {

    const p = fieldToCoordinate(field);

    const directions = [
        { x: 1, y: 0 },
        { x: -1, y: 0 },
        { x: 0, y: 1 },
        { x: 0, y: -1 }
    ];

    const result = [];

    for (const d of directions) {

        const x = p.x + d.x;
        const y = p.y + d.y;

        const field2 =
            coordinateToField(x, y);

        if (field2 !== null) {
            result.push(field2);
        }
    }

    return result;
}


/* ============================================================
   ZUFÄLLIGEN WEG ERZEUGEN
   ============================================================

   Der Weg besteht aus benachbarten Feldern.

   Ein Feld wird innerhalb eines Weges nicht zweimal benutzt.
   ============================================================ */

function generateRandomPath() {

    const targetLength =
        randomInt(
            MIN_PATH_LENGTH,
            MAX_PATH_LENGTH
        );

    let start;

    // Mittelpunkt nicht als Start verwenden.
    do {
        start = randomInt(1, 25);
    } while (start === CENTER_FIELD);

    const result = [start];

    const used = new Set(result);

    let current = start;

    while (result.length < targetLength) {

        let neighbors =
            getNeighbors(current)
            .filter(f => !used.has(f));

        // Wenn kein Weg mehr möglich ist:
        if (neighbors.length === 0) {
            break;
        }

        // Zufälligen nächsten Schritt auswählen.
        current =
            neighbors[
                randomInt(
                    0,
                    neighbors.length - 1
                )
            ];

        result.push(current);
        used.add(current);
    }

    return result;
}


/* ============================================================
   ZUFALLSZAHL
   ============================================================ */

function randomInt(min, max) {

    return Math.floor(
        Math.random() *
        (max - min + 1)
    ) + min;
}


/* ============================================================
   SPIELFELD AUFBAUEN
   ============================================================ */

function renderBoard() {

    const board =
        document.getElementById("board");

    board.innerHTML = "";

    for (let field = 1; field <= 25; field++) {

        const cell =
            document.createElement("div");

        cell.className = "cell";

        const coordinate =
            fieldToCoordinate(field);

        if (field === CENTER_FIELD) {
            cell.classList.add("center");
        }

        if (path.includes(field)) {
            cell.classList.add("path");
        }

        if (field === currentField) {
            cell.classList.add("current");
            cell.classList.add("figure");
        }

        cell.innerHTML = `
            <span class="cell-number">${field}</span>
            <span class="icon">
                ${field === currentField ? "⚖️" : ""}
            </span>
            <span class="cell-coordinate">
                (${coordinate.x},${coordinate.y})
            </span>
        `;

        cell.onclick = () => selectField(field);

        board.appendChild(cell);
    }

    if (solution && solution.field) {

        // Lösung nur anzeigen, wenn ausdrücklich aktiviert.
        if (solution.show) {

            const cells =
                board.children;

            cells[solution.field - 1]
                .classList.add("solution");
        }
    }
}


/* ============================================================
   FELD AUSWÄHLEN
   ============================================================ */

function selectField(field) {

    // Nur Felder des aktuellen Weges dürfen gewählt werden.
    const index = path.indexOf(field);

    if (index === -1) {
        showMessage(
            "Bitte wähle ein Feld des aktuellen Weges.",
            "error"
        );

        return;
    }

    currentPathIndex = index;
    currentField = field;

    solution = null;

    updateInformation();
    populateCounterFields();
    renderBoard();
    updatePathDisplay();

    hideCalculation();

    showMessage(
        "Aufgabe gewechselt. Berechne das notwendige Gegengewicht.",
        "info"
    );
}


/* ============================================================
   INFORMATIONEN AKTUALISIEREN
   ============================================================ */

function updateInformation() {

    const p =
        fieldToCoordinate(currentField);

    document.getElementById(
        "figureWeight"
    ).textContent =
        formatKg(FIGURE_WEIGHT);

    document.getElementById(
        "currentField"
    ).textContent =
        currentField;

    document.getElementById(
        "currentX"
    ).textContent =
        p.x;

    document.getElementById(
        "currentY"
    ).textContent =
        p.y;
}


/* ============================================================
   GEGENGEWICHT-FELDER EINTRAGEN
   ============================================================ */

function populateCounterFields() {

    const select =
        document.getElementById("counterField");

    select.innerHTML =
        `<option value="">Bitte Feld auswählen</option>`;

    for (let field = 1; field <= 25; field++) {

        if (field === CENTER_FIELD) {
            continue;
        }

        if (field === currentField) {
            continue;
        }

        const p =
            fieldToCoordinate(field);

        const option =
            document.createElement("option");

        option.value = field;

        option.textContent =
            `Feld ${field} (${p.x}, ${p.y})`;

        select.appendChild(option);
    }
}


/* ============================================================
   ANTWORT PRÜFEN
   ============================================================ */

function checkAnswer() {

    if (!currentField) {
        showMessage(
            "Es wurde noch kein Feld ausgewählt.",
            "error"
        );
        return;
    }

    const field =
        Number(
            document.getElementById(
                "counterField"
            ).value
        );

    const weight =
        Number(
            document.getElementById(
                "counterWeight"
            ).value
        );

    if (!field) {
        showMessage(
            "Bitte wähle ein Gegengewicht-Feld.",
            "error"
        );
        return;
    }

    if (!weight || weight <= 0) {
        showMessage(
            "Bitte gib ein gültiges Gewicht ein.",
            "error"
        );
        return;
    }

    const solutions =
        findSolutions(currentField);

    const matching =
        solutions.find(s =>
            s.field === field
        );

    if (!matching) {

        showMessage(
            `❌ Feld ${field} liegt nicht in der richtigen Gegenrichtung.
             Für ein echtes 2D-Gleichgewicht müssen die beiden Kräfte
             genau auf einer entgegengesetzten Linie durch Feld 13 liegen.`,
            "error"
        );

        return;
    }

    const difference =
        Math.abs(
            weight - matching.weight
        );

    const tolerance = 0.05;

    if (difference <= tolerance) {

        showMessage(
            `✅ Richtig!

             Feld ${field} ist eine gültige Gegenposition.
             Das benötigte Gewicht beträgt
             ${formatKg(matching.weight)}.

             Deine Eingabe:
             ${formatKg(weight)}

             Abweichung:
             ${formatKg(difference)}`,
            "success"
        );

        showCalculation();

        // automatisch zur nächsten Aufgabe
        setTimeout(() => {

            if (
                currentPathIndex <
                path.length - 1
            ) {

                currentPathIndex++;

                currentField =
                    path[currentPathIndex];

                solution = null;

                updateInformation();
                populateCounterFields();
                renderBoard();
                updatePathDisplay();

            }

        }, 1800);

    } else {

        showMessage(
            `❌ Fast, aber das Gewicht stimmt nicht.

             Benötigt:
             ${formatKg(matching.weight)}

             Deine Eingabe:
             ${formatKg(weight)}

             Differenz:
             ${formatKg(difference)}`,
            "error"
        );

        showCalculation();
    }
}


/* ============================================================
   BERECHNUNG ANZEIGEN
   ============================================================ */

function showCalculation() {

    if (!currentField) {
        return;
    }

    const solutions =
        findSolutions(currentField);

    const figure =
        fieldToCoordinate(currentField);

    const r1 =
        distanceFromCenter(currentField);

    let html = "";

    html += `
        <div>
            <strong>MATHEMATISCHE BERECHNUNG</strong>
        </div>
        <br>
    `;

    html += `
        Figur:<br>
        Feld ${currentField}<br>
        Koordinate: (${figure.x}, ${figure.y})<br>
        Gewicht: ${formatKg(FIGURE_WEIGHT)}<br>
        Abstand vom Drehpunkt:
        ${r1.toFixed(3)}
        <br><br>
    `;

    html += `
        <span class="formula">
        M = Gewicht × Abstand
        </span>
        <br>
    `;

    html += `
        M₁ =
        ${FIGURE_WEIGHT.toFixed(2)}
        ×
        ${r1.toFixed(3)}
        =
        ${(FIGURE_WEIGHT * r1).toFixed(3)}
        <br><br>
    `;

    if (solutions.length === 0) {

        html += `
            <span class="bad">
            Keine gültige Gegenposition gefunden.
            </span>
        `;

    } else {

        html += `
            <strong>Mögliche Gegengewichte:</strong>
            <br><br>
        `;

        for (const s of solutions) {

            const p =
                fieldToCoordinate(s.field);

            html += `
                Feld ${s.field}
                (${p.x}, ${p.y})
                →
                ${formatKg(s.weight)}
                <br>
            `;
        }

        html += `
            <br>
            <span class="formula">
            Formel:
            W₂ = W₁ × r₁ / r₂
            </span>
        `;
    }

    const calculation =
        document.getElementById(
            "calculation"
        );

    calculation.innerHTML = html;

    calculation.classList.remove("hidden");
}


/* ============================================================
   BERECHNUNG VERSTECKEN
   ============================================================ */

function hideCalculation() {

    document.getElementById(
        "calculation"
    ).classList.add("hidden");
}


/* ============================================================
   LÖSUNG ANZEIGEN
   ============================================================ */

function showSolution() {

    if (!currentField) {
        return;
    }

    const solutions =
        findSolutions(currentField);

    if (solutions.length === 0) {

        showMessage(
            "Für dieses Feld existiert keine gültige Gegenposition.",
            "error"
        );

        return;
    }

    solution = {
        ...solutions[0],
        show: true
    };

    const p =
        fieldToCoordinate(solution.field);

    document.getElementById(
        "counterField"
    ).value =
        solution.field;

    document.getElementById(
        "counterWeight"
    ).value =
        solution.weight.toFixed(1);

    renderBoard();

    showMessage(
        `💡 Eine mögliche Lösung:

         Feld ${solution.field}
         Koordinate (${p.x}, ${p.y})
         Gewicht ${formatKg(solution.weight)}`,
        "info"
    );

    showCalculation();
}


/* ============================================================
   NEUE AUFGABE
   ============================================================ */

function newChallenge() {

    path =
        generateRandomPath();

    currentPathIndex = 0;

    currentField =
        path[0];

    solution = null;

    updateInformation();
    populateCounterFields();
    renderBoard();
    updatePathDisplay();
    hideCalculation();

    showMessage(
        `🎲 Neuer zufälliger Weg erzeugt.
         Starte bei Feld ${currentField}.`,
        "info"
    );
}


/* ============================================================
   WEG ANZEIGEN
   ============================================================ */

function updatePathDisplay() {

    const container =
        document.getElementById(
            "pathList"
        );

    container.innerHTML = "";

    path.forEach((field, index) => {

        const item =
            document.createElement("div");

        item.className =
            "path-item";

        if (index === currentPathIndex) {
            item.classList.add("active");
        }

        item.textContent =
            field;

        item.onclick = () =>
            selectField(field);

        container.appendChild(item);
    });
}


/* ============================================================
   NACHRICHTEN
   ============================================================ */

function showMessage(text, type) {

    const box =
        document.getElementById(
            "message"
        );

    box.className =
        "message " + type;

    box.textContent =
        text;
}


/* ============================================================
   GEWICHT FORMATIEREN
   ============================================================ */

function formatKg(value) {

    return Number(value)
        .toFixed(2)
        .replace(".", ",") + " kg";
}


/* ============================================================
   INITIALISIERUNG
   ============================================================ */

function init() {

    newChallenge();
}

init();

</script>

</body>
</html>
