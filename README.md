<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>2026 東京夢幻之旅 💜</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <style>
        /* 核心修復：防止 Vue 未掛載前內容空白 */
        [v-cloak] { display: none !important; }

        /* 辰宇落雁體 載入 */
        @font-face {
            font-family: 'Chenyuluoyan';
            src: url('https://cdn.jsdelivr.net/gh/chenyu-shuo/Chenyuluoyan-Thin@main/Chenyuluoyan-Thin.ttf') format('truetype');
            font-display: swap;
        }

        :root {
            --primary-purple: #9D84B7;
            --soft-purple: #F8F6FF;
            --deep-purple: #6B4E8D;
        }

        body { 
            font-family: 'Chenyuluoyan', 'Noto Sans TC', sans-serif; 
            background-color: var(--soft-purple);
            color: #4A4A4A;
            margin: 0;
            padding: 0;
            overflow-x: hidden;
        }

        /* 橫向日期選單滾動 */
        .day-scroller {
            display: flex;
            overflow-x: auto;
            gap: 12px;
            padding-bottom: 10px;
            -webkit-overflow-scrolling: touch;
            scrollbar-width: none;
        }
        .day-scroller::-webkit-scrollbar { display: none; }

        .day-item {
            flex: 0 0 65px;
            height: 80px;
            border-radius: 24px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            background: white;
            border: 1px solid rgba(157, 132, 183, 0.1);
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .active-day {
            background-color: var(--primary-purple) !important;
            color: white !important;
            transform: scale(1.05) translateY(-2px);
            box-shadow: 0 8px 15px rgba(157, 132, 183, 0.3);
        }

        /* 日系感卡片 */
        .glass-card {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(10px);
            border-radius: 28px;
            border: 1px solid rgba(255, 255, 255, 0.5);
            box-shadow: 0 4px 15px rgba(0,0,0,0.03);
        }

        /* 底部導覽列 */
        .bottom-nav {
            background: rgba(255, 255, 255, 0.8);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            border-top: 1px solid rgba(255, 255, 255, 0.5);
        }
    </style>
</head>
<body>

    <div id="app" v-cloak class="max-w-md mx-auto min-h-screen pb-32">
        
        <header class="pt-10 pb-6 px-6 sticky top-0 bg-[#F8F6FF]/95 z-40">
            <div class="flex justify-between items-end mb-6">
                <div>
                    <h1 class="text-3xl font-bold text-[#6B4E8D]">Tokyo Trip</h1>
                    <p class="text-sm text-purple-400">2026 夢幻旅遊手帳 ✨</p>
                </div>
                <div class="text-right text-[#6B4E8D]">
                    <span class="text-[10px] opacity-60">今日匯率</span>
                    <p class="text-sm font-bold">1 JPY = {{ exchangeRate }} TWD</p>
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

        <main class="px-6">
            
            <div v-if="currentTab === 'itinerary'" class="space-y-6">
                <div class="glass-card p-4 flex items-center justify-between border-l-4 border-purple-300">
                    <div class="flex items-center space-x-3">
                        <span class="text-3xl">☀️</span>
                        <div>
                            <p class="text-xs text-gray-400">{{ days[selectedDay].location }}</p>
                            <p class="font-bold">21°C ‧ 晴時多雲</p>
                        </div>
                    </div>
                    <div class="text-[10px] text-purple-400 italic">"適合散步的好天氣"</div>
                </div>

                <div v-for="(item, idx) in itineraries[selectedDay]" :key="idx" 
                     class="glass-card p-6 relative">
                    <div class="flex gap-4">
                        <div class="text-sm font-bold text-purple-400 pt-1">{{ item.time }}</div>
                        <div class="flex-1">
                            <div class="flex items-center gap-2 mb-2">
                                <span class="w-1.5 h-1.5 rounded-full bg-purple-300"></span>
                                <h3 class="text-lg font-bold text-gray-700">{{ item.title }}</h3>
                            </div>
                            <p class="text-sm text-gray-500 leading-relaxed">{{ item.note }}</p>
                            
                            <div class="mt-4">
                                <a :href="'https://www.google.com/maps/search/?api=1&query=' + encodeURIComponent(item.title)" 
                                   target="_blank" 
                                   class="inline-flex items-center text-xs bg-purple-50 text-purple-600 px-4 py-2 rounded-full font-bold shadow-sm">
                                    <i class="fas fa-location-arrow mr-2"></i> 導航
                                </a>
                            </div>
                        </div>
                    </div>
                    <div v-if="item.must" class="absolute -top-2 -right-1 bg-[#FFD1DC] text-[#D14D72] text-[10px] px-3 py-1 rounded-full border border-white shadow-sm">
                        # {{ item.must }}
                    </div>
                </div>
            </div>

            <div v-if="currentTab === 'accounting'" class="space-y-6">
                <div class="glass-card p-8 text-center bg-white">
                    <p class="text-gray-400 text-sm mb-1">今日消費統計</p>
                    <div class="text-4xl font-bold text-[#6B4E8D]">
                        <span class="text-xl mr-1 font-normal">NT$</span>{{ totalExpenseTWD }}
                    </div>
                </div>

                <div class="glass-card p-6 bg-white/60">
                    <div class="space-y-4">
                        <input v-model="newExpense.name" type="text" placeholder="品名 (例如: 燒肉)" class="w-full bg-transparent border-b border-purple-100 p-2 text-sm focus:outline-none">
                        <div class="flex items-center gap-4">
                            <input v-model.number="newExpense.amount" type="number" placeholder="日幣金額 (JPY)" class="flex-1 bg-transparent border-b border-purple-100 p-2 text-sm focus:outline-none">
                            <button @click="addExpense" class="bg-[#9D84B7] text-white px-8 py-3 rounded-full shadow-lg active:scale-95 transition-all text-sm font-bold">
                                記帳
                            </button>
                        </div>
                    </div>
                </div>

                <div class="space-y-2">
                    <div v-for="(exp, idx) in expenses[selectedDay]" :key="idx" class="flex justify-between items-center p-3 border-b border-purple-50">
                        <span class="text-sm">{{ exp.name }}</span>
                        <div class="text-right">
                            <span class="font-bold text-purple-600 text-sm">¥ {{ exp.amount }}</span>
                            <p class="text-[10px] text-gray-400">≈ NT$ {{ Math.round(exp.amount * exchangeRate) }}</p>
                        </div>
                    </div>
                </div>
            </div>

            <div v-if="currentTab === 'map' || currentTab === 'info'" class="h-64 flex flex-col items-center justify-center text-purple-300">
                <i class="fas fa-magic text-5xl mb-4 opacity-20"></i>
                <p class="text-sm italic">旅程中的驚喜開發中 ✨</p>
            </div>

        </main>

        <nav class="fixed bottom-8 left-6 right-6 max-w-md mx-auto h-16 bottom-nav rounded-full shadow-2xl border border-white flex justify-around items-center px-4 z-50">
            <button @click="currentTab = 'itinerary'" :class="currentTab === 'itinerary' ? 'text-[#6B4E8D]' : 'text-gray-300'">
                <i class="fas fa-feather-alt text-xl"></i>
            </button>
            <button @click="currentTab = 'accounting'" :class="currentTab === 'accounting' ? 'text-[#6B4E8D]' : 'text-gray-300'">
                <i class="fas fa-coins text-xl"></i>
            </button>
            <button @click="currentTab = 'map'" :class="currentTab === 'map' ? 'text-[#6B4E8D]' : 'text-gray-300'">
                <i class="fas fa-map-pin text-xl"></i>
            </button>
            <button @click="currentTab = 'info'" :class="currentTab === 'info' ? 'text-[#6B4E8D]' : 'text-gray-300'">
                <i class="fas fa-heart text-xl"></i>
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
                        { time: '12:10', title: '成田機場抵達', note: '領取行李，前往 D・レガーロ 飯店辦理 Check-in。', must: '航班 IT280' },
                        { time: '15:30', title: '中目黑星巴克', note: '在目黑川畔喝杯咖啡，逛逛 La vita 小義大利。', must: '必喝限定' },
                        { time: '19:00', title: '上野阿美橫町', note: '藥妝與零食大採買，吃串燒感受氣氛。', must: '採買重點' }
                    ],
                    [
                        { time: '09:30', title: '淺草和服體驗', note: '預約：雅，換上和服走訪雷門與淺草寺。', must: '預約 JK2026' },
                        { time: '14:00', title: 'Suzukien 抹茶', note: '挑戰等級 7 的最濃郁抹茶冰淇淋。', must: '必吃甜點' },
                        { time: '18:30', title: '燒肉黑田 澀谷', note: '奢華和牛晚餐，開啟澀谷深夜漫步。', must: '必點牛舌' }
                    ],
                    [{ time: '08:30', title: '東京迪士尼海洋', note: '整天待在夢幻海洋，享受達菲熊的世界。', must: 'DPA 攻略' }],
                    [{ time: '08:30', title: '東京迪士尼樂園', note: 'Land 經典之旅，晚上的遊行不可錯過。', must: '美女與野獸' }],
                    [{ time: '12:00', title: '敘敘苑午餐', note: '高空景觀燒肉，一邊看東京美景一邊用餐。', must: '景觀首選' }],
                    [{ time: '09:00', title: '前往機場', note: '回程飛機較早，預留時間在免稅店最後採購。', must: '航班 IT281' }]
                ]);

                const newExpense = ref({ name: '', amount: null });
                const expenses = ref([[], [], [], [], [], []]);

                const totalExpenseTWD = computed(() => {
                    const currentExpenses = expenses.value[selectedDay.value] || [];
                    const totalJPY = currentExpenses.reduce((sum, item) => sum + (Number(item.amount) || 0), 0);
                    return Math.round(totalJPY * exchangeRate.value).toLocaleString();
                });

                const addExpense = () => {
                    if (!newExpense.value.name || !newExpense.value.amount) {
                        alert("別忘了寫出品名和金額唷！");
                        return;
                    }
                    expenses.value[selectedDay.value].push({
                        name: newExpense.value.name,
                        amount: newExpense.value.amount
                    });
                    newExpense.value = { name: '', amount: null };
                };

                return {
                    currentTab, selectedDay, days, itineraries, 
                    exchangeRate, expenses, newExpense, 
                    totalExpenseTWD, addExpense
                };
            }
        }).mount('#app');
    </script>
</body>
</html>
