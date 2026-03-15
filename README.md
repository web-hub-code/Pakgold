<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Official GBTS Prep Portal | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f0f4f8; overflow-x: hidden; }
        .master-card { background: white; border-radius: 30px; box-shadow: 0 25px 50px rgba(0,0,0,0.1); border-top: 10px solid #1e3a8a; }
        .q-card { border-bottom: 1px solid #edf2f7; padding: 25px 0; }
        .option-label { display: flex; align-items: center; padding: 15px; border: 2px solid #f1f5f9; border-radius: 15px; cursor: pointer; margin-top: 10px; transition: 0.2s; }
        .option-label:hover { background: #f0f7ff; border-color: #1e3a8a; }
        .correct { background: #dcfce7 !important; border-color: #22c55e !important; }
        .wrong { background: #fee2e2 !important; border-color: #ef4444 !important; }
        .sticky-nav { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(15px); }
        .btn-grad { background: linear-gradient(135deg, #1e3a8a 0%, #1e40af 100%); transition: 0.4s; }
        .btn-grad:hover { transform: translateY(-3px); box-shadow: 0 10px 20px rgba(30,58,138,0.3); }
    </style>
</head>
<body class="pb-20">

    <div id="welcomeBox" class="fixed inset-0 bg-blue-900 z-[150] flex items-center justify-center p-6 text-white text-center">
        <div class="max-w-4xl">
            <div class="mb-6 inline-block bg-white/10 p-4 rounded-full">
                <span class="text-6xl">👮</span>
            </div>
            <h1 class="text-5xl md:text-7xl font-black italic mb-4 tracking-tighter">GB POLICE PORTAL</h1>
            <p class="text-blue-300 font-bold mb-12 uppercase tracking-widest">Powered by Prime Solutions - Sweetie</p>
            
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <button onclick="startApp('practice')" class="bg-white text-blue-900 p-8 rounded-[40px] shadow-2xl hover:scale-105 transition">
                    <b class="text-xl uppercase">Practice Mode</b>
                    <p class="text-[10px] text-gray-400 mt-2">Study with instant help</p>
                </button>
                <button onclick="startApp('exam')" class="bg-yellow-500 text-white p-8 rounded-[40px] shadow-2xl hover:scale-105 transition">
                    <b class="text-xl uppercase">Final Exam</b>
                    <p class="text-[10px] text-yellow-100 mt-2">Official 100 Marks Pattern</p>
                </button>
                <button onclick="window.open('https://wa.me/03705519562')" class="bg-green-600 text-white p-8 rounded-[40px] shadow-2xl hover:scale-105 transition">
                    <b class="text-xl uppercase">Help Line</b>
                    <p class="text-[10px] text-green-100 mt-2">WhatsApp Support</p>
                </button>
            </div>
        </div>
    </div>

    <nav class="sticky-nav sticky top-0 shadow-sm p-4 z-50 flex justify-between items-center border-b border-gray-100">
        <div>
            <h2 class="font-black text-blue-900 text-2xl italic tracking-tighter">GBTS PREP</h2>
            <p class="text-[10px] text-gray-500 uppercase font-bold">Official 2026 Pattern</p>
        </div>
        <div id="timer" class="hidden bg-red-600 text-white px-6 py-2 rounded-2xl font-black font-mono animate-pulse">90:00</div>
    </nav>

    <div class="bg-yellow-100 text-yellow-800 py-2 text-xs font-bold text-center border-b border-yellow-200">
        ⚠️ Last Date to Apply: 25 March 2026 | Physical Test Date: May 2026
    </div>

    <main class="max-w-5xl mx-auto p-4 mt-8">
        
        <div class="bg-blue-900 rounded-[35px] p-8 text-white mb-10 flex flex-col md:flex-row justify-between items-center gap-6 shadow-xl">
            <div>
                <h3 class="font-black text-2xl">Physical Fitness Checker</h3>
                <p class="text-blue-300 text-sm italic">Running: 1.6 KM (1 Mile) for Male candidates</p>
            </div>
            <div class="flex gap-4">
                <input id="runTime" type="number" placeholder="Enter Minutes" class="bg-white/10 border border-white/20 p-3 rounded-xl text-white outline-none">
                <button onclick="checkPhysical()" class="bg-white text-blue-900 px-6 py-3 rounded-xl font-black uppercase text-xs hover:bg-yellow-400 transition">Check</button>
            </div>
        </div>

        <div class="master-card p-6 md:p-16">
            <div id="quizContainer">
                </div>
            <button onclick="showResult()" class="w-full btn-grad text-white py-8 rounded-[30px] font-black text-3xl shadow-2xl mt-12 uppercase">
                Submit Final Paper
            </button>
        </div>
    </main>

    <div id="resPortal" class="hidden fixed inset-0 bg-blue-950/98 backdrop-blur-3xl z-[200] flex items-center justify-center p-6 text-white text-center">
        <div class="max-w-sm w-full">
            <h3 class="text-blue-400 font-bold uppercase tracking-widest text-xs">Test Performance</h3>
            <div id="finalScore" class="text-9xl font-black my-6">0%</div>
            <p id="gradeMsg" class="font-bold text-lg mb-10"></p>
            <div class="space-y-4">
                <button onclick="location.reload()" class="w-full bg-white text-blue-900 py-4 rounded-2xl font-black uppercase shadow-xl">Retake Training</button>
                <button onclick="window.print()" class="w-full border-2 border-white/20 py-4 rounded-2xl font-bold uppercase">Download Result</button>
            </div>
        </div>
    </div>

    <script>
        const questionBank = [
            { q: "Choose the Correct Spelling:", a: "Lieutenant", b: "Leftenant", ans: "a", cat: "English" },
            { q: "He is afraid ______ the dark.", a: "From", b: "Of", ans: "b", cat: "English" },
            { q: "K2 is also known as:", a: "Everest", b: "Godwin-Austen", ans: "b", cat: "GB General Knowledge" },
            { q: "Capital of Gilgit Baltistan is:", a: "Skardu", b: "Gilgit", ans: "b", cat: "GB General Knowledge" },
            { q: "How many districts are in GB?", a: "10", b: "14", ans: "a", cat: "GB General Knowledge" },
            { q: "Solve: 15 * 4 - 10", a: "50", b: "60", ans: "a", cat: "Math" },
            { q: "25% of 1000 is:", a: "200", b: "250", ans: "b", cat: "Math" },
            { q: "First Ghazwa of Islam was?", a: "Abwa", b: "Badr", ans: "a", cat: "Islamiyat" },
            { q: "Zakat was made compulsory in?", a: "2 Hijri", b: "3 Hijri", ans: "a", cat: "Islamiyat" },
            { q: "Urdu k pehle shayar kon hain?", a: "Amir Khusro", b: "Mir Taqi Mir", ans: "a", cat: "Urdu" }
        ];

        let mode = '';
        let timeLeft = 5400;

        function speak(text) {
            let msg = new SpeechSynthesisUtterance(text);
            window.speechSynthesis.speak(msg);
        }

        function startApp(m) {
            mode = m;
            document.getElementById('welcomeBox').style.display = 'none';
            speak("Welcome! Lets start your preparation sweetie.");
            if(mode === 'exam') {
                document.getElementById('timer').classList.remove('hidden');
                startTimer();
            }
            renderQuestions();
        }

        function renderQuestions() {
            const container = document.getElementById('quizContainer');
            const shuffled = questionBank.sort(() => Math.random() - 0.5);
            container.innerHTML = '';
            
            shuffled.forEach((item, i) => {
                const html = `
                    <div class="q-card">
                        <span class="bg-blue-100 text-blue-800 text-[10px] font-black px-3 py-1 rounded-full uppercase tracking-tighter">${item.cat}</span>
                        <h4 class="text-xl font-bold text-gray-800 mt-4">${i+1}. ${item.q}</h4>
                        <div class="mt-4 space-y-2">
                            <label class="option-label" id="q${i}a_l"><input type="radio" name="q${i}" value="a" onchange="checkAns(this, '${item.ans}', 'q${i}a_l')"> ${item.a}</label>
                            <label class="option-label" id="q${i}b_l"><input type="radio" name="q${i}" value="b" onchange="checkAns(this, '${item.ans}', 'q${i}b_l')"> ${item.b}</label>
                        </div>
                    </div>
                `;
                container.insertAdjacentHTML('beforeend', html);
            });
        }

        function checkAns(input, correct, labelId) {
            if(mode === 'practice') {
                const label = document.getElementById(labelId);
                if(input.value === correct) {
                    label.classList.add('correct');
                    speak("Sahi jawab!");
                } else {
                    label.classList.add('wrong');
                    speak("Galat hai!");
                }
            }
        }

        function checkPhysical() {
            let t = document.getElementById('runTime').value;
            if(!t) return alert("Please enter time!");
            if(t <= 7) alert("Excellent! You are fit for GB Police.");
            else alert("Hard work needed! 1.6KM running must be under 7 minutes.");
        }

        function startTimer() {
            setInterval(() => {
                if(timeLeft <= 0) return;
                timeLeft--;
                let m = Math.floor(timeLeft/60); let s = timeLeft%60;
                document.getElementById('timer').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
            }, 1000);
        }

        function showResult() {
            document.getElementById('resPortal').classList.remove('hidden');
            document.getElementById('finalScore').innerText = "100%";
            document.getElementById('gradeMsg').innerText = "Excellent! You are ready for GBTS.";
            speak("Paper Submitted. Well done sweetie!");
        }
    </script>
</body>
</html>
