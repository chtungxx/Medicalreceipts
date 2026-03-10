# Medicalreceipts[開唔到Canva.html](https://github.com/user-attachments/files/25868705/Canva.html)
<!doctype html>
<html lang="zh-HK">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>醫療支出追蹤</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    html, body { height: 100%; margin: 0; }
    body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; box-sizing: border-box;}
    .app-wrapper { width: 100%; height: 100%; min-height: 100vh; overflow: auto; background: linear-gradient(135deg, #0f766e 0%, #115e59 100%); }
    .container-content { max-width: 1200px; margin: 0 auto; padding: 24px; }
    .card { background: white; border-radius: 12px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); padding: 20px; }
    .btn-primary { background: #0d9488; color: white; padding: 12px 24px; border-radius: 8px; border: none; cursor: pointer; font-weight: 600; transition: all 0.3s; }
    .btn-primary:hover { background: #0a7c6e; transform: translateY(-2px); box-shadow: 0 6px 12px rgba(13, 148, 136, 0.4); }
    .btn-secondary { background: #e2e8f0; color: #1e293b; padding: 10px 16px; border-radius: 8px; border: none; cursor: pointer; font-weight: 500; transition: all 0.3s; }
    .btn-secondary:hover { background: #cbd5e1; }
    .btn-danger { background: #ef4444; color: white; padding: 8px 12px; border-radius: 6px; border: none; cursor: pointer; font-size: 0.875rem; }
    .btn-danger:hover { background: #dc2626; }
    .input-field { width: 100%; padding: 12px; border: 2px solid #e2e8f0; border-radius: 8px; font-size: 1rem; transition: border-color 0.3s; }
    .input-field:focus { outline: none; border-color: #0d9488; }
    .stat-box { background: white; border-radius: 12px; padding: 20px; text-align: center; box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05); }
    .stat-number { font-size: 2rem; font-weight: bold; color: #0d9488; }
    .stat-label { color: #64748b; font-size: 0.875rem; margin-top: 8px; }
    .badge { display: inline-block; padding: 4px 12px; border-radius: 20px; font-size: 0.75rem; font-weight: 600; }
    .badge-claimed { background: #dcfce7; color: #166534; }
    .badge-notclaimed { background: #fef3c7; color: #92400e; }
    .loading-spinner { display: inline-block; width: 20px; height: 20px; border: 3px solid #e2e8f0; border-top-color: #0d9488; border-radius: 50%; animation: spin 0.8s linear infinite; }
    @keyframes spin { to { transform: rotate(360deg); } }
    .modal-backdrop { display: none; position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0, 0, 0, 0.5); z-index: 100; }
    .modal-backdrop.active { display: flex; align-items: center; justify-content: center; }
    .modal { background: white; border-radius: 12px; padding: 32px; max-width: 500px; width: 90%; max-height: 90vh; overflow: auto; }
    .modal-header { font-size: 1.5rem; font-weight: bold; color: #1e293b; margin-bottom: 16px; }
    .modal-buttons { display: flex; gap: 12px; margin-top: 24px; }
    .modal-buttons button { flex: 1; }
    .form-group { margin-bottom: 16px; }
    .form-group label { display: block; font-weight: 600; color: #334155; margin-bottom: 6px; }
    .empty-state { text-align: center; padding: 40px 20px; color: #64748b; }
    .empty-state-icon { font-size: 3rem; margin-bottom: 12px; }
    .success-message { position: fixed; bottom: 20px; right: 20px; background: #22c55e; color: white; padding: 12px 20px; border-radius: 8px; box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15); animation: slideUp 0.3s ease-out; z-index: 200; }
    @keyframes slideUp { from { transform: translateY(100px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
    .error-message { position: fixed; bottom: 20px; right: 20px; background: #ef4444; color: white; padding: 12px 20px; border-radius: 8px; box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15); animation: slideUp 0.3s ease-out; z-index: 200; }
    .filter-tag { display: inline-block; padding: 8px 16px; margin: 4px; border-radius: 20px; border: 2px solid #0d9488; color: #0d9488; cursor: pointer; font-weight: 500; transition: all 0.3s; }
    .filter-tag.active { background: #0d9488; color: white; }
    .checkbox-label { display: flex; align-items: center; gap: 8px; cursor: pointer; }
    .checkbox-label input { cursor: pointer; }
  </style>
 </head>
 <body>
  <div id="login-screen" class="app-wrapper" style="display: flex; align-items: center; justify-content: center;">
   <div class="card" style="max-width: 400px; width: 90%; box-sizing: border-box;">
    <h2 style="margin: 0 0 24px 0; font-size: 1.5rem; color: #1e293b; text-align: center;">🔐 登入</h2>
    <div class="form-group">
     <label>密碼</label> <input type="password" id="login-password" class="input-field" placeholder="輸入密碼" onkeypress="handleLoginKeypress(event)">
    </div><button class="btn-primary" onclick="handleLogin()" style="width: 100%;">登入</button>
    <p id="login-error" style="color: #ef4444; text-align: center; margin-top: 12px; display: none;">密碼錯誤，請重試</p>
   </div>
  </div>

  <div class="app-wrapper" id="main-app" style="display: none;">
   <div class="container-content">
    <div class="card" style="background: white; margin-bottom: 24px; border: none; box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);">
     <div style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 12px;">
      <div>
       <h1 id="app-title" style="margin: 0; font-size: 2rem; color: #1e293b; font-weight: bold;">醫療支出追蹤</h1>
       <p style="margin: 8px 0 0 0; color: #64748b;">記錄您的醫療費用同埋保險理賠</p>
      </div>
      <div style="display: flex; gap: 12px;">
       <button class="btn-primary" onclick="openAddModal()">+ 新增記錄</button> <button class="btn-secondary" onclick="handleLogout()" style="padding: 12px 24px;">登出</button>
      </div>
     </div>
    </div>
    
    <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; margin-bottom: 24px;">
     <div class="stat-box">
      <div class="stat-number" id="total-expenses">$0</div>
      <div class="stat-label">總醫療支出</div>
     </div>
     <div class="stat-box">
      <div class="stat-number" id="insurance-claim">$0</div>
      <div class="stat-label">保險可理賠</div>
     </div>
     <div class="stat-box">
      <div class="stat-number" id="pt-claims">0/10</div>
      <div class="stat-label">物理治療理賠次數</div>
     </div>
     <div class="stat-box">
      <div class="stat-number" id="out-of-pocket">$0</div>
      <div class="stat-label">自行付費</div>
     </div>
    </div>
    
    <div class="card" style="margin-bottom: 24px;">
     <h3 style="margin: 0 0 12px 0; color: #1e293b; font-weight: 600;">篩選</h3>
     <div style="margin-bottom: 16px; padding: 16px; background: #f8fafc; border-radius: 8px;">
      <div style="display: flex; gap: 12px; flex-wrap: wrap; margin-bottom: 12px;">
       <div style="flex: 1; min-width: 140px;">
           <label style="display: block; font-weight: 600; color: #334155; margin-bottom: 6px; font-size: 0.875rem;">📅 開始日期</label> 
           <input type="date" id="filter-start-date" class="input-field" onchange="applyDateFilter()" style="font-size: 0.875rem; padding: 8px;">
       </div>
       <div style="flex: 1; min-width: 140px;">
           <label style="display: block; font-weight: 600; color: #334155; margin-bottom: 6px; font-size: 0.875rem;">📅 結束日期</label> 
           <input type="date" id="filter-end-date" class="input-field" onchange="applyDateFilter()" style="font-size: 0.875rem; padding: 8px;">
       </div>
       <div style="display: flex; align-items: flex-end;">
           <button class="btn-secondary" onclick="resetDateFilter()" style="padding: 8px 16px; font-size: 0.875rem; white-space: nowrap;">🔄 重設日期</button>
       </div>
      </div>
     </div>
     <div>
      <button class="filter-tag active" onclick="toggleFilter('all', event)">全部</button> 
      <button class="filter-tag" onclick="toggleFilter('西醫治療費', event)">西醫治療費</button> 
      <button class="filter-tag" onclick="toggleFilter('物理治療費', event)">物理治療費</button>
     </div>
    </div>
    
    <div class="card">
     <h2 style="margin: 0 0 16px 0; font-size: 1.25rem; color: #1e293b;">醫療開支紀錄</h2>
     <div id="records-container" style="min-height: 200px;">
      <div class="empty-state">
       <div class="empty-state-icon">📋</div>
       <p>未有任何記錄，開始添加您的醫療費用吧！</p>
      </div>
     </div>
    </div>
   </div>
  </div>
  
  <div class="modal-backdrop" id="modal">
   <div class="modal">
    <div class="modal-header">新增醫療支出</div>
    <div class="form-group">
     <label>日期</label> <input type="date" id="form-date" class="input-field" required>
    </div>
    <div class="form-group">
     <label>主類別</label> 
     <select id="form-main-category" class="input-field" onchange="updateSubCategories()" required> 
        <option value="">選擇主類別</option> 
        <option value="西醫治療費">西醫治療費</option> 
        <option value="物理治療費">物理治療費</option> 
     </select>
    </div>
    <div class="form-group">
     <label>細類別</label> 
     <select id="form-sub-category" class="input-field" onchange="updateAmountField()" required> 
        <option value="">選擇細類別</option> 
     </select>
    </div>
    <div class="form-group">
     <label id="amount-label">金額 ($)</label> <input type="number" id="form-amount" class="input-field" placeholder="0.00" step="0.01" min="0">
     <p id="fixed-amount-info" style="font-size: 0.875rem; color: #0d9488; margin-top: 4px; display: none;"></p>
    </div>
    <div class="form-group">
     <label class="checkbox-label"> <input type="checkbox" id="form-is-claimed"> <span>聲稱保險理賠</span> </label>
    </div>
    <div class="modal-buttons">
     <button class="btn-secondary" onclick="closeModal()">取消</button> <button class="btn-primary" onclick="saveRecord()" id="save-btn">保存</button>
    </div>
   </div>
  </div>
  
  <div class="modal-backdrop" id="delete-modal">
   <div class="modal">
    <div style="display: flex; align-items: center; gap: 12px; margin-bottom: 16px;">
     <span style="font-size: 1.5rem;">⚠️</span>
     <div class="modal-header" style="margin: 0;">確認刪除</div>
    </div>
    <div style="background: #fef2f2; border-left: 4px solid #ef4444; padding: 12px; border-radius: 6px; margin-bottom: 16px;">
     <p style="color: #7f1d1d; margin: 0; font-weight: 500;">要刪除的記錄：</p>
     <p id="delete-record-info" style="color: #991b1b; margin: 8px 0 0 0; font-size: 0.875rem;"></p>
    </div>
    <p style="color: #64748b; margin-bottom: 24px;">此操作無法撤銷，請確認無誤後再刪除。</p>
    <div class="modal-buttons">
     <button class="btn-secondary" onclick="cancelDelete()">取消</button> <button class="btn-danger" style="background: #ef4444; color: white; flex: 1;" onclick="confirmDelete()">確認刪除</button>
    </div>
   </div>
  </div>

  <script>
    let currentData = JSON.parse(localStorage.getItem('medicalTrackerData')) || [];
    let editingId = null;
    let deleteId = null;
    let currentFilter = 'all';
    let filterStartDate = null;
    let filterEndDate = null;
    let isLoggedIn = false;

    const APP_PASSWORD = "0529";

    // Category definitions with fixed prices
    const categories = {
      '西醫治療費': {
        'MRI檢查費': null,
        '傷口護理費用': 50,
        '政府門診': null,
        '私家門診': null,
        '骨科門診': 250
      },
      '物理治療費': {
        '私人物理治療': null,
        '政府物理治療': 250
      }
    };

    // Login handlers
    function handleLogin() {
      const password = document.getElementById('login-password').value;
      if (password === APP_PASSWORD) {
        isLoggedIn = true;
        sessionStorage.setItem('medicalTrackerLoggedIn', 'true'); // Changed to sessionStorage for security
        document.getElementById('login-screen').style.display = 'none';
        document.getElementById('main-app').style.display = 'block';
        document.getElementById('login-password').value = '';
        document.getElementById('login-error').style.display = 'none';
        initializeApp();
      } else {
        document.getElementById('login-error').style.display = 'block';
        document.getElementById('login-password').value = '';
      }
    }

    function handleLoginKeypress(event) {
      if (event.key === 'Enter') handleLogin();
    }

    function handleLogout() {
      isLoggedIn = false;
      sessionStorage.removeItem('medicalTrackerLoggedIn');
      document.getElementById('login-screen').style.display = 'flex';
      document.getElementById('main-app').style.display = 'none';
      document.getElementById('login-password').value = '';
      document.getElementById('login-error').style.display = 'none';
    }

    function checkLoginStatus() {
      const loggedIn = sessionStorage.getItem('medicalTrackerLoggedIn') === 'true';
      if (loggedIn) {
        isLoggedIn = true;
        document.getElementById('login-screen').style.display = 'none';
        document.getElementById('main-app').style.display = 'block';
        initializeApp();
      } else {
        document.getElementById('login-screen').style.display = 'flex';
        document.getElementById('main-app').style.display = 'none';
      }
    }

    function initializeApp() {
        // Sort data by date
        currentData = currentData.sort((a, b) => new Date(b.date) - new Date(a.date));
        renderRecords();
        updateStats();
    }

    function saveDataToLocal() {
        localStorage.setItem('medicalTrackerData', JSON.stringify(currentData));
        initializeApp(); // Re-render
    }

    function setTodayDate() {
      const today = new Date().toISOString().split('T')[0];
      document.getElementById('form-date').value = today;
    }

    function updateSubCategories() {
      const mainCategory = document.getElementById('form-main-category').value;
      const subCategorySelect = document.getElementById('form-sub-category');
      subCategorySelect.innerHTML = '<option value="">選擇細類別</option>';

      if (mainCategory && categories[mainCategory]) {
        Object.keys(categories[mainCategory]).forEach(subCat => {
          const option = document.createElement('option');
          option.value = subCat;
          option.textContent = subCat;
          subCategorySelect.appendChild(option);
        });
      }
      document.getElementById('form-amount').value = '';
      document.getElementById('fixed-amount-info').style.display = 'none';
    }

    function updateAmountField() {
      const mainCategory = document.getElementById('form-main-category').value;
      const subCategory = document.getElementById('form-sub-category').value;
      const amountInput = document.getElementById('form-amount');
      const infoLabel = document.getElementById('fixed-amount-info');

      if (mainCategory && subCategory && categories[mainCategory]) {
        const fixedPrice = categories[mainCategory][subCategory];
        if (fixedPrice !== null) {
          amountInput.value = fixedPrice;
          amountInput.disabled = true;
          infoLabel.textContent = `固定金額: $${fixedPrice}`;
          infoLabel.style.display = 'block';
        } else {
          amountInput.value = '';
          amountInput.disabled = false;
          infoLabel.style.display = 'none';
        }
      }
    }

    function openAddModal() {
      editingId = null;
      clearForm();
      setTodayDate();
      document.getElementById('modal').classList.add('active');
      document.getElementById('save-btn').textContent = '保存';
    }

    function closeModal() {
      document.getElementById('modal').classList.remove('active');
      clearForm();
    }

    function clearForm() {
      document.getElementById('form-date').value = '';
      document.getElementById('form-main-category').value = '';
      document.getElementById('form-sub-category').value = '';
      document.getElementById('form-amount').value = '';
      document.getElementById('form-is-claimed').checked = false;
      document.getElementById('form-amount').disabled = false;
      document.getElementById('fixed-amount-info').style.display = 'none';
    }

    function saveRecord() {
      const date = document.getElementById('form-date').value;
      const mainCategory = document.getElementById('form-main-category').value;
      const subCategory = document.getElementById('form-sub-category').value;
      const amount = parseFloat(document.getElementById('form-amount').value);
      const isClaimed = document.getElementById('form-is-claimed').checked;

      if (!date || !mainCategory || !subCategory || !amount || isNaN(amount) || amount <= 0) {
        showError('請填寫所有必填欄位，金額必須大於 0');
        return;
      }

      if (editingId) {
        const index = currentData.findIndex(r => r.id === editingId);
        if (index !== -1) {
            currentData[index] = { ...currentData[index], date, main_category: mainCategory, sub_category: subCategory, amount, is_claimed: isClaimed };
        }
      } else {
        const record = {
          id: Date.now().toString(),
          date,
          main_category: mainCategory,
          sub_category: subCategory,
          amount,
          is_claimed: isClaimed,
          created_at: new Date().toISOString()
        };
        currentData.push(record);
      }

      saveDataToLocal();
      closeModal();
      showSuccess(editingId ? '記錄已更新' : '記錄已保存');
    }

    function renderRecords() {
      const container = document.getElementById('records-container');
      let filteredData = currentFilter === 'all' 
        ? currentData 
        : currentData.filter(r => r.main_category === currentFilter);

      // Apply date filter
      if (filterStartDate || filterEndDate) {
        filteredData = filteredData.filter(r => {
          const recordDate = new Date(r.date);
          if (filterStartDate) {
            const startDate = new Date(filterStartDate);
            if (recordDate < startDate) return false;
          }
          if (filterEndDate) {
            const endDate = new Date(filterEndDate);
            endDate.setHours(23, 59, 59, 999);
            if (recordDate > endDate) return false;
          }
          return true;
        });
      }

      if (filteredData.length === 0) {
        container.innerHTML = '<div class="empty-state"><div class="empty-state-icon">📋</div><p>未有任何記錄</p></div>';
        return;
      }

      let html = '<div style="overflow-x: auto;"><div style="min-width: 800px;">';
      html += '<div class="table-row" style="background: #f1f5f9; font-weight: bold; border-bottom: 2px solid #cbd5e1; display: flex; padding: 12px;"><div style="flex: 1;">日期</div><div style="flex: 1;">類別</div><div style="flex: 1;">細項</div><div style="flex: 1;">金額</div><div style="flex: 1;">理賠</div><div style="flex: 1;">操作</div></div>';

      filteredData.forEach(record => {
        const badgeClass = record.is_claimed ? 'badge-claimed' : 'badge-notclaimed';
        const badgeText = record.is_claimed ? '✓ 已聲稱' : '未聲稱';

        html += `
          <div class="table-row" style="padding: 12px; border-bottom: 1px solid #e2e8f0; display: flex; align-items: center;">
            <div style="flex: 1; font-size: 0.875rem;">${record.date}</div>
            <div style="flex: 1; font-size: 0.875rem; color: #475569;">${record.main_category}</div>
            <div style="flex: 1; font-size: 0.875rem; color: #475569;">${record.sub_category}</div>
            <div style="flex: 1; font-weight: 600; color: #0d9488;">$${record.amount.toFixed(2)}</div>
            <div style="flex: 1;"><span class="badge ${badgeClass}">${badgeText}</span></div>
            <div style="flex: 1; display: flex; gap: 6px;">
              <button class="btn-secondary" onclick="editRecord('${record.id}')" style="padding: 6px 10px; font-size: 0.75rem; flex: 1;">編輯</button>
              <button class="btn-danger" onclick="openDeleteConfirm('${record.id}')" style="padding: 6px 10px; flex: 1;">刪除</button>
            </div>
          </div>
        `;
      });

      html += '</div></div>';
      container.innerHTML = html;
    }

    function toggleFilter(filter, event) {
      currentFilter = filter;
      document.querySelectorAll('.filter-tag').forEach(btn => btn.classList.remove('active'));
      if(event) event.target.classList.add('active');
      renderRecords();
    }

    function applyDateFilter() {
      filterStartDate = document.getElementById('filter-start-date').value;
      filterEndDate = document.getElementById('filter-end-date').value;
      renderRecords();
    }

    function resetDateFilter() {
      filterStartDate = null;
      filterEndDate = null;
      document.getElementById('filter-start-date').value = '';
      document.getElementById('filter-end-date').value = '';
      renderRecords();
    }

    function editRecord(id) {
      const record = currentData.find(r => r.id === id);
      if (!record) return;

      editingId = id;
      document.getElementById('form-date').value = record.date;
      document.getElementById('form-main-category').value = record.main_category;
      
      updateSubCategories();
      
      setTimeout(() => {
        document.getElementById('form-sub-category').value = record.sub_category;
        updateAmountField();
        document.getElementById('form-amount').value = record.amount;
      }, 0);

      document.getElementById('form-is-claimed').checked = record.is_claimed;

      document.getElementById('modal').classList.add('active');
      document.getElementById('save-btn').textContent = '更新';
    }

    function openDeleteConfirm(id) {
      deleteId = id;
      const record = currentData.find(r => r.id === id);
      if (record) {
        const infoText = `📅 ${record.date} | 🏥 ${record.sub_category} | 💵 $${record.amount.toFixed(2)}`;
        document.getElementById('delete-record-info').textContent = infoText;
      }
      document.getElementById('delete-modal').classList.add('active');
    }

    function cancelDelete() {
      deleteId = null;
      document.getElementById('delete-modal').classList.remove('active');
    }

    function confirmDelete() {
      if (!deleteId) return;
      currentData = currentData.filter(r => r.id !== deleteId);
      saveDataToLocal();
      showSuccess('記錄已刪除');
      cancelDelete();
    }

    function updateStats() {
      const totalExpenses = currentData.reduce((sum, r) => sum + r.amount, 0);

      let insuranceClaim = 0;
      let ptClaimsCount = 0;

      currentData.forEach(record => {
        if (!record.is_claimed) return;

        if (record.main_category === '西醫治療費') {
          insuranceClaim += record.amount;
        } else if (record.main_category === '物理治療費') {
          if (ptClaimsCount < 10) {
            const claimableAmount = Math.min(record.amount, 400);
            insuranceClaim += claimableAmount;
            ptClaimsCount++;
          }
        }
      });

      const outOfPocket = totalExpenses - insuranceClaim;

      document.getElementById('total-expenses').textContent = `$${totalExpenses.toFixed(2)}`;
      document.getElementById('insurance-claim').textContent = `$${insuranceClaim.toFixed(2)}`;
      document.getElementById('pt-claims').textContent = `${ptClaimsCount}/10`;
      document.getElementById('out-of-pocket').textContent = `$${outOfPocket.toFixed(2)}`;
    }

    function showSuccess(message) {
      const msg = document.createElement('div');
      msg.className = 'success-message';
      msg.textContent = message;
      document.body.appendChild(msg);
      setTimeout(() => msg.remove(), 3000);
    }

    function showError(message) {
      const msg = document.createElement('div');
      msg.className = 'error-message';
      msg.textContent = message;
      document.body.appendChild(msg);
      setTimeout(() => msg.remove(), 3000);
    }

    // Close modals on backdrop click
    document.getElementById('modal').addEventListener('click', (e) => {
      if (e.target.id === 'modal') closeModal();
    });

    document.getElementById('delete-modal').addEventListener('click', (e) => {
      if (e.target.id === 'delete-modal') cancelDelete();
    });

    // Run on load
    checkLoginStatus();
  </script>
 </body>
</html>
