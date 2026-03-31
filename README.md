<html lang="ur" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Exam Hub | Nazim</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&family=Noto+Nastaliq+Urdu:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            /* تھوڑا روشن ڈارک شیڈز */
            --bg-main: #1e293b; 
            --glass-bg: rgba(255, 255, 255, 0.05);
            --glass-border: rgba(255, 255, 255, 0.1);
            --accent-blue: #38bdf8;
            --accent-green: #4ade80;
            --text-main: #f1f5f9;
            --text-dim: #94a3b8;
        }

        body {
            font-family: 'Poppins', 'Noto Nastaliq Urdu', sans-serif;
            background: var(--bg-main);
            /* ہلکا سا گریڈینٹ تاکہ بالکل کالا نہ لگے */
            background-image: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            color: var(--text-main);
            margin: 0;
            line-height: 1.6;
        }

        header {
            padding: 60px 20px;
            text-align: center;
            background: rgba(15, 23, 42, 0.8);
            backdrop-filter: blur(10px);
            border-bottom: 1px solid var(--glass-border);
        }

        header h1 {
            font-size: 2.8rem;
            margin: 0;
            color: var(--accent-blue);
            text-shadow: 0 0 20px rgba(56, 189, 248, 0.3);
        }

        .container {
            max-width: 1100px;
            margin: -30px auto 50px;
            padding: 0 20px;
        }

        /* Category Sections */
        .section-box {
            background: var(--glass-bg);
            border: 1px solid var(--glass-border);
            border-radius: 24px;
            padding: 30px;
            margin-bottom: 40px;
            backdrop-filter: blur(12px);
        }

        h2.cat-title {
            color: var(--accent-blue);
            font-size: 1.5rem;
            margin-bottom: 25px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        /* Question Cards - Cleaner Look */
        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 15px;
        }

        .card {
            background: rgba(255, 255, 255, 0.03);
            border: 1px solid var(--glass-border);
            padding: 20px;
            border-radius: 16px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .card:hover {
            background: rgba(255, 255, 255, 0.07);
            border-color: var(--accent-blue);
            transform: translateY(-3px);
        }

        .q-text {
            font-weight: 500;
            color: var(--text-main);
            display: block;
        }

        .a-text {
            display: none;
            margin-top: 12px;
            color: var(--accent-green);
            font-weight: bold;
            font-size: 1.1rem;
            border-top: 1px solid rgba(255,255,255,0.1);
            padding-top: 10px;
        }

        .card.open .a-text {
            display: block;
        }

        /* Math Table */
        .symbol-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
            gap: 10px;
        }

        .symbol-item {
            background: rgba(56, 189, 248, 0.1);
            padding: 15px;
            border-radius: 12px;
            text-align: center;
            border: 1px solid rgba(56, 189, 248, 0.2);
        }

        .sym { font-size: 1.5rem; color: var(--accent-blue); display: block; }
        .lab { font-size: 0.8rem; color: var(--text-dim); }

        footer {
            text-align: center;
            padding: 40px;
            color: var(--text-dim);
            font-size: 0.9rem;
        }

    </style>
</head>
<body>

<header>
    <h1>GB Police Prep Portal</h1>
    <p style="color: var(--text-dim)">باصلاحیت امیدوار: محمد ناظم</p>
</header>

<div class="container">

    <div class="section-box">
        <h2 class="cat-title">🏔️ گلگت بلتستان اسپیشل</h2>
        <div class="grid">
            <div class="card" onclick="this.classList.toggle('open')">
                <span class="q-text">K2 کی کل بلندی کتنی ہے؟</span>
                <div class="a-text">8,611 میٹر</div>
            </div>
            <div class="card" onclick="this.classList.toggle('open')">
                <span class="q-text">ضلع غذر کا رقبہ کتنا ہے؟</span>
                <div class="a-text">9,635 مربع کلومیٹر</div>
            </div>
            <div class="card" onclick="this.classList.toggle('open')">
                <span class="q-text">GB کی پہلی خاتون گورنر؟</span>
                <div class="a-text">بیگم شمع خالد</div>
            </div>
        </div>
    </div>

    <div class="section-box">
        <h2 class="cat-title">📐 ریاضی کی علامات</h2>
        <div class="symbol-grid">
            <div class="symbol-item"><span class="sym">∴</span><span class="lab">لہٰذا</span></div>
            <div class="symbol-item"><span class="sym">∵</span><span class="lab">کیونکہ</span></div>
            <div class="symbol-item"><span class="sym">≅</span><span class="lab">مماثل ہے</span></div>
            <div class="symbol-item"><span class="sym">∝</span><span class="lab">تناسب</span></div>
            <div class="symbol-item"><span class="sym">∈</span><span class="lab">رکن ہے</span></div>
            <div class="symbol-item"><span class="sym">∞</span><span class="lab">لامحدود</span></div>
        </div>
    </div>

    <div class="section-box">
        <h2 class="cat-title">📖 اسلامیات و مطالعہ پاکستان</h2>
        <div class="grid">
            <div class="card" onclick="this.classList.toggle('open')">
                <span class="q-text">اردو کس زبان کا لفظ ہے؟</span>
                <div class="a-text">ترکی (لشکر)</div>
            </div>
            <div class="card" onclick="this.classList.toggle('open')">
                <span class="q-text">فتحِ مکہ پر آپ ﷺ نے کونسی سورہ تلاوت فرمائی؟</span>
                <div class="a-text">سورہ الفتح</div>
            </div>
            <div class="card" onclick="this.classList.toggle('open')">
                <span class="q-text">پاکستان کا کل رقبہ کتنا ہے؟</span>
                <div class="a-text">796,096 مربع کلومیٹر</div>
            </div>
        </div>
    </div>

</div>

<footer>
    <p>Prime Solutions © 2026 | Created for Muhammad Nazim</p>
</footer>

</body>
</html>
