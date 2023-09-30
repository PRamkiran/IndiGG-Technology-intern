HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>Interactive Quiz</title>
</head>
<body>
    <div class="quiz-container">
        <h1>Quiz Title</h1>
        <div id="question-container">
            <p id="question">Question 1: What is 2 + 2?</p>
            <div class="options">
                <label>
                    <input type="radio" name="answer" value="A"> A) 3
                </label>
                <label>
                    <input type="radio" name="answer" value="B"> B) 4
                </label>
                <label>
                    <input type="radio" name="answer" value="C"> C) 5
                </label>
                <label>
                    <input type="radio" name="answer" value="D"> D) 6
                </label>
            </div>
        </div>
        <button id="submit-button">Submit</button>
        <div id="feedback"></div>
        <div id="score"></div>
        <button id="next-button" style="display: none;">Next</button>
    </div>
    <script src="script.js"></script>
</body>
</html>

CSS
body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
}

.quiz-container {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

h1 {
    text-align: center;
    margin-bottom: 20px;
}

#question {
    font-size: 18px;
    margin-bottom: 10px;
}

.options label {
    display: block;
    margin-bottom: 10px;
}

#submit-button, #next-button {
    display: block;
    margin-top: 20px;
    padding: 10px 20px;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

#submit-button:hover, #next-button:hover {
    background-color: #0056b3;
}

JAVA SCRIPT
const questions = [
    {
        question: "What is 2 + 2?",
        options: ["3", "4", "5", "6"],
        answer: "B"
    },
    {
        question: "What is the capital of France?",
        options: ["London", "Berlin", "Paris", "Madrid"],
        answer: "C"
    },
    // Add more questions here
];

let currentQuestionIndex = 0;
let score = 0;

const questionElement = document.getElementById("question");
const optionsElement = document.querySelector(".options");
const submitButton = document.getElementById("submit-button");
const nextButton = document.getElementById("next-button");
const feedbackElement = document.getElementById("feedback");
const scoreElement = document.getElementById("score");

function displayQuestion() {
    const currentQuestion = questions[currentQuestionIndex];
    questionElement.textContent = `Question ${currentQuestionIndex + 1}: ${currentQuestion.question}`;

    optionsElement.innerHTML = "";
    currentQuestion.options.forEach((option, index) => {
        const label = document.createElement("label");
        label.innerHTML = `<input type="radio" name="answer" value="${String.fromCharCode(65 + index)}"> ${option}`;
        optionsElement.appendChild(label);
    });

    submitButton.style.display = "block";
    nextButton.style.display = "none";
    feedbackElement.textContent = "";
}

function checkAnswer() {
    const selectedOption = document.querySelector('input[name="answer"]:checked');
    if (!selectedOption) {
        feedbackElement.textContent = "Please select an answer.";
        return;
    }

    const userAnswer = selectedOption.value;
    const currentQuestion = questions[currentQuestionIndex];
    if (userAnswer === currentQuestion.answer) {
        score++;
        feedbackElement.textContent = "Correct!";
    } else {
        feedbackElement.textContent = "Incorrect. The correct answer is " + currentQuestion.options[parseInt(currentQuestion.answer, 36) - 10];
    }

    submitButton.style.display = "none";
    nextButton.style.display = "block";
}

function nextQuestion() {
    currentQuestionIndex++;
    if (currentQuestionIndex < questions.length) {
        displayQuestion();
    } else {
        // Display final score
        questionElement.textContent = "Quiz Completed!";
        optionsElement.innerHTML = "";
        submitButton.style.display = "none";
        nextButton.style.display = "none";
        feedbackElement.textContent = "";
        scoreElement.textContent = `Your Score: ${score}/${questions.length}`;
    }
}

submitButton.addEventListener("click", checkAnswer);
nextButton.addEventListener("click", nextQuestion);

// Initial display
displayQuestion();
