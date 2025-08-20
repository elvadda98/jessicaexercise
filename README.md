<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vocabulary Practice Game</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            min-height: 100vh;
            padding: 20px;
            color: #333;
        }

        .container {
            max-width: 900px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.15);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            padding: 35px;
            text-align: center;
            position: relative;
        }

        .header::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="20" cy="20" r="2" fill="rgba(255,255,255,0.1)"/><circle cx="80" cy="30" r="1.5" fill="rgba(255,255,255,0.1)"/><circle cx="40" cy="70" r="1" fill="rgba(255,255,255,0.1)"/><circle cx="90" cy="80" r="2.5" fill="rgba(255,255,255,0.1)"/></svg>');
        }

        .header h1 {
            font-size: 2.8em;
            margin-bottom: 15px;
            position: relative;
            z-index: 1;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .header p {
            font-size: 1.3em;
            opacity: 0.95;
            position: relative;
            z-index: 1;
            max-width: 600px;
            margin: 0 auto;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 15px;
            padding: 25px;
            background: #f8f9fa;
            flex-wrap: wrap;
            border-bottom: 1px solid #e9ecef;
        }

        .nav-btn {
            padding: 14px 28px;
            border: none;
            border-radius: 30px;
            cursor: pointer;
            font-size: 17px;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .nav-btn.active {
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(71,118,230,0.35);
        }

        .nav-btn:not(.active) {
            background: white;
            color: #555;
            border: 2px solid #e9ecef;
        }

        .nav-btn:hover:not(.active) {
            background: #e9ecef;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.12);
        }

        .game-section {
            display: none;
            padding: 35px;
            min-height: 450px;
        }

        .game-section.active {
            display: block;
            animation: fadeIn 0.6s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(25px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .word-bank {
            background: linear-gradient(135deg, #f5f7fa, #eef2f8);
            padding: 28px;
            border-radius: 18px;
            margin-bottom: 30px;
            border: 2px solid #4776E6;
            box-shadow: 0 6px 20px rgba(71,118,230,0.18);
        }

        .word-bank h3 {
            color: #2c3e50;
            margin-bottom: 18px;
            text-align: center;
            font-size: 1.5em;
        }

        .word-options {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            justify-content: center;
        }

        .word-option {
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            padding: 12px 22px;
            border-radius: 25px;
            font-weight: bold;
            font-size: 17px;
            box-shadow: 0 4px 12px rgba(71,118,230,0.3);
            transition: all 0.3s ease;
            cursor: default;
        }

        .word-option:hover {
            transform: translateY(-3px);
            box-shadow: 0 7px 18px rgba(71,118,230,0.4);
        }

        .question {
            background: #f8f9fa;
            padding: 28px;
            border-radius: 18px;
            margin-bottom: 25px;
            border-left: 6px solid #4776E6;
            box-shadow: 0 6px 18px rgba(0,0,0,0.06);
        }

        .question h3 {
            color: #2c3e50;
            margin-bottom: 18px;
            font-size: 1.4em;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .question h3:before {
            content: "•";
            color: #4776E6;
            font-size: 1.8em;
        }

        .question p {
            font-size: 18px;
            line-height: 1.6;
        }

        .fill-blank {
            background: #fff;
            border: 2px solid #ddd;
            border-radius: 10px;
            padding: 10px 15px;
            font-size: 17px;
            margin: 0 6px;
            min-width: 140px;
            transition: border-color 0.3s ease;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }

        .fill-blank:focus {
            outline: none;
            border-color: #4776E6;
            box-shadow: 0 0 0 3px rgba(71,118,230,0.2);
        }

        .fill-blank.correct {
            border-color: #4CAF50;
            background: #e8f5e8;
        }

        .fill-blank.incorrect {
            border-color: #f44336;
            background: #ffeaea;
        }

        .options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 18px;
            margin-top: 18px;
        }

        .option {
            padding: 18px 22px;
            border: 2px solid #ddd;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
            font-size: 16px;
        }

        .option:hover {
            border-color: #4776E6;
            transform: translateY(-3px);
            box-shadow: 0 6px 18px rgba(71,118,230,0.15);
        }

        .option.selected {
            background: #4776E6;
            color: white;
            border-color: #4776E6;
        }

        .option.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .option.incorrect {
            background: #f44336;
            color: white;
            border-color: #f44336;
        }

        .matching-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 35px;
            margin-top: 25px;
        }

        .match-column h4 {
            text-align: center;
            margin-bottom: 20px;
            color: #2c3e50;
            font-size: 1.3em;
            padding-bottom: 10px;
            border-bottom: 2px solid #e9ecef;
        }

        .match-item {
            padding: 18px;
            margin: 10px 0;
            border: 2px solid #ddd;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
            font-size: 16px;
        }

        .match-item:hover {
            border-color: #4776E6;
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(71,118,230,0.2);
        }

        .match-item.selected {
            background: #f0f4ff;
            border-color: #4776E6;
            box-shadow: 0 4px 12px rgba(71,118,230,0.2);
        }

        .match-item.matched {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
            cursor: default;
            transform: scale(0.98);
        }

        .check-btn {
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            border: none;
            padding: 18px 35px;
            border-radius: 30px;
            cursor: pointer;
            font-size: 18px;
            font-weight: bold;
            margin: 30px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 6px 20px rgba(71,118,230,0.3);
        }

        .check-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 9px 25px rgba(71,118,230,0.4);
        }

        .check-btn:active {
            transform: translateY(1px);
        }

        .feedback {
            margin-top: 20px;
            padding: 18px;
            border-radius: 12px;
            font-weight: bold;
            text-align: center;
            animation: slideIn 0.4s ease;
            font-size: 17px;
        }

        @keyframes slideIn {
            from { transform: translateX(-25px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .score {
            position: fixed;
            top: 25px;
            right: 25px;
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            padding: 18px 25px;
            border-radius: 30px;
            font-weight: bold;
            box-shadow: 0 6px 20px rgba(71,118,230,0.35);
            z-index: 1000;
            font-size: 18px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .score-number {
            font-size: 1.2em;
            background: white;
            color: #4776E6;
            width: 35px;
            height: 35px;
            border-radius: 50%;
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }

        @media (max-width: 768px) {
            .matching-container {
                grid-template-columns: 1fr;
                gap: 25px;
            }
            
            .nav-buttons {
                flex-direction: column;
                align-items: center;
            }
            
            .nav-btn {
                width: 220px;
            }
            
            .score {
                position: static;
                margin: 20px auto;
                display: inline-flex;
                width: auto;
            }
            
            .header h1 {
                font-size: 2.2em;
            }
            
            .header p {
                font-size: 1.1em;
            }
        }
    </style>
</head>
<body>
    <div class="score">Score: <span class="score-number" id="score">0</span>/20</div>
    
    <div class="container">
        <div class="header">
            <h1>📚 Vocabulary Builder</h1>
            <p>Practice and master these useful English words and phrases!</p>
        </div>

        <div class="nav-buttons">
            <button class="nav-btn active" onclick="showSection('fill-gaps')">Fill in the Gaps</button>
            <button class="nav-btn" onclick="showSection('matching')">Match Meanings</button>
            <button class="nav-btn" onclick="showSection('multiple-choice')">Multiple Choice</button>
        </div>

        <!-- Fill in the Gaps Section -->
        <div id="fill-gaps" class="game-section active">
            <div class="word-bank">
                <h3>📝 Word Bank - Choose from these words:</h3>
                <div class="word-options">
                    <span class="word-option">weak point</span>
                    <span class="word-option">share</span>
                    <span class="word-option">how did it go?</span>
                    <span class="word-option">polite</span>
                </div>
            </div>

            <div id="questions-container">
                <!-- Questions will be dynamically inserted here -->
            </div>

            <button class="check-btn" onclick="checkFillBlanks()">Check Answers</button>
        </div>

        <!-- Matching Section with Shuffled Meanings -->
        <div id="matching" class="game-section">
            <h2 style="text-align: center; margin-bottom: 25px; color: #2c3e50;">Match the words with their meanings</h2>
            <div class="matching-container">
                <div class="match-column">
                    <h4>Words</h4>
                    <div class="match-item" data-word="weak point" onclick="selectMatch(this)">weak point</div>
                    <div class="match-item" data-word="share" onclick="selectMatch(this)">share</div>
                    <div class="match-item" data-word="how did it go?" onclick="selectMatch(this)">how did it go?</div>
                    <div class="match-item" data-word="polite" onclick="selectMatch(this)">polite</div>
                </div>
                <div class="match-column">
                    <h4>Meanings</h4>
                    <div class="match-item" data-meaning="weak point" onclick="selectMatch(this)">A vulnerable area or aspect where someone/something can be attacked or criticized</div>
                    <div class="match-item" data-meaning="share" onclick="selectMatch(this)">To have or use something at the same time as someone else</div>
                    <div class="match-item" data-meaning="how did it go?" onclick="selectMatch(this)">A question asking about the outcome or success of something</div>
                    <div class="match-item" data-meaning="polite" onclick="selectMatch(this)">Showing good manners and respect for others</div>
                </div>
            </div>
            <div class="feedback" id="matching-feedback" style="display: none;"></div>
        </div>

        <!-- Multiple Choice Section -->
        <div id="multiple-choice" class="game-section">
            <div class="question">
                <h3>Question 1: What does "weak point" typically refer to?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">A strong argument</div>
                    <div class="option" onclick="selectOption(this, true)">A vulnerable area or aspect</div>
                    <div class="option" onclick="selectOption(this, false)">A physical exercise</div>
                    <div class="option" onclick="selectOption(this, false)">A type of measurement</div>
                </div>
                <div class="feedback" id="mc-feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 2: If someone asks "how did it go?", they want to know:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">The location of an event</div>
                    <div class="option" onclick="selectOption(this, false)">The cost of something</div>
                    <div class="option" onclick="selectOption(this, true)">The outcome or success of something</div>
                    <div class="option" onclick="selectOption(this, false)">The time something started</div>
                </div>
                <div class="feedback" id="mc-feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 3: A polite person is someone who:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Speaks very loudly</div>
                    <div class="option" onclick="selectOption(this, false)">Interrupts others frequently</div>
                    <div class="option" onclick="selectOption(this, true)">Shows good manners and respect</div>
                    <div class="option" onclick="selectOption(this, false)">Always arrives late</div>
                </div>
                <div class="feedback" id="mc-feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 4: When you share something, you:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Keep it to yourself</div>
                    <div class="option" onclick="selectOption(this, false)">Sell it for profit</div>
                    <div class="option" onclick="selectOption(this, true)">Use or have it together with others</div>
                    <div class="option" onclick="selectOption(this, false)">Hide it from view</div>
                </div>
                <div class="feedback" id="mc-feedback-4" style="display: none;"></div>
            </div>
        </div>
    </div>

    <script>
        let score = 0;
        let selectedWord = null;
        let selectedMeaning = null;
        let matchedPairs = [];
        
        // Define the questions for the fill-in-the-blanks section
        const questions = [
            {
                id: 1,
                text: "Math has always been my <input type='text' class='fill-blank' data-answer='weak point' placeholder='answer'> in school.",
                answer: "weak point"
            },
            {
                id: 2,
                text: "Could you please <input type='text' class='fill-blank' data-answer='share' placeholder='answer'> your notes with me? I missed the last class.",
                answer: "share"
            },
            {
                id: 3,
                text: "I heard you had your job interview yesterday. <input type='text' class='fill-blank' data-answer='how did it go?' placeholder='answer'>?",
                answer: "how did it go?"
            },
            {
                id: 4,
                text: "It's important to be <input type='text' class='fill-blank' data-answer='polite' placeholder='answer'> to everyone, even when you're having a bad day.",
                answer: "polite"
            },
            {
                id: 5,
                text: "The castle's main <input type='text' class='fill-blank' data-answer='weak point' placeholder='answer'> was its poorly defended eastern wall.",
                answer: "weak point"
            },
            {
                id: 6,
                text: "In many cultures, it's considered <input type='text' class='fill-blank' data-answer='polite' placeholder='answer'> to remove your shoes before entering someone's home.",
                answer: "polite"
            },
            {
                id: 7,
                text: "Would you like to <input type='text' class='fill-blank' data-answer='share' placeholder='answer'> a dessert with me? The portions here are huge.",
                answer: "share"
            },
            {
                id: 8,
                text: "I saw you talking to the manager earlier. <input type='text' class='fill-blank' data-answer='how did it go?' placeholder='answer'>? Are you getting a promotion?",
                answer: "how did it go?"
            },
            {
                id: 9,
                text: "Her inability to say no is her biggest <input type='text' class='fill-blank' data-answer='weak point' placeholder='answer'> - people are always taking advantage of her.",
                answer: "weak point"
            },
            {
                id: 10,
                text: "It's only <input type='text' class='fill-blank' data-answer='polite' placeholder='answer'> to wait until everyone has been served before starting to eat.",
                answer: "polite"
            },
            {
                id: 11,
                text: "Many streaming services allow you to <input type='text' class='fill-blank' data-answer='share' placeholder='answer'> your account with family members.",
                answer: "share"
            },
            {
                id: 12,
                text: "You had your driving test this morning, right? <input type='text' class='fill-blank' data-answer='how did it go?' placeholder='answer'>? Did you pass?",
                answer: "how did it go?"
            }
        ];
        
        // Function to shuffle the questions array
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }
        
        // Function to render the shuffled questions
        function renderQuestions() {
            const container = document.getElementById('questions-container');
            container.innerHTML = '';
            
            const shuffledQuestions = shuffleArray([...questions]);
            
            shuffledQuestions.forEach((question, index) => {
                const questionDiv = document.createElement('div');
                questionDiv.className = 'question';
                questionDiv.innerHTML = `
                    <h3>Question ${index + 1}:</h3>
                    <p>${question.text}</p>
                    <div class="feedback" id="feedback-${question.id}" style="display: none;"></div>
                `;
                container.appendChild(questionDiv);
            });
        }
        
        // Call the function to render questions when the page loads
        window.onload = function() {
            renderQuestions();
        };

        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.game-section').forEach(section => {
                section.classList.remove('active');
            });
            
            // Remove active class from all buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionId).classList.add('active');
            
            // Add active class to clicked button
            event.target.classList.add('active');
            
            // If it's the fill-gaps section, reshuffle the questions
            if (sectionId === 'fill-gaps') {
                renderQuestions();
            }
        }

        function checkFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            let correctCount = 0;
            
            blanks.forEach((blank) => {
                const userAnswer = blank.value.toLowerCase().trim();
                const correctAnswer = blank.dataset.answer.toLowerCase();
                
                if (userAnswer === correctAnswer) {
                    blank.classList.remove('incorrect');
                    blank.classList.add('correct');
                    correctCount++;
                } else {
                    blank.classList.remove('correct');
                    blank.classList.add('incorrect');
                }
            });
            
            // Show feedback for each question
            questions.forEach(question => {
                const feedback = document.getElementById(`feedback-${question.id}`);
                const blank = document.querySelector(`.fill-blank[data-answer="${question.answer}"]`);
                
                if (blank && blank.classList.contains('correct')) {
                    feedback.textContent = '✅ Correct!';
                    feedback.className = 'feedback correct';
                } else {
                    feedback.textContent = `❌ Incorrect. The correct answer is: "${question.answer}"`;
                    feedback.className = 'feedback incorrect';
                }
                feedback.style.display = 'block';
            });
            
            updateScore();
        }

        function selectMatch(element) {
            if (element.classList.contains('matched')) return;
            
            if (element.dataset.word) {
                // Word selected
                if (selectedWord) selectedWord.classList.remove('selected');
                selectedWord = element;
                element.classList.add('selected');
            } else {
                // Meaning selected
                if (selectedMeaning) selectedMeaning.classList.remove('selected');
                selectedMeaning = element;
                element.classList.add('selected');
            }
            
            // Check if we have both word and meaning selected
            if (selectedWord && selectedMeaning) {
                checkMatch();
            }
        }

        function checkMatch() {
            const feedback = document.getElementById('matching-feedback');
            
            if (selectedWord.dataset.word === selectedMeaning.dataset.meaning) {
                // Correct match
                selectedWord.classList.remove('selected');
                selectedWord.classList.add('matched');
                selectedMeaning.classList.remove('selected');
                selectedMeaning.classList.add('matched');
                
                matchedPairs.push(selectedWord.dataset.word);
                
                feedback.textContent = '✅ Correct match!';
                feedback.className = 'feedback correct';
                
                // Check if all matches are complete
                if (matchedPairs.length === 4) {
                    feedback.textContent = '🎉 Congratulations! You matched all terms correctly!';
                }
            } else {
                // Incorrect match
                feedback.textContent = '❌ Incorrect match. Try again.';
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
            selectedWord = null;
            selectedMeaning = null;
            
            updateScore();
        }

        function selectOption(element, isCorrect) {
            // Remove selection from all options in this question
            const options = element.parentElement.querySelectorAll('.option');
            options.forEach(opt => {
                opt.classList.remove('selected');
            });
            
            // Mark the selected option
            element.classList.add('selected');
            
            const questionNumber = element.closest('.question').querySelector('h3').textContent.match(/\d+/)[0];
            const feedback = document.getElementById(`mc-feedback-${questionNumber}`);
            
            if (isCorrect) {
                element.classList.add('correct');
                feedback.textContent = '✅ Correct!';
                feedback.className = 'feedback correct';
            } else {
                element.classList.add('incorrect');
                feedback.textContent = '❌ Incorrect. Try again.';
                feedback.className = 'feedback incorrect';
                
                // Show the correct answer
                options.forEach(opt => {
                    if (opt.onclick.toString().includes('true')) {
                        opt.classList.add('correct');
                    }
                });
            }
            
            feedback.style.display = 'block';
            updateScore();
        }

        function updateScore() {
            // Calculate score for fill-in-the-blank
            let fillScore = 0;
            document.querySelectorAll('.fill-blank.correct').forEach(blank => {
                if (!blank.classList.contains('counted')) {
                    fillScore++;
                    blank.classList.add('counted');
                }
            });
            
            // Calculate score for matching (each pair is 1 point)
            const matchScore = matchedPairs.length;
            
            // Calculate score for multiple choice (each correct is 1 point)
            let mcScore = 0;
            document.querySelectorAll('.option.correct.selected').forEach(opt => {
                mcScore++;
            });
            
            // Total score
            score = fillScore + matchScore + mcScore;
            document.getElementById('score').textContent = score;
        }
    </script>
</body>
</html>
