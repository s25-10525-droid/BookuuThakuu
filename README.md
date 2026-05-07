<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>애니 장르별 캐릭터 퀴즈</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700;900&display=swap');
        
        body {
            font-family: 'Noto Sans KR', sans-serif;
            transition: background 0.5s ease;
        }

        .bg-default { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); }
        .bg-shonen { background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 99%, #fecfef 100%); }
        .bg-fantasy { background: linear-gradient(135deg, #a18cd1 0%, #fbc2eb 100%); }
        .bg-sliceoflife { background: linear-gradient(135deg, #84fab0 0%, #8fd3f4 100%); }

        .glass-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 1.5rem;
            box-shadow: 0 20px 50px rgba(0,0,0,0.15);
        }

        .option-btn {
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
            border: 2px solid #f3f4f6;
        }

        .option-btn:hover:not(:disabled) {
            transform: scale(1.02);
            border-color: currentColor;
            background-color: rgba(0, 0, 0, 0.02);
        }

        .correct {
            background-color: #10b981 !important;
            color: white !important;
            border-color: #059669 !important;
        }

        .wrong {
            background-color: #ef4444 !important;
            color: white !important;
            border-color: #dc2626 !important;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .animate-fade { animation: fadeIn 0.4s ease-out forwards; }
    </style>
</head>
<body class="bg-default min-h-screen flex items-center justify-center p-4">

    <div id="game-container" class="w-full max-w-lg glass-card p-6 md:p-10">
        
        <!-- Category Selection Screen -->
        <div id="category-screen" class="animate-fade">
            <div class="text-center mb-8">
                <h1 class="text-4xl font-black text-gray-800 mb-2">애니 캐릭터 퀴즈</h1>
                <p class="text-gray-600">원하는 장르를 선택하여 도전하세요!</p>
            </div>
            
            <div class="grid gap-4">
                <button onclick="selectCategory('shonen')" class="group flex items-center p-5 bg-white rounded-2xl border-2 border-orange-100 hover:border-orange-400 transition-all text-left">
                    <div class="w-14 h-14 bg-orange-100 rounded-xl flex items-center justify-center text-orange-500 text-2xl mr-4 group-hover:bg-orange-500 group-hover:text-white transition-colors">
                        <i class="fas fa-fire"></i>
                    </div>
                    <div>
                        <h3 class="font-bold text-xl text-gray-800">소년 만화</h3>
                        <p class="text-sm text-gray-500">원나블부터 최신 주술회전까지!</p>
                    </div>
                </button>

                <button onclick="selectCategory('fantasy')" class="group flex items-center p-5 bg-white rounded-2xl border-2 border-purple-100 hover:border-purple-400 transition-all text-left">
                    <div class="w-14 h-14 bg-purple-100 rounded-xl flex items-center justify-center text-purple-500 text-2xl mr-4 group-hover:bg-purple-500 group-hover:text-white transition-colors">
                        <i class="fas fa-magic"></i>
                    </div>
                    <div>
                        <h3 class="font-bold text-xl text-gray-800">판타지 / 이세계</h3>
                        <p class="text-sm text-gray-500">마법과 모험이 가득한 세계관</p>
                    </div>
                </button>

                <button onclick="selectCategory('sliceoflife')" class="group flex items-center p-5 bg-white rounded-2xl border-2 border-green-100 hover:border-green-400 transition-all text-left">
                    <div class="w-14 h-14 bg-green-100 rounded-xl flex items-center justify-center text-green-500 text-2xl mr-4 group-hover:bg-green-500 group-hover:text-white transition-colors">
                        <i class="fas fa-heart"></i>
                    </div>
                    <div>
                        <h3 class="font-bold text-xl text-gray-800">일상 / 로맨스</h3>
                        <p class="text-sm text-gray-500">설레는 연애와 따뜻한 일상 이야기</p>
                    </div>
                </button>
            </div>
        </div>

        <!-- Quiz Screen -->
        <div id="quiz-screen" class="hidden animate-fade">
            <div class="flex justify-between items-center mb-4">
                <button onclick="showCategories()" class="text-gray-400 hover:text-gray-600 transition">
                    <i class="fas fa-arrow-left"></i> 목록으로
                </button>
                <div class="text-right">
                    <span id="progress-text" class="font-bold text-indigo-600">문제 1 / 10</span>
                </div>
            </div>
            
            <div class="w-full bg-gray-100 rounded-full h-3 mb-8 overflow-hidden">
                <div id="progress-bar" class="bg-indigo-500 h-full transition-all duration-500" style="width: 0%"></div>
            </div>

            <div class="mb-10 min-h-[100px] flex items-center justify-center">
                <h2 id="question-text" class="text-2xl font-bold text-gray-800 text-center leading-snug"></h2>
            </div>

            <div id="options-container" class="grid gap-3">
                <!-- Buttons will be injected here -->
            </div>

            <div class="mt-6 text-center">
                <p id="score-display" class="text-gray-500 font-medium">현재 점수: 0</p>
            </div>
        </div>

        <!-- Result Screen -->
        <div id="result-screen" class="hidden text-center animate-fade">
            <div id="result-icon" class="text-7xl mb-4"></div>
            <h2 class="text-3xl font-black text-gray-800 mb-2">결과 리포트</h2>
            <div class="bg-gray-50 rounded-3xl p-8 mb-8 mt-4 border border-gray-100">
                <p class="text-gray-500 text-lg mb-1">나의 최종 점수는</p>
                <div class="flex items-center justify-center">
                    <span id="final-score" class="text-6xl font-black text-indigo-600">0</span>
                    <span class="text-2xl font-bold text-gray-400 ml-2">/ 100</span>
                </div>
                <p id="result-message" class="mt-4 text-gray-700 font-bold text-lg"></p>
            </div>
            <div class="flex flex-col gap-3">
                <button onclick="restartCategory()" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-4 rounded-2xl shadow-lg transition">
                    이 장르 다시 풀기
                </button>
                <button onclick="showCategories()" class="w-full bg-white border-2 border-gray-200 text-gray-700 font-bold py-4 rounded-2xl hover:bg-gray-50 transition">
                    다른 장르 선택하기
                </button>
            </div>
        </div>
    </div>

    <script>
        const allQuizzes = {
            shonen: [
                { q: "나뭇잎 마을의 닌자로, 몸속에 구미가 봉인된 주인공은?", a: ["나루토", "사스케", "카카시", "가아라"], c: 0 },
                { q: "밀짚모자 일당의 선장으로, 고무고무 열매 능력자는?", a: ["조로", "루피", "상디", "에이스"], c: 1 },
                { q: "귀살대 소속으로 '해의 호흡'을 사용하는 소년은?", a: ["젠이츠", "기유", "탄지로", "렌고쿠"], c: 2 },
                { q: "주술고전 1학년으로 '양면 스쿠나'의 그릇이 된 소년은?", a: ["메구미", "유지", "고죠", "나나미"], c: 1 },
                { q: "드래곤볼의 주인공으로 사이어인 이름은 '카카로트'인 인물은?", a: ["베지터", "오반", "오공", "피콜로"], c: 2 },
                { q: "헌터X헌터의 주인공으로 아버지를 찾기 위해 헌터가 된 소년은?", a: ["곤", "키르아", "크라피카", "레오리오"], c: 0 },
                { q: "나의 히어로 아카데미아에서 '원 포 올'을 계승받은 소년은?", a: ["바쿠고", "토도로키", "미도리야", "올마이트"], c: 2 },
                { q: "데스노트를 이용해 세상을 바꾸려 한 천재 '키라'는?", a: ["L", "멜로", "라이토", "니아"], c: 2 },
                { q: "블랙 클로버에서 마력이 전혀 없지만 마법제가 되려는 소년은?", a: ["유노", "야미", "아스타", "노엘"], c: 2 },
                { q: "하이큐에서 작은 키에도 불구하고 미들 블로커로 활약하는 소년은?", a: ["카게야마", "히나타", "츠키시마", "노야"], c: 1 }
            ],
            fantasy: [
                { q: "소드 아트 온라인에서 '검은 검사'로 불리는 주인공은?", a: ["클라인", "키리토", "유지오", "아스나"], c: 1 },
                { q: "전생했더니 슬라임이었던 건에 대하여의 주인공 이름은?", a: ["리무루", "베니마루", "밀림", "시온"], c: 0 },
                { q: "RE:제로부터 시작하는 이세계 생활에서 사망귀환 능력을 가진 소년은?", a: ["라인하르트", "스바루", "페리스", "오토"], c: 1 },
                { q: "강철의 연금술사에서 국가 연금술사 자격을 가진 형은?", a: ["알폰스", "에드워드", "로이", "휴즈"], c: 1 },
                { q: "무직전생에서 이세계로 전생해 마법 천재가 된 주인공은?", a: ["루데우스", "에리스", "실피", "록시"], c: 0 },
                { q: "이 멋진 세계에 축복을!의 주인공으로 운 수치가 매우 높은 소년은?", a: ["카즈마", "다크니스", "미츠루기", "바닐"], c: 0 },
                { q: "오버로드에서 나자릭 지하대분묘의 통치자인 언데드 마법사는?", a: ["아인즈", "데미우르고스", "코퀴토스", "세바스"], c: 0 },
                { q: "방패 용사 성공담에서 사성 용사 중 한 명으로 소환된 주인공은?", a: ["모토야스", "이츠키", "나오후미", "렌"], c: 2 },
                { q: "일곱 개의 대죄 단장이자 분노의 죄를 상징하는 인물은?", a: ["반", "킹", "에스카노르", "멜리오다스"], c: 3 },
                { q: "노 게임 노 라이프에서 게임으로 모든 것이 결정되는 세계에 온 오빠는?", a: ["소라", "시로", "지브릴", "스테프"], c: 0 }
            ],
            sliceoflife: [
                { q: "너의 이름은.에서 미츠하와 몸이 바뀌는 도쿄 소년의 이름은?", a: ["타키", "쇼타", "츠카사", "켄토"], c: 0 },
                { q: "4월은 너의 거짓말에서 피아노를 다시 치게 되는 주인공은?", a: ["코세이", "와타리", "에미", "타케시"], c: 0 },
                { q: "빙과에서 '신경 쓰여요!'라는 말에 휘말리는 에너지 절약주의 소년은?", a: ["사토시", "호타로", "이리스", "토오베"], c: 1 },
                { q: "봇치 더 록!에서 극심한 대인기피증을 가진 기타리스트는?", a: ["니지카", "료", "히토리", "이쿠요"], c: 2 },
                { q: "토라도라!에서 험악한 인상이지만 가사 만능인 주인공은?", a: ["류지", "유사쿠", "키타무라", "노토"], c: 0 },
                { q: "목소리의 형태에서 귀가 들리지 않는 소녀 쇼코를 괴롭혔던 소년은?", a: ["유즈루", "나가츠카", "쇼야", "마시바"], c: 2 },
                { q: "카구야 님은 고백받고 싶어에서 학생회장이자 천재인 주인공은?", a: ["이시가미", "시로가네", "후지와라", "하야사카"], c: 1 },
                { q: "최애의 아이에서 아이의 아들이자 복수를 꿈꾸는 배우는?", a: ["아쿠아", "고로", "멜트", "타이키"], c: 0 },
                { q: "스파이 패밀리에서 타인의 마음을 읽을 수 있는 초능력 소녀는?", a: ["요르", "아냐", "실비아", "피오나"], c: 2 },
                { q: "바이올렛 에버가든에서 '사랑'의 의미를 배우러 여행하는 대필가는?", a: ["바이올렛", "카틀레야", "아이리스", "에리카"], c: 0 }
            ]
        };

        let currentCategory = '';
        let currentIdx = 0;
        let score = 0;
        let canAnswer = true;

        function selectCategory(cat) {
            currentCategory = cat;
            currentIdx = 0;
            score = 0;
            
            // Update Background Theme
            document.body.className = `bg-${cat} min-h-screen flex items-center justify-center p-4`;
            
            document.getElementById('category-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            loadQuestion();
        }

        function showCategories() {
            document.body.className = 'bg-default min-h-screen flex items-center justify-center p-4';
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.add('hidden');
            document.getElementById('category-screen').classList.remove('hidden');
        }

        function loadQuestion() {
            canAnswer = true;
            const qData = allQuizzes[currentCategory][currentIdx];
            const total = allQuizzes[currentCategory].length;

            document.getElementById('question-text').innerText = qData.q;
            document.getElementById('progress-text').innerText = `문제 ${currentIdx + 1} / ${total}`;
            document.getElementById('score-display').innerText = `현재 점수: ${score}`;
            
            const progress = ((currentIdx + 1) / total) * 100;
            document.getElementById('progress-bar').style.width = `${progress}%`;

            const container = document.getElementById('options-container');
            container.innerHTML = '';

            qData.a.forEach((opt, i) => {
                const btn = document.createElement('button');
                btn.className = 'option-btn w-full text-left p-4 rounded-xl bg-white text-gray-700 font-bold shadow-sm flex justify-between items-center';
                btn.innerHTML = `<span>${opt}</span><i class="fas fa-circle-notch opacity-0"></i>`;
                btn.onclick = () => checkAnswer(i, btn);
                container.appendChild(btn);
            });
        }

        function checkAnswer(idx, btn) {
            if (!canAnswer) return;
            canAnswer = false;

            const correctIdx = allQuizzes[currentCategory][currentIdx].c;
            const buttons = document.querySelectorAll('.option-btn');

            if (idx === correctIdx) {
                btn.classList.add('correct');
                score += 10;
            } else {
                btn.classList.add('wrong');
                buttons[correctIdx].classList.add('correct');
            }

            setTimeout(() => {
                currentIdx++;
                if (currentIdx < allQuizzes[currentCategory].length) {
                    loadQuestion();
                } else {
                    showResult();
                }
            }, 1200);
        }

        function showResult() {
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.remove('hidden');
            document.getElementById('final-score').innerText = score;

            const icon = document.getElementById('result-icon');
            const msg = document.getElementById('result-message');

            if (score === 100) {
                icon.innerText = "👑";
                msg.innerText = "완벽합니다! 당신은 전설적인 오타쿠군요!";
            } else if (score >= 70) {
                icon.innerText = "⭐️";
                msg.innerText = "대단해요! 애니메이션을 정말 많이 아시네요!";
            } else if (score >= 40) {
                icon.innerText = "👍";
                msg.innerText = "기초는 탄탄하시군요! 조금만 더 정진해봐요!";
            } else {
                icon.innerText = "📖";
                msg.innerText = "입덕의 길은 언제나 열려 있습니다. 다시 도전!";
            }
        }

        function restartCategory() {
            currentIdx = 0;
            score = 0;
            document.getElementById('result-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            loadQuestion();
        }
    </script>
</body>
</html>
