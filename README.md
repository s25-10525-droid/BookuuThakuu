# BookuuThakuu
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>애니 캐릭터 퀴즈</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700&display=swap');
        body {
            font-family: 'Noto Sans KR', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }
        .glass-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 1.5rem;
            box-shadow: 0 10px 25px rgba(0,0,0,0.2);
        }
        .option-btn {
            transition: all 0.2s ease;
            border: 2px solid transparent;
        }
        .option-btn:hover {
            transform: translateY(-2px);
            border-color: #667eea;
            background-color: #f0f4ff;
        }
        .correct {
            background-color: #48bb78 !important;
            color: white !important;
            border-color: #2f855a !important;
        }
        .wrong {
            background-color: #f56565 !important;
            color: white !important;
            border-color: #c53030 !important;
        }
    </style>
</head>
<body class="flex items-center justify-center p-4">

    <div id="game-container" class="w-full max-w-md glass-card p-8">
        <!-- Start Screen -->
        <div id="start-screen" class="text-center">
            <div class="mb-6">
                <i class="fas fa-tv text-6xl text-indigo-600 mb-4"></i>
                <h1 class="text-3xl font-bold text-gray-800">애니 캐릭터 퀴즈</h1>
                <p class="text-gray-600 mt-2">당신은 얼마나 많은 캐릭터를 알고 있나요?</p>
            </div>
            <button onclick="startGame()" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-4 rounded-xl text-xl transition shadow-lg">
                게임 시작하기
            </button>
        </div>

        <!-- Quiz Screen -->
        <div id="quiz-screen" class="hidden">
            <div class="flex justify-between items-center mb-6">
                <span id="progress-text" class="text-indigo-600 font-bold">문제 1 / 10</span>
                <span id="score-text" class="text-gray-600">점수: 0</span>
            </div>
            
            <div class="w-full bg-gray-200 rounded-full h-2.5 mb-8">
                <div id="progress-bar" class="bg-indigo-600 h-2.5 rounded-full" style="width: 10%"></div>
            </div>

            <div class="mb-8">
                <p id="question-text" class="text-xl font-bold text-gray-800 leading-relaxed text-center">
                    질문이 여기에 표시됩니다.
                </p>
            </div>

            <div id="options-container" class="space-y-3">
                <!-- Options will be injected here -->
            </div>
        </div>

        <!-- Result Screen -->
        <div id="result-screen" class="hidden text-center">
            <h2 class="text-3xl font-bold text-gray-800 mb-4">퀴즈 종료!</h2>
            <div class="bg-indigo-50 rounded-2xl p-6 mb-6">
                <p class="text-gray-600 mb-2">당신의 최종 점수는</p>
                <p id="final-score" class="text-5xl font-black text-indigo-600">0</p>
                <p class="text-gray-600 mt-2">점 입니다!</p>
            </div>
            <p id="result-message" class="text-lg text-gray-700 mb-8 font-medium"></p>
            <button onclick="restartGame()" class="w-full bg-gray-800 hover:bg-black text-white font-bold py-4 rounded-xl transition">
                다시 도전하기
            </button>
        </div>
    </div>

    <script>
        const quizData = [
            {
                question: "고무고무 열매를 먹고 해적왕이 되려는 소년은?",
                options: ["나루토", "몽키 D. 루피", "이치고", "손오공"],
                correct: 1
            },
            {
                question: "거인을 구축하기 위해 조사병단에 입단한 주인공은?",
                options: ["리바이", "미카사", "에렌 예거", "아르민"],
                correct: 2
            },
            {
                question: "나뭇잎 마을의 호카게를 꿈꾸는 구미호의 인주력은?",
                options: ["우치하 사스케", "우즈마키 나루토", "카카시", "가아라"],
                correct: 1
            },
            {
                question: "데스노트를 주워 범죄자들을 심판하려 했던 천재 고등학생은?",
                options: ["엘(L)", "야가미 라이토", "니아", "멜로"],
                correct: 1
            },
            {
                question: "귀살대에 들어가 혈귀가 된 동생을 구하려는 소년은?",
                options: ["젠이츠", "이노스케", "카마도 탄지로", "기유"],
                correct: 2
            },
            {
                question: "강철의 연금술사라 불리며 동생의 몸을 찾으려는 소년은?",
                options: ["에드워드 엘릭", "알폰스 엘릭", "로이 머스탱", "스카"],
                correct: 0
            },
            {
                question: "포켓몬 마스터가 되기 위해 피카츄와 여행하는 주인공은?",
                options: ["웅이", "이슬이", "한지우", "바람이"],
                correct: 2
            },
            {
                question: "검은색 코트를 입고 '아인크라드' 게임을 클리어한 검은 검사는?",
                options: ["키리토", "클라인", "에길", "히스클리프"],
                correct: 0
            },
            {
                question: "주술회전의 주인공으로 양면 스쿠나의 손가락을 먹은 소년은?",
                options: ["후시구로 메구미", "이타도리 유지", "고죠 사토루", "게토 스구루"],
                correct: 1
            },
            {
                question: "드래곤볼을 모아 소원을 빌고, 초사이언으로 변신하는 전사는?",
                options: ["베지터", "피콜로", "손오공", "크리린"],
                correct: 2
            }
        ];

        let currentQuestion = 0;
        let score = 0;
        let canAnswer = true;

        function startGame() {
            document.getElementById('start-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            loadQuestion();
        }

        function loadQuestion() {
            canAnswer = true;
            const q = quizData[currentQuestion];
            document.getElementById('question-text').innerText = q.question;
            document.getElementById('progress-text').innerText = `문제 ${currentQuestion + 1} / ${quizData.length}`;
            document.getElementById('score-text').innerText = `점수: ${score}`;
            
            const progressPercent = ((currentQuestion + 1) / quizData.length) * 100;
            document.getElementById('progress-bar').style.width = `${progressPercent}%`;

            const container = document.getElementById('options-container');
            container.innerHTML = '';

            q.options.forEach((option, index) => {
                const button = document.createElement('button');
                button.className = 'option-btn w-full text-left p-4 rounded-xl border-2 border-gray-100 bg-white text-gray-700 font-medium flex justify-between items-center';
                button.innerHTML = `
                    <span>${option}</span>
                    <i class="fas fa-chevron-right text-gray-300"></i>
                `;
                button.onclick = () => checkAnswer(index, button);
                container.appendChild(button);
            });
        }

        function checkAnswer(index, btn) {
            if (!canAnswer) return;
            canAnswer = false;

            const correct = quizData[currentQuestion].correct;
            const buttons = document.querySelectorAll('.option-btn');

            if (index === correct) {
                btn.classList.add('correct');
                score += 10;
            } else {
                btn.classList.add('wrong');
                buttons[correct].classList.add('correct');
            }

            setTimeout(() => {
                currentQuestion++;
                if (currentQuestion < quizData.length) {
                    loadQuestion();
                } else {
                    showResult();
                }
            }, 1500);
        }

        function showResult() {
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.remove('hidden');
            document.getElementById('final-score').innerText = score;

            let message = "";
            if (score === 100) message = "완벽합니다! 당신은 진정한 애니 박사군요! 🏆";
            else if (score >= 70) message = "대단해요! 애니메이션을 아주 좋아하시는군요! ✨";
            else if (score >= 40) message = "나쁘지 않아요! 조금만 더 정진해볼까요? 😊";
            else message = "아쉬워요! 다시 한번 도전해보세요! 💪";

            document.getElementById('result-message').innerText = message;
        }

        function restartGame() {
            currentQuestion = 0;
            score = 0;
            document.getElementById('result-screen').classList.add('hidden');
            document.getElementById('start-screen').classList.remove('hidden');
        }
    </script>
</body>
</html>
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>애니 장르별 캐릭터 퀴즈</title>
