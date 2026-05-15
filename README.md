[gemini-code-1778848477481.html](https://github.com/user-attachments/files/27801427/gemini-code-1778848477481.html)
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>2026 東京夢幻之旅</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700&display=swap');
        body { font-family: 'Noto Sans TC', sans-serif; background-color: #F3F0FF; -webkit-tap-highlight-color: transparent; }
        .hide-scrollbar::-webkit-scrollbar { display: none; }
        .glass-morphism { background: rgba(255, 255, 255, 0.7); backdrop-filter: blur(10px); border: 1px solid rgba(255, 255, 255, 0.3); }
        .day-scroller { scroll-behavior: smooth; -webkit-overflow-scrolling: touch; }
        .active-day { background-color: #8B5CF6 !important; color: white !important; transform: scale(1.05); }
    </style>
</head>
<body>
    <div id="app" class="max-w-md mx-auto min-h-screen pb-24 relative">
        
        <header class="sticky top-0 z-50 glass-morphism pt-6 pb-2 px-4 shadow-sm">
            <div class="flex justify-between items-center mb-4">
                <h1 class="text-xl font-bold text-purple-900">2026 Tokyo Trip 💜</h1>
                <div class="text-xs text-purple-500 bg-purple-100 px-2 py-1 rounded-full">
                    1 JPY = {{ exchangeRate }} TWD
                </div>
            </div>
            
            <div class="flex overflow-x-auto hide-scrollbar space-x-3 pb-2 day-scroller">
                <div v-for="(day, index) in days" :key="index" 
                     @click="selectedDay = index"
                     :class="['flex-shrink-0 w-16 h-20 rounded-2xl flex flex-col items-center justify-center transition-all cursor-pointer bg-white text-purple-400 border border-purple-100 shadow-sm', 
                              selectedDay === index ? 'active-day' : '']">
                    <span class="text-xs">{{ day.month }}/{{ day.date }}</span>
                    <span class="text-lg font-bold">D{{ index + 1 }}</span>
                    <span class="text-[10px]">{{ day.weekday }}</span>
                </div>
            </div>
        </header>

        <main class="px-4 mt-4">
            
            <div v-if="currentTab === 'itinerary'" class="space-y-4">
                <div class="bg-gradient-to-r from-purple-400 to-indigo-400 rounded-3xl p-4 text-white flex justify-between items-center shadow-lg">
                    <div>
                        <p class="text-sm opacity-90">{{ days[selectedDay].location }} 天氣</p>
                        <h2 class="text-2xl font-bold">{{ weather.temp }}°C / {{ weather.status }}</h2>
                    </div>
                    <i class="fas fa-cloud-sun text-4xl"></i>
                </div>

                <div v-for="(item, idx) in currentDayItinerary" :key="idx" class="bg-white rounded-2xl p-4 shadow-sm border border-purple-50 relative">
                    <div class="flex items-start">
                        <div class="w-12 text-sm font-medium text-purple-500 pt-1">{{ item.time }}</div>
                        <div class="flex-1">
                            <div class="flex items-center space-x-2">
                                <span :class="['px-2 py-0.5 rounded text-[10px] text-white', getTagColor(item.type)]">{{ item.type }}</span>
                                <h3 class="font-bold text-gray-800">{{ item.title }}</h3>
                            </div>
                            <p class="text-sm text-gray-500 mt-1">{{ item.note }}</p>
                            
                            <div class="mt-3 flex space-x-2">
                                <a :href="'https://www.google.com/maps/search/?api=1&query=' + item.title" target="_blank" 
                                   class="text-xs bg-purple-50 text-purple-600 px-3 py-1.5 rounded-lg flex items-center">
                                    <i class="fas fa-location-arrow mr-1"></i> 導航前往
                                </a>
                                <button v-if="item.id" @click="editItem(item)" class="text-xs text-gray-400 px-2 py-1.5">編輯</button>
                            </div>
                        </div>
                    </div>
                    <div v-if="item.must" class="absolute -top-2 -right-1 bg-yellow-400 text-white text-[10px] font-bold px-2 py-0.5 rounded-bl-lg rounded-tr-lg shadow-sm">
                        {{ item.must }}
                    </div>
                </div>
                
                <button @click="isModalOpen = true" class="w-full py-4 border-2 border-dashed border-purple-200 rounded-2xl text-purple-300 flex items-center justify-center">
                    <i class="fas fa-plus mr-2"></i> 新增行程項目
                </button>
            </div>

            <div v-if="currentTab === 'accounting'" class="space-y-4">
                <div class="bg-white rounded-3xl p-6 shadow-sm text-center">
                    <p class="text-gray-400 text-sm">今日累計支出 (約台幣)</p>
                    <h2 class="text-3xl font-black text-purple-600 mt-1">NT$ {{ totalExpenseTWD }}</h2>
                </div>

                <div class="bg-white rounded-2xl p-4 shadow-sm">
                    <h3 class="font-bold text-gray-700 mb-4">新增支出</h3>
                    <div class="grid grid-cols-2 gap-3 mb-3">
                        <input v-model="newExpense.name" placeholder="項目內容" class="bg-gray-50 border-none rounded-xl p-3 text-sm focus:ring-2 focus:ring-purple-400">
                        <div class="relative">
                            <input v-model.number="newExpense.amount" type="number" placeholder="日幣金額" class="w-full bg-gray-50 border-none rounded-xl p-3 text-sm focus:ring-2 focus:ring-purple-400">
                            <span class="absolute right-3 top-3 text-gray-400 text-xs">JPY</span>
                        </div>
                    </div>
                    <button @click="addExpense" class="w-full bg-purple-500 text-white py-3 rounded-xl font-bold shadow-md active:scale-95 transition-transform">記錄支出</button>
                </div>

                <div class="space-y-2">
                    <div v-for="(exp, idx) in currentDayExpenses" :key="idx" class="bg-white p-4 rounded-xl flex justify-between items-center shadow-sm">
                        <div>
                            <p class="font-medium text-gray-800">{{ exp.name }}</p>
                            <p class="text-[10px] text-gray-400">{{ exp.time }}</p>
                        </div>
                        <div class="text-right">
                            <p class="text-purple-600 font-bold">¥ {{ exp.amount }}</p>
                            <p class="text-[10px] text-gray-400">≈ NT$ {{ Math.round(exp.amount * exchangeRate) }}</p>
                        </div>
                    </div>
                </div>
            </div>

            <div v-if="currentTab === 'map'" class="space-y-4">
                <div class="bg-white rounded-3xl overflow-hidden shadow-md aspect-square relative">
                    <div class="w-full h-full bg-purple-50 flex flex-col items-center justify-center text-purple-300">
                        <i class="fas fa-map-marked-alt text-6xl mb-4"></i>
                        <p class="text-sm">正在載入 {{ days[selectedDay].location }} 地圖...</p>
                        <div class="mt-4 flex flex-wrap justify-center gap-2 px-4">
                            <span v-for="spot in currentDayItinerary" class="bg-white text-purple-500 text-[10px] px-2 py-1 rounded shadow-sm">📍 {{ spot.title }}</span>
                        </div>
                    </div>
                    <button class="absolute bottom-4 right-4 w-12 h-12 bg-white rounded-full shadow-lg flex items-center justify-center text-purple-600">
                        <i class="fas fa-crosshairs"></i>
                    </button>
                </div>
                <div class="bg-white p-4 rounded-2xl">
                    <h3 class="font-bold text-sm text-gray-700 mb-2">備用景點 / 口袋名單</h3>
                    <ul class="text-xs text-gray-500 space-y-2">
                        <li>• 中目黑星巴克旗艦店 (若有空檔)</li>
                        <li>• 澀谷 MEGA Don Quijote (深夜採買)</li>
                    </ul>
                </div>
            </div>

            <div v-if="currentTab === 'info'" class="space-y-4">
                <div class="bg-white rounded-2xl p-5 shadow-sm border-l-4 border-purple-500">
                    <h3 class="font-bold text-purple-900 mb-3"><i class="fas fa-plane-departure mr-2"></i>航班資訊</h3>
                    <div class="flex justify-between items-center text-sm">
                        <div class="text-center">
                            <p class="text-lg font-bold">KHH</p>
                            <p class="text-xs text-gray-400">08:00</p>
                        </div>
                        <div class="flex-1 flex flex-col items-center px-4">
                            <p class="text-[10px] text-purple-400 mb-1">IT280</p>
                            <div class="w-full h-[1px] bg-purple-200 relative">
                                <i class="fas fa-plane absolute -top-1.5 left-1/2 -translate-x-1/2 text-purple-300"></i>
                            </div>
                        </div>
                        <div class="text-center">
                            <p class="text-lg font-bold">NRT</p>
                            <p class="text-xs text-gray-400">12:10</p>
                        </div>
                    </div>
                </div>

                <div class="bg-white rounded-2xl p-5 shadow-sm">
                    <h3 class="font-bold text-purple-900 mb-2"><i class="fas fa-hotel mr-2"></i>住宿地點</h3>
                    <p class="text-sm font-bold text-gray-700">D・レガーロ</p>
                    <p class="text-xs text-gray-500 mt-1">〒272-0138 千葉県市川市南行徳２丁目２１</p>
                    <div class="mt-3 grid grid-cols-2 gap-2">
                        <button class="bg-purple-50 text-purple-600 py-2 rounded-lg text-xs font-medium">查看地圖</button>
                        <button class="bg-purple-50 text-purple-600 py-2 rounded-lg text-xs font-medium">複製地址</button>
                    </div>
                </div>

                <div class="bg-red-50 rounded-2xl p-5 shadow-sm border border-red-100">
                    <h3 class="font-bold text-red-600 mb-2"><i class="fas fa-phone-alt mr-2"></i>緊急聯絡</h3>
                    <div class="flex justify-between text-sm text-red-800">
                        <span>日本報警 / 急救</span>
                        <span class="font-bold">110 / 119</span>
                    </div>
                </div>
            </div>

        </main>

        <nav class="fixed bottom-0 left-0 right-0 max-w-md mx-auto glass-morphism border-t border-purple-100 flex justify-around items-center h-20 px-6 z-50">
            <div @click="currentTab = 'itinerary'" :class="['flex flex-col items-center transition-colors', currentTab === 'itinerary' ? 'text-purple-600' : 'text-gray-400']">
                <i class="fas fa-calendar-alt text-xl"></i>
                <span class="text-[10px] mt-1">行程</span>
            </div>
            <div @click="currentTab = 'accounting'" :class="['flex flex-col items-center transition-colors', currentTab === 'accounting' ? 'text-purple-600' : 'text-gray-400']">
                <i class="fas fa-wallet text-xl"></i>
                <span class="text-[10px] mt-1">記帳</span>
            </div>
            <div @click="currentTab = 'map'" :class="['flex flex-col items-center transition-colors', currentTab === 'map' ? 'text-purple-600' : 'text-gray-400']">
                <i class="fas fa-map-marked-alt text-xl"></i>
                <span class="text-[10px] mt-1">地圖</span>
            </div>
            <div @click="currentTab = 'info'" :class="['flex flex-col items-center transition-colors', currentTab === 'info' ? 'text-purple-600' : 'text-gray-400']">
                <i class="fas fa-suitcase text-xl"></i>
                <span class="text-[10px] mt-1">工具</span>
            </div>
        </nav>

        <div v-if="isModalOpen" class="fixed inset-0 z-[100] bg-black bg-opacity-50 flex items-center justify-center p-6">
            <div class="bg-white w-full rounded-3xl p-6">
                <h3 class="font-bold text-lg mb-4">新增行程項目</h3>
                <p class="text-gray-400 text-sm mb-6">（此處可設計詳細表單輸入時間地點...）</p>
                <button @click="isModalOpen = false" class="w-full bg-purple-500 text-white py-3 rounded-xl font-bold">關閉</button>
            </div>
        </div>

    </div>

    <script>
        const { createApp, ref, computed, onMounted } = Vue;

        createApp({
            setup() {
                const currentTab = ref('itinerary');
                const selectedDay = ref(0);
                const exchangeRate = ref(0.21); // 預設匯率
                const isModalOpen = ref(false);
                
                const days = ref([
                    { month: '10', date: '26', weekday: 'Mon', location: '中目黑' },
                    { month: '10', date: '27', weekday: 'Tue', location: '淺草/澀谷' },
                    { month: '10', date: '28', weekday: 'Wed', location: '迪士尼海洋' },
                    { month: '10', date: '29', weekday: 'Thu', location: '迪士尼樂園' },
                    { month: '10', date: '30', weekday: 'Fri', location: '原宿/新宿' },
                    { month: '10', date: '31', weekday: 'Sat', location: '成田機場' }
                ]);

                // 模擬行程資料
                const itineraries = ref([
                    [
                        { id: 1, time: '12:10', type: '交通', title: '抵達成田機場', note: '領取行李並前往飯店放行李', must: '入境審查' },
                        { id: 2, time: '15:30', type: '景點', title: '中目黑散策', note: '星巴克山手通店、La vita 小義大利', must: '必喝限定咖啡' },
                        { id: 3, time: '19:00', type: '購物', title: '上野阿美橫町', note: '藥妝與零食採買', must: '二木菓子' }
                    ],
                    [
                        { id: 4, time: '09:30', type: '體驗', title: '淺草和服體驗', note: '江户和装工房 雅', must: '預約號: JK-2026' },
                        { id: 5, time: '14:00', type: '美食', title: 'Suzukien 抹茶', note: '挑戰第七級特濃抹茶冰淇淋', must: '必吃美食' },
                        { id: 6, time: '18:30', type: '美食', title: '燒肉黑田 澀谷', note: '深夜和牛饗宴', must: '必點：厚切牛舌' }
                    ],
                    [ { time: '08:30', type: '樂園', title: '東京迪士尼海洋', note: '建議先抽 DPA 與預約等候卡', must: 'FP攻略' } ],
                    [ { time: '08:30', type: '樂園', title: '東京迪士尼樂園', note: '從南行德站搭接駁巴士直達', must: '必買：公主系列' } ],
                    [ { time: '12:00', type: '美食', title: '敘敘苑 燒肉', note: '東京歌劇城 53F 高空景觀', must: '午間套餐划算' } ],
                    [ { time: '09:00', type: '交通', title: '成田機場', note: '09:25 前需抵達櫃台報到', must: 'IT281' } ]
                ]);

                // 記帳資料
                const newExpense = ref({ name: '', amount: null });
                const expenses = ref([[], [], [], [], [], []]);

                const currentDayItinerary = computed(() => itineraries.value[selectedDay.value]);
                const currentDayExpenses = computed(() => expenses.value[selectedDay.value]);
                const totalExpenseTWD = computed(() => {
                    const totalJPY = currentDayExpenses.value.reduce((sum, item) => sum + (item.amount || 0), 0);
                    return Math.round(totalJPY * exchangeRate.value);
                });

                const weather = ref({ temp: '21', status: '晴朗' });

                const getTagColor = (type) => {
                    const colors = {
                        '交通': 'bg-blue-400',
                        '景點': 'bg-emerald-400',
                        '美食': 'bg-orange-400',
                        '體驗': 'bg-pink-400',
                        '購物': 'bg-indigo-400',
                        '樂園': 'bg-purple-400'
                    };
                    return colors[type] || 'bg-gray-400';
                };

                const addExpense = () => {
                    if (!newExpense.value.name || !newExpense.value.amount) return;
                    expenses.value[selectedDay.value].push({
                        name: newExpense.value.name,
                        amount: newExpense.value.amount,
                        time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
                    });
                    newExpense.value = { name: '', amount: null };
                };

                return {
                    currentTab, selectedDay, days, itineraries, currentDayItinerary, 
                    getTagColor, weather, exchangeRate, expenses, newExpense, 
                    currentDayExpenses, totalExpenseTWD, addExpense, isModalOpen
                };
            }
        }).mount('#app');
    </script>
</body>
</html>
