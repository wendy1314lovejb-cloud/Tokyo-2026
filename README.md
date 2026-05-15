<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>2026 東京夢幻之旅 💜</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <style>
        /* 辰宇落雁體載入 */
        @font-face {
            font-family: 'Chenyuluoyan';
            src: url('https://cdn.jsdelivr.net/gh/chenyu-shuo/Chenyuluoyan-Thin@main/Chenyuluoyan-Thin.ttf') format('truetype');
            font-display: swap;
        }

        body { 
            font-family: 'Chenyuluoyan', 'Noto Sans TC', sans-serif; 
            background-color: #F8F6FF;
            color: #4A4A4A;
            margin: 0; padding: 0;
        }

        /* 日系感卡片 */
        .glass-card {
            background: rgba(255, 255, 255, 0.9);
            border-radius: 24px;
            border: 1px solid rgba(157, 132, 183, 0.1);
            box-shadow: 0 4px 15px rgba(157, 132, 183, 0.05);
        }

        .active-day {
            background-color: #9D84B7 !important;
            color: white !important;
        }
    </style>
</head>
<body>

    <div id="vue-error-check" style="text-align:center; padding:50px; color:#9D84B7;">
        <i class="fas fa-spinner fa-spin text-3xl mb-4"></i>
        <p>正在載入妳的夢幻手帳...💜</p>
        <p style="font-size:12px; color:#ccc; mt-2">如果此畫面停留太久，請確認網路連線或嘗試重新整理</p>
    </div>

    <div id="app" style="display:none;">
        <header class="pt-10 px-6 sticky top-0 bg-[#F8F6FF]/95 z-40">
            <div class="flex justify-between items-center mb-6">
                <div>
                    <h1 class="text-3xl font-bold text-[#6B4E8D]">東京之旅 💜</h1>
                    <p class="text-xs text-purple-400">2026 Tokyo Memories</p>
                </div>
                <div class="text-right text-[#6B4E8D] bg-white px-3 py-1 rounded-xl border border-purple-50">
                    <span class="text-[10px] opacity-60">1 JPY ≈</span>
                    <p class="text-sm font-bold">{{ exchangeRate }} TWD</p>
                </div>
            </div>
            
            <div class="flex overflow-x-auto gap-3 pb-4 no-scrollbar">
                <div v-for="(day, index) in days" :key="index" 
                     @click="selectedDay = index"
                     class="flex-shrink-0 w-16 h-20 rounded-2xl flex flex-col items-center justify-center transition-all bg-white border border-purple-100"
                     :class="selectedDay === index ? 'active-day' : 'text-purple-300'">
                    <span class="text-[10px]">{{ day.month }}/{{ day.date }}</span>
                    <span class="text-xl font-bold">D{{ index + 1 }}</span>
                </div>
            </div>
        </header>

        <main class="px-6 mt-4">
            <div v-if="currentTab === 'itinerary'" class="space-y-6">
                <div class="glass-card p-4 flex items-center justify-between border-l-4 border-purple-200">
                    <div class="flex items-center space-x-3">
                        <span class="text-3xl">☁️</span>
                        <div>
                            <p class="text-xs text-gray-400">{{ days[selectedDay].location }}</p>
                            <p class="font-bold text-lg">22°C ‧ 舒適</p>
                        </div>
                    </div>
                    <div class="text-[10px] text-purple-300 italic font-bold"># 今天也要開心唷</div>
                </div>

                <div v-for="(item, idx) in itineraries[selectedDay]" :key="idx" class="glass-card p-5 relative">
                    <div class="flex gap-4">
                        <div class="text-sm font-bold text-purple-400 w-10">{{ item.time }}</div>
                        <div class="flex-1">
                            <h3 class="text-lg font-bold text-gray-700 mb-1">{{ item.title }}</h3>
                            <p class="text-sm text-gray-500 leading-relaxed">{{ item.note }}</p>
                            <div class="mt-4">
                                <a :href="'https://www.google.com/maps/search/?api=1&query=' + encodeURIComponent(item.title)" 
                                   target="_blank" class="inline-flex items-center text-xs bg-purple-50 text-purple-600 px-4 py-2 rounded-full font-bold">
                                    <i class="fas fa-map-marker-alt mr-2"></i> 導航
                                </a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div v-if="currentTab === 'accounting'" class="space-y-6">
                <div class="glass-card p-8 text-center">
                    <p class="text-gray-400 text-sm mb-1">今日支出 (NT$)</p>
                    <div class="text-4xl font-bold text-[#6B4E8D]">{{ totalExpenseTWD }}</div>
                </div>
                <div class="glass-card p-6">
                    <input v-model="newExpense.name" placeholder="消費項目" class="w-full bg-transparent border-b border-purple-100 p-2 mb-4 text-sm focus:outline-none">
                    <div class="flex gap-2">
                        <input v-model.number="newExpense.amount" type="number" placeholder="日幣金額" class="flex-1 bg-transparent border-b border-purple-100 p-2 text-sm focus:outline-none">
                        <button @click="addExpense" class="bg-[#9D84B7] text-white px-6 py-2 rounded-full text-sm">記帳</button>
                    </div>
                </div>
                <div v-for="(exp, i) in expenses[selectedDay]" :key="i" class="flex justify-between p-3 border-b border-purple-50">
                    <span class="text-sm">{{ exp.name }}</span>
                    <span class="text-sm font-bold text-purple-600">¥ {{ exp.amount }}</span>
                </div>
            </div>
        </main>

        <nav class="fixed bottom-8 left-6 right-6 h-16 bg-white/90 backdrop-blur-xl rounded-full shadow-2xl border border-white flex justify-around items-center z-50">
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

    <script src="https://cdn.jsdelivr.net/npm/vue@3/dist/vue.global.js"></script>
    <script>
        // 確認 Vue 是否載入
        if (typeof Vue === 'undefined') {
            document.getElementById('vue-error-check').innerHTML = '<p style="color:red;">網路連線錯誤：無法載入 Vue 框架，請重新整理網頁。</p>';
        } else {
            const { createApp, ref, computed, onMounted } = Vue;
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
                            { time: '12:10', title: '成田機場抵達', note: '航班 IT280，前往飯店放行李。' },
                            { time: '15:30', title: '中目黑散策', note: '星巴克山手通店、La vita 小義大利。' },
                            { time: '19:00', title: '上野阿美橫町', note: '藥妝採買與零食。' }
                        ],
                        [
                            { time: '09:30', title: '淺草和服體驗', note: '穿上美美的和服逛淺草寺。' },
                            { time: '14:00', title: 'Suzukien 抹茶', note: '挑戰最強第 7 級抹茶冰淇淋。' },
                            { time: '18:30', title: '燒肉黑田 澀谷', note: '慶祝美好的第二晚。' }
                        ],
                        [{ time: '08:30', title: '東京迪士尼海洋', note: '達菲熊！' }],
                        [{ time: '08:30', title: '東京迪士尼樂園', note: '公主夢幻之旅。' }],
                        [{ time: '12:00', title: '敘敘苑午餐', note: '高空燒肉美景。' }],
                        [{ time: '09:00', title: '前往機場', note: '再見東京！' }]
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

                    onMounted(() => {
                        // 啟動 App 並隱藏載入提示
                        document.getElementById('vue-error-check').style.display = 'none';
                        document.getElementById('app').style.display = 'block';
                    });

                    return { currentTab, selectedDay, days, itineraries, exchangeRate, expenses, newExpense, totalExpenseTWD, addExpense };
                }
            }).mount('#app');
        }
    </script>
</body>
</html>
