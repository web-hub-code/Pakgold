<html lang="ur" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Elite Portal | Muhammad Nazim</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&family=Noto+Nastaliq+Urdu:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #f8fafc;
            --card: #ffffff;
            --text: #1e293b;
            --accent: #0ea5e9;
            --border: #e2e8f0;
            --nav: rgba(255, 255, 255, 0.9);
            --hero-grad: linear-gradient(135deg, #0ea5e9, #6366f1);
        }

        [data-theme="dark"] {
            --bg: #020617;
            --card: #0f172a;
            --text: #f1f5f9;
            --accent: #38bdf8;
            --border: #1e293b;
            --nav: rgba(15, 23, 42, 0.9);
            --hero-grad: linear-gradient(135deg, #1e293b, #0f172a);
        }

        body {
            font-family: 'Poppins', 'Noto Nastaliq Urdu', sans-serif;
            background-color: var(--bg);
            color: var(--text);
            margin: 0;
            transition: 0.4s;
            overflow-x: hidden;
        }

        /* Login System */
        #login-screen {
            position: fixed;
            inset: 0;
            background: var(--bg);
            z-index: 10000;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .login-box {
            background: var(--card);
            padding: 40px;
            border-radius: 30px;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.2);
            width: 90%;
            max-width: 400px;
            text-align: center;
            border: 1px solid var(--border);
        }

        .login-box input {
            width: 100%;
            padding: 15px;
            margin: 20px 0;
            border-radius: 12px;
            border: 2px solid var(--border);
            background: var(--bg);
            color: var(--text);
            font-size: 1rem;
        }

        /* Main UI */
        #main-app { display: none; }

        header {
            position: sticky;
            top: 0;
            background: var(--nav);
            backdrop-filter: blur(15px);
            padding: 15px 5%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border);
            z-index: 1000;
        }

        .hero {
            background: var(--hero-grad);
            padding: 60px 20px;
            text-align: center;
            color: white;
            border-radius: 0 0 50px 50px;
            margin-bottom: 40px;
        }

        .container { max-width: 1200px; margin: 0 auto; padding: 20px; }

        .section-title {
            font-size: 1.8rem;
            margin: 40px 0 20px;
            color: var(--accent);
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
            gap: 20px;
        }

        .card {
            background: var(--card);
            padding: 25px;
            border-radius: 20px;
            border: 1px solid var(--border);
            cursor: pointer;
            transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .card:hover {
            transform: translateY(-5px);
            border-color: var(--accent);
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
        }

        .q-label { font-size: 0.75rem; font-weight: bold; color: var(--accent); opacity: 0.8; }
        .question { display: block; margin: 10px 0; font-size: 1.15rem; font-weight: 500; }
        
        .answer {
            max-height: 0;
            overflow: hidden;
            transition: 0.4s;
            color: #22c55e;
            background: rgba(34, 197, 94, 0.1);
            border-radius: 10px;
            padding: 0 15px;
        }

        .card.active .answer {
            max-height: 150px;
            padding: 15px;
            margin-top: 15px;
        }

        .symbol-box {
            background: var(--card);
            border: 1px solid var(--border);
            padding: 20px;
            border-radius: 15px;
            text-align: center;
        }

        .symbol-box b { font-size: 2rem; color: var(--accent); display: block; }

        .btn-theme {
            background: var(--accent);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 50px;
            cursor: pointer;
            font-weight: 600;
        }

        footer { text-align: center; padding: 60px; opacity: 0.6; font-size: 0.9rem; }
    </style>
</head>
<body data-theme="light">

<div id="login-screen">
    <div class="login-box">
        <h2>GB Police Portal</h2>
        <p>اپنا نام درج کریں</p>
        <input type="text" id="userIn" placeholder="نام یہاں لکھیں...">
        <button class="btn-theme" style="width: 100%;" onclick="startApp()">پورٹل کھولیں</button>
    </div>
</div>

<div id="main-app">
    <header>
        <div id="user-tag" style="font-weight: 700;"></div>
        <button class="btn-theme" onclick="toggleTheme()">🌓 موڈ تبدیل کریں</button>
    </header>

    <div class="hero">
        <h1 style="margin: 0; font-size: 2.5rem;">تیاری مرکز (Exam Hub)</h1>
        <p>تمام تصاویر کا مکمل ڈیٹا ایک ہی جگہ</p>
    </div>

    <div class="container">
        <h2 class="section-title">🏔️ گلگت بلتستان و عمومی معلومات</h2>
        <div class="grid" id="gk-container"></div>

        <h2 class="section-title">🌙 اسلامیات</h2>
        <div class="grid" id="isl-container"></div>

        <h2 class="section-title">📐 ریاضی کی علامات</h2>
        <div class="grid" style="grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));" id="math-container"></div>
    </div>

    <footer>
        <p>Developed by Prime Solutions | © 2026 Muhammad Nazim</p>
    </footer>
</div>

<script>
    const data = {
        gk: [
            { q: "K2 کی کل بلندی کتنی ہے؟", a: "8611 میٹر" },
            { q: "گلگت بلتستان کا کل رقبہ کتنا ہے؟", a: "72,971 مربع کلومیٹر" },
            { q: "ضلع غذر کا رقبہ کتنا ہے؟", a: "9,635 مربع کلومیٹر" },
            { q: "پاکستان کا پہلا مارشل لاء کب لگا؟", a: "اکتوبر 1958" },
            { q: "واخان کی پٹی پاکستان کو کس سے الگ کرتی ہے؟", a: "تاجکستان" },
            { q: "اردو ہندی تنازعہ کب شروع ہوا؟", a: "1867 میں" },
            { q: "پاکستان کا کل رقبہ (بشمول آزاد کشمیر)؟", a: "796,096 مربع کلومیٹر" },
            { q: "شاہراہِ ریشم کی لمبائی کتنی ہے؟", a: "1300 کلومیٹر" },
            { q: "پاکستان UN کا رکن کب بنا؟", a: "30 ستمبر 1947" },
            { q: "OIC کا ہیڈ کوارٹر کہاں ہے؟", a: "جدہ (سعودی عرب)" },
            { q: "اردو کس زبان کا لفظ ہے؟", a: "ترکی (لشکر)" },
            { q: "پاکستان میں سوئی گیس کب دریافت ہوئی؟", a: "1952 میں" },
            { q: "رقبے کے لحاظ سے سب سے بڑا اسلامی ملک؟", a: "قازقستان" },
            { q: "پاکستان کا سب سے بڑا گلیشیئر کون سا ہے؟", a: "سیاچن (76 کلومیٹر)" }
        ],
        isl: [
            { q: "پہلے حافظ قرآن صحابی کا نام؟", a: "حضرت عثمان غنی (R.A)" },
            { q: "فتح مکہ پر آپ ﷺ نے کونسی سورہ تلاوت فرمائی؟", a: "سورہ الفتح" },
            { q: "اسلام قبول کرنے والے پہلے رومی صحابی؟", a: "حضرت صہیب رومی (R.A)" },
            { q: "غزوہ خندق میں مدینہ کا محاصرہ کتنے دن رہا؟", a: "30 دن" },
            { q: "زکوٰۃ و عشر آرڈیننس کب نافذ ہوا؟", a: "20 جون 1980" },
            { q: "پہلی وحی میں قرآن کی کتنی آیات نازل ہوئیں؟", a: "پانچ (5)" },
            { q: "قرآن پاک کی سب سے طویل سورہ کون سی ہے؟", a: "سورہ البقرہ" },
            { q: "حضرت ابراہیم علیہ السلام کی جائے پیدائش؟", a: "عراق (اور)" }
        ],
        math: [
            { s: "∴", m: "لہٰذا" }, { s: "∵", m: "کیونکہ" }, { s: "≅", m: "مماثل ہے" },
            { s: "∝", m: "تناسب" }, { s: "∞", m: "لامحدود" }, { s: "∈", m: "رکن ہے" },
            { s: "≠", m: "برابر نہیں" }, { s: "⊥", m: "عمودی" }
        ]
    };

    function startApp() {
        const name = document.getElementById('userIn').value;
        if(name) {
            localStorage.setItem('gb_user', name);
            renderUI(name);
        }
    }

    function renderUI(name) {
        document.getElementById('login-screen').style.display = 'none';
        document.getElementById('main-app').style.display = 'block';
        document.getElementById('user-tag').innerText = `👤 ${name}`;
        
        const gkCont = document.getElementById('gk-container');
        const islCont = document.getElementById('isl-container');
        const mathCont = document.getElementById('math-container');

        data.gk.forEach(i => {
            gkCont.innerHTML += `<div class="card" onclick="this.classList.toggle('active')"><span class="q-label">جنرل نالج</span><span class="question">${i.q}</span><div class="answer">${i.a}</div></div>`;
        });

        data.isl.forEach(i => {
            islCont.innerHTML += `<div class="card" onclick="this.classList.toggle('active')"><span class="q-label">اسلامیات</span><span class="question">${i.q}</span><div class="answer">${i.a}</div></div>`;
        });

        data.math.forEach(i => {
            mathCont.innerHTML += `<div class="symbol-box"><b>${i.s}</b><small>${i.m}</small></div>`;
        });
    }

    function toggleTheme() {
        const body = document.body;
        const theme = body.getAttribute('data-theme') === 'light' ? 'dark' : 'light';
        body.setAttribute('data-theme', theme);
    }

    window.onload = () => {
        const saved = localStorage.getItem('gb_user');
        if(saved) renderUI(saved);
    }
</script>

</body>
</html>
