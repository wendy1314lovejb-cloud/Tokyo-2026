<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>2026 Tokyo Trip 💜</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700&display=swap');
        
        :root {
            --lavender-light: #F3F0FF;
            --lavender-main: #D8B4FE;
            --lavender-dark: #8B5CF6;
        }

        body {
            font-family: 'Noto Sans TC', sans-serif;
            /* 使用你上傳的格紋背景風格 */
            background-color: var(--lavender-light);
            background-image: radial-gradient(#D1D5DB 0.5px, transparent 0.5px);
            background-size: 20px 20px;
            margin: 0;
            padding: 0;
            color: #4B5563;
        }

        /* 隱藏滾動條但保有功能 */
        .hide-scrollbar::-webkit-scrollbar { display: none; }
        
        /* 模擬 App 的底層容器 */
        .app-container {
            max-width: 500px;
            margin: 0 auto;
            min-height: 100vh;
            background: rgba(255, 255, 255, 0.6);
            backdrop-filter: blur(5px);
            position: relative;
            padding-bottom: 90px;
        }

        /* 日系極簡卡片 */
        .itinerary-card {
            border: 2px solid white;
            box-shadow: 0 4px 15px rgba(216, 180, 254, 0.2);
            border-radius: 24px;
            background: white;
            transition: all 0.3s ease;
        }

        /* 橫向日期選單選中效果 */
        .day-active {
            background-color: var(--lavender-dark) !important;
            color: white !important;
            transform: translateY(-5px);
            box-shadow: 0 4px 10px rgba(139, 92, 246, 0.4);
        }

        /* 裝飾性小插圖位置 */
        .sticker {
            position: absolute;
            pointer-events: none;
            z-index: 10;
            width: 60px;
            opacity: 0.8;
        }
    </style>
</head>
<body>

<div id="app" class="app-container">
    <img src="https://img.icons8.com/color/96/cinnamoroll.png" class="sticker top-4 right-4">
    
    <header class="p-6 pb-2">
        <div class="flex justify-between items-end mb-4">
            <div>
                <h1 class="text-2xl font-bold text-purple-600">2026 東京之旅</h1>
                <p class="text-xs text-purple-400">Dreamy Trip in Tokyo 💜</p>
            </div>
            <div class="bg-white px-3 py-1 rounded-full text-[10px] shadow-sm border border-purple-100 text-purple-600">
                1 JPY = {{ exchangeRate }} TWD
            </div>
        </div>

        <div class="flex overflow-x-auto hide-scrollbar space-x-3 py-2">
            <div v-for="(day, index) in days" :key="index" 
                 @click="selectedDayIndex = index"
                 :class="['flex-shrink-0 w-16 h-20 rounded-2xl flex flex-col items-center justify-center bg-white border border-purple-50 transition-all cursor-pointer', 
                          selectedDayIndex === index ? 'day-active' : 'text-purple-300']">
                <span class="text-[10px] uppercase">{{ day.weekday }}</span>
                <span class="text-lg font-bold">{{ index + 1 }}</span>
                <span class="text-[10px]">{{ day.date }}</span>
            </div>
        </div>
    </header>

    <main class="px-6">
        
        <div v-if="currentTab === 'itinerary'" class="space-y-4 animate-fadeIn">
            <div class="bg-gradient-to-br from-purple-100 to-white p-4 rounded-3xl flex justify-between items-center border border-white">
                <div>
                    <p class="text-xs text-purple-400 font-medium">{{ days[selectedDayIndex].location }}</p>
                    <h2 class="text-xl font-bold text-purple-700">{{ mockWeather.temp }}°C {{ mockWeather.desc }}</h2>
                </div>
                <div class="text-3xl">☁️</div>
            </div>

            <div v-for="(item, idx) in itineraries[selectedDayIndex]" :key="idx" class="itinerary-card p-4 relative overflow-hidden">
                <div class="flex">
                    <div class="w-14 flex flex-col items-center border-r border-purple-50 mr-4">
                        <span class="text-sm font-bold text-purple-600">{{ item.time }}</span>
                        <div class="w-0.5 h-full bg-purple-50 my-2"></div>
                    </div>
                    <div class="flex-1">
                        <div class="flex justify-between items-start">
                            <div>
                                <span class="bg-purple-100 text-purple-500 text-[10px] px-2 py-0.5 rounded-full">{{ item.type }}</span>
                                <h3 class="font-bold text-gray-700 mt-1">{{ item.title }}</h3>
                            </div>
                            <button @click="deleteItem(idx)" class="text-gray-300 hover:text-red-400"><i class="fas fa-times text-xs"></i></button>
                        </div>
                        <p class="text-xs text-gray-400 mt-2">{{ item.note }}</p>
                        
                        <div class="mt-4 flex justify-between items-center">
                            <a :href="'https://www.google.com/maps/search/?api=1&query=' + item.title" target="_blank"
                               class="text-[10px] bg-purple-500 text-white px-4 py-2 rounded-full shadow-sm active:scale-95 transition-transform">
                                <i class="fas fa-location-arrow mr-1"></i> 開啟導航
                            </a>
                            <span v-if="item.must" class="text-[10px] text-pink-400 font-bold">✨ {{ item.must }}</span>
                        </div>
                    </div>
                </div>
            </div>

            <button @click="openAddModal" class="w-full py-4 rounded-3xl border-2 border-dashed border-purple-200 text-purple-300 text-sm">
                <i class="fas fa-plus-circle mr-1"></i> 新增行程地點
            </button>
        </div>

        <div v-if="currentTab === 'accounting'" class="space-y-4 animate-fadeIn">
            <div class="bg-white rounded-3xl p-6 shadow-sm border border-purple-50 text-center">
                <p class="text-xs text-gray-400 mb-1">今日總支出 (台幣)</p>
                <h2 class="text-3xl font-black text-purple-600">NT$ {{ totalExpenseTWD }}</h2>
            </div>

            <div class="bg-white rounded-3xl p-5 shadow-sm">
                <div class="grid grid-cols-2 gap-3 mb-4">
                    <input v-model="newExp.name" placeholder="項目內容" class="w-full bg-purple-50 rounded-2xl p-3 text-sm focus:outline-none border-none">
                    <div class="relative">
                        <input v-model.number="newExp.jpy" type="number" placeholder="日幣 ¥" class="w-full bg-purple-50 rounded-2xl p-3 text-sm focus:outline-none border-none">
                    </div>
                </div>
                <button @click="addExpense" class="w-full bg-purple-500 text-white py-3 rounded-2xl font-bold shadow-lg shadow-purple-100">新增這筆帳</button>
            </div>

            <div class="space-y-2 mt-4">
                <div v-for="(exp, i) in expenses[selectedDayIndex]" :key="i" class="flex justify-between items-center bg-white/80 p-4 rounded-2xl shadow-sm">
                    <span class="text-sm font-medium">{{ exp.name }}</span>
                    <div class="text-right">
                        <p class="text-sm font-bold text-purple-600">¥ {{ exp.jpy }}</p>
                        <p class="text-[10px] text-gray-400">≈ NT$ {{ Math.round(exp.jpy * exchangeRate) }}</p>
                    </div>
                </div>
            </div>
        </div>

    </main>

    <nav class="fixed bottom-6 left-1/2 -translate-x-1/2 w-[90%] max-w-[450px] bg-white/90 backdrop-blur-md h-16 rounded-3xl shadow-2xl flex justify-around items-center px-4 border border-white z-50">
        <div @click="currentTab = 'itinerary'" :class="['flex flex-col items-center', currentTab === 'itinerary' ? 'text-purple-600' : 'text-gray-300']">
            <i class="fas fa-map-marked-alt text-xl"></i>
            <span class="text-[9px] mt-1 font-bold">行程</span>
        </div>
        <div @click="currentTab = 'accounting'" :class="['flex flex-col items-center', currentTab === 'accounting' ? 'text-purple-600' : 'text-gray-300']">
            <i class="fas fa-wallet text-xl"></i>
            <span class="text-[9px] mt-1 font-bold">記帳</span>
        </div>
        <div class="relative -top-4 bg-purple-500 w-12 h-12 rounded-full flex items-center justify-center text-white shadow-lg shadow-purple-200">
            <i class="fas fa-camera"></i>
        </div>
        <div @click="currentTab = 'info'" :class="['flex flex-col items-center', currentTab === 'info' ? 'text-purple-600' : 'text-gray-300']">
            <i class="fas fa-info-circle text-xl"></i>
            <span class="text-[9px] mt-1 font-bold">資訊</span>
        </div>
        <div @click="currentTab = 'settings'" :class="['flex flex-col items-center', currentTab === 'settings' ? 'text-purple-600' : 'text-gray-300']">
            <i class="fas fa-cog text-xl"></i>
            <span class="text-[9px] mt-1 font-bold">設定</span>
        </div>
    </nav>

