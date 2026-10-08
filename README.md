<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Kugel Rätsel</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    background: #111827;
    color: white;
    font-family: Arial, sans-serif;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

.game {
    width: min(95vw, 650px);
    text-align: center;
}

h1 {
    margin-bottom: 5px;
}

.subtitle {
    color: #9ca3af;
    margin-bottom: 20px;
}

.panel {
    background: #1f2937;
    padding: 25px;
    border-radius: 20px;
    box-shadow: 0 20px 50px rgba(0,0,0,.4);
}

.stage {
    color: #60a5fa;
    font-size: 13px;
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
    font-size: 45px;
    font-weight: bold;
    margin: 10px;
    color: #22c55e;
}

.timer.warning {
    color: #f59e0b;
}

.timer.danger {
    color: #ef4444;
}

.board {
    width: min(90vw, 450px);
    aspect-ratio: 1;
    margin: 20px auto;

    display: grid;
    grid-template-columns: repeat(5, 1fr);

    gap: 6px;
    padding: 6px;

    background: #374151;
    border-radius: 12px;
}

.cell {
    background: #111827;
    border-radius: 8px;

    display: flex;
    align-items: center;
    justify-content: center;
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

.red {
    background: #ef4444;
}

.blue {
    background: #3b82f6;
}

.green {
    background: #22c55e;
}

.yellow {
    background: #facc15;
}

.purple {
    background: #a855f7;
}

.answers {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;

    max-width: 500px;
    margin: 20px auto;
}

.answer {
    min-height: 65px;

    border: none;
    border-radius: 12px;

    background: #374151;
    color: white;

    font-size: 19px;
    font-weight: bold;

    cursor: pointer;
    transition: .15s;
}

.answer:hover {
    background: #4b5563;
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
    font-size: 32px;
    font-weight: bold;
    margin: 20px;
}

.correct {
    color: #22c55e;
}

.wrong {
    color: #ef4444;
}

.info {
    color: #9ca3af;
    line-height: 1.5;
}

.hidden {
    display: none !important;
}

@media(max-width:500px) {

    .panel {
        padding: 15px;
    }

    .question {
        font-size: 21px;
    }

    .answer {
        font-size: 16px;
    }
}
</style>
</head>


<body>

<div class="game">

<h1>🔴 Kugel-Rätsel</h1>

<div class="subtitle">
    Beobachten · Denken · Entscheiden
</div>


<div class="panel">


<!-- START -->

<div id="startScreen">

    <div class="stage">
        START
    </div>

    <h2>Baue das Spielfeld nach</h2>

    <p class="info">
        Baue die angezeigte Anordnung mit deinen
        Kugeln auf deinem echten Spielfeld nach.
    </p>

    <button class="start" onclick="startGame()">
        Spiel starten
    </button>

</div>



<!-- AUFBAU -->

<div id="buildScreen" class="hidden">

    <div class="stage">
        AUFBAU
    </div>

    <h2>Baue diese Anordnung nach</h2>

    <div id="buildBoard" class="board"></div>

    <p class="info">
        Wenn dein echtes 5×5-Spielfeld fertig ist:
    </p>

    <button class="confirm" onclick="beginQuiz()">
        Aufbau fertig
    </button>

</div>



<!-- FRAGEN -->

<div id="questionScreen" class="hidden">

    <div id="difficulty" class="stage"></div>

    <div id="question" class="question"></div>

    <div id="timer" class="timer">
        15
    </div>

    <div id="answers" class="answers"></div>

    <div class="info">
        Nur ein Versuch!
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
==================================================
5×5 SPIELBRETT
==================================================
*/

const board = [

    ["G","R","B","Y","G"],

    ["B","Y","R","G","B"],

    ["R","G","Y","B","R"],

    ["Y","B","G","R","Y"],

    ["G","R","Y","G","B"]

];


/*
==================================================
FARBEN
==================================================
*/

const colors = {

    R: {
        name: "Rot",
        class: "red"
    },

    B: {
        name: "Blau",
        class: "blue"
    },

    G: {
        name: "Grün",
        class: "green"
    },

    Y: {
        name: "Gelb",
        class: "yellow"
    },

    P: {
        name: "Lila",
        class: "purple"
    }

};


/*
==================================================
ANZAHL DER FARBEN AUTOMATISCH ZÄHLEN
==================================================
*/

function countColor(color) {

    let count = 0;

    for (let row of board) {

        for (let cell of row) {

            if (cell === color) {
                count++;
            }

        }

    }

    return count;
}


/*
==================================================
ALLE FARBEN
==================================================
*/

function allColorNames() {

    return [
        "Rot",
        "Blau",
        "Grün",
        "Gelb",
        "Lila",
        "Keine"
    ];

}


/*
==================================================
FARBE -> BUCHSTABE
==================================================
*/

function colorCode(name) {

    for (let code in colors) {

        if (colors[code].name === name) {
            return code;
        }

    }

    return null;
}


/*
==================================================
BRETT ZEICHNEN
==================================================
*/

function drawBoard(elementId) {

    const boardElement =
        document.getElementById(elementId);

    boardElement.innerHTML = "";

    board.forEach(row => {

        row.forEach(color => {

            const cell =
                document.createElement("div");

            cell.className = "cell";

            const ball =
                document.createElement("div");

            ball.className =
                "ball " + colors[color].class;

            cell.appendChild(ball);

            boardElement.appendChild(cell);

        });

    });

}


/*
==================================================
FRAGEN GENERIEREN
==================================================
*/

function createQuestions() {

    const questions = [];


    /*
    ----------------------------------------------
    EINFACH
    ----------------------------------------------
    */

    questions.push({

        difficulty: "EINFACH",

        text: "Wie viele grüne Kugeln?",

        answer: countColor("G"),

        answers: createNumberAnswers(
            countColor("G")
        ),

        time: 15

    });


    questions.push({

        difficulty: "EINFACH",

        text: "Wie viele blaue Kugeln?",

        answer: countColor("B"),

        answers: createNumberAnswers(
            countColor("B")
        ),

        time: 15

    });


    questions.push({

        difficulty: "EINFACH",

        text: "Wie viele gelbe Kugeln?",

        answer: countColor("Y"),

        answers: createNumberAnswers(
            countColor("Y")
        ),

        time: 15

    });


    /*
    ----------------------------------------------
    MITTEL
    ----------------------------------------------
    */

    // Was liegt unter der Kugel bei B2?

    const belowB2 = board[2][1];

    questions.push({

        difficulty: "MITTEL",

        text: "Welche Kugel liegt unter B2?",

        answer: colors[belowB2].name,

        answers: shuffleAnswers(
            colors[belowB2].name
        ),

        time: 12

    });


    // Was liegt rechts neben A2?

    const rightA2 = board[0][2];

    questions.push({

        difficulty: "MITTEL",

        text: "Welche Kugel liegt rechts neben A2?",

        answer: colors[rightA2].name,

        answers: shuffleAnswers(
            colors[rightA2].name
        ),

        time: 12

    });


    /*
    ----------------------------------------------
    SCHWER
    ----------------------------------------------
    */

    // Zwei Felder rechts von A1

    const twoRight =
        board[0][2];

    questions.push({

        difficulty: "SCHWER",

        text: "Welche Kugel liegt 2 Felder rechts von A1?",

        answer: colors[twoRight].name,

        answers: shuffleAnswers(
            colors[twoRight].name
        ),

        time: 10

    });


    // Zwei Felder unter A3

    const twoDown =
        board[2][2];

    questions.push({

        difficulty: "SCHWER",

        text: "Welche Kugel liegt 2 Felder unter A3?",

        answer: colors[twoDown].name,

        answers: shuffleAnswers(
            colors[twoDown].name
        ),

        time: 10

    });


    /*
    ----------------------------------------------
    SEHR SCHWER
    ----------------------------------------------
    */

    /*
        A1 = Grün
        rechts davon = Rot
        darunter = Grün
    */

    const step1 = board[0][1];
    const step2 = board[1][1];

    questions.push({

        difficulty: "SEHR SCHWER",

        text: "Was liegt unter der Kugel rechts von A1?",

        answer: colors[step2].name,

        answers: shuffleAnswers(
            colors[step2].name
        ),

        time: 8

    });


    /*
    ----------------------------------------------
    ZÄHLFRAGE MIT 2 FARBEN
    ----------------------------------------------
    */

    const blue =
        countColor("B");

    const yellow =
        countColor("Y");

    const total =
        blue + yellow;

    questions.push({

        difficulty: "MITTEL",

        text: "Wie viele gelbe und blaue Kugeln?",

        answer: total,

        answers: createNumberAnswers(total),

        time: 12

    });


    return questions;

}


/*
==================================================
ZAHLENANTWORTEN ERSTELLEN
==================================================
*/

function createNumberAnswers(correct) {

    const answers = new Set();

    answers.add(correct);

    while (answers.size < 6) {

        const variation =
            Math.floor(Math.random() * 7) - 3;

        const value =
            correct + variation;

        if (value >= 0) {
            answers.add(value);
        }

    }

    return shuffle(
        [...answers]
    );

}


/*
==================================================
FARBANTWORTEN
==================================================
*/

function shuffleAnswers(correct) {

    const answers = allColorNames();

    return shuffle(
        answers
    );

}


/*
==================================================
MISCHEN
==================================================
*/

function shuffle(array) {

    return array
        .map(value => ({
            value,
            sort: Math.random()
        }))
        .sort((a,b) => a.sort - b.sort)
        .map(item => item.value);

}


/*
==================================================
SCREENS
==================================================
*/

function show(id) {

    document
        .getElementById(id)
        .classList
        .remove("hidden");

}


function hide(id) {

    document
        .getElementById(id)
        .classList
        .add("hidden");

}


/*
==================================================
SPIEL STARTEN
==================================================
*/

function startGame() {

    hide("startScreen");

    show("buildScreen");

    drawBoard("buildBoard");

}


/*
==================================================
QUIZ STARTEN
==================================================
*/

let questions = [];

let currentQuestion = 0;

let timerInterval;

let answered = false;


function beginQuiz() {

    questions =
        createQuestions();

    currentQuestion = 0;

    hide("buildScreen");

    show("questionScreen");

    loadQuestion();

}


/*
==================================================
FRAGE LADEN
==================================================
*/

function loadQuestion() {

    clearInterval(timerInterval);

    answered = false;

    const q =
        questions[currentQuestion];

    document
        .getElementById("difficulty")
        .textContent =
        q.difficulty;

    document
        .getElementById("question")
        .textContent =
        q.text;


    const answers =
        document.getElementById("answers");

    answers.innerHTML = "";


    q.answers.forEach(answer => {

        const button =
            document.createElement("button");

        button.className =
            "answer";

        button.textContent =
            answer;

        button.onclick = () => {

            checkAnswer(
                answer,
                button
            );

        };

        answers.appendChild(button);

    });


    startTimer(q.time);

}


/*
==================================================
TIMER
==================================================
*/

function startTimer(seconds) {

    const timer =
        document.getElementById("timer");

    let time =
        seconds;

    timer.textContent =
        time;

    timer.className =
        "timer";


    timerInterval =
        setInterval(() => {

            time--;

            timer.textContent =
                time;


            if (time <= 5) {

                timer.className =
                    "timer danger";

            }
            else if (time <= 8) {

                timer.className =
                    "timer warning";

            }


            if (time <= 0) {

                clearInterval(
                    timerInterval
                );

                timeExpired();

            }

        }, 1000);

}


/*
==================================================
ANTWORT PRÜFEN
==================================================
*/

function checkAnswer(answer, button) {

    if (answered) return;

    answered = true;

    clearInterval(
        timerInterval
    );

    const q =
        questions[currentQuestion];


    const buttons =
        document.querySelectorAll(
            ".answer"
        );

    buttons.forEach(btn => {

        btn.disabled = true;

    });


    if (
        String(answer) ===
        String(q.answer)
    ) {

        button.classList.add(
            "correct"
        );

        showResult(
            true,
            "RICHTIG!",
            "Die richtige Antwort ist: " +
            q.answer
        );

    }
    else {

        button.classList.add(
            "wrong"
        );

        buttons.forEach(btn => {

            if (
                String(btn.textContent) ===
                String(q.answer)
            ) {

                btn.classList.add(
                    "correct"
                );

            }

        });


        showResult(
            false,
            "FALSCH!",
            "Richtig wäre: " +
            q.answer
        );

    }

}


/*
==================================================
ZEIT ABGELAUFEN
==================================================
*/

function timeExpired() {

    if (answered) return;

    answered = true;

    const q =
        questions[currentQuestion];


    const buttons =
        document.querySelectorAll(
            ".answer"
        );


    buttons.forEach(btn => {

        btn.disabled = true;

        if (
            String(btn.textContent) ===
            String(q.answer)
        ) {

            btn.classList.add(
                "correct"
            );

        }

    });


    showResult(
        false,
        "ZEIT ABGELAUFEN!",
        "Richtig wäre: " +
        q.answer
    );

}


/*
==================================================
ERGEBNIS
==================================================
*/

function showResult(
    correct,
    title,
    info
) {

    hide("questionScreen");

    show("resultScreen");


    const result =
        document.getElementById("result");

    result.textContent =
        title;

    result.className =
        "result " +
        (correct
            ? "correct"
            : "wrong");


    document
        .getElementById("resultInfo")
        .textContent =
        info;

}


/*
==================================================
NÄCHSTE FRAGE
==================================================
*/

function nextQuestion() {

    currentQuestion++;


    if (
        currentQuestion >=
        questions.length
    ) {

        currentQuestion = 0;

    }


    hide("resultScreen");

    show("questionScreen");

    loadQuestion();

}

</script>

</body>
</html>
