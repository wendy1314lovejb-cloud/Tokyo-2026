
<!DOCTYPE html>
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

        * {
            box-sizing: border-box;
        }

        body {
            font-family: 'Noto Sans TC', sans-serif;
            background-color: var(--lavender-light);
            background-image: radial-gradient(#D1D5DB 0.5px, transparent 0.5px);
            background-size: 20px 20px;
            margin: 0;
            padding: 0;
            color: #4B5563;
        }

        .hide-scrollbar::-webkit-scrollbar { display: none; }
        .hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        
        .app-container {
            max-width: 500px;
            margin: 0 auto;
            min-height: 100vh;
            background: rgba(255, 255, 255, 0.6);
            backdrop-filter: blur(5px);
            position: relative;
            padding-bottom: 90px;
        }

        .itinerary-card {
            border: 2px solid white;
            box-shadow: 0 4px 15px rgba(216, 180, 254, 0.2);
            border-radius: 24px;
            background: white;
            transition: all 0.3s ease;
        }

        .itinerary-card:hover {
            box-shadow: 0 6px 20px rgba(216, 180, 254, 0.3);
            transform: translateY(-2px);
        }

        .day-active {
            background-color: var(--lavender-dark) !important;
            color: white !important;
            transform: translateY(-5px);
            box-shadow: 0 4px 10px rgba(139, 92, 246, 0.4);
        }

        .sticker {
            position: absolute;
            pointer-events: none;
            z-index: 10;
            width: 60px;
            opacity: 0.8;
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .animate-fadeIn {
            animation: fadeIn 0.4s ease-out;
        }

        /* Modal 樣式 */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.5);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 100;
            backdrop-filter: blur(3px);
        }

        .modal-content {
            background: white;
            border-radius: 24px;
            padding: 24px;
            max-width: 90%;
            width: 400px;
            max-height: 90vh;
            overflow-y: auto;
            box-shadow: 0 20px 60px rgba(139, 92, 246, 0.3);
            animation: fadeIn 0.3s ease-out;
        }

        .input-field {
            background-color: #F3F0FF;
            border: 2px solid transparent;
            border-radius: 16px;
            padding: 12px 16px;
            font-family: inherit;
            font-size: 14px;
            transition: all 0.2s ease;
        }

        .input-field:focus {
            outline: none;
            border-color: var(--lavender-dark);
            background-color: white;
            box-shadow: 0 0 0 3px rgba(139, 92, 246, 0.1);
        }

        .btn-primary {
            background: linear-gradient(135deg, #a78bfa 0%, #8B5CF6 100%);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 4px 15px rgba(139, 92, 246, 0.3);
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(139, 92, 246, 0.4);
        }

        .btn-primary:active {
            transform: scale(0.98);
        }

        .btn-danger {
            background: #FEE2E2;
            color: #DC2626;
            border: none;
            padding: 8px 12px;
            border-radius: 12px;
            font-size: 12px;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .btn-danger:hover {
            background: #FECACA;
            transform: scale(1.05);
        }

        .empty-state {
            text-align: center;
            padding: 40px 20px;
            color: #B0B5C0;
        }

        .empty-state i {
            font-size: 48px;
            margin-bottom: 16px;
            opacity: 0.5;
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
                 :class="['flex-shrink-0 w-16 h-20 rounded-2xl flex flex-col items-center justify-center bg-white border border-purple-50 transition-all cursor-pointer hover:border-purple-200', 
                          selectedDayIndex === index ? 'day-active' : 'text-purple-300']">
                <span class="text-[10px] uppercase">{{ day.weekday }}</span>
                <span class="text-lg font-bold">{{ index + 1 }}</span>
                <span class="text-[10px]">{{ day.date }}</span>
            </div>
        </div>
    </header>

    <main class="px-6">
        
        <!-- 行程 Tab -->
        <div v-if="currentTab === 'itinerary'" class="space-y-4 animate-fadeIn">
            <div class="bg-gradient-to-br from-purple-100 to-white p-4 rounded-3xl flex justify-between items-center border border-white">
                <div>
                    <p class="text-xs text-purple-400 font-medium">{{ days[selectedDayIndex].location }}</p>
                    <h2 class="text-xl font-bold text-purple-700">{{ mockWeather.temp }}°C {{ mockWeather.desc }}</h2>
                </div>
                <div class="text-3xl">☁️</div>
            </div>

            <div v-if="itineraries[selectedDayIndex].length === 0" class="empty-state">
                <i class="fas fa-calendar-plus"></i>
                <p>本日尚無行程安排</p>
                <p class="text-xs mt-2">點擊下方按鈕新增行程</p>
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
                            <button @click="deleteItem(idx)" class="btn-danger">
                                <i class="fas fa-trash text-xs"></i>
                            </button>
                        </div>
                        <p class="text-xs text-gray-400 mt-2">{{ item.note }}</p>
                        
                        <div class="mt-4 flex justify-between items-center flex-wrap gap-2">
                            <a :href="'https://www.google.com/maps/search/?api=1&query=' + encodeURIComponent(item.title)" 
                               target="_blank" rel="noopener noreferrer"
                               class="text-[10px] bg-purple-500 text-white px-4 py-2 rounded-full shadow-sm active:scale-95 transition-transform">
                                <i class="fas fa-location-arrow mr-1"></i> 開啟導航
                            </a>
                            <span v-if="item.must" class="text-[10px] text-pink-400 font-bold">✨ {{ item.must }}</span>
                        </div>
                    </div>
                </div>
            </div>

            <button @click="showAddModal = true" class="w-full py-4 rounded-3xl border-2 border-dashed border-purple-200 text-purple-300 text-sm hover:border-purple-400 hover:text-purple-400 transition-all">
                <i class="fas fa-plus-circle mr-1"></i> 新增行程地點
            </button>
        </div>

        <!-- 記帳 Tab -->
        <div v-if="currentTab === 'accounting'" class="space-y-4 animate-fadeIn">
            <div class="bg-white rounded-3xl p-6 shadow-sm border border-purple-50 text-center">
                <p class="text-xs text-gray-400 mb-1">{{ days[selectedDayIndex].date }} 日支出 (台幣)</p>
                <h2 class="text-3xl font-black text-purple-600">NT$ {{ totalExpenseTWD }}</h2>
                <p class="text-xs text-gray-300 mt-2">¥{{ totalExpenseJPY }}</p>
            </div>

            <div class="bg-white rounded-3xl p-5 shadow-sm border border-purple-50">
                <div class="grid grid-cols-1 gap-3 mb-4">
                    <input v-model="newExp.name" placeholder="項目內容 (如：午餐、門票)" class="input-field w-full">
                    <input v-model.number="newExp.jpy" type="number" placeholder="日幣 ¥" class="input-field w-full" min="0" step="100">
                </div>
                <button @click="addExpense" class="btn-primary w-full font-bold py-3">
                    <i class="fas fa-plus mr-2"></i>新增這筆帳
                </button>
            </div>

            <div v-if="expenses[selectedDayIndex].length === 0" class="empty-state">
                <i class="fas fa-wallet"></i>
                <p>本日尚無支出記錄</p>
            </div>

            <div class="space-y-2 mt-4">
                <div v-for="(exp, i) in expenses[selectedDayIndex]" :key="i" class="flex justify-between items-center bg-white/80 p-4 rounded-2xl shadow-sm border border-purple-50">
                    <span class="text-sm font-medium flex-1">{{ exp.name }}</span>
                    <div class="text-right">
                        <p class="text-sm font-bold text-purple-600">¥ {{ exp.jpy }}</p>
                        <p class="text-[10px] text-gray-400">≈ NT$ {{ Math.round(exp.jpy * exchangeRate) }}</p>
                    </div>
                    <button @click="deleteExpense(i)" class="btn-danger ml-2">
                        <i class="fas fa-trash text-xs"></i>
                    </button>
                </div>
            </div>
        </div>

        <!-- 資訊 Tab -->
        <div v-if="currentTab === 'info'" class="space-y-4 animate-fadeIn">
            <div class="itinerary-card p-6">
                <h2 class="text-xl font-bold text-purple-600 mb-4"><i class="fas fa-plane mr-2"></i>旅程概覽</h2>
                <div class="space-y-3 text-sm">
                    <div class="flex justify-between">
                        <span class="text-gray-600">起點：</span>
                        <span class="font-bold">高雄小港機場</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-gray-600">終點：</span>
                        <span class="font-bold">東京成田機場</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-gray-600">行程天數：</span>
                        <span class="font-bold">{{ days.length }} 天</span>
                    </div>
                    <div class="flex justify-between pt-3 border-t border-purple-50">
                        <span class="text-gray-600">累計支出：</span>
                        <span class="font-bold text-purple-600">NT$ {{ totalAllExpense }}</span>
                    </div>
                </div>
            </div>

            <div class="itinerary-card p-6">
                <h2 class="text-xl font-bold text-purple-600 mb-4"><i class="fas fa-info-circle mr-2"></i>小提示</h2>
                <ul class="text-xs space-y-2 text-gray-600">
                    <li>💳 日本多數地方可使用信用卡或 Suica 卡</li>
                    <li>🏮 購物退稅請帶護照正本</li>
                    <li>🚆 JR Pass 可在機場購買</li>
                    <li>📱 建議購買日本當地 SIM 卡或 WiFi 租賃</li>
                    <li>⏰ 日本比台灣快 1 小時</li>
                </ul>
            </div>

            <div class="itinerary-card p-6">
                <h2 class="text-xl font-bold text-purple-600 mb-4"><i class="fas fa-phone mr-2"></i>緊急聯絡</h2>
                <ul class="text-xs space-y-2 text-gray-600">
                    <li>🚨 警察：110</li>
                    <li>🏥 救急車：119</li>
                    <li>🔥 消防：119</li>
                    <li>📞 台灣駐日代表處：03-3280-7800</li>
                </ul>
            </div>
        </div>

        <!-- 設定 Tab -->
        <div v-if="currentTab === 'settings'" class="space-y-4 animate-fadeIn">
            <div class="itinerary-card p-6">
                <h2 class="text-xl font-bold text-purple-600 mb-4"><i class="fas fa-sliders-h mr-2"></i>匯率設定</h2>
                <div class="space-y-3">
                    <div>
                        <label class="text-xs text-gray-600 block mb-2">日幣對台幣匯率 (1 JPY = ? TWD)</label>
                        <div class="flex gap-2">
                            <input v-model.number="exchangeRate" type="number" step="0.01" class="input-field flex-1" placeholder="0.21">
                            <button @click="updateExchangeRate" class="btn-primary px-4">更新</button>
                        </div>
                    </div>
                </div>
            </div>

            <div class="itinerary-card p-6">
                <h2 class="text-xl font-bold text-purple-600 mb-4"><i class="fas fa-database mr-2"></i>數據管理</h2>
                <div class="space-y-2">
                    <button @click="saveToLocalStorage" class="btn-primary w-full py-2 text-sm">
                        <i class="fas fa-save mr-2"></i>手動保存數據
                    </button>
                    <button @click="clearAllData" class="w-full py-2 bg-red-100 text-red-600 rounded-lg text-sm font-bold hover:bg-red-200 transition-all">
                        <i class="fas fa-trash mr-2"></i>清除所有數據
                    </button>
                </div>
            </div>

            <div class="itinerary-card p-6">
                <h2 class="text-xl font-bold text-gray-600 mb-2"><i class="fas fa-info-circle mr-2"></i>應用資訊</h2>
                <p class="text-xs text-gray-500">2026 Tokyo Trip Planner</p>
                <p class="text-xs text-gray-400 mt-1">Version 1.1.0</p>
            </div>
        </div>

    </main>

    <!-- 導航欄 -->
    <nav class="fixed bottom-6 left-1/2 -translate-x-1/2 w-[90%] max-w-[450px] bg-white/90 backdrop-blur-md h-16 rounded-3xl shadow-2xl flex justify-around items-center px-4 border border-white z-50">
        <div @click="currentTab = 'itinerary'" :class="['flex flex-col items-center cursor-pointer transition-all', currentTab === 'itinerary' ? 'text-purple-600' : 'text-gray-300']">
            <i class="fas fa-map-marked-alt text-xl"></i>
            <span class="text-[9px] mt-1 font-bold">行程</span>
        </div>
        <div @click="currentTab = 'accounting'" :class="['flex flex-col items-center cursor-pointer transition-all', currentTab === 'accounting' ? 'text-purple-600' : 'text-gray-300']">
            <i class="fas fa-wallet text-xl"></i>
            <span class="text-[9px] mt-1 font-bold">記帳</span>
        </div>
        <div class="relative -top-4 bg-gradient-to-br from-purple-400 to-purple-600 w-12 h-12 rounded-full flex items-center justify-center text-white shadow-lg shadow-purple-200 cursor-pointer hover:shadow-purple-300 transition-all hover:scale-110">
            <i class="fas fa-camera"></i>
        </div>
        <div @click="currentTab = 'info'" :class="['flex flex-col items-center cursor-pointer transition-all', currentTab === 'info' ? 'text-purple-600' : 'text-gray-300']">
            <i class="fas fa-info-circle text-xl"></i>
            <span class="text-[9px] mt-1 font-bold">資訊</span>
        </div>
        <div @click="currentTab = 'settings'" :class="['flex flex-col items-center cursor-pointer transition-all', currentTab === 'settings' ? 'text-purple-600' : 'text-gray-300']">
            <i class="fas fa-cog text-xl"></i>
            <span class="text-[9px] mt-1 font-bold">設定</span>
        </div>
    </nav>

    <!-- 新增行程 Modal -->
    <div v-if="showAddModal" class="modal-overlay" @click.self="showAddModal = false">
        <div class="modal-content">
            <h2 class="text-xl font-bold text-purple-600 mb-4">新增行程</h2>
            <div class="space-y-3">
                <input v-model="newItem.time" type="time" placeholder="時間" class="input-field w-full">
                <input v-model="newItem.title" placeholder="地點名稱" class="input-field w-full">
                <select v-model="newItem.type" class="input-field w-full">
                    <option value="交通">交通</option>
                    <option value="美食">美食</option>
                    <option value="景點">景點</option>
                    <option value="樂園">樂園</option>
                    <option value="體驗">體驗</option>
                    <option value="其他">其他</option>
                </select>
                <textarea v-model="newItem.note" placeholder="備註說明" class="input-field w-full" rows="3"></textarea>
                <input v-model="newItem.must" placeholder="重要提醒 (選填)" class="input-field w-full">
                <div class="flex gap-3 pt-4">
                    <button @click="showAddModal = false" class="flex-1 py-2 border-2 border-purple-200 text-purple-600 rounded-lg font-bold hover:bg-purple-50">
                        取消
                    </button>
                    <button @click="addItem" class="btn-primary flex-1 py-2">
                        確定新增
                    </button>
                </div>
            </div>
        </div>
    </div>

</div>

<script>
const { createApp, ref, computed, watch } = Vue;

createApp({
    setup() {
        const currentTab = ref('itinerary');
        const selectedDayIndex = ref(0);
        const exchangeRate = ref(0.21);
        const showAddModal = ref(false);
        
        const days = ref([
            { date: '10/26', weekday: 'Mon', location: '中目黑' },
            { date: '10/27', weekday: 'Tue', location: '淺草/澀谷' },
            { date: '10/28', weekday: 'Wed', location: '迪士尼海洋' },
            { date: '10/29', weekday: 'Thu', location: '迪士尼樂園' },
            { date: '10/30', weekday: 'Fri', location: '原宿/新宿' },
            { date: '10/31', weekday: 'Sat', location: '成田機場' },
        ]);

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
        const newItem = ref({ time: '', title: '', type: '景點', note: '', must: '' });

        const mockWeather = computed(() => {
            return { temp: '21', desc: '晴時多雲' };
        });

        const totalExpenseJPY = computed(() => {
            return expenses.value[selectedDayIndex.value].reduce((acc, cur) => acc + cur.jpy, 0);
        });

        const totalExpenseTWD = computed(() => {
            return Math.round(totalExpenseJPY.value * exchangeRate.value);
        });

        const totalAllExpense = computed(() => {
            return Math.round(expenses.value.flat().reduce((acc, cur) => acc + cur.jpy, 0) * exchangeRate.value);
        });

        const deleteItem = (idx) => {
            itineraries.value[selectedDayIndex.value].splice(idx, 1);
        };

        const addExpense = () => {
            if (newExp.value.name && newExp.value.jpy && newExp.value.jpy > 0) {
                expenses.value[selectedDayIndex.value].push({ ...newExp.value });
                newExp.value = { name: '', jpy: null };
            }
        };

        const deleteExpense = (i) => {
            expenses.value[selectedDayIndex.value].splice(i, 1);
        };

        const addItem = () => {
            if (newItem.value.time && newItem.value.title) {
                itineraries.value[selectedDayIndex.value].push({ ...newItem.value });
                newItem.value = { time: '', title: '', type: '景點', note: '', must: '' };
                showAddModal.value = false;
            } else {
                alert('請輸入時間和地點名稱');
            }
        };

        const updateExchangeRate = () => {
            // 可接入真實匯率 API
            alert(`匯率已更新為 1 JPY = ${exchangeRate.value} TWD`);
            saveToLocalStorage();
        };

        const saveToLocalStorage = () => {
            const data = {
                itineraries: itineraries.value,
                expenses: expenses.value,
                exchangeRate: exchangeRate.value,
                timestamp: new Date().toISOString()
            };
            localStorage.setItem('tokyoTripData', JSON.stringify(data));
            alert('數據已保存 ✨');
        };

        const loadFromLocalStorage = () => {
            const saved = localStorage.getItem('tokyoTripData');
            if (saved) {
                const data = JSON.parse(saved);
                itineraries.value = data.itineraries;
                expenses.value = data.expenses;
                exchangeRate.value = data.exchangeRate;
            }
        };

        const clearAllData = () => {
            if (confirm('確定要清除所有數據嗎？此操作無法撤銷')) {
                itineraries.value = [[], [], [], [], [], []];
                expenses.value = [[], [], [], [], [], []];
                localStorage.removeItem('tokyoTripData');
                alert('所有數據已清除');
            }
        };

        // 自動保存
        watch([itineraries, expenses, exchangeRate], () => {
            saveToLocalStorage();
        }, { deep: true });

        // 初始化時載入數據
        loadFromLocalStorage();

        return {
            currentTab, selectedDayIndex, days, itineraries, deleteItem,
            mockWeather, exchangeRate, expenses, newExp, totalExpenseTWD, totalExpenseJPY, totalAllExpense,
            addExpense, deleteExpense, showAddModal, newItem, addItem, updateExchangeRate,
            saveToLocalStorage, clearAllData
        };
    }
}).mount('#app');
</script>

</body>
</html>
