<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>SOP Interactive Dashboard</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
        :root {
            --primary: #2186eb;
            --secondary: #f7fbff;
            --accent: #0a4fa3;
            --rounded: 22px;
        }
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background: linear-gradient(135deg, #e3f0ff 0%, #ffffff 100%);
            color: #0a4fa3;
            min-height: 100vh;
        }
        header {
            background: var(--primary);
            color: #fff;
            text-align: center;
            padding: 2rem 1rem 1.2rem 1rem;
            border-bottom-left-radius: var(--rounded);
            border-bottom-right-radius: var(--rounded);
            box-shadow: 0 4px 18px rgba(33,134,235,0.13);
        }
        header h1 {
            font-size: 2.3rem;
            letter-spacing: 2px;
            margin-bottom: 0.2rem;
        }
        header p {
            font-size: 1.1rem;
            opacity: 0.92;
        }
        main {
            max-width: 900px;
            margin: 2rem auto;
            background: var(--secondary);
            border-radius: var(--rounded);
            box-shadow: 0 6px 24px rgba(33,134,235,0.11);
            padding: 2rem 2.5rem 2.5rem 2.5rem;
            min-height: 350px;
        }
        .dashboard {
            display: none;
        }
        .dashboard.active {
            display: block;
            animation: fadeIn 1s;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        /* Character avatar */
        .character-box {
            display: flex;
            align-items: center;
            gap: 2.5rem;
            margin-bottom: 2.2rem;
        }
        .character-avatar {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            background: linear-gradient(135deg, #fff 60%, #d7eaff 100%);
            border: 4px solid var(--primary);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3.5rem;
            box-shadow: 0 8px 40px rgba(33,134,235,0.18);
        }
        .char-dialogue {
            background: #fff;
            border: 2px solid var(--primary);
            border-radius: 20px;
            padding: 1.2rem 1.5rem;
            font-size: 1.13rem;
            color: var(--accent);
            max-width: 430px;
            box-shadow: 0 2px 11px rgba(33,134,235,0.08);
        }
        .dashboard-btns {
            margin-top: 2.2rem;
            display: flex;
            gap: 1.5rem;
            flex-wrap: wrap;
        }
        .dashboard-btns button {
            background: linear-gradient(90deg, var(--primary) 60%, #4fc3f7 100%);
            color: #fff;
            border: none;
            border-radius: 14px;
            padding: 0.9rem 2.1rem;
            font-size: 1.11rem;
            font-weight: 600;
            cursor: pointer;
            box-shadow: 0 2px 11px rgba(33,134,235,0.07);
            transition: background 0.18s, color 0.18s, transform 0.11s;
        }
        .dashboard-btns button:hover {
            background: linear-gradient(90deg, #fff 5%, var(--primary) 90%);
            color: var(--accent);
            transform: scale(1.04);
        }
        .quiz-box {
            background: #fff;
            border: 2px solid var(--primary);
            border-radius: 16px;
            padding: 1.5rem;
            margin-top: 0.5rem;
            box-shadow: 0 2px 14px rgba(33,134,235,0.07);
        }
        .quiz-question {
            color: var(--primary);
            font-size: 1.13rem;
            margin-bottom: 1.2rem;
            font-weight: 600;
        }
        .quiz-options button {
            display: block;
            width: 100%;
            background: linear-gradient(90deg, #fff 0%, #e3f0ff 100%);
            color: var(--accent);
            border: 2px solid #e3f0ff;
            border-radius: 10px;
            padding: 0.7rem;
            margin-bottom: 0.8rem;
            font-size: 1rem;
            font-weight: 500;
            cursor: pointer;
            transition: background 0.17s, color 0.17s, border 0.13s;
        }
        .quiz-options button:hover:not([disabled]) {
            background: linear-gradient(90deg, #4fc3f7 0%, var(--primary) 90%);
            color: #fff;
            border: 2px solid var(--primary);
        }
        .quiz-feedback {
            margin-top: 0.8rem;
            font-weight: bold;
            font-size: 1.08rem;
            min-height: 1.2rem;
        }
        .quiz-score {
            margin-top: 1.5rem;
            font-size: 1.13rem;
            color: var(--accent);
            font-weight: 600;
        }
        .nav-btn, .restart-btn {
            background: linear-gradient(90deg, #fff 10%, var(--primary) 90%);
            color: var(--accent);
            border: 2px solid var(--primary);
            border-radius: 12px;
            padding: 0.7rem 1.2rem;
            font-size: 1rem;
            font-weight: 600;
            margin-top: 1.3rem;
            cursor: pointer;
            margin-right: 1rem;
            transition: background 0.18s, color 0.18s, border 0.13s;
        }
        .nav-btn:hover, .restart-btn:hover {
            background: linear-gradient(90deg, var(--primary) 60%, #4fc3f7 100%);
            color: #fff;
        }
        /* Info Dashboard */
        .info-section {
            margin-bottom: 2rem;
        }
        .info-section h3 {
            color: var(--primary);
            margin-bottom: 0.7rem;
            font-size: 1.18rem;
        }
        .sop-card {
            background: linear-gradient(120deg, #e3f0ff 0%, #b3d8fd 100%);
            color: var(--accent);
            border-radius: 16px;
            box-shadow: 0 4px 18px rgba(33,134,235,0.10);
            padding: 1.6rem 1.2rem 1.2rem 1.2rem;
            font-size: 1.09rem;
        }
        ul.sop-list {
            margin: 0.6rem 0 0 1.5rem;
            list-style: disc;
        }
        @media (max-width: 700px) {
            main { padding: 0.8rem; }
            .character-box { flex-direction: column; gap: 1.2rem;}
            .char-dialogue { max-width: 100%; }
        }
    </style>
</head>
<body>
    <header>
        <h1>SOP Interactive Dashboard</h1>
        <p>Explore, learn, and quiz yourself about Standard Operational Procedures!</p>
    </header>
    <main>
        <!-- Dashboard 1: Character Introduction -->
        <div class="dashboard active" id="dashboard-main">
            <div class="character-box">
                <div class="character-avatar" aria-label="Character">👩‍💼</div>
                <div class="char-dialogue">
                    <b>Hello!</b> I’m Sofia, your SOP Guide.<br>
                    <br>
                    <b>Standard Operational Procedure (SOP)</b> is a set of written instructions that explain how to carry out routine operations safely, efficiently, and consistently.<br>
                    On this dashboard, you can:
                    <ul style="margin-top:0.7rem;margin-bottom:0.3rem;">
                        <li>Learn what SOPs are and why they matter</li>
                        <li>Test your knowledge with fun quizzes</li>
                    </ul>
                    <b>Choose where to start below!</b>
                </div>
            </div>
            <div class="dashboard-btns">
                <button onclick="goToDashboard('dashboard-quiz')">📝 Take the Quiz</button>
                <button onclick="goToDashboard('dashboard-info')">ℹ️ SOP Info</button>
            </div>
        </div>
        <!-- Dashboard 2: Quiz -->
        <div class="dashboard" id="dashboard-quiz">
            <div class="quiz-box" id="quiz-box"></div>
            <button class="nav-btn" onclick="goToDashboard('dashboard-main')">⬅️ Back to Main</button>
        </div>
        <!-- Dashboard 3: SOP Info -->
        <div class="dashboard" id="dashboard-info">
            <section class="sop-card">
                <div class="info-section">
                    <h3>📖 Definition: What is an SOP?</h3>
                    <p>
                        <b>Standard Operational Procedure (SOP)</b> is a detailed, written set of instructions designed to ensure that routine operations are performed consistently and correctly. SOPs are used in organizations to guarantee that all tasks are completed the right way every time.
                    </p>
                </div>
                <div class="info-section">
                    <h3>❓ Why use SOPs?</h3>
                    <ul class="sop-list">
                        <li>Ensure consistency and quality in processes</li>
                        <li>Improve safety and reduce risk of mistakes</li>
                        <li>Train new team members more easily</li>
                        <li>Comply with regulations and standards</li>
                        <li>Simplify audits and reviews</li>
                    </ul>
                </div>
                <div class="info-section">
                    <h3>✨ Key Points about SOPs:</h3>
                    <ul class="sop-list">
                        <li>Should be clear, simple, and easy to follow</li>
                        <li>Regularly reviewed and updated as needed</li>
                        <li>Help organizations maintain high standards</li>
                        <li>Can be digital or written on paper</li>
                    </ul>
                </div>
                <div class="info-section">
                    <h3>🛠️ How to Create a Good SOP?</h3>
                    <ul class="sop-list">
                        <li>Identify the process that needs an SOP</li>
                        <li>Break down the process into clear steps</li>
                        <li>Use simple language and visual aids if possible</li>
                        <li>Test the steps with team members</li>
                        <li>Update regularly with feedback and improvements</li>
                    </ul>
                </div>
            </section>
            <button class="nav-btn" onclick="goToDashboard('dashboard-main')">⬅️ Back to Main</button>
        </div>
    </main>
    <script>
        // Dashboard navigation
        function goToDashboard(dashId) {
            document.querySelectorAll('.dashboard').forEach(d => d.classList.remove('active'));
            document.getElementById(dashId).classList.add('active');
            // If quiz dashboard, load or reset the quiz
            if (dashId === 'dashboard-quiz') {
                restartQuiz(true);
            }
        }

        // Quiz logic
        const quizData = [
            {
                question: "What does SOP stand for?",
                options: [
                    "Standard Operational Procedure",
                    "Systematic Organization Plan",
                    "Simple Operation Process",
                    "Standard Option Policy"
                ],
                answer: 0
            },
            {
                question: "Which is NOT a benefit of using SOPs?",
                options: [
                    "Ensures consistency",
                    "Helps reduce mistakes",
                    "Increases confusion",
                    "Aids in training new staff"
                ],
                answer: 2
            },
            {
                question: "Who should follow SOPs in an organization?",
                options: [
                    "Only managers",
                    "Everyone involved in the process",
                    "Just new employees",
                    "External visitors"
                ],
                answer: 1
            },
            {
                question: "How often should an SOP be reviewed?",
                options: [
                    "Never",
                    "Only when there is a problem",
                    "Regularly and when improvements are needed",
                    "Once every decade"
                ],
                answer: 2
            },
            {
                question: "Which of these is a key feature of a good SOP?",
                options: [
                    "Written in complex language",
                    "Clear and easy to follow",
                    "Never updated",
                    "Hidden from staff"
                ],
                answer: 1
            }
        ];
        let currentQuestion = 0;
        let score = 0;

        function loadQuiz() {
            const quizBox = document.getElementById('quiz-box');
            if (currentQuestion < quizData.length) {
                const q = quizData[currentQuestion];
                quizBox.innerHTML = `
                    <div class="quiz-question">${currentQuestion+1}. ${q.question}</div>
                    <div class="quiz-options">
                        ${q.options.map((opt, i) => `
                            <button onclick="checkAnswer(${i})" id="option${i}">${opt}</button>
                        `).join('')}
                    </div>
                    <div class="quiz-feedback" id="feedback"></div>
                    <div class="quiz-score">Score: ${score} / ${quizData.length}</div>
                `;
            } else {
                quizBox.innerHTML = `
                    <div class="quiz-score" style="font-size:1.2rem;">
                        🎉 You scored <b>${score}</b> out of <b>${quizData.length}</b>!
                    </div>
                    <button class="restart-btn" onclick="restartQuiz(false)">Try Again</button>
                    <button class="nav-btn" onclick="goToDashboard('dashboard-info')">📘 Learn More About SOPs</button>
                `;
            }
        }

        window.checkAnswer = function(selected) {
            const q = quizData[currentQuestion];
            const feedback = document.getElementById('feedback');
            const buttons = document.querySelectorAll('.quiz-options button');
            buttons.forEach(btn => btn.disabled = true);
            if (selected === q.answer) {
                feedback.innerHTML = "<span style='color:#2186eb'>Correct! 🎉</span>";
                score++;
            } else {
                feedback.innerHTML = `<span style='color:#e96443'>Incorrect.</span> The right answer is: <b>${q.options[q.answer]}</b>`;
            }
            setTimeout(() => {
                currentQuestion++;
                loadQuiz();
            }, 1200);
        };

        // If from dashboard, reset quiz
        function restartQuiz(fromDashboard) {
            currentQuestion = 0;
            score = 0;
            loadQuiz();
            // If coming from dashboard, scroll to quiz top
            if (fromDashboard) {
                setTimeout(() => {
                    document.getElementById('dashboard-quiz').scrollIntoView({behavior: "smooth"});
                }, 50);
            }
        }

        // First load: main dashboard visible, quiz loaded when user enters quiz dashboard
    </script>
</body>
</html>
