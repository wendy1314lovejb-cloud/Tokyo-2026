[Tokyo2026.html.html](https://github.com/user-attachments/files/27811941/Tokyo2026.html.html)
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>2026 東京夢幻之旅 ✨</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        /* 辰宇落雁體載入 */
        @font-face {
            font-family: 'Chenyuluoyan';
            src: url('https://cdn.jsdelivr.net/gh/chenyu-shuo/Chenyuluoyan-Thin@main/Chenyuluoyan-Thin.ttf') format('truetype');
            font-display: swap;
        }

        :root {
            --primary: #9D84B7;
            --secondary: #6B4E8D;
            --bg: #F8F6FF;
            --accent-pink: #FFD1DC;
            --safe-bottom: env(safe-area-inset-bottom);
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

        body {
            margin: 0;
            font-family: 'Chenyuluoyan', "PingFang TC", sans-serif;
            background-color: var(--bg);
            color: #4A4A4A;
            padding-bottom: calc(100px + var(--safe-bottom));
            letter-spacing: 0.05em;
            background-image: radial-gradient(var(--primary) 0.5px, transparent 0.5px);
            background-size: 25px 25px;
        }

        /* UI 元件 */
        .header {
            background: linear-gradient(135deg, #A29BFE, var(--primary));
            color: white; padding: calc(30px + env(safe-area-inset-top)) 20px 20px;
            text-align: center; border-bottom-left-radius: 30px; border-bottom-right-radius: 30px;
            box-shadow: 0 4px 20px rgba(157, 132, 183, 0.2);
            position: sticky; top: 0; z-index: 100;
        }

        .day-scroller { display: flex; overflow-x: auto; gap: 10px; padding: 15px; scrollbar-width: none; }
        .day-scroller::-webkit-scrollbar { display: none; }
        .day-btn { flex: 0 0 130px; height: 60px; background: white; border-radius: 20px; display: flex; align-items: center; justify-content: center; border: 1px solid #EEE; transition: 0.3s; font-size: 0.95rem; color: var(--primary); font-weight: bold; }
        .day-btn.active { background: var(--primary); color: white; transform: translateY(-2px); box-shadow: 0 5px 12px rgba(157, 132, 183, 0.3); }

        .container { padding: 0 15px; max-width: 500px; margin: 0 auto; }
        .card { background: rgba(255, 255, 255, 0.95); border-radius: 24px; padding: 18px; margin-bottom: 15px; border: 1px solid rgba(255, 255, 255, 0.6); box-shadow: 0 4px 12px rgba(157, 132, 183, 0.05); }
        
        /* 校正圖示文字間距：加入 gap: 8px */
        .card-title { font-size: 1.2rem; font-weight: bold; color: var(--secondary); display: flex; align-items: center; gap: 8px; margin-bottom: 10px; }

        /* 分帳樣式 */
        .total-banner { background: var(--secondary); color: white; border-radius: 20px; padding: 20px; text-align: center; margin-bottom: 20px; }
        .settlement-card { background: white; border: 2px solid var(--accent-pink); border-radius: 20px; padding: 18px; margin-bottom: 20px; }
        .payer-selector, .split-selector { display: flex; gap: 5px; margin-bottom: 10px; }
        .selector-btn { flex: 1; padding: 8px; border-radius: 10px; border: 1px solid #EEE; background: white; font-family: 'Chenyuluoyan'; font-size: 0.9rem; white-space: nowrap; outline: none; }
        .selector-btn.active { background: var(--primary); color: white; border-color: var(--primary); }
        .split-input-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; padding-top: 5px; border-top: 1px dashed #F3F0FF; }
        .split-input-row input { width: 80px; padding: 5px; border-radius: 8px; border: 1px solid #EEE; text-align: right; font-family: 'Chenyuluoyan'; }
        
        .expense-item { border-bottom: 1px dashed #DDD; padding: 10px 0; }
        .del-btn { color: #FFB7B2; border: none; background: none; font-size: 1rem; }

        /* 彈窗與美照 */
        .modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.4); backdrop-filter: blur(4px); display: none; justify-content: center; align-items: flex-end; z-index: 2000; }
        .modal-content { background: white; width: 100%; max-width: 500px; border-radius: 30px 30px 0 0; padding: 30px 25px calc(30px + var(--safe-bottom)); transform: translateY(100%); transition: 0.3s ease; max-height: 85vh; overflow-y: auto; position: relative; }
        .modal-overlay.active { display: flex; }
        .modal-overlay.active .modal-content { transform: translateY(0); }
        .modal-close { position: absolute; top: 20px; right: 20px; font-size: 1.5rem; color: #CCC; cursor: pointer; }
        .spot-img { width: 100%; height: 180px; object-fit: cover; border-radius: 18px; margin-bottom: 15px; }
        
        .nav-link-btn { background: #E0D7EE; color: #6B4E8D; text-decoration: none; padding: 12px; border-radius: 15px; font-size: 0.9rem; display: flex; align-items: center; justify-content: center; font-weight: bold; margin-top: 15px; }

        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(15px); display: flex; justify-content: space-around; padding: 12px 0 calc(12px + var(--safe-bottom)); box-shadow: 0 -5px 20px rgba(0,0,0,0.05); z-index: 1000; }
        .nav-item { background: none; border: none; color: #D1D1D1; display: flex; flex-direction: column; align-items: center; flex: 1; outline: none; }
        .nav-item.active { color: var(--secondary); }
        
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .tag-must { font-size: 0.75rem; background: var(--accent-pink); color: #D14D72; padding: 2px 8px; border-radius: 8px; font-weight: bold; }
    </style>
</head>
<body>

<div class="app-container">
    <header class="header"><h1>東京夢幻之旅 ✨</h1></header>

    <div id="day-selector-container" class="day-scroller">
        <div class="day-btn active" onclick="switchDay(0, this)">Day1 (10/26)</div>
        <div class="day-btn" onclick="switchDay(1, this)">Day2 (10/27)</div>
        <div class="day-btn" onclick="switchDay(2, this)">Day3 (10/28)</div>
        <div class="day-btn" onclick="switchDay(3, this)">Day4 (10/29)</div>
        <div class="day-btn" onclick="switchDay(4, this)">Day5 (10/30)</div>
        <div class="day-btn" onclick="switchDay(5, this)">Day6 (10/31)</div>
    </div>

    <main class="container">
        <div id="tab-itinerary" class="tab-content active"><div id="itinerary-list"></div></div>

        <div id="tab-info" class="tab-content">
            <div class="card">
                <div class="card-title"><i class="fas fa-plane"></i> ✈️ 航班資訊 (Flight Info)</div>
                <p style="font-size:0.95rem; line-height:1.8;">
                    <b>去程 (10/26 周一)：</b><br>IT280 ｜ 高雄 (KHH) 08:00 ➔ 成田 (NRT) 12:10<br>
                    <b>回程 (10/31 周六)：</b><br>IT281 ｜ 成田 (NRT) 11:25 ➔ 高雄 (KHH) 15:05
                </p>
            </div>
            <div class="card">
                <div class="card-title"><i class="fas fa-hotel"></i> 🏨 住宿資訊</div>
                <p><b>D・レガーロ</b></p>
                <p style="font-size:0.85rem; line-height:1.5;">〒272-0138 千葉県市川市南行德２丁目２１</p>
                <a href="https://maps.app.goo.gl/5eHii3QuEX1P41EQ7" target="_blank" class="nav-link-btn">打開飯店精準座標</a>
            </div>
            <div class="card" style="background:#FFF5F7; border: 1px dashed #FFB7B2;">
                <div class="card-title" style="color:#D14D72;"><i class="fas fa-phone-alt"></i> 🆘 緊急聯絡</div>
                <p style="font-size:0.95rem; line-height:1.8;">
                    警察直撥：<b>110</b><br>
                    急救直撥：<b>119</b><br>
                    駐日代表處：<a href="tel:+81332807811" style="color:var(--secondary);">+81-3-3280-7811</a>
                </p>
            </div>
        </div>

        <div id="tab-budget" class="tab-content">
            <div class="total-banner">
                <p style="font-size:0.8rem; opacity:0.8;">整趟旅程累計支出 (約台幣)</p>
                <h2 style="font-size:2rem; margin:5px 0;">NT$ <span id="total-twd">0</span></h2>
                <p id="current-rate-display" style="font-size:0.7rem; opacity:0.7;">正在連網抓取當日匯率...</p>
            </div>

            <div class="settlement-card">
                <div class="card-title" style="justify-content:center; font-size:1rem;"><i class="fas fa-balance-scale"></i> Settle Up (結算日幣 ¥)</div>
                <div id="settlement-result" style="margin-top:10px; line-height:1.6; text-align:center;"></div>
            </div>

            <div class="card">
                <div class="card-title">✏️ Add Expense</div>
                <p style="font-size:0.75rem; color:var(--primary); margin:10px 0 5px;">Paid by...</p>
                <div class="payer-selector">
                    <button class="selector-btn active payer-btn" onclick="setPayer('蒂', this)">蒂</button>
                    <button class="selector-btn payer-btn" onclick="setPayer('丞', this)">丞</button>
                    <button class="selector-btn payer-btn" onclick="setPayer('頻', this)">頻</button>
                </div>
                <input type="text" id="itemName" placeholder="品名" style="width:100%; padding:12px; margin-bottom:10px; border-radius:12px; border:1px solid #EEE; font-family:'Chenyuluoyan'; outline:none;">
                <input type="number" id="itemPrice" placeholder="日幣金額 ¥" style="width:100%; padding:12px; margin-bottom:15px; border-radius:12px; border:1px solid #EEE; font-family:'Chenyuluoyan'; outline:none;">
                
                <p style="font-size:0.75rem; color:var(--primary); margin:10px 0 5px;">Split by... (日幣計算)</p>
                <div class="split-selector">
                    <button class="selector-btn active split-method-btn" onclick="setSplitMethod('equal', this)">平分</button>
                    <button class="selector-btn split-method-btn" onclick="setSplitMethod('percent', this)">百分比%</button>
                    <button class="selector-btn split-method-btn" onclick="setSplitMethod('exact', this)">金額¥</button>
                </div>
                <div id="split-inputs-container"></div>
                <button onclick="addExpense()" style="width:100%; padding:15px; background:var(--primary); color:white; border:none; border-radius:15px; font-family:'Chenyuluoyan'; font-weight:bold; font-size:1.1rem; margin-top:10px;">存入手帳 💜</button>
            </div>
            <div class="card"><div id="expense-list"></div></div>
        </div>
    </main>

    <div id="detail-modal" class="modal-overlay" onclick="closeModal(event)">
        <div class="modal-content" onclick="event.stopPropagation()">
            <span class="modal-close" onclick="closeModal(null)">×</span>
            <div id="modal-body"></div>
        </div>
    </div>

    <nav class="bottom-nav">
        <button class="nav-item active" onclick="switchTab('tab-itinerary', this)"><i class="fas fa-calendar-alt"></i><span>行程</span></button>
        <button class="nav-item" onclick="switchTab('tab-info', this)"><i class="fas fa-info-circle"></i><span>資訊</span></button>
        <button class="nav-item" onclick="switchTab('tab-budget', this)"><i class="fas fa-coins"></i><span>記帳</span></button>
    </nav>
</div>

<script>
    let exchangeRate = 0.215; 
    let currentPayer = '蒂';
    let currentSplitMethod = 'equal';
    const users = ['蒂', '丞', '頻'];

    const tripData = [
        { spots: [
            { time: "下午", title: "抵達成田機場", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Narita_International_Airport_Terminal_2_2013.jpg/640px-Narita_International_Airport_Terminal_2_2013.jpg", desc: "抵達後辦理入境，購買 Suica。這趟冒險正式開始！", buy: "Suica 卡 (綁定 Apple Pay)", transport: "成田機場 (Skyliner) ➔ 船橋 (總武線) ➔ 西船橋 (東西線) ➔ 南行德", must: "IT280" },
            { time: "15:30", title: "中目黑 星巴克旗艦店", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/6/6b/Starbucks_Reserve_Roastery_Tokyo_2019.jpg/640px-Starbucks_Reserve_Roastery_Tokyo_2019.jpg", desc: "全球僅六間 Reserve Roastery。紅銅櫻花噴泉必拍。旁邊山手通店亦可逛。", buy: "限定咖啡豆、Roastery 限定馬克杯", transport: "南行德 ➔ 茅場町 ➔ 中目黑", must: "必喝旗艦限定" },
            { time: "17:30", title: "小義大利 La vita", img: "https://lh3.googleusercontent.com/p/AF1QipP-n-f-n-H=s1600-w640", desc: "自由之丘的歐風角落。石板路與小橋，彷彿置身威尼斯。", buy: "質感生活雜貨", transport: "中目黑 ➔ 自由之丘 (步行5分)", must: "網美拍照點" },
            { time: "19:30", title: "上野阿美橫町", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a2/Ameya_Yokocho_Entrance.jpg/640px-Ameya_Yokocho_Entrance.jpg", desc: "藥妝、海鮮、零食的集散地，感受濃濃的庶民活力。", buy: "二木菓子、OS Drug 藥妝、志村商店", transport: "自由之丘 ➔ 上野", must: "藥妝零食聖地" }
        ]},
        { spots: [
            { time: "09:30", title: "江户和装工房 雅 (和服)", img: "https://lh3.googleusercontent.com/p/AF1QipMT1T9jK8-I5Z_V6Z8X5-6_H_H-f-n-H=s1600-w640", desc: "穿上精緻和服漫步淺草。預約代號 JK2026。", buy: "和服寫真回憶", transport: "南行德 ➔ 淺草", must: "JK2026" },
            { time: "11:00", title: "淺草寺 / 雷門", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/c/c5/Kaminarimon_at_night.jpg/640px-Kaminarimon_at_night.jpg", desc: "穿越大燈籠，漫步商店街吃小吃。", buy: "炸饅頭、雷門御守", transport: "淺草站即達", must: "求個御守" },
            { time: "14:00", title: "Suzukien 抹茶甜點", img: "https://lh3.googleusercontent.com/p/AF1QipN-T-f-n-H=s1600-w640", desc: "壽壽喜園。挑戰世界最濃 No.7 抹茶冰淇淋！", buy: "抹茶系列甜點", transport: "淺草寺後方步行 5 分", must: "挑戰 No.7" },
            { time: "16:00", title: "今戶神社 & 晴空塔", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/4/4b/Tokyo_Skytree_at_night.jpg/640px-Tokyo_Skytree_at_night.jpg", desc: "招財貓聖地祈求良緣，隨後前往晴空塔賞景。", buy: "招財貓御守、晴空塔 Banana", transport: "淺草 ➔ 押上站", must: "看日落" },
            { time: "19:00", title: "燒肉黑田 (澀谷)", img: "https://lh3.googleusercontent.com/p/AF1QipO-n-f-n-H=s1600-w640", desc: "隱身澀谷的高級燒肉饗宴。特選 A5 厚切牛舌必點。", buy: "高級和牛體驗", transport: "押上 ➔ 澀谷", must: "必點牛舌" }
        ]},
        { spots: [{ time: "全天", title: "東京迪士尼海洋 (Sea)", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Tokyo_DisneySea_Entrance.jpg/640px-Tokyo_DisneySea_Entrance.jpg", desc: "全新「夢幻泉鄉」區盛大開幕！冰雪奇緣與小飛俠的世界。", buy: "達菲熊周邊、夢幻泉鄉限定商品", transport: "南行德 ➔ 葛西/浦安 ➔ 巴士 ➔ 舞濱", must: "必搶 DPA" }]},
        { spots: [{ time: "全天", title: "東京迪士尼樂園 (Land)", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Cinderella_Castle_Tokyo_Disneyland_2019.jpg/640px-Cinderella_Castle_Tokyo_Disneyland_2019.jpg", desc: "公主夢幻日。美女與野獸城堡巡禮。", buy: "米奇爆米花桶、公主髮飾", transport: "交通同前日", must: "公主夢" }]},
        { spots: [
            { time: "10:00", title: "東京都廳 ➔ 明治神宮", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/d/d0/Tokyo_Metropolitan_Government_Building_2012.jpg/640px-Tokyo_Metropolitan_Government_Building_2012.jpg", desc: "免費展望台。隨後往神宮森林散步。", buy: "明治神宮御守", transport: "南行德 ➔ 日本橋 ➔ 新宿", must: "免費觀景" },
            { time: "12:30", title: "敘敘苑 (歌劇城 53F)", img: "https://lh3.googleusercontent.com/p/AF1QipN-T-f-n-H=s1600-w640", desc: "邊吃燒肉邊俯瞰東京景緻。", buy: "敘敘苑燒肉醬", transport: "初台站直結", must: "窗邊景觀" },
            { time: "15:00", title: "原宿竹下通 ➔ 伊勢丹", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/3/3a/Takeshita_Street_Harajuku.jpg/640px-Takeshita_Street_Harajuku.jpg", desc: "潮流發源地與高級百貨。", buy: "虎屋羊羹、年輪蛋糕", transport: "原宿步行即達", must: "甜點天堂" },
            { time: "19:00", title: "澀谷 SKY 夜景", img: "http://googleusercontent.com/profile/picture/36", desc: "360度展望台俯瞰澀谷十字路口。", buy: "限定周邊", transport: "新宿 ➔ 澀谷", must: "提早預約" },
            { time: "21:00", title: "歌舞伎町散策", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/Kabukicho_Ichibangai_Gate.jpg/640px-Kabukicho_Ichibangai_Gate.jpg", desc: "感受不夜城霓虹與哥吉拉大樓。", buy: "驚安殿堂採買", transport: "澀谷 ➔ 新宿", must: "拍哥吉拉" }
        ]},
        { spots: [{ time: "08:15", title: "前往成田機場", img: "https://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Narita_International_Airport_Terminal_2_2013.jpg/640px-Narita_International_Airport_Terminal_2_2013.jpg", desc: "回程報到 (IT281)。結束夢幻之旅。", buy: "機場免稅最後採購", transport: "南行德 ➔ 船橋 ➔ 京成本線 ➔ 成田機場", must: "準時抵達" }] }
    ];

    let expenses = JSON.parse(localStorage.getItem('tokyo_split_final_v1')) || [];

    // --- 記帳與分帳邏輯 ---
    function setPayer(name, btn) {
        currentPayer = name;
        document.querySelectorAll('.payer-btn').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
    }

    function setSplitMethod(method, btn) {
        currentSplitMethod = method;
        document.querySelectorAll('.split-method-btn').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        renderSplitInputs();
    }

    function renderSplitInputs() {
        const container = document.getElementById('split-inputs-container');
        if (currentSplitMethod === 'equal') {
            container.innerHTML = `<p style="text-align:center; font-size:0.8rem; color:#888;">平均分攤每人 1/3</p>`;
        } else {
            const unit = currentSplitMethod === 'percent' ? '%' : '¥';
            container.innerHTML = users.map(u => `
                <div class="split-input-row"><span>${u} 負擔</span><div><input type="number" class="split-val" data-user="${u}"> ${unit}</div></div>
            `).join('');
        }
    }

    async function fetchRate() {
        try {
            const res = await fetch('https://api.exchangerate-api.com/v4/latest/JPY');
            const data = await res.json(); exchangeRate = data.rates.TWD;
            document.getElementById('current-rate-display').innerText = `即時匯率 1 JPY = ${exchangeRate.toFixed(4)} TWD`;
        } catch (e) { document.getElementById('current-rate-display').innerText = "匯率抓取失敗，採用預設 0.215"; }
    }

    function addExpense() {
        const name = document.getElementById('itemName').value;
        const totalJPY = parseFloat(document.getElementById('itemPrice').value);
        if(!name || isNaN(totalJPY)) return alert("記得填品名跟日幣喔！🎀");

        let splits = {};
        if (currentSplitMethod === 'equal') {
            users.forEach(u => splits[u] = totalJPY / 3);
        } else if (currentSplitMethod === 'percent') {
            let sumP = 0;
            document.querySelectorAll('.split-val').forEach(el => {
                let p = parseFloat(el.value) || 0;
                splits[el.dataset.user] = (totalJPY * p) / 100; sumP += p;
            });
            if (sumP !== 100) return alert("百分比總和須為 100%！");
        } else {
            let sumE = 0;
            document.querySelectorAll('.split-val').forEach(el => {
                let v = parseFloat(el.value) || 0;
                splits[el.dataset.user] = v; sumE += v;
            });
            if (sumE !== totalJPY) return alert(`加總應為 ¥${totalJPY}`);
        }
        expenses.push({ id: Date.now(), name, totalJPY, payer: currentPayer, splits });
        localStorage.setItem('tokyo_split_final_v1', JSON.stringify(expenses));
        document.getElementById('itemName').value = ''; document.getElementById('itemPrice').value = '';
        updateBudgetUI();
    }

    function deleteExpense(id) {
        expenses = expenses.filter(e => e.id !== id);
        localStorage.setItem('tokyo_split_final_v1', JSON.stringify(expenses));
        updateBudgetUI();
    }

    function updateBudgetUI() {
        const list = document.getElementById('expense-list');
        const netBalances = { '蒂': 0, '丞': 0, '頻': 0 };

        list.innerHTML = expenses.length ? expenses.map(e => {
            netBalances[e.payer] += e.totalJPY;
            users.forEach(u => netBalances[u] -= e.splits[u]);
            return `<div class="expense-item"><b>${e.name}</b> (${e.payer}付)<br><small>¥ ${e.totalJPY.toLocaleString()}</small><button onclick="deleteExpense(${e.id})" class="del-btn" style="float:right;"><i class="fas fa-trash"></i></button></div>`;
        }).reverse().join('') : '<p style="text-align:center; color:#CCC;">尚無紀錄 💸</p>';

        const totalJPY = expenses.reduce((s, e) => s + e.totalJPY, 0);
        document.getElementById('total-twd').innerText = Math.round(totalJPY * exchangeRate).toLocaleString();

        let debtors = users.map(u => ({ name: u, bal: netBalances[u] })).filter(u => u.bal < -0.1).sort((a,b) => a.bal - b.bal);
        let creditors = users.map(u => ({ name: u, bal: netBalances[u] })).filter(u => u.bal > 0.1).sort((a,b) => b.bal - a.bal);
        let inst = [];
        let d = 0, c = 0;
        while (d < debtors.length && c < creditors.length) {
            let amt = Math.min(Math.abs(debtors[d].bal), creditors[c].bal);
            inst.push(`<div style="font-weight:bold;">${debtors[d].name} ➜ ${creditors[c].name} <span style="color:#D14D72;">¥ ${Math.round(amt).toLocaleString()}</span></div>`);
            debtors[d].bal += amt; creditors[c].bal -= amt;
            if (Math.abs(debtors[d].bal) < 0.1) d++; if (Math.abs(creditors[c].bal) < 0.1) c++;
        }
        document.getElementById('settlement-result').innerHTML = inst.length ? inst.join('') : '<div style="color:#2E7D32;">帳目目前完美平衡 ✨</div>';
    }

    // --- 行程、分頁、彈窗邏輯 ---
    function switchDay(idx, btn) {
        document.querySelectorAll('.day-btn').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        renderDay(idx);
    }
    function renderDay(idx) {
        document.getElementById('itinerary-list').innerHTML = tripData[idx].spots.map(s => `
            <div class="card" onclick='showModal(${JSON.stringify(s)})'>
                <div class="card-title"><span>${s.time}</span><span>${s.title}</span></div>
                <div style="margin-top:5px;"><span class="tag-must"># ${s.must}</span></div>
            </div>`).join('');
    }
    function showModal(s) {
        document.getElementById('modal-body').innerHTML = `
            <img src="${s.img}" class="spot-img">
            <div style="font-size:1.4rem; color:var(--secondary); margin-bottom:10px; border-bottom:2px dashed var(--accent-pink); padding-bottom:5px;">${s.title}</div>
            <div style="font-weight:bold; color:var(--primary); margin-top:10px;">📖 介紹</div><div style="font-size:0.95rem; line-height:1.6; margin-bottom:10px;">${s.desc}</div>
            <div style="font-weight:bold; color:var(--primary);">🛍️ 推薦購買</div><div style="font-size:0.9rem; margin-bottom:10px;">${s.buy}</div>
            <div style="font-weight:bold; color:var(--primary);">🚆 交通建議</div><div style="font-size:0.85rem; background:#F0FFF0; padding:10px; border-radius:10px; color:#2E7D32;">${s.transport}</div>
            <a href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(s.title)}" target="_blank" class="nav-link-btn"><i class="fas fa-location-arrow mr-2"></i> 開啟地圖導航</a>`;
        document.getElementById('detail-modal').classList.add('active');
    }
    function closeModal(e) { if (!e || e.target === document.getElementById('detail-modal')) document.getElementById('detail-modal').classList.remove('active'); }
    function switchTab(tabId, btn) {
        document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
        document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
        document.getElementById(tabId).classList.add('active');
        btn.classList.add('active');
        document.getElementById('day-selector-container').style.display = (tabId === 'tab-itinerary') ? 'flex' : 'none';
    }

    window.onload = () => { fetchRate(); renderDay(0); renderSplitInputs(); updateBudgetUI(); };
</script>
</body>
</html>