</div>

<script>
const { createApp, ref, computed } = Vue;

createApp({
    setup() {
        const currentTab = ref('itinerary');
        const selectedDayIndex = ref(0);
        const exchangeRate = ref(0.21); // 可手動或透過API更新
        
        const days = ref([
            { date: '10/26', weekday: 'Mon', location: '中目黑' },
            { date: '10/27', weekday: 'Tue', location: '淺草/澀谷' },
            { date: '10/28', weekday: 'Wed', location: '迪士尼海洋' },
            { date: '10/29', weekday: 'Thu', location: '迪士尼樂園' },
            { date: '10/30', weekday: 'Fri', location: '原宿/新宿' },
            { date: '10/31', weekday: 'Sat', location: '成田機場' },
        ]);

        // 初始化行程假資料 (根據你的內容)
        const itineraries = ref([
            [
                { time: '12:10', type: '交通', title: '成田機場', note: '抵達並前往飯店放行李', must: '入境通關' },
                { time: '15:30', type: '美食', title: '中目黑星巴克', note: '山手通店、La vita 小義大利', must: '必喝咖啡' }
            ],
            [
                { time: '09:30', type: '體驗', title: '淺草和服', note: '江户和装工房 雅', must: '預約確認' },
                { time: '18:30', type: '美食', title: '燒肉黑田 澀谷', note: '深夜和牛饗宴', must: '必點牛舌' }
            ],
            [ { time: '08:30', type: '樂園', title: '東京迪士尼海洋', note: '全日夢幻之旅', must: 'FP抽籤' } ],
            [ { time: '08:30', type: '樂園', title: '東京迪士尼樂園', note: '公主夢幻雙日', must: '必看遊行' } ],
            [ { time: '12:00', type: '美食', title: '敘敘苑 燒肉', note: '53F高空景觀', must: '景觀位預約' } ],
            [ { time: '09:00', type: '交通', title: '成田機場', note: '提前2小時報到回高雄', must: 'IT281' } ]
        ]);

        const expenses = ref([[], [], [], [], [], []]);
        const newExp = ref({ name: '', jpy: null });

        const mockWeather = computed(() => {
            return { temp: '21', desc: '晴時多雲' };
        });

        const totalExpenseTWD = computed(() => {
            const sum = expenses.value[selectedDayIndex.value].reduce((acc, cur) => acc + cur.jpy, 0);
            return Math.round(sum * exchangeRate.value);
        });

        const deleteItem = (idx) => {
            itineraries.value[selectedDayIndex.value].splice(idx, 1);
        };

        const addExpense = () => {
            if (newExp.value.name && newExp.value.jpy) {
                expenses.value[selectedDayIndex.value].push({ ...newExp.value });
                newExp.value = { name: '', jpy: null };
            }
        };

        return {
            currentTab, selectedDayIndex, days, itineraries, deleteItem,
            mockWeather, exchangeRate, expenses, newExp, totalExpenseTWD, addExpense
        };
    }
}).mount('#app');
</script>

</body>
</html>
