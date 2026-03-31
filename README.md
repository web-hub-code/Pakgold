<html lang="ur" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Elite Master Portal | Muhammad Nazim</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&family=Noto+Nastaliq+Urdu:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #f8fafc; --card: #ffffff; --text: #1e293b; --accent: #0ea5e9; --border: #e2e8f0; --nav: rgba(255, 255, 255, 0.9);
        }
        [data-theme="dark"] {
            --bg: #020617; --card: #0f172a; --text: #f1f5f9; --accent: #38bdf8; --border: #1e293b; --nav: rgba(15, 23, 42, 0.9);
        }

        body { font-family: 'Poppins', 'Noto Nastaliq Urdu', sans-serif; background-color: var(--bg); color: var(--text); margin: 0; transition: 0.4s; overflow-x: hidden; }

        /* Progress Bar */
        #top-progress { position: fixed; top: 0; left: 0; width: 0%; height: 5px; background: linear-gradient(to right, var(--accent), #6366f1); z-index: 5000; transition: 0.3s; }

        /* Header & Nav */
        header { position: sticky; top: 0; background: var(--nav); backdrop-filter: blur(15px); padding: 10px 5%; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid var(--border); z-index: 1000; }
        .search-input { background: var(--bg); border: 1px solid var(--border); padding: 8px 15px; border-radius: 50px; color: var(--text); outline: none; width: 200px; }

        /* Login System */
        #login-overlay { position: fixed; inset: 0; background: var(--bg); z-index: 10000; display: flex; align-items: center; justify-content: center; }
        .login-card { background: var(--card); padding: 40px; border-radius: 30px; box-shadow: 0 20px 50px rgba(0,0,0,0.1); text-align: center; border: 1px solid var(--border); width: 350px; }
        
        /* Stats Box */
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 15px; margin-bottom: 30px; }
        .stat-card { background: var(--accent); color: white; padding: 20px; border-radius: 20px; text-align: center; }

        /* Grid & Cards */
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 20px; }
        .card { background: var(--card); padding: 25px; border-radius: 20px; border: 1px solid var(--border); cursor: pointer; transition: 0.3s; position: relative; }
        .card:hover { transform: translateY(-5px); border-color: var(--accent); box-shadow: 0 15px 30px rgba(0,0,0,0.1); }
        .ans { max-height: 0; overflow: hidden; transition: 0.4s; color: #10b981; font-weight: bold; border-top: 0px solid transparent; }
        .card.active .ans { max-height: 200px; padding-top: 15px; margin-top: 15px; border-top: 1px dashed var(--border); }

        /* Buttons */
        .btn { padding: 10px 20px; border-radius: 50px; border: none; cursor: pointer; font-weight: 600; transition: 0.3s; }
        .btn-main { background: var(--accent); color: white; }
        .voice-icon { position: absolute; left: 15px; top: 15px; cursor: pointer; opacity: 0.4; font-size: 1.2rem; }
        .voice-icon:hover { opacity: 1; }

        /* Mock Test Overlay */
        #test-mode { position: fixed; inset: 0; background: var(--bg); z-index: 8000; display: none; flex-direction: column; align-items: center; justify-content: center; padding: 20px; }
    </style>
</head>
<body data-theme="light">

<div id="top-progress"></div>

<div id="login-overlay">
    <div class="login-card">
        <h2 style="color:var(--accent)">Prime Solutions</h2>
        <p>اپنا نام درج کریں</p>
        <input type="text" id="uName" placeholder="آپ کا نام..." style="width:100%; padding:12px; margin:20px 0; border-radius:10px; border:1px solid var(--border);">
        <button class="btn btn-main" style="width:100%" onclick="login()">پورٹل کھولیں</button>
    </div>
</div>

<div id="test-mode">
    <h2 id="timer" style="font-size: 3rem; color: var(--accent);">10:00</h2>
    <div class="card" style="width: 100%; max-width: 600px; text-align: center;">
        <p id="test-q" style="font-size: 1.5rem;"></p>
        <button class="btn btn-main" onclick="nextTestQ()">اگلا سوال</button>
    </div>
    <button class="btn" style="margin-top:20px; color:red" onclick="exitTest()">ٹیسٹ ختم کریں</button>
</div>

<header>
    <div id="welcome-msg" style="font-weight:700"></div>
    <input type="text" class="search-input" id="searchBar" placeholder="سوال ڈھونڈیں..." onkeyup="filterQs()">
    <div style="display:flex; gap:10px">
        <button class="btn btn-main" onclick="startTest()">⏱️ موک ٹیسٹ</button>
        <button class="btn" onclick="toggleTheme()" style="background:var(--card); border:1px solid var(--border)">🌓</button>
    </div>
</header>

<div class="container" id="app" style="display:none; padding: 20px 5%;">
    <div class="stats-grid">
        <div class="stat-card"><h3>تیاری</h3><h2 id="perc">0%</h2></div>
        <div class="stat-card" style="background:#6366f1"><h3>کل سوالات</h3><h2 id="totalCount">0</h2></div>
    </div>

    <h2 style="border-right: 4px solid var(--accent); padding-right: 15px;">📚 مکمل اسٹڈی مٹیریل (تمام تصاویر)</h2>
    <div class="grid" id="mainGrid"></div>
</div>

<script>
    // Full data from all your images + extras
    const masterData = [
        { q: "K2 کی کل بلندی کتنی ہے؟", a: "8611 میٹر", c: "GB" },
        { q: "ضلع غذر کا رقبہ کتنا ہے؟", a: "9,635 مربع کلومیٹر", c: "GB" },
        { q: "زکوٰۃ و عشر آرڈیننس کب نافذ ہوا؟", a: "20 جون 1980", c: "Islamic" },
        { q: "پاکستان UN کا رکن کب بنا؟", a: "30 ستمبر 1947", c: "GK" },
        { q: "فتح مکہ پر آپ ﷺ نے کونسی سورہ تلاوت فرمائی؟", a: "سورہ الفتح", c: "Islamic" },
        { q: "اردو ہندی تنازعہ کب شروع ہوا؟", a: "1867 میں", c: "History" },
        { q: "واخان کی پٹی پاکستان کو کس سے الگ کرتی ہے؟", a: "تاجکستان", c: "Geography" },
        { q: "دنیا کا سب سے بڑا اسلامی ملک (رقبہ)؟", a: "قازقستان", c: "World" },
        { q: "اردو کس زبان کا لفظ ہے؟", a: "ترکی (لشکر)", c: "General" },
        { q: "پاکستان میں سوئی گیس کب دریافت ہوئی؟", a: "1952 میں", c: "History" },
        { q: "OIC کا صدر دفتر کہاں واقع ہے؟", a: "جدہ (سعودی عرب)", c: "GK" },
        { q: "پہلے حافظ قرآن صحابی کون تھے؟", a: "حضرت عثمان غنی (R.A)", c: "Islamic" },
        { q: "اگر قطار میں آپ دونوں طرف سے 9ویں نمبر پر ہیں، تو کل کتنے لوگ ہیں؟", a: "17 لوگ", c: "IQ" },
        { q: "شاہِ ایران نے پہلی مرتبہ پاکستان کا دورہ کب کیا؟", a: "1950 میں", c: "History" },
        { q: "آبادی کے لحاظ سے پاکستان کا سب سے بڑا شہر؟", a: "کراچی", c: "GK" }
    ];

    let viewed = new Set();

    function login() {
        const name = document.getElementById('uName').value;
        if(name) {
            localStorage.setItem('nazim_user', name);
            loadApp(name);
        }
    }

    function loadApp(name) {
        document.getElementById('login-overlay').style.display = 'none';
        document.getElementById('app').style.display = 'block';
        document.getElementById('welcome-msg').innerText = `👤 خوش آمدید، ${name}`;
        renderCards();
    }

    function renderCards() {
        const grid = document.getElementById('mainGrid');
        grid.innerHTML = '';
        masterData.forEach((item, index) => {
            grid.innerHTML += `
                <div class="card" onclick="toggleCard(${index}, this)" data-q="${item.q}">
                    <span class="voice-icon" onclick="event.stopPropagation(); readAloud('${item.q}. جواب ہے ${item.a}')">🔊</span>
                    <small style="color:var(--accent)">#${item.c}</small>
                    <div style="margin-top:10px">${item.q}</div>
                    <div class="ans">${item.a}</div>
                </div>`;
        });
        document.getElementById('totalCount').innerText = masterData.length;
    }

    function toggleCard(i, el) {
        el.classList.toggle('active');
        viewed.add(i);
        let p = Math.round((viewed.size / masterData.length) * 100);
        document.getElementById('perc').innerText = p + '%';
        document.getElementById('top-progress').style.width = p + '%';
    }

    function readAloud(txt) {
        window.speechSynthesis.cancel();
        let utterance = new SpeechSynthesisUtterance(txt);
        utterance.lang = 'ur-PK';
        window.speechSynthesis.speak(utterance);
    }

    function filterQs() {
        let val = document.getElementById('searchBar').value.toLowerCase();
        document.querySelectorAll('.card').forEach(c => {
            c.style.display = c.getAttribute('data-q').toLowerCase().includes(val) ? 'block' : 'none';
        });
    }

    // Mock Test Logic
    let tInterval;
    function startTest() {
        document.getElementById('test-mode').style.display = 'flex';
        let s = 600;
        tInterval = setInterval(() => {
            s--;
            let m = Math.floor(s/60); let sec = s%60;
            document.getElementById('timer').innerText = `${m}:${sec<10?'0'+sec:sec}`;
            if(s<=0) exitTest();
        }, 1000);
        nextTestQ();
    }

    function nextTestQ() {
        const rand = masterData[Math.floor(Math.random()*masterData.length)];
        document.getElementById('test-q').innerText = rand.q;
    }

    function exitTest() {
        clearInterval(tInterval);
        document.getElementById('test-mode').style.display = 'none';
    }

    function toggleTheme() {
        const b = document.body;
        b.setAttribute('data-theme', b.getAttribute('data-theme')==='light'?'dark':'light');
    }

    window.onload = () => {
        const saved = localStorage.getItem('nazim_user');
        if(saved) loadApp(saved);
    }
</script>

</body>
</html>
