# SCT_WD_3
Quiz game application
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Interactive Quiz Game</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background: linear-gradient(135deg, #667eea, #764ba2);
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        margin: 0;
        color: #333;
    }

    .quiz-container {
        background: white;
        width: 600px;
        max-width: 95%;
        padding: 25px;
        border-radius: 12px;
        box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    }

    h2 {
        margin-top: 0;
    }

    .options label {
        display: block;
        margin: 8px 0;
        padding: 8px;
        border-radius: 6px;
        cursor: pointer;
        transition: 0.2s;
    }

    .options label:hover {
        background: #f0f0f0;
    }

    button {
        margin-top: 15px;
        padding: 10px 15px;
        border: none;
        border-radius: 6px;
        background: #667eea;
        color: white;
        cursor: pointer;
        font-size: 16px;
    }

    button:hover {
        background: #5a67d8;
    }

    .result {
        font-size: 20px;
        font-weight: bold;
    }

    .hidden {
        display: none;
    }

    .progress {
        font-size: 14px;
        margin-bottom: 10px;
        color: #555;
    }
</style>
</head>
<body>

<div class="quiz-container">
    <div class="progress" id="progress"></div>
    <h2 id="question"></h2>
    <div class="options" id="options"></div>
    <button id="nextBtn">Next</button>
    <div class="result hidden" id="result"></div>
</div>

<script>
const quizData = [
    {
        type: "single",
        question: "Which language runs in the browser?",
        options: ["Python", "Java", "C++", "JavaScript"],
        answer: "JavaScript"
    },
    {
        type: "multi",
        question: "Select all frontend technologies:",
        options: ["HTML", "CSS", "Python", "JavaScript"],
        answer: ["HTML", "CSS", "JavaScript"]
    },
    {
        type: "fill",
        question: "Fill in the blank: The capital of France is ____.",
        answer: "Paris"
    }
];

let currentQuestion = 0;
let score = 0;

const questionEl = document.getElementById("question");
const optionsEl = document.getElementById("options");
const nextBtn = document.getElementById("nextBtn");
const resultEl = document.getElementById("result");
const progressEl = document.getElementById("progress");

function loadQuestion() {
    const q = quizData[currentQuestion];
    questionEl.textContent = q.question;
    optionsEl.innerHTML = "";
    progressEl.textContent = `Question ${currentQuestion + 1} of ${quizData.length}`;

    if (q.type === "single") {
        q.options.forEach(option => {
            const label = document.createElement("label");
            label.innerHTML = `
                <input type="radio" name="answer" value="${option}">
                ${option}
            `;
            optionsEl.appendChild(label);
        });
    }

    if (q.type === "multi") {
        q.options.forEach(option => {
            const label = document.createElement("label");
            label.innerHTML = `
                <input type="checkbox" name="answer" value="${option}">
                ${option}
            `;
            optionsEl.appendChild(label);
        });
    }

    if (q.type === "fill") {
        const input = document.createElement("input");
        input.type = "text";
        input.id = "fillAnswer";
        input.placeholder = "Type your answer here...";
        input.style.width = "100%";
        input.style.padding = "8px";
        optionsEl.appendChild(input);
    }
}

function checkAnswer() {
    const q = quizData[currentQuestion];
    let userAnswer;

    if (q.type === "single") {
        const selected = document.querySelector("input[name='answer']:checked");
        if (!selected) return false;
        userAnswer = selected.value;
        if (userAnswer === q.answer) score++;
    }

    if (q.type === "multi") {
        const selected = Array.from(document.querySelectorAll("input[name='answer']:checked"))
                              .map(input => input.value);
        if (selected.length === 0) return false;
        if (JSON.stringify(selected.sort()) === JSON.stringify(q.answer.sort())) {
            score++;
        }
    }

    if (q.type === "fill") {
        const input = document.getElementById("fillAnswer");
        if (!input.value) return false;
        userAnswer = input.value.trim();
        if (userAnswer.toLowerCase() === q.answer.toLowerCase()) {
            score++;
        }
    }

    return true;
}

nextBtn.addEventListener("click", () => {
    if (!checkAnswer()) {
        alert("Please answer the question before proceeding!");
        return;
    }

    currentQuestion++;

    if (currentQuestion < quizData.length) {
        loadQuestion();
    } else {
        showResult();
    }
});

function showResult() {
    questionEl.classList.add("hidden");
    optionsEl.classList.add("hidden");
    nextBtn.classList.add("hidden");
    progressEl.classList.add("hidden");

    resultEl.classList.remove("hidden");
    resultEl.textContent = `🎉 You scored ${score} out of ${quizData.length}!`;
}

loadQuestion();
</script>

</body>
</html>
