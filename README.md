# 💜 
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="東京2026">
    <title>2026 東京夢幻之旅 ✨</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        @font-face {
            font-family: 'Chenyuluoyan';
            src: url('https://cdn.jsdelivr.net/gh/chenyu-shuo/Chenyuluoyan-Thin@main/Chenyuluoyan-Thin.ttf') format('truetype');
            font-display: swap;
        }

        :root {
            --primary: #9D84B7; 
            --secondary: #6B4E8D;
            --bg: #FDFBFF;
            --accent-pink: #FFD1DC;
            --safe-top: env(safe-area-inset-top);
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
            background-size: 30px 30px;
        }

        /* Header */
        .header {
            background: linear-gradient(135deg, #B19CD9, var(--primary));
            color: white; padding: calc(25px + var(--safe-top)) 20px 20px;
            text-align: center; border-bottom-left-radius: 35px; border-bottom-right-radius: 35px;
            box-shadow: 0 4px 15px rgba(157, 132, 183, 0.2);
            position: sticky; top: 0; z-index: 100;
        }
        .header h1 { margin: 0; font-size: 1.6rem; text-shadow: 1px 1px 2px rgba(0,0,0,0.1); }

        /* Day Scroller */
        .day-scroller { display: flex; overflow-x: auto; gap: 12px; padding: 15px; scrollbar-width: none; }
        .day-scroller::-webkit-scrollbar { display: none; }
        .day-btn { flex: 0 0 110px; height: 50px; background: white; border-radius: 18px; display: flex; align-items: center; justify-content: center; border: 1px solid #F0E6FF; transition: 0.3s; font-size: 0.9rem; color: var(--primary); font-weight: bold; }
        .day-btn.active { background: var(--primary); color: white; transform: scale(1.05); box-shadow: 0 5px 12px rgba(157, 132, 183, 0.25); }

        /* Main Cards */
        .container { padding: 0 15px; max-width: 500px; margin: 0 auto; }
        .card { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(5px); border-radius: 24px; padding: 20px; margin-bottom: 15px; border: 1px solid rgba(255, 255, 255, 0.8); box-shadow: 0 8px 20px rgba(157, 132, 183, 0.08); }
        .card-title { font-size: 1.15rem; font-weight: bold; color: var(--secondary); display: flex; align-items: center; gap: 10px; margin-bottom: 12px; }

        /* Budget UI */
        .total-banner { background: linear-gradient(135deg, var(--secondary), #4B367C); color: white; border-radius: 25px; padding: 25px; text-align: center; margin-bottom: 20px; }
        .settlement-card { background: #FFF9FB; border: 2px dashed var(--accent-pink); border-radius: 22px; padding: 18px; margin-bottom: 20px; }
        .payer-selector, .split-selector { display: flex; gap: 8px; margin-bottom: 12px; }
        .selector-btn { flex: 1; padding: 10px; border-radius: 12px; border: 1px solid #EEE; background: white; font-family: 'Chenyuluoyan'; font-size: 0.9rem; cursor: pointer; transition: 0.2s; }
        .selector-btn.active { background: var(--primary); color: white; border-color: var(--primary); }
        .split-input-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; padding-top: 8px; border-top: 1px dashed #EEDDFF; }
        .split-input-row input { width: 90px; padding: 6px; border-radius: 10px; border: 1px solid #DDD; text-align: right; font-family: 'Chenyuluoyan'; outline: none; }
        
        .expense-item { border-bottom: 1px solid #F5F5F5; padding: 12px 0; display: flex; justify-content: space-between; align-items: center; }
        .del-btn { color: #FFB7B2; border: none; background: none; font-size: 1.1rem; padding: 5px; }

        /* Modal */
        .modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.5); backdrop-filter: blur(6px); display: none; justify-content: center; align-items: flex-end; z-index: 2000; }
        .modal-content { background: white; width: 100%; max-width: 500px; border-radius: 35px 35px 0 0; padding: 35px 25px calc(30px + var(--safe-bottom)); transform: translateY(100%); transition: 0.4s; max-height: 90vh; overflow-y: auto; position: relative; }
        .modal-overlay.active { display: flex; }
        .modal-overlay.active .modal-content { transform: translateY(0); }
        .modal-close { position: absolute; top: 25px; right: 25px; font-size: 1.8rem; color: #DDD; cursor: pointer; }
        .spot-img { width: 100%; height: 220px; object-fit: cover; border-radius: 25px; margin-bottom: 20px; }
        
        .nav-link-btn { background: #F0E6FF; color: var(--secondary); text-decoration: none; padding: 14px; border-radius: 18px; font-size: 0.95rem; display: flex; align-items: center; justify-content: center; font-weight: bold; margin-top: 20px; }

        /* Bottom Nav */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(20px); display: flex; justify-content: space-around; padding: 12px 0 calc(12px + var(--safe-bottom)); border-top: 1px solid rgba(157, 132, 183, 0.1); z-index: 1000; }
        .nav-item { background: none; border: none; color: #C0B0D0; display: flex; flex-direction: column; align-items: center; flex: 1; outline: none; font-family: 'Chenyuluoyan'; gap: 4px; }
        .nav-item i { font-size: 1.3rem; }
        .nav-item span { font-size: 0.75rem; }
        .nav-item.active { color: var(--secondary); transition: 0.3s; }
        
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .tag-must { font-size: 0.75rem; background: var(--accent-pink); color: #D14D72; padding: 3px 10px; border-radius: 10px; font-weight: bold; }
    </style>
</head>
<body>

<div class="app-container">
    <header class="header"><h1>東京夢幻之旅 2026 ✨</h1></header>

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
                <div class="card-title"><i class="fas fa-plane-departure"></i> 航班資訊</div>
                <p style="font-size:1rem; line-height:1.8;">
                    <b>去程 IT280：</b>高雄 08:00 ➔ 成田 12:10<br>
                    <b>回程 IT281：</b>成田 11:25 ➔ 高雄 15:05
                </p>
            </div>
            
            <div class="card">
                <div class="card-title"><i class="fas fa-shopping-cart"></i> 東京熱門必買大眾清單</div>
                <ul style="padding-left: 20px; font-size: 1rem; line-height: 2;">
                    <li>🧀 <b>New York Perfect Cheese</b> (起司奶油脆餅)</li>
                    <li>🧈 <b>Press Butter Sand</b> (焦糖夾心餅乾)</li>
                    <li>🥖 <b>Sugar Butter Tree</b> (砂糖奶油夾心)</li>
                    <li>🍱 <b>茅乃舍高湯包</b> (日料必備)</li>
                    <li>☀️ <b>安耐曬 Anessa</b> (強效防曬)</li>
                    <li>🍵 <b>一蘭拉麵即食包</b> (經典伴手禮)</li>
                    <li>🛒 <b>Loft/Tokyu Hands 限定文具貼紙</b></li>
                </ul>
            </div>

            <div class="card">
                <div class="card-title"><i class="fas fa-hotel"></i> 住宿地點導航</div>
                <p><b>D・レガーロ (南行德)</b></p>
                <img src="https://images.unsplash.com/photo-1598214813160-580a91e57a3d?q=80&w=800" class="spot-img">
                <p style="font-size:0.85rem; color:#666;">〒272-0138 千葉県市川市南行德２丁目２１</p>
                <a href="https://maps.app.goo.gl/1nyp2nPgscnQ9BL5A" target="_blank" class="nav-link-btn"><i class="fas fa-map-marked-alt"></i> 查看住宿正確導航網址</a>
            </div>
        </div>

        <div id="tab-budget" class="tab-content">
            <div class="total-banner">
                <p style="font-size:0.85rem; opacity:0.9;">總支出 (約台幣)</p>
                <h2 style="font-size:2.4rem; margin:8px 0;">NT$ <span id="total-twd">0</span></h2>
                <p id="current-rate-display" style="font-size:0.75rem; opacity:0.8;">獲取即時匯率中...</p>
            </div>

            <div class="settlement-card">
                <div class="card-title" style="justify-content:center; font-size:1rem; color:#D14D72;"><i class="fas fa-hand-holding-usd"></i> 結算帳目 (日幣 ¥)</div>
                <div id="settlement-result" style="margin-top:10px; line-height:1.8; text-align:center;"></div>
            </div>

            <div class="card">
                <div class="card-title">新增支出 💸</div>
                <p style="font-size:0.8rem; color:var(--primary); margin:5px 0;">付款人</p>
                <div class="payer-selector">
                    <button class="selector-btn active payer-btn" onclick="setPayer('蒂', this)">蒂</button>
                    <button class="selector-btn payer-btn" onclick="setPayer('丞', this)">丞</button>
                    <button class="selector-btn payer-btn" onclick="setPayer('頻', this)">頻</button>
                </div>
                <input type="text" id="itemName" placeholder="品名" style="width:100%; padding:14px; margin-bottom:10px; border-radius:15px; border:1px solid #EEE; font-family:'Chenyuluoyan'; outline:none;">
                <input type="number" id="itemPrice" placeholder="日幣金額 ¥" style="width:100%; padding:14px; margin-bottom:15px; border-radius:15px; border:1px solid #EEE; font-family:'Chenyuluoyan'; outline:none;">
                
                <p style="font-size:0.8rem; color:var(--primary); margin:5px 0;">分攤方式</p>
                <div class="split-selector">
                    <button class="selector-btn active split-method-btn" onclick="setSplitMethod('equal', this)">平均分攤</button>
                    <button class="selector-btn split-method-btn" onclick="setSplitMethod('exact', this)">自訂金額</button>
                </div>
                <div id="split-inputs-container"></div>
                <button onclick="addExpense()" style="width:100%; padding:18px; background:var(--primary); color:white; border:none; border-radius:18px; font-family:'Chenyuluoyan'; font-weight:bold; font-size:1.1rem; margin-top:10px;">記錄一筆 💜</button>
            </div>
            <div id="expense-list"></div>
        </div>
    </main>

    <div id="detail-modal" class="modal-overlay" onclick="closeModal(event)">
        <div class="modal-content" onclick="event.stopPropagation()">
            <span class="modal-close" onclick="closeModal(null)">×</span>
            <div id="modal-body"></div>
        </div>
    </div>

    <nav class="bottom-nav">
        <button class="nav-item active" onclick="switchTab('tab-itinerary', this)"><i class="fas fa-map-signs"></i><span>行程</span></button>
        <button class="nav-item" onclick="switchTab('tab-info', this)"><i class="fas fa-info-circle"></i><span>資訊</span></button>
        <button class="nav-item" onclick="switchTab('tab-budget', this)"><i class="fas fa-calculator"></i><span>記帳</span></button>
    </nav>
</div>

<script>
    let exchangeRate = 0.215; 
    let currentPayer = '蒂';
    let currentSplitMethod = 'equal';
    const users = ['蒂', '丞', '頻'];

    const tripData = [
        { spots: [
            { time: "下午", title: "抵達成田機場", img: "https://images.unsplash.com/photo-1570710891163-6d3b5c47248b?q=80&w=800", desc: "抵達後辦理入境，購買或儲值 Suica。", buy: "Suica 卡 (Apple Pay)", transport: "Skyliner ➔ 船橋 ➔ 南行德", must: "IT280 航班" },
            { time: "16:00", title: "中目黑 星巴克旗艦店", img: "https://images.unsplash.com/photo-1608144372917-742713919e34?q=80&w=800", desc: "全球僅六間的星巴克臻選烘焙工坊，建築由隈研吾設計。", buy: "Roastery 限定隨行杯", transport: "地鐵中目黑站", must: "必拍櫻花柱" },
            { time: "17:30", title: "自由之丘 La vita", img: "https://images.unsplash.com/photo-1545239351-ef35f43d514b?q=80&w=800", desc: "被譽為東京的小威尼斯，充滿異國風情的建築與水道。", buy: "質感生活雜貨", transport: "自由之丘站", must: "網美拍照點" },
            { time: "19:30", title: "上野阿美橫町商店街", img: "https://static.gltjp.com/glt/data/directory/12000/11018/20200603_132446_67d0ab79_w1920.webp", desc: "藥妝、零食、海鮮的集散地，感受東京的下町活力。", buy: "二木菓子、OS Drug", transport: "JR/地鐵上野站", must: "藥妝採購" }
        ]},
        { spots: [
            { time: "10:00", title: "和裝工房 雅 (和服體驗)", img: "https://images.unsplash.com/photo-1493976040374-85c8e12f0c0e?q=80&w=800", desc: "預約代號 JK2026，穿上精緻和服慢步淺草。", buy: "和服寫真回憶", transport: "南行德 ➔ 淺草站", must: "JK2026" },
            { time: "11:00", title: "淺草寺 / 雷門", img: "https://images.unsplash.com/photo-1583212292454-1fe6229603b7?q=80&w=800", desc: "穿越巨大雷門大紅燈籠，祈求一年好運。", buy: "淺草御守", transport: "淺草站即達", must: "必拍雷門" },
            { time: "14:00", title: "壽壽喜園 抹茶冰淇淋", img: "https://img.bigfang.tw/2024/06/1717336115-4efdd2f969559e8b1c92e99f32ded48e.jpg", desc: "挑戰世界最濃 No.7 抹茶冰淇淋！", buy: "抹茶系列點心", transport: "淺草寺後方步行 5 分", must: "挑戰 No.7" },
            { time: "16:00", title: "今戶神社 / 晴空塔", img: "https://corritrip.jp/jpn/blog/wp/wp-content/uploads/2024/09/pixta_96888508_S-1.jpg", desc: "招財貓發源地祈求良緣，隨後前往晴空塔欣賞高空美景。", buy: "貓咪御守", transport: "淺草 ➔ 押上站", must: "高空夕陽" },
            { time: "19:00", title: "燒肉黑田 (澀谷店)", img: "https://cdn.myfunnow.com/imgs/blog/album/%E7%87%92%E8%82%89%20%E9%BB%91%E7%94%B0%E7%92%B0%E5%A2%831_e4b495.png", desc: "品嚐 A5 頂級和牛，享受絕佳的炭火燒肉氛圍。", buy: "燒肉體驗", transport: "地鐵澀谷站", must: "必點厚切牛舌" }
        ]},
        { spots: [{ time: "全天", title: "東京迪士尼海洋", img: "https://images.unsplash.com/photo-1542359649-31e03ad4d92c?q=80&w=800", desc: "探索全新「夢幻泉鄉 (Fantasy Springs)」區域，走進冰雪奇緣與彼得潘的世界。", buy: "達菲周邊商品", transport: "南行德 ➔ 舞濱站 ➔ 單軌", must: "搶 DPA 卡" }]},
        { spots: [{ time: "全天", title: "東京迪士尼樂園", img: "https://images.unsplash.com/photo-1545239351-ef35f43d514b?q=80&w=800", desc: "經典樂園日，走入美女與野獸城堡，實現公主夢。", buy: "造型爆米花桶", transport: "舞濱站步行", must: "公主風格日" }]},
        { spots: [
            { time: "10:00", title: "東京都廳展望台", img: "https://tenjo.tw/wp-content/uploads/2024/11/JAP_7302.jpg", desc: "免費俯瞰新宿與東京壯觀的天際線景觀。", buy: "都廳紀念小物", transport: "南行德 ➔ 新宿站", must: "免費登頂" },
            { time: "11:30", title: "明治神宮", img: "https://jingu-artfest.jp/wp-content/uploads/2019/12/04torii_MG_6604.jpg", desc: "在東京都心的森林大鳥居下漫步，洗滌心靈。", buy: "心願繪馬", transport: "JR 原宿站", must: "森林浴" },
            { time: "13:00", title: "敘敘苑燒肉 (Opera City 53F)", img: "https://natasha-traveler.tw/wp-content/uploads/2025/06/tokyo-jojoen-yakinuku-review-20.jpg", desc: "邊品味頂級燒肉，邊從 53 樓高處眺望城市景致。", buy: "敘敘苑燒肉醬", transport: "初台站直通", must: "高空美景席" },
            { time: "15:00", title: "原宿竹下通 / 伊勢丹百貨", img: "https://upload.wikimedia.org/wikipedia/commons/4/4d/Takeshita_Street_in_December_2018.jpg", desc: "從潮流尖端的竹下通逛到高級優雅的伊勢丹百貨。", buy: "潮流飾品、伴手禮", transport: "原宿 ➔ 新宿", must: "瘋狂購物" },
            { time: "19:00", title: "澀谷 SKY (360度展望台)", img: "https://www.daisyyohoho.com/wp-content/uploads/2025/03/shibuya-sky-53.jpg", desc: "站在澀谷最高點，360 度無死角環視東京最美夜景。", buy: "限定周邊照片", transport: "地鐵澀谷站", must: "提早 1 個月預約" },
            { time: "21:00", title: "歌舞伎町散策 / 哥吉拉大樓", img: "https://wow-japan.com/wp-content/uploads/2020/10/godzilla-1.jpg", desc: "感受東京不夜城的霓虹燈火，尋找巨大的哥吉拉頭像。", buy: "驚安殿堂免稅品", transport: "新宿站東口", must: "哥吉拉拍照" }
        ]},
        { spots: [{ time: "08:15", title: "前往成田機場 (IT281)", img: "https://www.jal.co.jp/twl/zhtw/inter/airport/nrt/info/img/e_nrt1_4.gif", desc: "帶著滿滿的回憶與戰利品，準備搭機返程。", buy: "免稅伴手禮", transport: "南行德 ➔ 成田機場", must: "準時抵達機場" }] }
    ];

    let expenses = JSON.parse(localStorage.getItem('tokyo_expenses_v2026')) || [];

    // --- 記帳邏輯 ---
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
            container.innerHTML = `<p style="text-align:center; font-size:0.8rem; color:#AAA; margin:10px 0;">三人平均分攤 1/3</p>`;
        } else {
            container.innerHTML = users.map(u => `
                <div class="split-input-row"><span>${u} 負擔</span><div>¥ <input type="number" class="split-val" data-user="${u}"></div></div>
            `).join('');
        }
    }

    async function fetchRate() {
        try {
            const res = await fetch('https://api.exchangerate-api.com/v4/latest/JPY');
            const data = await res.json(); exchangeRate = data.rates.TWD;
            document.getElementById('current-rate-display').innerText = `當日即時匯率: 1 JPY ≈ ${exchangeRate.toFixed(4)} TWD`;
        } catch (e) { document.getElementById('current-rate-display').innerText = "匯率抓取失敗，採用預設 0.215"; }
    }

    function addExpense() {
        const name = document.getElementById('itemName').value;
        const totalJPY = parseFloat(document.getElementById('itemPrice').value);
        if(!name || isNaN(totalJPY)) return alert("記得填品名與金額喔！✨");

        let splits = {};
        if (currentSplitMethod === 'equal') {
            users.forEach(u => splits[u] = totalJPY / users.length);
        } else {
            let sumE = 0;
            document.querySelectorAll('.split-val').forEach(el => {
                let v = parseFloat(el.value) || 0;
                splits[el.dataset.user] = v; sumE += v;
            });
            if (Math.abs(sumE - totalJPY) > 1) return alert(`自訂金額總和需為 ¥${totalJPY}！`);
        }
        
        expenses.push({ id: Date.now(), name, totalJPY, payer: currentPayer, splits });
        localStorage.setItem('tokyo_expenses_v2026', JSON.stringify(expenses));
        document.getElementById('itemName').value = ''; document.getElementById('itemPrice').value = '';
        updateBudgetUI();
    }

    function deleteExpense(id) {
        expenses = expenses.filter(e => e.id !== id);
        localStorage.setItem('tokyo_expenses_v2026', JSON.stringify(expenses));
        updateBudgetUI();
    }

    function updateBudgetUI() {
        const list = document.getElementById('expense-list');
        const netBalances = { '蒂': 0, '丞': 0, '頻': 0 };

        list.innerHTML = expenses.map(e => {
            netBalances[e.payer] += e.totalJPY;
            users.forEach(u => netBalances[u] -= e.splits[u]);
            return `<div class="expense-item"><div><b>${e.name}</b><br><small>${e.payer}付 ¥${e.totalJPY.toLocaleString()}</small></div><button onclick="deleteExpense(${e.id})" class="del-btn"><i class="fas fa-trash-alt"></i></button></div>`;
        }).reverse().join('');

        const totalJPY = expenses.reduce((s, e) => s + e.totalJPY, 0);
        document.getElementById('total-twd').innerText = Math.round(totalJPY * exchangeRate).toLocaleString();

        let debtors = users.map(u => ({ name: u, bal: netBalances[u] })).filter(u => u.bal < -0.1).sort((a,b) => a.bal - b.bal);
        let creditors = users.map(u => ({ name: u, bal: netBalances[u] })).filter(u => u.bal > 0.1).sort((a,b) => b.bal - a.bal);
        let inst = [];
        let d = 0, c = 0;
        while (d < debtors.length && c < creditors.length) {
            let amt = Math.min(Math.abs(debtors[d].bal), creditors[c].bal);
            inst.push(`<div><b>${debtors[d].name}</b> ➜ <b>${creditors[c].name}</b> <span style="color:#D14D72;">¥ ${Math.round(amt).toLocaleString()}</span></div>`);
            debtors[d].bal += amt; creditors[c].bal -= amt;
            if (Math.abs(debtors[d].bal) < 0.1) d++; if (Math.abs(creditors[c].bal) < 0.1) c++;
        }
        document.getElementById('settlement-result').innerHTML = inst.length ? inst.join('') : '帳目平衡 ✨';
    }

    // --- 行程邏輯 ---
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
            <img src="${s.img}" class="spot-img" onerror="this.src='https://images.unsplash.com/photo-1542931237-323a19592736?q=80&w=800'">
            <div style="font-size:1.4rem; color:var(--secondary); font-weight:bold; margin-bottom:10px; border-bottom: 2px dashed var(--accent-pink); padding-bottom: 5px;">${s.title}</div>
            <div style="font-weight:bold; color:var(--primary);">📖 景點介紹</div><div style="font-size:0.95rem; line-height:1.6; margin-bottom:15px;">${s.desc}</div>
            <div style="font-weight:bold; color:var(--primary);">🛍️ 推薦購買</div><div style="font-size:0.95rem; margin-bottom:15px;">${s.buy}</div>
            <div style="font-weight:bold; color:var(--primary);">🚆 交通建議</div><div style="font-size:0.9rem; background:#F8F6FF; padding:12px; border-radius:15px; color:#5D4E7A; border:1px solid #EEDDFF;">${s.transport}</div>
            <a href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(s.title)}" target="_blank" class="nav-link-btn"><i class="fas fa-location-arrow"></i> 開啟 Google 地圖導航</a>`;
        document.getElementById('detail-modal').classList.add('active');
    }

    function closeModal(e) { if (!e || e.target === document.getElementById('detail-modal')) document.getElementById('detail-modal').classList.remove('active'); }
    
    function switchTab(tabId, btn) {
        document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
        document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
        document.getElementById(tabId).classList.add('active');
        btn.classList.add('active');
        document.getElementById('day-selector-container').style.display = (tabId === 'tab-itinerary') ? 'flex' : 'none';
        window.scrollTo(0, 0);
    }

    window.onload = () => { fetchRate(); renderDay(0); renderSplitInputs(); updateBudgetUI(); };
</script>
</body>
</html>
