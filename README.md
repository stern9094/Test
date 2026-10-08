<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kugel-Rätsel</title>

<style>
    * {
        box-sizing: border-box;
    }

    body {
        margin: 0;
        min-height: 100vh;
        background: #111827;
        color: white;
        font-family: Arial, sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .game {
        width: min(95vw, 700px);
        text-align: center;
    }

    h1 {
        margin-bottom: 8px;
    }

    .subtitle {
        color: #9ca3af;
        margin-bottom: 25px;
    }

    .screen {
        background: #1f2937;
        border-radius: 20px;
        padding: 25px;
        box-shadow: 0 15px 50px rgba(0,0,0,.4);
    }

    .phase {
        color: #60a5fa;
        font-size: 14px;
        font-weight: bold;
        letter-spacing: 2px;
        margin-bottom: 12px;
    }

    .question {
        font-size: 27px;
        font-weight: bold;
        min-height: 70px;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .timer {
        font-size: 42px;
        font-weight: bold;
        color: #22c55e;
        margin: 10px 0 20px;
    }

    .timer.warning {
        color: #f59e0b;
    }

    .timer.danger {
        color: #ef4444;
        animation: pulse .5s infinite alternate;
    }

    @keyframes pulse {
        from { transform: scale(1); }
        to { transform: scale(1.08); }
    }

    .board {
        width: min(90vw, 450px);
        aspect-ratio: 1;
        margin: 20px auto;
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        gap: 7px;
        background: #374151;
        padding: 7px;
        border-radius: 12px;
    }

    .cell {
        background: #111827;
        border-radius: 8px;
        display: flex;
        justify-content: center;
        align-items: center;
        aspect-ratio: 1;
    }

    .ball {
        width: 65%;
        aspect-ratio: 1;
        border-radius: 50%;
        box-shadow:
            inset -5px -7px 10px rgba(0,0,0,.35),
            inset 4px 4px 8px rgba(255,255,255,.35),
            0 3px 5px rgba(0,0,0,.4);
    }

    .red { background: #ef4444; }
    .blue { background: #3b82f6; }
    .green { background: #22c55e; }
    .yellow { background: #facc15; }
    .purple { background: #a855f7; }

    .answers {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 12px;
        max-width: 500px;
        margin: 20px auto;
    }

    .answer {
        border: none;
        border-radius: 12px;
        padding: 18px 10px;
        background: #374151;
        color: white;
        font-size: 20px;
        font-weight: bold;
        cursor: pointer;
        transition: .15s;
        min-height: 65px;
    }

    .answer:hover {
        background: #4b5563;
        transform: translateY(-2px);
    }

    .answer:active {
        transform: scale(.97);
    }

    .answer.correct {
        background: #16a34a;
    }

    .answer.wrong {
        background: #dc2626;
    }

    button.start,
    button.confirm,
    button.next {
        border: none;
        border-radius: 12px;
        padding: 16px 30px;
        background: #2563eb;
        color: white;
        font-size: 18px;
        font-weight: bold;
        cursor: pointer;
    }

    button.start:hover,
    button.confirm:hover,
    button.next:hover {
        background: #3b82f6;
    }

    .result {
        font-size: 30px;
        font-weight: bold;
        margin: 20px 0;
    }

    .correct-text {
        color: #22c55e;
    }

    .wrong-text {
        color: #ef4444;
    }

    .info {
        color: #9ca3af;
        margin-top: 15px;
    }

    .hidden {
        display: none !important;
    }

    @media (max-width: 500px) {
        .screen {
            padding: 15px;
        }

        .question {
            font-size: 21px;
        }

        .answers {
            gap: 8px;
        }

        .answer {
            font-size: 16px;
            padding: 12px 5px;
        }
    }
</style>
</head>

<body>

<div class="game">

    <h1>🔴 Kugel-Rätsel</h1>
    <div class="subtitle">
        Beobachten · Denken · Schnell entscheiden
    </div>

    <div class="screen">

        <!-- START -->
        <div id="startScreen">
            <div class="phase">BEREIT?</div>

            <h2>Baue das Spielfeld nach</h2>

            <p class="info">
                Baue die angezeigte Anordnung mit deinen Kugeln
                auf deinem echten 5×5-Spielfeld nach.
            </p>

            <button class="start" onclick="startGame()">
                Spiel starten
            </button>
        </div>


        <!-- AUFBAU -->
        <div id="buildScreen" class="hidden">

            <div class="phase">1 · AUFBAU</div>

            <h2>Baue diese Anordnung nach</h2>

            <div id="buildBoard" class="board"></div>

            <p class="info">
                Wenn dein echtes Spielfeld fertig aufgebaut ist:
            </p>

            <button class="confirm" onclick="startQuestions()">
                Aufbau fertig
            </button>

        </div>


        <!-- FRAGE -->
        <div id="questionScreen" class="hidden">

            <div class="phase" id="difficulty">
                FRAGE
            </div>

            <div id="question" class="question"></div>

            <div id="timer" class="timer">15</div>

            <div id="answers" class="answers"></div>

            <div id="questionBoard" class="board"></div>

            <div class="info">
                Nur eine Antwort ist richtig!
            </div>

        </div>


        <!-- ERGEBNIS -->
        <div id="resultScreen" class="hidden">

            <div id="result" class="result"></div>

            <p id="resultInfo" class="info"></p>

            <button class="next" onclick="nextQuestion()">
                Nächste Frage
            </button>

        </div>

    </div>
</div>


<script>

/*
    ==========================================
    SPIELDATEN
    ==========================================
*/

const colors = {
    R: { name: "Rot", class: "red" },
    B: { name: "Blau", class: "blue" },
    G: { name: "Grün", class: "green" },
    Y: { name: "Gelb", class: "yellow" },
    P: { name: "Lila", class: "purple" }
};


/*
    Unser Beispiel-5x5-Spielfeld
*/

const board = [
    ["G", "R", "B", "Y", "G"],
    ["B", "Y", "R", "G", "B"],
    ["R", "G", "Y", "B", "R"],
    ["Y", "B", "G", "R", "Y"],
    ["G", "R", "Y", "G", "B"]
];


/*
    Fragen.
    
    answer = richtige Antwort
    answers = 6 mögliche Antworten
    time = Zeit in Sekunden
*/

const questions = [

    {
        difficulty: "EINFACH",
        text: "Wie viele grüne Kugeln?",
        answers: ["3", "4", "5", "6", "7", "8"],
        answer: "5",
        time: 15
    },

    {
        difficulty: "EINFACH",
        text: "Wie viele gelbe und blaue Kugeln?",
        answers: ["7", "8", "9", "10", "11", "12"],
        answer: "10",
        time: 15
    },

    {
        difficulty: "MITTEL",
        text: "Welche Kugel liegt unter Blau?",
        answers: ["Rot", "Blau", "Grün", "Gelb", "Lila", "Keine"],
        answer: "Rot",
        time: 12
    },

    {
        difficulty: "MITTEL",
        text: "Was liegt zwischen Blau und Grün von oben nach unten?",
        answers: ["Rot", "Blau", "Grün", "Gelb", "Lila", "Keine"],
        answer: "Gelb",
        time: 12
    },

    {
        difficulty: "SCHWER",
        text: "Welche Kugel liegt 2 Felder rechts von Grün?",
        answers: ["Rot", "Blau", "Grün", "Gelb", "Lila", "Keine"],
        answer: "Gelb",
        time: 10
    },

    {
        difficulty: "SCHWER",
        text: "Welche Kugel liegt diagonal unter Blau?",
        answers: ["Rot", "Blau", "Grün", "Gelb", "Lila", "Keine"],
        answer: "Gelb",
        time: 10
    },

    {
        difficulty: "SEHR SCHWER",
        text: "Welche Kugel liegt unter der Kugel rechts von Grün?",
        answers: ["Rot", "Blau", "Grün", "Gelb", "Lila", "Keine"],
        answer: "Blau",
        time: 7
    }

];


let currentQuestion = 0;
let timerInterval = null;
let timeLeft = 0;
let answered = false;


/*
    ==========================================
    HILFSFUNKTIONEN
    ==========================================
*/

function show(id) {
    document.getElementById(id).classList.remove("hidden");
}

function hide(id) {
    document.getElementById(id).classList.add("hidden");
}


/*
    Spielfeld zeichnen
*/

function drawBoard(elementId) {

    const element = document.getElementById(elementId);

    element.innerHTML = "";

    board.forEach(row => {

        row.forEach(color => {

            const cell = document.createElement("div");
            cell.className = "cell";

            const ball = document.createElement("div");
            ball.className = "ball " + colors[color].class;

            cell.appendChild(ball);
            element.appendChild(cell);

        });

    });
}


/*
    ==========================================
    START
    ==========================================
*/

function startGame() {

    hide("startScreen");
    show("buildScreen");

    drawBoard("buildBoard");
}


/*
    ==========================================
    FRAGEN STARTEN
    ==========================================
*/

function startQuestions() {

    hide("buildScreen");
    show("questionScreen");

    currentQuestion = 0;

    loadQuestion();
}


/*
    ==========================================
    FRAGE LADEN
    ==========================================
*/

function loadQuestion() {

    clearInterval(timerInterval);

    answered = false;

    const q = questions[currentQuestion];

    document.getElementById("difficulty").textContent =
        q.difficulty;

    document.getElementById("question").textContent =
        q.text;

    drawBoard("questionBoard");

    const answers = document.getElementById("answers");

    answers.innerHTML = "";

    q.answers.forEach(answer => {

        const button = document.createElement("button");

        button.className = "answer";
        button.textContent = answer;

        button.addEventListener("click", () => {

            checkAnswer(answer, button);

        });

        answers.appendChild(button);

    });

    startTimer(q.time);
}


/*
    ==========================================
    COUNTDOWN
    ==========================================
*/

function startTimer(seconds) {

    clearInterval(timerInterval);

    timeLeft = seconds;

    const timer = document.getElementById("timer");

    timer.textContent = timeLeft;

    timer.className = "timer";

    timerInterval = setInterval(() => {

        timeLeft--;

        timer.textContent = timeLeft;

        if (timeLeft <= 5) {
            timer.className = "timer danger";
        }
        else if (timeLeft <= 8) {
            timer.className = "timer warning";
        }

        if (timeLeft <= 0) {

            clearInterval(timerInterval);

            timeExpired();

        }

    }, 1000);
}


/*
    ==========================================
    ANTWORT PRÜFEN
    ==========================================
*/

function checkAnswer(answer, button) {

    if (answered) return;

    answered = true;

    clearInterval(timerInterval);

    const q = questions[currentQuestion];

    const allButtons =
        document.querySelectorAll(".answer");

    allButtons.forEach(btn => {
        btn.disabled = true;
    });

    if (answer === q.answer) {

        button.classList.add("correct");

        showResult(
            true,
            "RICHTIG!",
            "Die Antwort war " + q.answer + "."
        );

    }
    else {

        button.classList.add("wrong");

        allButtons.forEach(btn => {

            if (btn.textContent === q.answer) {
                btn.classList.add("correct");
            }

        });

        showResult(
            false,
            "FALSCH!",
            "Richtig wäre: " + q.answer
        );
    }
}


/*
    ==========================================
    ZEIT ABGELAUFEN
    ==========================================
*/

function timeExpired() {

    if (answered) return;

    answered = true;

    const q = questions[currentQuestion];

    const allButtons =
        document.querySelectorAll(".answer");

    allButtons.forEach(btn => {

        btn.disabled = true;

        if (btn.textContent === q.answer) {
            btn.classList.add("correct");
        }

    });

    showResult(
        false,
        "ZEIT ABGELAUFEN!",
        "Richtig wäre: " + q.answer
    );
}


/*
    ==========================================
    ERGEBNIS
    ==========================================
*/

function showResult(correct, title, info) {

    hide("questionScreen");

    show("resultScreen");

    const result =
        document.getElementById("result");

    result.textContent = title;

    result.className =
        "result " + (correct
            ? "correct-text"
            : "wrong-text");

    document.getElementById("resultInfo")
        .textContent = info;

}


/*
    ==========================================
    NÄCHSTE FRAGE
    ==========================================
*/

function nextQuestion() {

    currentQuestion++;

    if (currentQuestion >= questions.length) {

        currentQuestion = 0;

        document.getElementById("result").textContent =
            "RUNDE BEENDET";

        document.getElementById("result").className =
            "result correct-text";

        document.getElementById("resultInfo").textContent =
            "Alle Fragen wurden gespielt.";

        document.querySelector(".next").textContent =
            "Neue Runde";

        document.querySelector(".next").onclick =
            () => location.reload();

        return;
    }

    hide("resultScreen");
    show("questionScreen");

    loadQuestion();
}

</script>

</body>
</html>
