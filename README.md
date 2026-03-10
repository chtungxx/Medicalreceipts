[請於Koder開啟.html](https://github.com/user-attachments/files/25868799/Koder.html)
<!DOCTYPE html>
<html lang="zh-HK">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>醫療開支與保險理賠追蹤</title>
    
    <style>
        /* 基礎顏色與排版 (純手工編寫，不依賴任何外部連結) */
        :root {
            --slate-50: #f8fafc; --slate-100: #f1f5f9; --slate-200: #e2e8f0; 
            --slate-400: #94a3b8; --slate-500: #64748b; --slate-700: #334155; --slate-800: #1e293b;
            --teal-50: #f0fdfa; --teal-600: #0d9488; --teal-700: #0f766e;
            --orange-50: #fff7ed; --orange-100: #ffedd5; --orange-600: #ea580c;
            --blue-50: #eff6ff; --blue-100: #dbeafe; --blue-500: #3b82f6; --blue-600: #2563eb; --blue-700: #1d4ed8;
            --red-50: #fef2f2; --red-500: #ef4444; --red-600: #dc2626;
        }
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; background-color: var(--slate-50); color: var(--slate-800); line-height: 1.5; -webkit-font-smoothing: antialiased; padding: 20px; padding-bottom: 80px; }
        
        /* 頂部 Header */
        .container { max-width: 1100px; margin: 0 auto; }
        .header { display: flex; flex-direction: column; justify-content: space-between; align-items: flex-start; gap: 15px; padding-bottom: 20px; border-bottom: 1px solid var(--slate-200); margin-bottom: 24px; }
        @media(min-width: 600px) { .header { flex-direction: row; align-items: center; } }
        .header-title-box { display: flex; align-items: center; gap: 15px; }
        .header-icon { background: linear-gradient(135deg, #14b8a6, #0f766e); color: white; padding: 12px; border-radius: 12px; display: flex; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        .header h1 { font-size: 24px; font-weight: 800; }
        .header p { font-size: 14px; color: var(--slate-500); margin-top: 4px; font-weight: bold; }
        
        /* 按鈕設計 */
        .btn { display: inline-flex; align-items: center; justify-content: center; gap: 8px; padding: 10px 20px; border-radius: 8px; font-weight: bold; cursor: pointer; transition: all 0.2s; border: none; font-size: 15px; outline: none; }
        .btn-export { background-color: var(--slate-800); color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        .btn-export:active { transform: translateY(2px); }
        .btn-primary { background: linear-gradient(to right, var(--teal-600), var(--teal-700)); color: white; width: 100%; padding: 16px; border-radius: 12px; font-size: 18px; margin-top: 10px; box-shadow: 0 4px 10px rgba(13, 148, 136, 0.3); }
        .btn-danger { background-color: var(--red-600); color: white; width: 100%; padding: 12px;}
        .btn-cancel { background-color: var(--slate-100); color: var(--slate-700); width: 100%; padding: 12px;}
        
        /* Dashboard 統計版面 */
        .dashboard { display: grid; grid-template-columns: 1fr; gap: 20px; margin-bottom: 24px; }
        @media(min-width: 600px) { .dashboard { grid-template-columns: 1fr 1fr; } }
        @media(min-width: 900px) { .dashboard { grid-template-columns: 1fr 1fr 1fr 1fr; } }
        .stat-card { background: white; padding: 24px; border-radius: 16px; box-shadow: 0 2px 4px rgba(0,0,0,0.05); border: 1px solid var(--slate-200); }
        .stat-card.teal { background: linear-gradient(135deg, var(--teal-50), white); border-color: #ccfbf1; }
        .stat-card.orange { background: linear-gradient(135deg, var(--orange-50), white); border-color: var(--orange-100); }
        .stat-card.blue { background: linear-gradient(135deg, var(--blue-50), white); border-color: var(--blue-100); position: relative; overflow: hidden;}
        .stat-title { display: flex; align-items: center; gap: 8px; font-size: 14px; font-weight: bold; margin-bottom: 10px; }
        .stat-value { font-size: 32px; font-weight: 900; }
        .progress-bg { width: 100%; height: 8px; background-color: #bfdbfe; border-radius: 4px; margin-top: 12px; overflow: hidden; }
        .progress-bar { height: 100%; background-color: var(--blue-600); transition: width 0.5s ease; }
        .progress-bar.danger { background-color: var(--red-500); }
        
        /* 左右排版 */
        .main-layout { display: grid; grid-template-columns: 1fr; gap: 24px; }
        @media(min-width: 1000px) { .main-layout { grid-template-columns: 360px 1fr; } }
        .panel { background: white; border-radius: 16px; padding: 24px; box-shadow: 0 2px 4px rgba(0,0,0,0.05); border: 1px solid var(--slate-200); }
        .panel-title { display: flex; align-items: center; gap: 8px; font-size: 20px; font-weight: 900; margin-bottom: 20px; color: var(--slate-800); }
        
        /* 表單設計 */
        .form-group { margin-bottom: 18px; }
        .form-label { display: block; font-size: 14px; font-weight: bold; color: var(--slate-700); margin-bottom: 8px; }
        .form-control { width: 100%; padding: 14px; background-color: var(--slate-50); border: 1px solid var(--slate-200); border-radius: 10px; font-size: 16px; font-family: inherit; color: var(--slate-800); outline: none; font-weight: 500;}
        .form-control:focus { background-color: white; border-color: var(--teal-600); box-shadow: 0 0 0 3px #ccfbf1; }
        .form-control[readonly] { background-color: var(--slate-100); color: var(--slate-500); cursor: not-allowed; }
        select.form-control { appearance: none; background-image: url("data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%2364748b%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-12.9%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2012.9l128%20127.9c3.6%203.6%207.8%205.4%2012.8%205.4s9.2-1.8%2012.8-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-12.8%200-5-1.9-9.2-5.5-12.8z%22%2F%3E%3C%2Fsvg%3E"); background-repeat: no-repeat; background-position: right 14px top 50%; background-size: 12px auto; }
        optgroup { font-weight: bold; color: var(--teal-700); background: white;}
        option { font-weight: normal; color: var(--slate-800); }
        
        .input-group { position: relative; }
        .input-prefix { position: absolute; left: 16px; top: 14px; color: var(--slate-500); font-weight: bold; font-size: 18px; }
        .input-group .form-control { padding-left: 36px; font-weight: 800; font-size: 18px; }
        
        /* 理賠選項區塊 */
        .claim-box { display: flex; align-items: flex-start; gap: 12px; padding: 16px; background-color: var(--slate-50); border: 2px solid var(--slate-200); border-radius: 12px; cursor: pointer; margin-top: 24px; margin-bottom: 8px;}
        .claim-box.active { background-color: var(--teal-50); border-color: #5eead4; }
        .claim-box input[type="checkbox"] { width: 22px; height: 22px; margin-top: 2px; accent-color: var(--teal-600); }
        .claim-box-title { font-size: 15px; font-weight: bold; color: var(--slate-700); display: block;}
        .claim-box.active .claim-box-title { color: var(--teal-700); }
        .claim-box-desc { font-size: 12px; color: var(--slate-500); margin-top: 4px; display: block;}
        
        .hint-box { background-color: #eff6ff; color: var(--blue-600); padding: 12px; border-radius: 8px; border: 1px solid var(--blue-100); font-size: 13px; font-weight: 500; display: flex; align-items: flex-start; gap: 8px; display: none; margin-bottom: 16px;}
        .hint-red { color: var(--red-500); font-size: 13px; font-weight: bold; margin-top: 8px; display: flex; align-items: center; gap: 6px; }

        /* 警告橫幅 */
        #jsWarning { display: none; }

        /* 篩選與列表設計 */
        .filter-bar { display: flex; flex-direction: column; gap: 15px; background-color: var(--slate-50); padding: 16px; border-radius: 12px; margin-bottom: 20px; border: 1px solid var(--slate-200); }
        @media(min-width: 700px) { .filter-bar { flex-direction: row; align-items: flex-end; } }
        .filter-item { flex: 1; }
        .filter-label { font-size: 12px; font-weight: 800; color: var(--slate-500); margin-bottom: 6px; display: block; text-transform: uppercase; }
        
        .records-list { display: flex; flex-direction: column; gap: 12px; max-height: 600px; overflow-y: auto; padding-right: 5px; }
        .record-card { display: flex; flex-direction: column; justify-content: space-between; padding: 20px; background: white; border: 1px solid var(--slate-200); border-radius: 12px; position: relative; overflow: hidden; gap: 16px; }
        @media(min-width: 600px) { .record-card { flex-direction: row; align-items: center; } }
        .record-deco { position: absolute; left: 0; top: 0; bottom: 0; width: 6px; }
        .record-deco.west { background-color: #818cf8; }
        .record-deco.physio { background-color: #60a5fa; }
        .record-info { flex: 1; padding-left: 8px; }
        .record-tags { display: flex; flex-wrap: wrap; align-items: center; gap: 8px; margin-bottom: 10px; }
        .tag { padding: 4px 8px; border-radius: 6px; font-size: 12px; font-weight: 800; display: inline-flex; align-items: center; gap: 4px; }
        .tag-date { background-color: var(--slate-100); color: var(--slate-500); }
        .tag-cat.west { background-color: #e0e7ff; color: #4338ca; border: 1px solid #c7d2fe; }
        .tag-cat.physio { background-color: #dbeafe; color: #1d4ed8; border: 1px solid #bfdbfe; }
        .tag-claim { background-color: var(--teal-50); color: var(--teal-700); border: 1px solid #ccfbf1; }
        .record-title { font-size: 18px; font-weight: 900; color: var(--slate-800); }
        .record-note { font-size: 12px; font-weight: bold; display: inline-flex; align-items: center; gap: 4px; margin-top: 8px; }
        .record-note.gray { color: var(--slate-500); }
        .record-note.red { color: var(--red-500); background: var(--red-50); padding: 2px 6px; border-radius: 4px; border: 1px solid #fecaca; }
        
        .record-amounts { display: flex; align-items: center; gap: 20px; border-top: 1px solid var(--slate-100); padding-top: 16px; padding-left: 8px; }
        @media(min-width: 600px) { .record-amounts { border-top: none; padding-top: 0; padding-left: 0; gap: 24px; } }
        .amt-box { display: flex; flex-direction: column; min-width: 60px; }
        @media(min-width: 600px) { .amt-box { align-items: flex-end; } }
        .amt-label { font-size: 10px; font-weight: 900; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 4px; }
        .amt-value { font-size: 18px; font-weight: 900; }
        .amt-box.total .amt-label { color: var(--slate-400); }
        .amt-box.total .amt-value { color: var(--slate-800); }
        .amt-box.claim .amt-label { color: var(--teal-600); }
        .amt-box.claim .amt-value { color: var(--teal-600); }
        .amt-box.self .amt-label { color: var(--orange-600); }
        .amt-box.self .amt-value { color: var(--orange-600); }
        
        .btn-delete { background: transparent; border: none; color: var(--slate-300); padding: 8px; cursor: pointer; border-radius: 8px; }
        
        .empty-state { text-align: center; padding: 60px 20px; color: var(--slate-400); }
        .empty-state h3 { font-size: 18px; font-weight: bold; color: var(--slate-500); margin-top: 12px; }
        
        /* 彈窗設計 */
        .modal-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background-color: rgba(15, 23, 42, 0.6); display: flex; align-items: center; justify-content: center; z-index: 100; opacity: 0; pointer-events: none; transition: opacity 0.2s; backdrop-filter: blur(4px); }
        .modal-overlay.show { opacity: 1; pointer-events: auto; }
        .modal { background: white; padding: 24px; border-radius: 20px; width: 90%; max-width: 350px; text-align: center; transform: scale(0.9); transition: transform 0.2s; box-shadow: 0 20px 25px -5px rgba(0,0,0,0.1); }
        .modal-overlay.show .modal { transform: scale(1); }
        .modal-icon { width: 56px; height: 56px; background-color: var(--red-50); color: var(--red-600); border-radius: 50%; display: flex; align-items: center; justify-content: center; margin: 0 auto 16px; }
        .modal h3 { font-size: 20px; font-weight: bold; margin-bottom: 8px; }
        .modal p { font-size: 14px; color: var(--slate-500); margin-bottom: 24px; }
        .modal-actions { display: flex; gap: 12px; }
    </style>
</head>
<body>

    <!-- 刪除確認彈窗 -->
    <div id="deleteModal" class="modal-overlay">
        <div class="modal">
            <div class="modal-icon">
                <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m21.73 18-8-14a2 2 0 0 0-3.48 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3Z"></path><line x1="12" y1="9" x2="12" y2="13"></line><line x1="12" y1="17" x2="12.01" y2="17"></line></svg>
            </div>
            <h3>確定要刪除這筆記錄？</h3>
            <p>此操作無法復原。刪除後，理賠金額將會重新計算。</p>
            <div class="modal-actions">
                <button class="btn btn-cancel" onclick="closeDeleteModal()">取消</button>
                <button class="btn btn-danger" onclick="confirmDelete()">確認刪除</button>
            </div>
        </div>
    </div>

    <div class="container">
        
        <!-- 頂部 Header -->
        <div class="header">
            <div class="header-title-box">
                <div class="header-icon">
                    <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"></polyline></svg>
                </div>
                <div>
                    <h1>醫療開支與保險理賠</h1>
                    <p>已啟用 iPad 特製匯出功能 (分享選單)</p>
                </div>
            </div>
            <!-- 按下這裡會觸發新的分享功能 -->
            <button class="btn btn-export" onclick="exportToCSV()">
                <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path><polyline points="7 10 12 15 17 10"></polyline><line x1="12" y1="15" x2="12" y2="3"></line></svg>
                匯出 Excel
            </button>
        </div>

        <!-- 統計面板 -->
        <div class="dashboard">
            <div class="stat-card">
                <div class="stat-title" style="color: var(--slate-500)"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="4" y="2" width="16" height="20" rx="2" ry="2"></rect><line x1="8" y1="6" x2="16" y2="6"></line><line x1="16" y1="14" x2="16" y2="18"></line></svg> 總醫療開支</div>
                <div id="statTotal" class="stat-value" style="color: var(--slate-800)">$0</div>
            </div>
            <div class="stat-card teal">
                <div class="stat-title" style="color: var(--teal-700)"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z"></path></svg> 保險可理賠</div>
                <div id="statClaimable" class="stat-value" style="color: var(--teal-600)">$0</div>
            </div>
            <div class="stat-card orange">
                <div class="stat-title" style="color: var(--orange-700)"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="12" y1="1" x2="12" y2="23"></line><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"></path></svg> 自行付費</div>
                <div id="statSelfPaid" class="stat-value" style="color: var(--orange-600)">$0</div>
            </div>
            <div class="stat-card blue">
                <div class="stat-title" style="color: var(--blue-700)"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="12"></line></svg> 物理治療次數</div>
                <div style="display: flex; align-items: baseline; gap: 4px;">
                    <span id="statPhysio" class="stat-value" style="color: var(--blue-700)">0</span>
                    <span style="color: var(--blue-500); font-weight: bold;">/ 10 次</span>
                </div>
                <div class="progress-bg"><div id="statPhysioBar" class="progress-bar" style="width: 0%;"></div></div>
            </div>
        </div>

        <div class="main-layout">
            <!-- 左側表單 -->
            <div>
                <div class="panel">
                    <div class="panel-title">
                        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="var(--teal-600)" stroke-width="2"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="16"></line><line x1="8" y1="12" x2="16" y2="12"></line></svg>
                        新增醫療開支
                    </div>

                    <div class="form-group">
                        <label class="form-label">日期</label>
                        <input type="date" id="inputDate" class="form-control">
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">治療項目 (請選擇)</label>
                        <select id="inputSubcategory" class="form-control" onchange="handleSelectionChange()">
                            <option value="" disabled selected>請選擇項目...</option>
                            <optgroup label="西醫治療費">
                                <option value="MRI檢查費" data-cat="西醫治療費">MRI檢查費 (自填金額)</option>
                                <option value="傷口護理費用" data-cat="西醫治療費" data-amount="50">傷口護理費用 ($50)</option>
                                <option value="政府門診" data-cat="西醫治療費">政府門診 (自填金額)</option>
                                <option value="私家門診" data-cat="西醫治療費">私家門診 (自填金額)</option>
                                <option value="骨科門診" data-cat="西醫治療費" data-amount="250">骨科門診 ($250)</option>
                            </optgroup>
                            <optgroup label="物理治療費">
                                <option value="私人物理治療" data-cat="物理治療費">私人物理治療 (自填金額)</option>
                                <option value="政府物理治療" data-cat="物理治療費" data-amount="250">政府物理治療 ($250)</option>
                            </optgroup>
                        </select>
                    </div>

                    <div class="form-group">
                        <label class="form-label">所屬類別 (自動判定)</label>
                        <input type="text" id="inputCategory" class="form-control" readonly placeholder="選擇項目後自動顯示" style="background-color: var(--slate-100); color: var(--slate-500); font-weight:bold;">
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">金額 (HKD)</label>
                        <div class="input-group">
                            <span class="input-prefix">$</span>
                            <input type="number" id="inputAmount" min="1" class="form-control" placeholder="0">
                        </div>
                        <div id="fixedAmountHint" class="hint-red" style="display: none;">
                            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="12"></line><line x1="12" y1="16" x2="12.01" y2="16"></line></svg>
                            此項目為固定收費，已自動填入
                        </div>
                    </div>
                    
                    <label id="claimLabel" class="claim-box">
                        <input type="checkbox" id="inputClaimed" onchange="handleClaimToggle()">
                        <div class="claim-box-texts">
                            <span class="claim-box-title">準備申請保險理賠</span>
                            <span class="claim-box-desc">勾選後系統會自動為您計算理賠額</span>
                        </div>
                    </label>
                    
                    <div id="physioHint" class="hint-box">
                        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="flex-shrink:0"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="12"></line></svg>
                        <span>提示：物理治療每次最多理賠 <b>$400</b>，總期上限 <b>10 次</b>。</span>
                    </div>
                    
                    <button type="button" class="btn btn-primary" onclick="handleAddRecord()">
                        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="16"></line><line x1="8" y1="12" x2="16" y2="12"></line></svg>
                        立即加入記錄
                    </button>
                </div>
            </div>

            <!-- 右側列表 -->
            <div>
                <div class="panel" style="min-height: 600px;">
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px;">
                        <h2 style="font-size: 20px; font-weight: 900;">開支明細記錄</h2>
                        <span id="recordCount" style="background: var(--teal-600); color: white; padding: 4px 12px; border-radius: 20px; font-size: 14px; font-weight: bold;">共 0 筆</span>
                    </div>
                    
                    <div class="filter-bar">
                        <div class="filter-item">
                            <label class="filter-label">篩選類別</label>
                            <select id="filterCategory" class="form-control" onchange="renderList()">
                                <option value="全部">顯示全部</option>
                                <option value="西醫治療費">西醫治療費</option>
                                <option value="物理治療費">物理治療費</option>
                            </select>
                        </div>
                        <div class="filter-item">
                            <label class="filter-label">日期從</label>
                            <input type="date" id="filterStart" class="form-control" onchange="renderList()">
                        </div>
                        <div class="filter-item">
                            <label class="filter-label">日期至</label>
                            <input type="date" id="filterEnd" class="form-control" onchange="renderList()">
                        </div>
                        <div style="display: flex; align-items: flex-end;">
                            <button class="btn btn-cancel" style="padding: 12px; margin-bottom: 0;" onclick="clearFilters()">清除</button>
                        </div>
                    </div>

                    <div id="recordsContainer" class="records-list">
                        <!-- 記錄會顯示在這裡 -->
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        var records = [];

        // 立即設定今日日期
        var today = new Date();
        var tzOffset = today.getTimezoneOffset() * 60000;
        var localDate = new Date(today.getTime() - tzOffset).toISOString().split('T')[0];
        document.getElementById('inputDate').value = localDate;

        // 嘗試讀取本地儲存
        try {
            if (window.localStorage) {
                var saved = window.localStorage.getItem('med_records_final');
                if (saved) { records = JSON.parse(saved) || []; }
            }
        } catch(e) {}

        // 渲染列表
        renderList();

        // 處理下拉選單變更
        function handleSelectionChange() {
            var select = document.getElementById("inputSubcategory");
            var option = select.options[select.selectedIndex];
            if (!option || option.value === "") return;

            var cat = option.getAttribute("data-cat");
            var fixedAmount = option.getAttribute("data-amount");

            document.getElementById("inputCategory").value = cat;

            var amtInput = document.getElementById("inputAmount");
            var hintFixed = document.getElementById("fixedAmountHint");
            
            if (fixedAmount) {
                amtInput.value = fixedAmount;
                amtInput.readOnly = true;
                amtInput.style.backgroundColor = "var(--slate-100)";
                amtInput.style.color = "var(--slate-500)";
                hintFixed.style.display = "flex";
            } else {
                amtInput.value = "";
                amtInput.readOnly = false;
                amtInput.style.backgroundColor = "white";
                amtInput.style.color = "var(--slate-800)";
                hintFixed.style.display = "none";
            }

            handleClaimToggle();
        }

        function handleClaimToggle() {
            var isClaimed = document.getElementById("inputClaimed").checked;
            var cat = document.getElementById("inputCategory").value;
            var labelBox = document.getElementById("claimLabel");
            var physioHint = document.getElementById("physioHint");

            if (isClaimed) {
                labelBox.classList.add("active");
                if (cat === "物理治療費") {
                    physioHint.style.display = "flex";
                } else {
                    physioHint.style.display = "none";
                }
            } else {
                labelBox.classList.remove("active");
                physioHint.style.display = "none";
            }
        }

        function handleAddRecord() {
            var date = document.getElementById("inputDate").value;
            var sub = document.getElementById("inputSubcategory").value;
            var cat = document.getElementById("inputCategory").value;
            var amount = parseFloat(document.getElementById("inputAmount").value);
            var isClaimed = document.getElementById("inputClaimed").checked;

            if (!sub || !cat || isNaN(amount) || amount <= 0) {
                alert("請先選擇治療項目並確認金額正確！");
                return;
            }

            var id = 'id_' + new Date().getTime() + '_' + Math.floor(Math.random() * 1000);
            records.push({ id: id, date: date, category: cat, subcategory: sub, amount: amount, isClaimed: isClaimed });
            saveData();
            
            document.getElementById("inputSubcategory").selectedIndex = 0;
            document.getElementById("inputCategory").value = "";
            document.getElementById("inputAmount").value = "";
            document.getElementById("inputClaimed").checked = false;
            handleSelectionChange(); 
            renderList();
        }

        function deleteRecord(id) {
            if (confirm("確定要刪除這筆記錄嗎？(刪除後理賠金額會重新計算)")) {
                var newRecords = [];
                for (var i = 0; i < records.length; i++) {
                    if (records[i].id !== id) newRecords.push(records[i]);
                }
                records = newRecords;
                saveData();
                renderList();
            }
        }

        function saveData() {
            try {
                if (window.localStorage) {
                    window.localStorage.setItem('med_records_final', JSON.stringify(records));
                }
            } catch(e) {}
        }

        function clearFilters() {
            document.getElementById('filterCategory').value = '全部';
            document.getElementById('filterStart').value = '';
            document.getElementById('filterEnd').value = '';
            renderList();
        }

        function renderList() {
            var sorted = [];
            for (var i = 0; i < records.length; i++) { sorted.push(records[i]); }
            sorted.sort(function(a, b) { return new Date(a.date) - new Date(b.date); });
            
            var physioCount = 0;
            var processed = [];
            
            for (var j = 0; j < sorted.length; j++) {
                var r = sorted[j];
                var claimAmt = 0;
                var note = '';
                
                if (r.isClaimed) {
                    if (r.category === '西醫治療費') {
                        claimAmt = r.amount; note = '全額理賠';
                    } else if (r.category === '物理治療費') {
                        if (physioCount < 10) {
                            claimAmt = Math.min(r.amount, 400); physioCount++; note = '已用額度 (' + physioCount + '/10)';
                        } else {
                            claimAmt = 0; note = '已達10次理賠上限';
                        }
                    }
                } else {
                    note = '不申請理賠';
                }
                processed.push({ id: r.id, date: r.date, category: r.category, subcategory: r.subcategory, amount: r.amount, isClaimed: r.isClaimed, claimAmt: claimAmt, selfAmt: r.amount - claimAmt, note: note });
            }

            processed.sort(function(a, b) { return new Date(b.date) - new Date(a.date); });

            var fCat = document.getElementById('filterCategory').value;
            var fStart = document.getElementById('filterStart').value;
            var fEnd = document.getElementById('filterEnd').value;

            var filtered = [];
            for (var k = 0; k < processed.length; k++) {
                var pr = processed[k];
                if ((fCat === '全部' || pr.category === fCat) && (!fStart || pr.date >= fStart) && (!fEnd || pr.date <= fEnd)) {
                    filtered.push(pr);
                }
            }

            var tExp = 0, tClaim = 0, tSelf = 0, tPhysio = 0;
            for (var m = 0; m < filtered.length; m++) {
                tExp += filtered[m].amount; tClaim += filtered[m].claimAmt; tSelf += filtered[m].selfAmt;
            }
            for (var n = 0; n < processed.length; n++) {
                if (processed[n].category === '物理治療費' && processed[n].isClaimed && processed[n].claimAmt > 0) tPhysio++;
            }

            document.getElementById('statTotal').innerText = '$' + tExp.toLocaleString();
            document.getElementById('statClaimable').innerText = '$' + tClaim.toLocaleString();
            document.getElementById('statSelfPaid').innerText = '$' + tSelf.toLocaleString();
            document.getElementById('statPhysio').innerText = tPhysio;
            
            var pBar = document.getElementById('statPhysioBar');
            pBar.style.width = Math.min(tPhysio * 10, 100) + '%';
            if (tPhysio >= 10) pBar.classList.add('danger'); else pBar.classList.remove('danger');

            document.getElementById('recordCount').innerText = '共 ' + filtered.length + ' 筆';

            var container = document.getElementById('recordsContainer');
            if (filtered.length === 0) {
                container.innerHTML = '<div class="empty-state"><svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"></polyline></svg><h3>這裡空空如也</h3><p>請在左側表單輸入您的第一筆醫療開支</p></div>';
                return;
            }

            var html = '';
            for (var x = 0; x < filtered.length; x++) {
                var fr = filtered[x];
                var isWest = (fr.category === '西醫治療費');
                var tagClass = isWest ? 'west' : 'physio';
                var noteColor = (fr.note === '已達10次理賠上限') ? 'red' : 'gray';
                var claimBadge = fr.isClaimed ? '<span class="tag tag-claim">✓ 已申報</span>' : '';

                html += '<div class="record-card"><div class="record-deco ' + tagClass + '"></div><div class="record-info"><div class="record-tags"><span class="tag tag-date">📅 ' + fr.date + '</span><span class="tag tag-cat ' + tagClass + '">' + fr.category + '</span>' + claimBadge + '</div><div class="record-title">' + fr.subcategory + '</div>';
                if (fr.note) { html += '<div class="record-note ' + noteColor + '">' + fr.note + '</div>'; }
                html += '</div><div class="record-amounts"><div class="amt-box total"><span class="amt-label">總開支</span><span class="amt-value">$' + fr.amount + '</span></div><div class="amt-box claim"><span class="amt-label">理賠金額</span><span class="amt-value">$' + fr.claimAmt + '</span></div><div class="amt-box self"><span class="amt-label">自費金額</span><span class="amt-value">$' + fr.selfAmt + '</span></div><button class="btn-delete" onclick="deleteRecord(\'' + fr.id + '\')" title="刪除">🗑️</button></div></div>';
            }
            container.innerHTML = html;
        }

        // ==========================================
        // 匯出 Excel (加入 iPad/Koder 專屬分享功能)
        // ==========================================
        function exportToCSV() {
            if (records.length === 0) return alert("目前沒有記錄可匯出！");
            
            var sorted = [];
            for (var i = 0; i < records.length; i++) { sorted.push(records[i]); }
            sorted.sort(function(a, b) { return new Date(a.date) - new Date(b.date); });
            
            var pCount = 0;
            var rows = [];
            for (var j = 0; j < sorted.length; j++) {
                var r = sorted[j];
                var cAmt = 0, note = '不申請理賠';
                if (r.isClaimed) {
                    if (r.category === '西醫治療費') { cAmt = r.amount; note = '全額理賠'; }
                    else if (r.category === '物理治療費') {
                        if (pCount < 10) { cAmt = Math.min(r.amount, 400); pCount++; note = '已用額度 (' + pCount + '/10)'; }
                        else { note = '已達10次理賠上限'; }
                    }
                }
                rows.push([r.date, r.category, '"' + r.subcategory + '"', r.amount, (r.isClaimed ? '是' : '否'), cAmt, (r.amount - cAmt), '"' + note + '"'].join(','));
            }

            var csv = "\uFEFF日期,類別,治療項目,總金額(HKD),是否申請理賠,可理賠金額(HKD),自費金額(HKD),理賠備註\n" + rows.join('\n');
            var fileName = '醫療開支紀錄_' + new Date().toISOString().split('T')[0] + '.csv';

            // iPad 專用：嘗試呼出「分享選單」(Share Sheet)
            try {
                var file = new File([csv], fileName, { type: 'text/csv;charset=utf-8;' });
                if (navigator.canShare && navigator.canShare({ files: [file] })) {
                    // 如果支援，呼叫 iPad 原生分享
                    navigator.share({
                        files: [file],
                        title: '醫療開支紀錄',
                        text: '這是您的醫療開支 CSV 檔案。請點擊「儲存到檔案」或分享。'
                    }).catch(function(e) {
                        if (e.name !== 'AbortError') fallbackDownload(csv, fileName);
                    });
                } else {
                    fallbackDownload(csv, fileName);
                }
            } catch(e) {
                fallbackDownload(csv, fileName);
            }
        }

        // 後備方案一：傳統下載
        function fallbackDownload(csv, fileName) {
            try {
                var blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
                var link = document.createElement('a');
                link.href = URL.createObjectURL(blob);
                link.download = fileName;
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
            } catch(e) {
                // 如果 Koder 乜都擋，出大絕
                showRawCSVFallback(csv);
            }
        }

        // 終極後備大絕：顯示文字讓用戶自己 Copy
        function showRawCSVFallback(csv) {
            var div = document.createElement('div');
            div.style.position = 'fixed'; div.style.top = '10%'; div.style.left = '5%'; div.style.right = '5%'; div.style.bottom = '10%';
            div.style.backgroundColor = 'white'; div.style.padding = '20px'; div.style.zIndex = '9999';
            div.style.boxShadow = '0 10px 25px rgba(0,0,0,0.5)'; div.style.borderRadius = '16px';
            div.style.display = 'flex'; div.style.flexDirection = 'column';
            
            var title = document.createElement('h3');
            title.innerText = '⚠️ 由於您的應用程式阻擋了自動下載，請手動複製以下內容：';
            title.style.marginBottom = '10px'; title.style.fontSize = '16px'; title.style.color = '#ef4444';
            
            var textarea = document.createElement('textarea');
            textarea.value = csv;
            textarea.style.flex = '1'; textarea.style.width = '100%'; textarea.style.marginBottom = '15px';
            textarea.style.padding = '10px'; textarea.style.fontFamily = 'monospace'; textarea.style.borderRadius = '8px';
            
            var btnDiv = document.createElement('div');
            btnDiv.style.display = 'flex'; btnDiv.style.gap = '10px';
            
            var closeBtn = document.createElement('button');
            closeBtn.innerText = '關閉';
            closeBtn.style.padding = '12px 20px'; closeBtn.style.background = '#e2e8f0'; closeBtn.style.border = 'none'; closeBtn.style.borderRadius = '8px'; closeBtn.style.fontWeight = 'bold'; closeBtn.style.flex = '1';
            closeBtn.onclick = function() { document.body.removeChild(div); };
            
            var copyBtn = document.createElement('button');
            copyBtn.innerText = '複製全部內容';
            copyBtn.style.padding = '12px 20px'; copyBtn.style.background = '#0d9488'; copyBtn.style.color = 'white'; copyBtn.style.border = 'none'; copyBtn.style.borderRadius = '8px'; copyBtn.style.fontWeight = 'bold'; copyBtn.style.flex = '2';
            copyBtn.onclick = function() { 
                textarea.select(); 
                document.execCommand('copy'); 
                copyBtn.innerText = '✅ 已複製！請貼上到空白的 Excel 或備忘錄中'; 
            };
            
            btnDiv.appendChild(closeBtn);
            btnDiv.appendChild(copyBtn);
            div.appendChild(title);
            div.appendChild(textarea);
            div.appendChild(btnDiv);
            document.body.appendChild(div);
        }
    </script>
</body>
</html>
