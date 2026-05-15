[gemini-code-1778859697217.html](https://github.com/user-attachments/files/27807444/gemini-code-1778859697217.html)
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>2026 東京夢幻之旅 💜</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <style>
        /* 引入 辰宇落雁體 */
        @font-face {
            font-family: 'Chenyuluoyan';
            src: url('https://cdn.jsdelivr.net/gh/chenyu-shuo/Chenyuluoyan-Thin@main/Chenyuluoyan-Thin.ttf') format('truetype');
            font-display: swap;
        }

        :root {
            --primary-purple: #9D84B7;
            --soft-purple: #F8F6FF;
            --lavender-deep: #6B4E8D;
            --accent-pink: #FFD1DC;
        }

        body { 
            font-family: 'Chenyuluoyan', 'Noto Sans TC', sans-serif; 
            background-color: var(--soft-purple);
            color: #4A4A4A;
            margin: 0; padding: 0;
            letter-spacing: 0.03em;
        }

        /* 防止 Vue 載入前的閃爍 */
        [v-cloak] { display: none; }

        /* 日系極簡卡片設計 */
        .glass-card {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(10px);
            border-radius: 24px;
            border: 1px solid rgba(255, 255, 255, 0.5);
            box-shadow: 0 4px 15px rgba(157, 132, 183, 0.05);
        }

        /* 橫向日期捲動 */
        .day-scroller {
            display: flex;
            overflow-x: auto;
            gap: 12px;
            padding: 10px 0;
            scrollbar-width: none;
            -webkit-overflow-scrolling: touch;
        }
        .day-scroller::-webkit-scrollbar { display: none; }

        .day-item {
            flex: 0 0 65px;
            height: 80px;
            border-radius: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            background: white;
            border: 1px solid rgba(157, 132, 183, 0.1);
            transition: all 0.3s ease;
        }

        .active-day {
            background-color: var(--primary-purple) !important;
            color: white !important;
            transform: translateY(-2px);
            box-shadow: 0 5px 12px rgba(157, 132, 183, 0.3);
        }

        .highlight-tag {
            background: var(--accent-pink);
            color: #D14D72;
            padding: 2px 8px;
            border-radius: 10px;
            font-size: 0.7rem;
            font-weight: bold;
        }

        /* 底部導覽列 */
        .nav-bar {
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(20px);
            border-top: 1px solid rgba(157, 132, 183, 0.1);
        }
    </style>
</head>
<body>

    <div id="app" v-cloak class="max-w-md mx-auto min-h-screen pb-32">
        
        <header class="pt-10 px-6 sticky top-0 bg-[#F8F6FF]/95 z-40">
            <div class="flex justify-between items-center mb-6">
                <div>
                    <h1 class="text-3xl font-bold text-[#6B4E8D]">東京夢幻之旅 💜</h1>
                    <p class="text-xs text-purple-400">2026 Tokyo Handbook</p>
                </div>
                <div class="text-right text-[#6B4E8D]">
                    <span class="text-[10px] opacity-60">1 JPY ≈</span>
                    <p class="text-sm font-bold">{{ exchangeRate }} TWD</p>
                </div>
            </div>
            
            <div class="day-scroller">
                <div v-for="(day, index) in days" :key="index" 
                     @click="selectedDay = index"
                     class="day-item cursor-pointer"
                     :class="selectedDay === index ? 'active-day' : 'text-purple-300'">
                    <span class="text-[10px]">{{ day.month }}/{{ day.date }}</span>
                    <span class="text-xl font-bold">D{{ index + 1 }}</span>
                </div>
            </div>
        </header>

        <main class="px-6 mt-4">
            
            <div v-if="currentTab === 'itinerary'" class="space-y-6">
                <div class="glass-card p-4 flex items-center justify-between">
                    <div class="flex items-center space-x-3">
                        <span class="text-3xl">☁️</span>
                        <div>
                            <p class="text-xs text-gray-400">{{ days[selectedDay].location }}</p>
                            <p class="font-bold text-lg">22°C ‧ 晴時多雲</p>
                        </div>
                    </div>
                    <div class="text-[10px] text-purple-400 italic font-bold"># 今日也要美美的</div>
                </div>

                <div v-for="(item, idx) in itineraries[selectedDay]" :key="idx" 
                     class="glass-card p-6 relative">
                    <div class="flex gap-4">
                        <div class="text-sm font-bold text-purple-400 pt-1 w-12">{{ item.time }}</div>
                        <div class="flex-1">
                            <div class="flex items-center gap-2 mb-2">
                                <span class="w-1.5 h-1.5 rounded-full bg-purple-300"></span>
                                <h3 class="text-lg font-bold text-gray-700">{{ item.title }}</h3>
                            </div>
                            
                            <div class="flex flex-wrap gap-1 mb-2">
                                <span v-if="item.must" class="highlight-tag"># {{ item.must }}</span>
                            </div>

                            <p class="text-sm text-gray-500 leading-relaxed">{{ item.note }}</p>
                            
                            <div class="mt-4 flex gap-2">
                                <a :href="'https://www.google.com/maps/search/?api=1&query=' + encodeURIComponent(item.title)" 
                                   target="_blank" 
                                   class="inline-flex items-center text-xs bg-purple-50 text-purple-600 px-4 py-2 rounded-full font-bold shadow-sm">
                                    <i class="fas fa-location-arrow mr-2"></i> 導航前往
                                </a>
                                <button @click="removeItem(idx)" class="text-[10px] text-gray-300">刪除</button>
                            </div>
                        </div>
                    </div>
                </div>

                <button @click="addItem" class="w-full py-4 border-2 border-dashed border-purple-200 rounded-3xl text-purple-300 text-sm">
                    + 新增行程 (手帳感)
                </button>
            </div>

            <div v-if="currentTab === 'accounting'" class="space-y-6">
                <div class="glass-card p-8 text-center">
                    <p class="text-gray-400 text-sm mb-1">今日支出統計</p>
                    <div class="text-4xl font-bold text-[#6B4E8D]">
                        <span class="text-xl mr-1 font-normal">NT$</span>{{ totalExpenseTWD }}
                    </div>
                </div>

                <div class="glass-card p-6">
                    <div class="space-y-4">
                        <input v-model="newExpense.name" type="text" placeholder="買了什麼好東西呢？" class="w-full bg-transparent border-b border-purple-100 p-2 text-sm focus:outline-none">
                        <div class="flex items-center gap-4">
                            <input v-model.number="newExpense.amount" type="number" placeholder="日幣金額" class="flex-1 bg-transparent border-b border-purple-100 p-2 text-sm focus:outline-none">
                            <button @click="addExpense" class="bg-[#9D84B7] text-white px-8 py-3 rounded-full shadow-lg text-sm font-bold">記帳</button>
                        </div>
                    </div>
                </div>

                <div class="space-y-2">
                    <div v-for="(exp, i) in expenses[selectedDay]" :key="i" class="flex justify-between items-center p-3 border-b border-purple-50">
                        <span class="text-sm">{{ exp.name }}</span>
                        <div class="text-right">
                            <span class="font-bold text-purple-600 text-sm">¥ {{ exp.amount }}</span>
                            <p class="text-[10px] text-gray-400">≈ NT$ {{ Math.round(exp.amount * exchangeRate) }}</p>
                        </div>
                    </div>
                </div>
            </div>

            <div v-if="currentTab === 'info'" class="space-y-4">
                <div class="glass-card p-5 border-l-4 border-purple-300">
                    <h3 class="font-bold text-purple-900 mb-2">✈️ 航班資訊</h3>
                    <p class="text-xs">去程：10/26 IT280 (08:00 KHH ➔ 12:10 NRT)</p>
                    <p class="text-xs">回程：10/31 IT281 (11:25 NRT ➔ 15:05 KHH)</p>
                </div>
                <div class="glass-card p-5 border-l-4 border-pink-200">
                    <h3 class="font-bold text-purple-900 mb-2">🏨 住宿地點</h3>
                    <p class="text-xs font-bold">D・レガーロ</p>
                    <p class="text-[10px] text-gray-500">〒272-0138 千葉県市川市南行徳２丁目２１</p>
                </div>
                <div class="glass-card p-5 border-l-4 border-red-300 bg-red-50/20">
                    <h3 class="font-bold text-red-600 mb-2">🆘 緊急電話</h3>
                    <p class="text-xs">警察：110 / 急救：119</p>
                </div>
            </div>
        </main>

        <nav class="fixed bottom-8 left-6 right-6 h-16 nav-bar rounded-full shadow-2xl flex justify-around items-center px-4 z-50">
            <button @click="currentTab = 'itinerary'" :class="currentTab === 'itinerary' ? 'text-[#6B4E8D]' : 'text-gray-300'">
                <i class="fas fa-feather-alt text-xl"></i>
            </button>
            <button @click="currentTab = 'accounting'" :class="currentTab === 'accounting' ? 'text-[#6B4E8D]' : 'text-gray-300'">
                <i class="fas fa-coins text-xl"></i>
            </button>
            <button @click="currentTab = 'info'" :class="currentTab === 'info' ? 'text-[#6B4E8D]' : 'text-gray-300'">
                <i class="fas fa-info-circle text-xl"></i>
            </button>
        </nav>
    </div>

    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script>
        const { createApp, ref, computed } = Vue;

        createApp({
            setup() {
                const currentTab = ref('itinerary');
                const selectedDay = ref(0);
                const exchangeRate = ref(0.21);
                
                const days = [
                    { month: '10', date: '26', location: '中目黑' },
                    { month: '10', date: '27', location: '淺草/澀谷' },
                    { month: '10', date: '28', location: '迪士尼海洋' },
                    { month: '10', date: '29', location: '迪士尼樂園' },
                    { month: '10', date: '30', location: '原宿/新宿' },
                    { month: '10', date: '31', location: '成田機場' }
                ];

                const itineraries = ref([
                    [
                        { time: '12:10', title: '成田機場抵達', note: '領取行李，前往 D・レガーロ 飯店放行李。', must: 'IT280' },
                        { time: '15:30', title: '中目黑星巴克', note: '星巴克山手通店、La vita 小義大利，感受氣氛。', must: '必喝限定' },
                        { time: '19:00', title: '上野阿美橫町', note: '藥妝與零食大採買。', must: '必買清單' }
                    ],
                    [
                        { time: '09:30', title: '淺草和服體驗', note: '預約：雅 和服店，拍下雷門最美時刻。', must: '預約號: JK2026' },
                        { time: '14:00', title: 'Suzukien 抹茶', note: '挑戰最強第 7 級抹茶冰淇淋。', must: '必吃美食' },
                        { time: '18:30', title: '燒肉黑田 澀谷', note: '奢華和牛晚餐。', must: '必點牛舌' }
                    ],
                    [{ time: '08:30', title: '東京迪士尼海洋', note: '達菲熊！我們來了！', must: 'DPA 攻略' }],
                    [{ time: '08:30', title: '東京迪士尼樂園', note: 'Land 公主夢幻之旅，看美女與野獸。', must: '公主夢' }],
                    [{ time: '12:00', title: '敘敘苑午餐', note: '高空燒肉景觀，看整座東京。', must: '景觀餐廳' }],
                    [{ time: '09:00', title: '回程機場', note: '09:25 前抵達櫃台報到，再見東京！', must: 'IT281' }]
                ]);

                const newExpense = ref({ name: '', amount: null });
                const expenses = ref([[], [], [], [], [], []]);

                const totalExpenseTWD = computed(() => {
                    const totalJPY = expenses.value[selectedDay.value].reduce((sum, item) => sum + (Number(item.amount) || 0), 0);
                    return Math.round(totalJPY * exchangeRate.value).toLocaleString();
                });

                const addExpense = () => {
                    if (!newExpense.value.name || !newExpense.value.amount) return;
                    expenses.value[selectedDay.value].push({ ...newExpense.value });
                    newExpense.value = { name: '', amount: null };
                };

                const removeItem = (idx) => itineraries.value[selectedDay.value].splice(idx, 1);

                const addItem = () => {
                    const t = prompt("景點名稱？");
                    if(t) itineraries.value[selectedDay.value].push({ time: '10:00', title: t, note: '新增備註', must: '' });
                };

                return {
                    currentTab, selectedDay, days, itineraries, 
                    exchangeRate, expenses, newExpense, 
                    totalExpenseTWD, addExpense, removeItem, addItem
                };
            }
        }).mount('#app');
    </script>
</body>
</html>
