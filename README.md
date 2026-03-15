<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Official Mock Test | 2026</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap');
        body { font-family: 'Roboto', sans-serif; scroll-behavior: smooth; }
        .section-card { border-left: 5px solid #1e3a8a; background: white; transition: 0.3s; }
        .option-box:hover { background-color: #f1f5f9; border-color: #1e3a8a; }
        input[type="radio"] { accent-color: #1e3a8a; }
    </style>
</head>
<body class="bg-gray-100">

    <nav class="sticky top-0 z-50 bg-blue-900 text-white p-4 shadow-xl">
        <div class="max-w-5xl mx-auto flex justify-between items-center">
            <div>
                <h1 class="font-bold text-xl">GB Police Exam Portal</h1>
                <p class="text-xs text-blue-300">Constable (BPS-07) Mock Paper</p>
            </div>
            <div class="bg-blue-800 px-4 py-2 rounded-lg font-mono text-lg font-bold text-yellow-400" id="timer">
                90:00
            </div>
        </div>
    </nav>

    <div class="max-w-5xl mx-auto p-4 md:p-8">
        <form id="officialExamForm">

            <div class="section-card p-6 mb-8 rounded-r-xl shadow-md">
                <h2 class="text-blue-900 font-black text-xl mb-6 border-b pb-2 uppercase italic">Section A: English (20 Marks)</h2>
                
                <div class="space-y-6">
                    <div>
                        <p class="font-semibold mb-3">1. He is proficient ______ English.</p>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-2">
                            <label class="option-box p-3 border rounded-lg cursor-pointer flex items-center"><input type="radio" name="q1" value="B" class="mr-2"> In</label>
                            <label class="option-box p-3 border rounded-lg cursor-pointer flex items-center"><input type="radio" name="q1" value="A" class="mr-2"> At</label>
                        </div>
                    </div>
                    <div>
                        <p class="font-semibold mb-3">2. Choose the Correct Spelling:</p>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-2">
                            <label class="option-box p-3 border rounded-lg cursor-pointer flex items-center"><input type="radio" name="q2" value="A" class="mr-2"> Occasion</label>
                            <label class="option-box p-3 border rounded-lg cursor-pointer flex items-center"><input type="radio" name="q2" value="C" class="mr-2"> Occassion</label>
                        </div>
                    </div>
                </div>
            </div>

            <div class="section-card p-6 mb-8 rounded-r-xl shadow-md border-l-green-600">
                <h2 class="text-green-700 font-black text-xl mb-6 border-b pb-2 uppercase italic">Section B: GB Geography & GK (20 Marks)</h2>
                
                <div class="space-y-6">
                    <div>
                        <p class="font-semibold mb-3">3. K2 is located in which district of GB?</p>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-2">
                            <label class="option-box p-3 border rounded-lg cursor-pointer flex items-center"><input type="radio" name="q3" value="C" class="mr-2"> Shigar</label>
                            <label class="option-box p-3 border rounded-lg cursor-pointer flex items-center"><input type="radio" name="q3" value="B" class="mr-2"> Skardu</label>
                        </div>
                    </div>
                </div>
            </div>

            <div class="section-card p-6 mb-8 rounded-r-xl shadow-md border-l-red-600">
                <h2 class="text-red-700 font-black text-xl mb-6 border-b pb-2 uppercase italic">Section C: Mathematics & IQ (20 Marks)</h2>
                
                <div class="space-y-6">
                    <div>
                        <p class="font-semibold mb-3">4. Calculate 25% of 80:</p>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-2">
                            <label class="option-box p-3 border rounded-lg cursor-pointer flex items-center"><input type="radio" name="q4" value="B" class="mr-2"> 20</label>
                            <label class="option-box p-3 border rounded-lg cursor-pointer flex items-center"><input type="radio" name="q4" value="A" class="mr-2"> 15</label>
                        </div>
                    </div>
                </div>
            </div>

            <div class="fixed bottom-6 right-6">
                <button type="button" onclick="showResult()" class="bg-blue-900 text-white font-bold py-4 px-8 rounded-full shadow-2xl hover:bg-black transition-all transform hover:scale-105">
                    Finish Test & Get Marks
                </button>
            </div>

        </form>
    </div>

    <div id="resultBox" class="hidden fixed inset-0 bg-black/80 backdrop-blur-sm flex items-center justify-center p-4 z-[100]">
        <div class="bg-white rounded-3xl p-8 max-w-sm w-full text-center">
            <h3 class="text-2xl font-bold mb-4">Exam Completed!</h3>
            <div class="text-6xl font-black text-blue-600 mb-2" id="finalScore">0</div>
            <p class="text-gray-500 mb-6 font-bold uppercase tracking-widest">Total Marks Scored</p>
            <button onclick="location.reload()" class="bg-gray-900 text-white w-full py-3 rounded-xl font-bold">Restart Test</button>
        </div>
    </div>

    <script>
        // Answers Configuration
        const key = { q1: "B", q2: "A", q3: "C", q4: "B" };

        function showResult() {
            let score = 0;
            const totalQuestions = Object.keys(key).length;
            const form = new FormData(document.getElementById('officialExamForm'));
            
            for(let [question, answer] of Object.entries(key)) {
                if(form.get(question) === answer) {
                    score++;
                }
            }

            // Calculate percentage based on 100
            const final = Math.round((score / totalQuestions) * 100);
            document.getElementById('finalScore').innerText = final + "/100";
            document.getElementById('resultBox').classList.remove('hidden');
        }

        // Timer
        let seconds = 5400;
        setInterval(() => {
            if(seconds <= 0) return;
            seconds--;
            let m = Math.floor(seconds / 60);
            let s = seconds % 60;
            document.getElementById('timer').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
        }, 1000);
    </script>
</body>
</html>
