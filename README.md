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
            --primary: #9D84B7; /* 夢幻紫 */
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
        .modal-content { background: white; width: 100%; max-width: 500px; border-radius: 35px 35px 0 0; padding: 35px 25px calc(30px + var(--safe-bottom)); transform: translateY(100%); transition: 0.4s cubic-bezier(0.165, 0.84, 0.44, 1); max-height: 90vh; overflow-y: auto; position: relative; }
        .modal-overlay.active { display: flex; }
        .modal-overlay.active .modal-content { transform: translateY(0); }
        .modal-close { position: absolute; top: 25px; right: 25px; font-size: 1.8rem; color: #DDD; cursor: pointer; }
        .spot-img { width: 100%; height: 220px; object-fit: cover; border-radius: 25px; margin-bottom: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        
        .nav-link-btn { background: #F0E6FF; color: var(--secondary); text-decoration: none; padding: 14px; border-radius: 18px; font-size: 0.95rem; display: flex; align-items: center; justify-content: center; font-weight: bold; margin-top: 20px; border: 1px solid #DDCBFF; }

        /* Bottom Nav */
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(20px); display: flex; justify-content: space-around; padding: 12px 0 calc(12px + var(--safe-bottom)); border-top: 1px solid rgba(157, 132, 183, 0.1); z-index: 1000; }
        .nav-item { background: none; border: none; color: #C0B0D0; display: flex; flex-direction: column; align-items: center; flex: 1; outline: none; font-family: 'Chenyuluoyan'; gap: 4px; }
        .nav-item i { font-size: 1.3rem; }
        .nav-item span { font-size: 0.75rem; }
        .nav-item.active { color: var(--secondary); transform: translateY(-2px); transition: 0.3s; }
        
        .tab-content { display: none; animation: fadeIn 0.4s ease; }
        .tab-content.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .tag-must { font-size: 0.75rem; background: var(--accent-pink); color: #D14D72; padding: 3px 10px; border-radius: 10px; font-weight: bold; }
        input::placeholder { color: #CCC; }
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
                    <b>去程 IT280：</b><br>10/26 高雄 08:00 ➔ 成田 12:10<br>
                    <b>回程 IT281：</b><br>10/31 成田 11:25 ➔ 高雄 15:05
                </p>
            </div>
            <div class="card">
                <div class="card-title"><i class="fas fa-hotel"></i> 住宿地點</div>
                <p><b>D・レガーロ (市川市)</b></p>
                <img src="https://cf.bstatic.com/xdata/images/hotel/max1024x768/755286737.jpg?k=b835710f5377acc38b264e7af4705d11b521551b48fd6bf22613628737584bb7&o=" alt="Accommodation" class="spot-img">
                <p style="font-size:0.85rem; color:#666;">〒272-0138 千葉県市川市南行德２丁目２１</p>
                <a href="https://maps.app.goo.gl/uXpDkq4b4xYm4jE96" target="_blank" class="nav-link-btn"><i class="fas fa-map-marked-alt"></i> 查看地圖位置</a>
            </div>
            <div class="card" style="background:#FFF5F7; border: 1px solid #FFD1DC;">
                <div class="card-title" style="color:#D14D72;"><i class="fas fa-suitcase-rolling"></i> 必備清單</div>
                <ul style="padding-left: 20px; font-size: 0.9rem; line-height: 1.8;">
                    <li>Suica (Apple Pay 綁定)</li>
                    <li>保養品（A醛精華、蘭蔻小黑瓶）</li>
                    <li>迪士尼預約與 DPA 手法</li>
                </ul>
            </div>
        </div>

        <div id="tab-budget" class="tab-content">
            <div class="total-banner">
                <p style="font-size:0.85rem; opacity:0.9;">總支出 (約台幣)</p>
                <h2 style="font-size:2.4rem; margin:8px 0;">NT$ <span id="total-twd">0</span></h2>
                <p id="current-rate-display" style="font-size:0.75rem; opacity:0.8;">獲取匯率中...</p>
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
                <input type="text" id="itemName" placeholder="品名 (例如: 燒肉)" style="width:100%; padding:14px; margin-bottom:10px; border-radius:15px; border:1px solid #EEE; font-family:'Chenyuluoyan'; outline:none;">
                <input type="number" id="itemPrice" placeholder="日幣金額 ¥" style="width:100%; padding:14px; margin-bottom:15px; border-radius:15px; border:1px solid #EEE; font-family:'Chenyuluoyan'; outline:none;">
                
                <p style="font-size:0.8rem; color:var(--primary); margin:5px 0;">分攤方式</p>
                <div class="split-selector">
                    <button class="selector-btn active split-method-btn" onclick="setSplitMethod('equal', this)">平分</button>
                    <button class="selector-btn split-method-btn" onclick="setSplitMethod('exact', this)">自訂金額</button>
                </div>
                <div id="split-inputs-container"></div>
                <button onclick="addExpense()" style="width:100%; padding:18px; background:var(--primary); color:white; border:none; border-radius:18px; font-family:'Chenyuluoyan'; font-weight:bold; font-size:1.1rem; margin-top:10px; box-shadow: 0 5px 15px rgba(157, 132, 183, 0.3);">記錄一筆 💜</button>
            </div>
            <div id="expense-list-container" style="margin-top: 10px;">
                <div id="expense-list"></div>
            </div>
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
            { time: "下午", title: "抵達成田機場", img: "https://media.istockphoto.com/id/636977274/photo/narita-international-airport-in-japan.jpg?s=612x612&w=0&k=20&c=VM3zabEBSGi4HH5TT0OGJihYkSvK6S-AtgcYJmvuM6A=", desc: "抵達後領取行李，辦理入境。第一件事是儲值 Suica。", buy: "Suica 卡", transport: "Skyliner ➔ 船橋 ➔ 南行德", must: "IT280 航班" },
            { time: "16:00", title: "中目黑 星巴克旗艦店", img: "https://media.timeout.com/images/105974575/image.jpg", desc: "全球最美星巴克之一，櫻花季必訪聖地。", buy: "旗艦店限定馬克杯", transport: "地鐵中目黑站", must: "必喝限定咖啡" },
            { time: "19:00", title: "阿美橫町逛街", img: "https://www.nippon.com/en/ncommon/contents/guide-to-japan/2923498/2923498.jpg", desc: "感受東京下町氛圍，藥妝補貨的好去處。", buy: "二木菓子、藥妝", transport: "JR 上野站", must: "藥妝採買" }
        ]},
        { spots: [
            { time: "10:00", title: "淺草和服體驗", img: "https://upload.wikimedia.org/wikipedia/commons/4/43/Sensoji_2023.jpg", desc: "預約代號 JK2026。穿上傳統和服漫步雷門。", buy: "和服寫真", transport: "淺草站", must: "JK2026" },
            { time: "14:00", title: "壽壽喜園 抹茶", img: "https://resources.matcha-jp.com/resize/720x2000/2026/02/27-259903.webp", desc: "挑戰世界最濃的 No.7 抹茶冰淇淋！", buy: "抹茶甜點", transport: "淺草寺後方", must: "抹茶控必吃" },
            { time: "19:00", title: "澀谷 燒肉黑田", img: "https://media.timeout.com/images/105612723/750/422/image.jpg", desc: "享受高品質和牛燒肉，慰勞一天的辛勞。", buy: "和牛套餐", transport: "澀谷站", must: "厚切牛舌" }
        ]},
        { spots: [{ time: "ALL DAY", title: "東京迪士尼海洋", img: "https://www.japan-experience.com/sites/default/files/images/content_images/tokyodisneysea1.jpg", desc: "全新『夢幻泉鄉』區域正式開啟！", buy: "達菲周邊", transport: "舞濱站轉單軌", must: "搶購 DPA" }]},
        { spots: [{ time: "ALL DAY", title: "東京迪士尼樂園", img: "https://travelingcanucks.com/wp-content/uploads/2019/11/tokyo-disneyland-japan-146.jpg", desc: "走入公主夢幻國度，美女與野獸必排。", buy: "米奇爆米花桶", transport: "舞濱站", must: "公主風日" }]},
        { spots: [
            { time: "11:00", title: "明治神宮", img: "https://substackcdn.com/image/fetch/$s_!CfxW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F263c9f2c-b0f2-461d-8e66-d9b41be42165_3579x2705.jpeg", desc: "在森林大鳥居下祈求好運。", buy: "御守", transport: "原宿站", must: "森呼吸" },
            { time: "19:00", title: "澀谷 SKY 夜景", img: "https://corritrip.jp/jpn/blog/wp/wp-content/uploads/2024/09/pixta_96888508_S-1.jpg", desc: "360度俯瞰東京十字路口最美夜景。", buy: "限定照片", transport: "澀谷站", must: "提前預約" }
        ]},
        { spots: [{ time: "08:30", title: "返程成田機場", img: "https://media.istockphoto.com/id/636977274/photo/narita-international-airport-in-japan.jpg?s=612x612&w=0&k=20&c=VM3zabEBSGi4HH5TT0OGJihYkSvK6S-AtgcYJmvuM6A=", desc: "最後在免稅店進行採購。", buy: "薯條三兄弟、白色戀人", transport: "南行德 ➔ 機場", must: "IT281 航班" }] }
    ];

    let expenses = JSON.parse(localStorage.getItem('tokyo_expenses_v2')) || [];

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
            container.innerHTML = `<p style="text-align:center; font-size:0.8rem; color:#AAA; margin:10px 0;">由 ${users.join('、')} 平均分擔</p>`;
        } else {
            container.innerHTML = users.map(u => `
                <div class="split-input-row"><span>${u} 支付</span><div>¥ <input type="number" class="split-val" data-user="${u}"></div></div>
            `).join('');
        }
    }

    async function fetchRate() {
        try {
            const res = await fetch('https://api.exchangerate-api.com/v4/latest/JPY');
            const data = await res.json(); exchangeRate = data.rates.TWD;
            document.getElementById('current-rate-display').innerText = `即時匯率: 1 JPY ≈ ${exchangeRate.toFixed(4)} TWD`;
        } catch (e) { document.getElementById('current-rate-display').innerText = "匯率抓取失敗，採用基準 0.215"; }
    }

    function addExpense() {
        const name = document.getElementById('itemName').value;
        const totalJPY = parseFloat(document.getElementById('itemPrice').value);
        if(!name || isNaN(totalJPY)) return alert("請填寫品名與金額 ✨");

        let splits = {};
        if (currentSplitMethod === 'equal') {
            users.forEach(u => splits[u] = totalJPY / users.length);
        } else {
            let sumE = 0;
            document.querySelectorAll('.split-val').forEach(el => {
                let v = parseFloat(el.value) || 0;
                splits[el.dataset.user] = v; sumE += v;
            });
            if (Math.abs(sumE - totalJPY) > 1) return alert(`加總應等於總額 ¥${totalJPY} (目前 ¥${sumE})`);
        }
        
        expenses.push({ id: Date.now(), name, totalJPY, payer: currentPayer, splits });
        localStorage.setItem('tokyo_expenses_v2', JSON.stringify(expenses));
        document.getElementById('itemName').value = ''; 
        document.getElementById('itemPrice').value = '';
        updateBudgetUI();
    }

    function deleteExpense(id) {
        if(confirm("確定要刪除這筆紀錄嗎？")) {
            expenses = expenses.filter(e => e.id !== id);
            localStorage.setItem('tokyo_expenses_v2', JSON.stringify(expenses));
            updateBudgetUI();
        }
    }

    function updateBudgetUI() {
        const list = document.getElementById('expense-list');
        const netBalances = { '蒂': 0, '丞': 0, '頻': 0 };

        list.innerHTML = expenses.length ? expenses.map(e => {
            netBalances[e.payer] += e.totalJPY;
            users.forEach(u => netBalances[u] -= e.splits[u]);
            return `<div class="expense-item"><div><b>${e.name}</b><br><small style="color:#AAA;">${e.payer} 付 ¥${e.totalJPY.toLocaleString()}</small></div><button onclick="deleteExpense(${e.id})" class="del-btn"><i class="fas fa-trash-alt"></i></button></div>`;
        }).reverse().join('') : '<p style="text-align:center; color:#CCC; margin-top:20px;">尚無消費紀錄 🛍️</p>';

        const totalJPY = expenses.reduce((s, e) => s + e.totalJPY, 0);
        document.getElementById('total-twd').innerText = Math.round(totalJPY * exchangeRate).toLocaleString();

        let debtors = users.map(u => ({ name: u, bal: netBalances[u] })).filter(u => u.bal < -0.1).sort((a,b) => a.bal - b.bal);
        let creditors = users.map(u => ({ name: u, bal: netBalances[u] })).filter(u => u.bal > 0.1).sort((a,b) => b.bal - a.bal);
        let inst = [];
        let d = 0, c = 0;
        while (d < debtors.length && c < creditors.length) {
            let amt = Math.min(Math.abs(debtors[d].bal), creditors[c].bal);
            inst.push(`<div><b>${debtors[d].name}</b> 給 <b>${creditors[c].name}</b> <span style="color:#D14D72;">¥ ${Math.round(amt).toLocaleString()}</span></div>`);
            debtors[d].bal += amt; creditors[c].bal -= amt;
            if (Math.abs(debtors[d].bal) < 0.1) d++; if (Math.abs(creditors[c].bal) < 0.1) c++;
        }
        document.getElementById('settlement-result').innerHTML = inst.length ? inst.join('') : '<div style="color:#2E7D32;">帳目平衡，目前誰也不欠誰 ✨</div>';
    }

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
            <div style="font-size:1.4rem; color:var(--secondary); margin-bottom:12px; border-bottom:2px dashed #FFD1DC; padding-bottom:8px; font-weight:bold;">${s.title}</div>
            <div style="font-weight:bold; color:var(--primary); margin-top:10px;">📖 行程介紹</div><div style="font-size:0.95rem; line-height:1.6; margin-bottom:15px;">${s.desc}</div>
            <div style="font-weight:bold; color:var(--primary);">🛍️ 推薦購買</div><div style="font-size:0.95rem; margin-bottom:15px;">${s.buy}</div>
            <div style="font-weight:bold; color:var(--primary);">🚆 交通方式</div><div style="font-size:0.9rem; background:#F8F6FF; padding:12px; border-radius:15px; color:#5D4E7A; line-height:1.5;">${s.transport}</div>
            <a href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(s.title)}" target="_blank" class="nav-link-btn"><i class="fas fa-location-arrow"></i> Google 地圖導航</a>`;
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
