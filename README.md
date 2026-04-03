[NewMed.html](https://github.com/user-attachments/files/26460022/NewMed.html)
<!DOCTYPE html>
<html lang="zh-HK">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>醫療開支追蹤 - 視覺升級版</title>
    
    <!-- 引入 Tailwind CSS 美化介面 -->
    <script src="https://cdn.tailwindcss.com"></script>

    <style>
        :root {
            --slate-50: #f8fafc; --slate-100: #f1f5f9; --slate-200: #e2e8f0; 
            --slate-400: #94a3b8; --slate-500: #64748b; --slate-700: #334155; --slate-800: #1e293b;
            --teal-50: #f0fdfa; --teal-600: #0d9488; --teal-700: #0f766e;
            --orange-50: #fff7ed; --orange-500: #f97316; --orange-600: #ea580c;
            --blue-50: #eff6ff; --blue-400: #60a5fa; --blue-600: #2563eb; --indigo-400: #818cf8;
            --red-500: #ef4444; --emerald-50: #ecfdf5; --emerald-600: #059669;
        }
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; background-color: var(--slate-50); color: var(--slate-800); padding: 20px; padding-bottom: 80px; }
        
        /* 自訂拉上拉下的捲軸設計 */
        .scroll-container {
            max-height: 640px; /* 固定高度，迫使出現捲軸 */
            overflow-y: auto;  /* 允許上下捲動 */
            overflow-x: hidden;
            padding-right: 12px;
        }
        .scroll-container::-webkit-scrollbar {
            width: 8px;
        }
        .scroll-container::-webkit-scrollbar-track {
            background: var(--slate-100); 
            border-radius: 8px;
        }
        .scroll-container::-webkit-scrollbar-thumb {
            background: var(--slate-400); 
            border-radius: 8px;
        }
        .scroll-container::-webkit-scrollbar-thumb:hover {
            background: var(--slate-500); 
        }

        .header-icon { background: linear-gradient(135deg, #14b8a6, #0f766e); color: white; padding: 12px; border-radius: 12px; display: flex; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        .panel { background: white; border-radius: 16px; padding: 24px; border: 1px solid var(--slate-200); box-shadow: 0 2px 8px rgba(0,0,0,0.04); }
        .form-control { width: 100%; padding: 14px; background-color: var(--slate-50); border: 1px solid var(--slate-200); border-radius: 10px; font-size: 16px; outline: none; transition: all 0.2s; font-weight: 600;}
        .form-control:focus { border-color: var(--teal-600); background-color: white; box-shadow: 0 0 0 3px var(--teal-50); }
        .form-control[readonly] { background-color: var(--slate-100); color: var(--slate-500); cursor: not-allowed; }
        
        .claim-box { display: flex; align-items: flex-start; gap: 12px; padding: 16px; background-color: var(--slate-50); border: 2px solid var(--slate-200); border-radius: 12px; cursor: pointer; margin-top: 16px; transition: all 0.2s;}
        .claim-box.active { background-color: var(--teal-50); border-color: #5eead4; }
        
        .modal-overlay { position: fixed; inset: 0; background: rgba(15, 23, 42, 0.6); display: none; align-items: center; justify-content: center; z-index: 100; backdrop-filter: blur(4px); }
        .modal-overlay.show { display: flex; }
        
        /* 截圖還原 - 卡片設計 */
        .record-card {
            background: white;
            border: 1px solid var(--slate-200);
            border-radius: 12px;
            padding: 16px 20px;
            position: relative;
            overflow: hidden;
            margin-bottom: 12px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.02);
            transition: all 0.2s;
        }
        @media(min-width: 768px) { 
            .record-card { flex-direction: row; align-items: center; justify-content: space-between; } 
        }
        .record-card:hover {
            border-color: #5eead4;
            box-shadow: 0 4px 12px rgba(13, 148, 136, 0.08);
        }
        .record-deco { position: absolute; left: 0; top: 0; bottom: 0; width: 6px; }
        .deco-west { background-color: var(--indigo-400); }
        .deco-physio { background-color: var(--blue-400); }
        
        .tag { padding: 4px 10px; border-radius: 6px; font-size: 12px; font-weight: 800; letter-spacing: 0.5px;}
        .tag-west { background-color: #e0e7ff; color: #4338ca; }
        .tag-physio { background-color: #dbeafe; color: #1d4ed8; }
        .tag-claim { background-color: var(--emerald-50); color: var(--emerald-600); border: 1px solid #a7f3d0; }
    </style>
</head>
<body>

    <!-- 刪除確認彈窗 -->
    <div id="deleteModal" class="modal-overlay">
        <div class="bg-white p-6 rounded-2xl max-w-sm w-full text-center shadow-2xl">
            <div class="w-14 h-14 bg-red-100 text-red-600 rounded-full flex items-center justify-center mx-auto mb-4">
                <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m21.73 18-8-14a2 2 0 0 0-3.48 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3Z"></path><line x1="12" y1="9" x2="12" y2="13"></line><line x1="12" y1="17" x2="12.01" y2="17"></line></svg>
            </div>
            <h3 class="text-xl font-bold mb-2">確定刪除此記錄？</h3>
            <p class="text-slate-500 mb-6 text-sm font-medium">刪除後相關理賠金額將會重新計算。</p>
            <div class="flex gap-3">
                <button class="flex-1 py-3 bg-slate-100 text-slate-700 rounded-xl font-bold" onclick="closeDeleteModal()">取消</button>
                <button class="flex-1 py-3 bg-red-600 text-white rounded-xl font-bold shadow-md" onclick="confirmDelete()">確認刪除</button>
            </div>
        </div>
    </div>

    <div class="container mx-auto max-w-6xl">
        
        <!-- Header -->
        <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 pb-6 border-b border-slate-200 gap-4">
            <div class="flex items-center gap-4">
                <div class="header-icon">
                    <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"></polyline></svg>
                </div>
                <div>
                    <h1 class="text-2xl font-black">醫療開支與保險理賠追蹤</h1>
                    <p class="text-slate-500 text-sm font-bold mt-1">單機極速版，資料僅儲存於本機</p>
                </div>
            </div>
            <button class="bg-slate-800 hover:bg-slate-900 text-white px-5 py-2.5 rounded-lg font-bold flex items-center gap-2 transition-colors shadow-md" onclick="exportToCSV()">
                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path><polyline points="7 10 12 15 17 10"></polyline><line x1="12" y1="15" x2="12" y2="3"></line></svg>
                匯出 Excel 備份
            </button>
        </div>

        <!-- 統計卡片 -->
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-8">
            <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm flex flex-col justify-center">
                <div class="text-slate-500 text-xs font-black uppercase mb-1 flex items-center gap-1">
                    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="4" y="2" width="16" height="20" rx="2" ry="2"></rect><line x1="8" y1="6" x2="16" y2="6"></line><line x1="16" y1="14" x2="16" y2="18"></line></svg>
                    總開支
                </div>
                <div id="statTotal" class="text-3xl font-black text-slate-800">$0</div>
            </div>
            <div class="bg-teal-50 p-5 rounded-2xl border border-teal-100 shadow-sm flex flex-col justify-center">
                <div class="text-teal-600 text-xs font-black uppercase mb-1 flex items-center gap-1">
                    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z"></path></svg>
                    可理賠總額
                </div>
                <div id="statClaimable" class="text-3xl font-black text-teal-600">$0</div>
            </div>
            <div class="bg-orange-50 p-5 rounded-2xl border border-orange-100 shadow-sm flex flex-col justify-center">
                <div class="text-orange-600 text-xs font-black uppercase mb-1 flex items-center gap-1">
                    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="12" y1="1" x2="12" y2="23"></line><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"></path></svg>
                    自費總額
                </div>
                <div id="statSelfPaid" class="text-3xl font-black text-orange-600">$0</div>
            </div>
            <div class="bg-blue-50 p-5 rounded-2xl border border-blue-100 shadow-sm flex flex-col justify-center">
                <div class="text-blue-600 text-xs font-black uppercase mb-1 flex items-center gap-1">
                    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="12"></line></svg>
                    物理治療次數
                </div>
                <div class="flex items-baseline gap-1">
                    <span id="statPhysio" class="text-3xl font-black text-blue-700">0</span>
                    <span class="text-blue-500 font-bold">/ 10 次</span>
                </div>
                <div class="w-full bg-blue-200 h-1.5 mt-2 rounded-full overflow-hidden">
                    <div id="statPhysioBar" class="h-full bg-blue-600 transition-all duration-500" style="width: 0%"></div>
                </div>
            </div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
            
            <!-- 左側表單 -->
            <div class="lg:col-span-4">
                <div class="panel sticky top-4">
                    <h2 class="text-xl font-black mb-6 flex items-center gap-2">
                        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" class="text-teal-600"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="16"></line><line x1="8" y1="12" x2="16" y2="12"></line></svg>
                        新增醫療開支
                    </h2>
                    
                    <div class="mb-5">
                        <label class="block font-bold text-slate-700 text-sm mb-2">日期</label>
                        <input type="date" id="inputDate" class="form-control">
                    </div>
                    
                    <!-- 【重點保護】將選項寫入 HTML，防止 iPad 清除 -->
                    <div class="mb-5">
                        <label class="block font-bold text-slate-700 text-sm mb-2">治療項目</label>
                        <select id="inputSubcategory" class="form-control" onchange="handleSelectionChange()">
                            <option value="" disabled selected>請選擇項目...</option>
                            <optgroup label="西醫治療費" class="font-bold text-teal-700 bg-white">
                                <option value="MRI檢查費" data-cat="西醫治療費" class="text-slate-800 font-normal">MRI檢查費 (自填金額)</option>
                                <option value="傷口護理費用" data-cat="西醫治療費" data-amt="50" class="text-slate-800 font-normal">傷口護理費用 ($50)</option>
                                <option value="政府門診" data-cat="西醫治療費" class="text-slate-800 font-normal">政府門診 (自填金額)</option>
                                <option value="私家門診" data-cat="西醫治療費" class="text-slate-800 font-normal">私家門診 (自填金額)</option>
                                <option value="骨科門診" data-cat="西醫治療費" data-amt="250" class="text-slate-800 font-normal">骨科門診 ($250)</option>
                            </optgroup>
                            <optgroup label="物理治療費" class="font-bold text-teal-700 bg-white">
                                <option value="私人物理治療" data-cat="物理治療費" class="text-slate-800 font-normal">私人物理治療 (自填金額)</option>
                                <option value="政府物理治療" data-cat="物理治療費" data-amt="250" class="text-slate-800 font-normal">政府物理治療 ($250)</option>
                            </optgroup>
                        </select>
                    </div>

                    <div class="mb-5 relative">
                        <label class="block font-bold text-slate-700 text-sm mb-2">金額 (HKD)</label>
                        <div class="relative">
                            <span class="absolute left-4 top-3 text-slate-500 font-black text-lg">$</span>
                            <input type="number" id="inputAmount" class="form-control font-black text-xl pl-9" placeholder="0">
                        </div>
                        <p id="fixedHint" class="hidden text-orange-600 text-xs mt-2 font-bold flex items-center gap-1">
                           <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="12"></line><line x1="12" y1="16" x2="12.01" y2="16"></line></svg>
                           此為固定項目收費，系統已自動填入
                        </p>
                    </div>

                    <label id="claimLabel" class="claim-box">
                        <input type="checkbox" id="inputClaimed" onchange="updateUI()" class="w-5 h-5 accent-teal-600 mt-0.5">
                        <div class="flex flex-col">
                            <span class="font-bold text-sm text-slate-800">準備申請保險理賠</span>
                            <span class="text-slate-500 text-xs mt-1">勾選後將自動計算理賠金額</span>
                        </div>
                    </label>

                    <div id="physioHint" class="hidden mt-3 p-3 bg-blue-50 rounded-lg border border-blue-100 flex items-start gap-2">
                        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="text-blue-500 mt-0.5 flex-shrink-0"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="12"></line><line x1="12" y1="16" x2="12.01" y2="16"></line></svg>
                        <p class="text-blue-700 text-[11px] font-bold leading-relaxed">提示：物理治療每次最多理賠 <strong>$400</strong>，總上限 <strong>10 次</strong>。</p>
                    </div>

                    <button class="bg-gradient-to-r from-teal-500 to-teal-600 hover:from-teal-600 hover:to-teal-700 text-white w-full py-4 rounded-xl font-black text-lg mt-6 shadow-lg transition-transform active:scale-95 flex justify-center items-center gap-2" onclick="handleAdd()">
                        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="16"></line><line x1="8" y1="12" x2="16" y2="12"></line></svg>
                        立即加入記錄
                    </button>
                </div>
            </div>

            <!-- 右側列表區 (還原截圖的精美佈局) -->
            <div class="lg:col-span-8">
                <div class="panel flex flex-col pt-6 pb-2 px-4 md:px-6">
                    
                    <!-- 標題與計數 -->
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-2xl font-black text-slate-800 tracking-tight">開支明細記錄</h2>
                        <span id="recordCount" class="text-sm font-black bg-teal-600 text-white px-4 py-1.5 rounded-full shadow-sm">共 0 筆</span>
                    </div>
                    
                    <!-- 篩選區 (白底圓角框) -->
                    <div class="flex flex-col md:flex-row gap-4 mb-6 bg-white p-4 rounded-xl border border-slate-200 shadow-sm items-end">
                        <div class="flex-1 w-full">
                            <label class="block text-xs font-black text-slate-400 mb-1.5">篩選類別</label>
                            <select id="filterCategory" class="w-full text-sm font-bold bg-white border-2 border-teal-500 text-teal-700 p-2.5 rounded-lg outline-none cursor-pointer" onchange="renderList()">
                                <option value="全部">顯示全部</option>
                                <option value="西醫治療費">西醫治療費</option>
                                <option value="物理治療費">物理治療費</option>
                            </select>
                        </div>
                        <div class="flex-1 w-full">
                            <label class="block text-xs font-black text-slate-400 mb-1.5">日期從</label>
                            <input type="date" id="filterStart" class="w-full text-sm font-bold bg-slate-50 border border-slate-200 p-2.5 rounded-lg outline-none focus:border-teal-500" onchange="renderList()">
                        </div>
                        <div class="flex-1 w-full">
                            <label class="block text-xs font-black text-slate-400 mb-1.5">日期至</label>
                            <input type="date" id="filterEnd" class="w-full text-sm font-bold bg-slate-50 border border-slate-200 p-2.5 rounded-lg outline-none focus:border-teal-500" onchange="renderList()">
                        </div>
                        <div class="w-full md:w-auto">
                            <button class="w-full md:w-auto text-sm font-bold bg-slate-100 text-slate-600 px-5 py-2.5 rounded-lg hover:bg-slate-200 transition-colors" onclick="clearFilters()">清除</button>
                        </div>
                    </div>

                    <!-- ✨ 獨立的捲動清單區 ✨ -->
                    <div id="recordsContainer" class="scroll-container flex-1">
                        <!-- 資料會載入到這裡 -->
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Pure JavaScript 核心邏輯 -->
    <script>
        // === 1. 系統變數與初始化 ===
        let records = [];
        let deleteId = null;

        document.addEventListener('DOMContentLoaded', () => {
            // 設定今日日期
            const dateInput = document.getElementById('inputDate');
            if (dateInput) {
                const today = new Date();
                const offset = today.getTimezoneOffset() * 60000;
                dateInput.value = (new Date(Date.now() - offset)).toISOString().split('T')[0];
            }

            // 讀取本地儲存
            try {
                if (window.localStorage) {
                    const saved = window.localStorage.getItem('med_records_visual_v1');
                    if (saved) records = JSON.parse(saved) || [];
                }
            } catch(e) {
                console.warn("LocalStorage blocked.");
            }

            renderList();
        });

        // === 2. 表單互動邏輯 ===
        window.handleSelectionChange = function() {
            const sel = document.getElementById('inputSubcategory');
            const opt = sel.options[sel.selectedIndex];
            if (!opt) return;
            
            const fixedAmt = opt.getAttribute('data-amt');
            const amtInp = document.getElementById('inputAmount');
            const hint = document.getElementById('fixedHint');
            
            if(fixedAmt) {
                amtInp.value = fixedAmt; 
                amtInp.readOnly = true;
                amtInp.style.backgroundColor = 'var(--slate-100)';
                amtInp.style.color = 'var(--slate-400)';
                if (hint) hint.classList.remove('hidden');
            } else {
                amtInp.value = ''; 
                amtInp.readOnly = false;
                amtInp.style.backgroundColor = 'white';
                amtInp.style.color = 'var(--slate-800)';
                if (hint) hint.classList.add('hidden');
            }
            window.updateUI();
        };

        window.updateUI = function() {
            const isClaimed = document.getElementById('inputClaimed').checked;
            const sel = document.getElementById('inputSubcategory');
            const opt = sel.options[sel.selectedIndex];
            const cat = opt ? opt.getAttribute('data-cat') : '';
            
            const label = document.getElementById('claimLabel');
            if (label) {
                if(isClaimed) label.classList.add('active');
                else label.classList.remove('active');
            }
            
            const physioHint = document.getElementById('physioHint');
            if (physioHint) {
                physioHint.style.display = (isClaimed && cat === '物理治療費') ? "flex" : "none";
            }
        };

        // === 3. 新增與刪除 ===
        window.handleAdd = function() {
            const date = document.getElementById('inputDate').value;
            const sel = document.getElementById('inputSubcategory');
            const sub = sel.value;
            const opt = sel.options[sel.selectedIndex];
            const cat = opt ? opt.getAttribute('data-cat') : '';
            const amount = parseFloat(document.getElementById('inputAmount').value);
            const claimed = document.getElementById('inputClaimed').checked;

            if (!sub || !cat || isNaN(amount) || amount <= 0 || !date) {
                alert("請填寫完整資訊 (日期、項目及金額)");
                return;
            }

            const newId = 'id_' + Date.now() + '_' + Math.floor(Math.random() * 10000);
            
            records.push({
                id: newId,
                date: date,
                category: cat,
                subcategory: sub,
                amount: amount,
                isClaimed: claimed,
                createdAt: Date.now()
            });

            saveData();
            
            // 清空表單
            sel.selectedIndex = 0;
            document.getElementById('inputAmount').value = '';
            document.getElementById('inputClaimed').checked = false;
            window.handleSelectionChange();
            
            renderList();
        };

        window.initiateDelete = function(id) { 
            deleteId = id; 
            document.getElementById('deleteModal').classList.add('show'); 
        };
        
        window.closeDeleteModal = function() { 
            deleteId = null; 
            document.getElementById('deleteModal').classList.remove('show'); 
        };
        
        window.confirmDelete = function() {
            if (deleteId) {
                records = records.filter(r => r.id !== deleteId);
                saveData();
                renderList();
            }
            closeDeleteModal();
        };

        function saveData() {
            try {
                if (window.localStorage) {
                    window.localStorage.setItem('med_records_visual_v1', JSON.stringify(records));
                }
            } catch(e) {}
        }

        window.clearFilters = function() {
            document.getElementById('filterCategory').value = '全部';
            document.getElementById('filterStart').value = '';
            document.getElementById('filterEnd').value = '';
            renderList();
        }

        // === 4. 畫面渲染 (重點還原截圖排版) ===
        window.renderList = function() {
            // 排序與計算理賠
            let sorted = [...records].sort((a, b) => new Date(a.date) - new Date(b.date));
            let physioCount = 0;
            
            let processed = sorted.map(r => {
                let claimAmt = 0; 
                let note = '';
                
                if(r.isClaimed) {
                    if(r.category === '西醫治療費') { 
                        claimAmt = r.amount; 
                    }
                    else if(r.category === '物理治療費') {
                        if(physioCount < 10) { 
                            claimAmt = Math.min(r.amount, 400); 
                            physioCount++; 
                            note = `(${physioCount}/10)`; 
                        } else { 
                            claimAmt = 0; 
                            note = '已達上限'; 
                        }
                    }
                }
                return { ...r, claimAmt, selfAmt: (r.amount || 0) - claimAmt, note };
            });

            // 列表排序：日期新到舊
            processed.sort((a, b) => new Date(b.date).getTime() - new Date(a.date).getTime() || (b.createdAt || 0) - (a.createdAt || 0));

            // 篩選過濾
            const fCat = document.getElementById('filterCategory').value;
            const fStart = document.getElementById('filterStart').value;
            const fEnd = document.getElementById('filterEnd').value;

            const filtered = processed.filter(r => {
                const matchCat = (fCat === '全部' || r.category === fCat);
                const matchStart = (!fStart || r.date >= fStart);
                const matchEnd = (!fEnd || r.date <= fEnd);
                return matchCat && matchStart && matchEnd;
            });

            // 更新上方統計面板
            let tE = 0, tC = 0, tS = 0, pC = 0;
            filtered.forEach(r => { tE += (r.amount || 0); tC += r.claimAmt; tS += r.selfAmt; });
            processed.forEach(r => { if(r.category === '物理治療費' && r.isClaimed && r.claimAmt > 0) pC++; });

            document.getElementById('statTotal').innerText = '$' + tE.toLocaleString();
            document.getElementById('statClaimable').innerText = '$' + tC.toLocaleString();
            document.getElementById('statSelfPaid').innerText = '$' + tS.toLocaleString();
            document.getElementById('statPhysio').innerText = pC;
            document.getElementById('statPhysioBar').style.width = (pC * 10) + '%';
            if(pC >= 10) document.getElementById('statPhysioBar').classList.add('danger');
            else document.getElementById('statPhysioBar').classList.remove('danger');
            
            document.getElementById('recordCount').innerText = `共 ${filtered.length} 筆`;

            // 渲染清單容器 (支援拉上拉下)
            const container = document.getElementById('recordsContainer');
            if(filtered.length === 0) {
                container.innerHTML = `
                    <div class="flex flex-col items-center justify-center py-24 text-slate-300">
                        <svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="mb-4 opacity-50"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"></polyline></svg>
                        <p class="font-black text-lg text-slate-400">沒有找到任何紀錄</p>
                    </div>`;
                return;
            }

            let htmlString = '';
            filtered.forEach(r => {
                const isWest = r.category === '西醫治療費';
                const decoClass = isWest ? 'deco-west' : 'deco-physio';
                const tagClass = isWest ? 'tag-west' : 'tag-physio';
                const claimBadge = r.isClaimed ? `<span class="tag tag-claim flex items-center gap-1"><svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><polyline points="20 6 9 17 4 12"></polyline></svg> 已申報</span>` : '';
                
                // 還原截圖的三直排金額設計
                htmlString += `
                <div class="record-card">
                    <div class="record-deco ${decoClass} rounded-l-xl"></div>
                    
                    <!-- 左半部：標籤與項目名稱 -->
                    <div class="flex-1 pl-2 md:pl-4 flex flex-col justify-center">
                        <div class="flex items-center gap-2 mb-2 flex-wrap">
                            <span class="tag ${tagClass}">${r.category}</span>
                            ${claimBadge}
                        </div>
                        <div class="font-black text-slate-800 text-lg md:text-xl tracking-tight">${r.subcategory}</div>
                        <div class="text-xs text-slate-400 font-bold mt-1.5 flex items-center gap-1.5">
                            ${r.date} ${r.note ? `<span class="${r.note === '已達上限' ? 'text-red-500' : 'text-slate-400'}">· ${r.note}</span>` : ''}
                        </div>
                    </div>
                    
                    <!-- 右半部：金額與刪除 -->
                    <div class="flex items-center justify-between md:justify-end gap-4 md:gap-8 w-full md:w-auto border-t md:border-none pt-4 md:pt-0 mt-2 md:mt-0">
                        <div class="flex flex-col items-center md:items-end min-w-[70px]">
                            <span class="text-[11px] font-black text-slate-500 mb-1 tracking-widest">總開支</span>
                            <span class="font-black text-slate-800 text-xl">$${r.amount}</span>
                        </div>
                        <div class="flex flex-col items-center md:items-end min-w-[70px]">
                            <span class="text-[11px] font-black text-emerald-600 mb-1 tracking-widest">理賠金額</span>
                            <span class="font-black text-emerald-600 text-xl">$${r.claimAmt}</span>
                        </div>
                        <div class="flex flex-col items-center md:items-end min-w-[70px]">
                            <span class="text-[11px] font-black text-orange-500 mb-1 tracking-widest">自費金額</span>
                            <span class="font-black text-orange-500 text-xl">$${r.selfAmt}</span>
                        </div>
                        
                        <div class="pl-2 md:border-l border-slate-100 h-full flex items-center">
                            <button onclick="initiateDelete('${r.id}')" class="p-2.5 text-slate-300 hover:text-red-500 hover:bg-red-50 rounded-lg transition-colors" title="刪除此記錄">
                                <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="3 6 5 6 21 6"></polyline><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path><line x1="10" y1="11" x2="10" y2="17"></line><line x1="14" y1="11" x2="14" y2="17"></line></svg>
                            </button>
                        </div>
                    </div>
                </div>
                `;
            });
            container.innerHTML = htmlString;
        };

        // === 5. 匯出 CSV ===
        window.exportToCSV = function() {
            if (records.length === 0) return alert("無資料可匯出");
            const headers = "日期,類別,治療項目,總額,是否理賠\n";
            const rows = records.map(r => `${r.date},${r.category},"${r.subcategory}",${r.amount},${r.isClaimed?'是':'否'}`).join('\n');
            const blob = new Blob(["\uFEFF" + headers + rows], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = `醫療單據_${new Date().toISOString().split('T')[0]}.csv`;
            
            try {
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
            } catch(e) {
                alert("由於 iPad 系統限制，請點擊右上角分享按鈕，在 Safari 中開啟以下載檔案。");
            }
        };
    </script>
</body>
</html>



