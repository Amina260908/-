<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Expense Tracker</title>
  <style>
    /* ───── RESET & BASE ───── */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #f3e8ff 0%, #fce4f3 50%, #ede9fe 100%);
      background-attachment: fixed;
      color: #2d1b4e;
      min-height: 100vh;
    }

    /* ───── HEADER ───── */
    header {
      background: linear-gradient(90deg, #7c3aed, #a855f7, #ec4899, #a855f7, #7c3aed);
      background-size: 300% 100%;
      animation: shimmer 6s ease infinite;
      padding: 20px 40px;
      box-shadow: 0 4px 20px rgba(124, 58, 237, 0.3);
    }

    @keyframes shimmer {
      0%   { background-position: 0% 50%; }
      50%  { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    header h1 {
      font-size: 22px;
      font-weight: 700;
      letter-spacing: -0.5px;
      color: #fff;
    }

    header p {
      font-size: 13px;
      color: rgba(255,255,255,0.75);
      margin-top: 2px;
    }

    /* ───── LAYOUT ───── */
    main {
      max-width: 860px;
      margin: 40px auto;
      padding: 0 20px;
    }

    /* ───── SUMMARY CARDS ───── */
    .summary {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 16px;
      margin-bottom: 32px;
    }

    .card {
      background: rgba(255,255,255,0.75);
      backdrop-filter: blur(10px);
      border-radius: 16px;
      padding: 20px 24px;
      border: 1px solid rgba(168, 85, 247, 0.2);
      box-shadow: 0 4px 16px rgba(124, 58, 237, 0.08);
    }

    .card .label {
      font-size: 12px;
      color: #a78bca;
      text-transform: uppercase;
      letter-spacing: 0.5px;
      margin-bottom: 8px;
    }

    .card .amount {
      font-size: 26px;
      font-weight: 700;
    }

    .card.balance .amount {
      background: linear-gradient(90deg, #7c3aed, #ec4899);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    .card.income  .amount { color: #059669; }
    .card.expense .amount { color: #e74c3c; }

    /* ───── FORM ───── */
    .form-section {
      background: rgba(255,255,255,0.75);
      backdrop-filter: blur(10px);
      border-radius: 16px;
      padding: 24px;
      border: 1px solid rgba(168, 85, 247, 0.2);
      box-shadow: 0 4px 16px rgba(124, 58, 237, 0.08);
      margin-bottom: 32px;
    }

    .form-section h2 {
      font-size: 15px;
      font-weight: 600;
      margin-bottom: 16px;
      color: #7c3aed;
    }

    .form-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }

    .form-grid .full {
      grid-column: 1 / -1;
    }

    label {
      display: block;
      font-size: 12px;
      color: #a855f7;
      margin-bottom: 5px;
    }

    input, select {
      width: 100%;
      padding: 10px 12px;
      border: 1px solid rgba(168, 85, 247, 0.25);
      border-radius: 8px;
      font-size: 14px;
      background: rgba(255,255,255,0.6);
      outline: none;
      transition: border 0.2s, box-shadow 0.2s;
      color: #2d1b4e;
    }

    input:focus, select:focus {
      border-color: #a855f7;
      background: #fff;
      box-shadow: 0 0 0 3px rgba(168, 85, 247, 0.15);
    }

    /* TYPE TOGGLE */
    .type-toggle {
      display: flex;
      gap: 8px;
    }

    .type-toggle button {
      flex: 1;
      padding: 10px;
      border: 1px solid rgba(168, 85, 247, 0.25);
      border-radius: 8px;
      background: rgba(255,255,255,0.6);
      font-size: 14px;
      cursor: pointer;
      transition: all 0.15s;
      color: #7c3aed;
    }

    .type-toggle button.active-income {
      background: linear-gradient(135deg, #d1fae5, #a7f3d0);
      border-color: #059669;
      color: #065f46;
      font-weight: 600;
    }

    .type-toggle button.active-expense {
      background: linear-gradient(135deg, #fce7f3, #fbcfe8);
      border-color: #ec4899;
      color: #9d174d;
      font-weight: 600;
    }

    /* SUBMIT BUTTON */
    .btn-add {
      width: 100%;
      padding: 12px;
      background: linear-gradient(90deg, #7c3aed, #a855f7, #ec4899);
      background-size: 200% 100%;
      animation: shimmer 4s ease infinite;
      color: #fff;
      border: none;
      border-radius: 8px;
      font-size: 14px;
      font-weight: 600;
      cursor: pointer;
      margin-top: 4px;
      transition: opacity 0.15s, transform 0.1s;
      box-shadow: 0 4px 15px rgba(168, 85, 247, 0.4);
    }

    .btn-add:hover {
      opacity: 0.9;
      transform: translateY(-1px);
    }

    /* ───── TRANSACTIONS LIST ───── */
    .list-section h2 {
      font-size: 15px;
      font-weight: 600;
      color: #7c3aed;
      margin-bottom: 16px;
    }

    .empty-state {
      text-align: center;
      padding: 40px;
      color: #c4b5fd;
      font-size: 14px;
      background: rgba(255,255,255,0.75);
      backdrop-filter: blur(10px);
      border-radius: 16px;
      border: 1px solid rgba(168, 85, 247, 0.2);
    }

    .transaction-item {
      background: rgba(255,255,255,0.75);
      backdrop-filter: blur(10px);
      border-radius: 14px;
      padding: 16px 20px;
      border: 1px solid rgba(168, 85, 247, 0.15);
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 10px;
      transition: box-shadow 0.15s, transform 0.15s;
    }

    .transaction-item:hover {
      box-shadow: 0 4px 16px rgba(124, 58, 237, 0.12);
      transform: translateY(-1px);
    }

    .tx-left {
      display: flex;
      align-items: center;
      gap: 14px;
    }

    .tx-icon {
      width: 38px;
      height: 38px;
      border-radius: 10px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 18px;
      flex-shrink: 0;
    }

    .tx-icon.income  { background: linear-gradient(135deg, #d1fae5, #a7f3d0); }
    .tx-icon.expense { background: linear-gradient(135deg, #fce7f3, #fbcfe8); }

    .tx-name {
      font-size: 14px;
      font-weight: 600;
      color: #2d1b4e;
    }

    .tx-meta {
      font-size: 12px;
      color: #a78bca;
      margin-top: 2px;
    }

    .tx-right {
      display: flex;
      align-items: center;
      gap: 16px;
    }

    .tx-amount {
      font-size: 16px;
      font-weight: 700;
    }

    .tx-amount.income  { color: #059669; }
    .tx-amount.expense { color: #e74c3c; }

    .btn-delete {
      background: none;
      border: none;
      cursor: pointer;
      color: #d8b4fe;
      font-size: 18px;
      line-height: 1;
      padding: 4px;
      transition: color 0.15s;
    }

    .btn-delete:hover { color: #ec4899; }

    /* ───── RESPONSIVE ───── */
    @media (max-width: 600px) {
      header { padding: 16px 20px; }
      main   { margin: 20px auto; }
      .summary { grid-template-columns: 1fr; }
      .form-grid { grid-template-columns: 1fr; }
      .form-grid .full { grid-column: 1; }
    }
  </style>
</head>
<body>

  <header>
    <h1>Expense Tracker</h1>
    <p>Personal Budget Manager</p>
  </header>

  <main>

    <!-- SUMMARY -->
    <div class="summary">
      <div class="card balance">
        <div class="label">Баланс</div>
        <div class="amount" id="balance">0 ₸</div>
      </div>
      <div class="card income">
        <div class="label">Доходы</div>
        <div class="amount" id="totalIncome">0 ₸</div>
      </div>
      <div class="card expense">
        <div class="label">Расходы</div>
        <div class="amount" id="totalExpense">0 ₸</div>
      </div>
    </div>

    <!-- FORM -->
    <div class="form-section">
      <h2>Добавить транзакцию</h2>
      <div class="form-grid">

        <div class="full">
          <label>Тип</label>
          <div class="type-toggle">
            <button id="btn-income"  onclick="setType('income')"  class="active-income">Доход</button>
            <button id="btn-expense" onclick="setType('expense')">Расход</button>
          </div>
        </div>

        <div>
          <label>Название</label>
          <input type="text" id="txName" placeholder="Зарплата, кафе, такси..." />
        </div>

        <div>
          <label>Сумма (₸)</label>
          <input type="number" id="txAmount" placeholder="0" min="0" />
        </div>

        <div>
          <label>Категория</label>
          <select id="txCategory">
            <option value="Другое">Другое</option>
            <option value="Еда">Еда</option>
            <option value="Транспорт">Транспорт</option>
            <option value="Жильё">Жильё</option>
            <option value="Развлечения">Развлечения</option>
            <option value="Здоровье">Здоровье</option>
            <option value="Одежда">Одежда</option>
            <option value="Зарплата">Зарплата</option>
            <option value="Подарок">Подарок</option>
          </select>
        </div>

        <div>
          <label>Дата</label>
          <input type="date" id="txDate" />
        </div>

        <div class="full">
          <button class="btn-add" onclick="addTransaction()">Добавить</button>
        </div>

      </div>
    </div>

    <!-- LIST -->
    <div class="list-section">
      <h2>История транзакций</h2>
      <div id="txList"></div>
    </div>

  </main>

  <script>
    // ─── EMOJI для категорий ───
    const categoryEmoji = {
      'Еда':          '🍔',
      'Транспорт':    '🚌',
      'Жильё':        '🏠',
      'Развлечения':  '🎮',
      'Здоровье':     '💊',
      'Одежда':       '👗',
      'Зарплата':     '💰',
      'Подарок':      '🎁',
      'Другое':       '📦',
    };

    // ─── СОСТОЯНИЕ ───
    // Загружаем транзакции из localStorage (или пустой массив)
    let transactions = JSON.parse(localStorage.getItem('transactions')) || [];
    let currentType = 'income';

    // Устанавливаем сегодняшнюю дату в поле по умолчанию
    document.getElementById('txDate').value = new Date().toISOString().split('T')[0];

    // ─── ПЕРЕКЛЮЧЕНИЕ ТИПА (доход/расход) ───
    function setType(type) {
      currentType = type;
      document.getElementById('btn-income').className  = type === 'income'  ? 'active-income'  : '';
      document.getElementById('btn-expense').className = type === 'expense' ? 'active-expense' : '';

      // Обновляем список категорий под тип
      const cat = document.getElementById('txCategory');
      if (type === 'income') {
        cat.value = cat.querySelector('option[value="Зарплата"]') ? 'Зарплата' : 'Другое';
      } else {
        cat.value = 'Другое';
      }
    }

    // ─── ДОБАВИТЬ ТРАНЗАКЦИЮ ───
    function addTransaction() {
      const name     = document.getElementById('txName').value.trim();
      const amount   = parseFloat(document.getElementById('txAmount').value);
      const category = document.getElementById('txCategory').value;
      const date     = document.getElementById('txDate').value;

      // Простая валидация
      if (!name)          return alert('Введите название');
      if (!amount || amount <= 0) return alert('Введите корректную сумму');
      if (!date)          return alert('Выберите дату');

      // Создаём объект транзакции
      const tx = {
        id:       Date.now(),      // уникальный ID (время создания)
        type:     currentType,
        name:     name,
        amount:   amount,
        category: category,
        date:     date,
      };

      // Добавляем в массив и сохраняем
      transactions.unshift(tx); // unshift = добавить в начало
      save();

      // Очищаем поля
      document.getElementById('txName').value   = '';
      document.getElementById('txAmount').value = '';
      document.getElementById('txDate').value   = new Date().toISOString().split('T')[0];
    }

    // ─── УДАЛИТЬ ТРАНЗАКЦИЮ ───
    function deleteTransaction(id) {
      transactions = transactions.filter(tx => tx.id !== id);
      save();
    }

    // ─── СОХРАНИТЬ И ПЕРЕРИСОВАТЬ ───
    function save() {
      localStorage.setItem('transactions', JSON.stringify(transactions));
      render();
    }

    // ─── ФОРМАТИРОВАТЬ ЧИСЛО ───
    function formatMoney(n) {
      return n.toLocaleString('ru-RU') + ' ₸';
    }

    // ─── ФОРМАТИРОВАТЬ ДАТУ ───
    function formatDate(str) {
      const [y, m, d] = str.split('-');
      return `${d}.${m}.${y}`;
    }

    // ─── ОТРИСОВАТЬ СТРАНИЦУ ───
    function render() {
      // Считаем суммы
      let income  = 0;
      let expense = 0;

      transactions.forEach(tx => {
        if (tx.type === 'income')  income  += tx.amount;
        if (tx.type === 'expense') expense += tx.amount;
      });

      const balance = income - expense;

      // Обновляем карточки
      document.getElementById('balance').textContent      = formatMoney(balance);
      document.getElementById('totalIncome').textContent  = formatMoney(income);
      document.getElementById('totalExpense').textContent = formatMoney(expense);

      // Отрисовываем список
      const list = document.getElementById('txList');

      if (transactions.length === 0) {
        list.innerHTML = '<div class="empty-state">Транзакций пока нет.<br>Добавьте первую!</div>';
        return;
      }

      list.innerHTML = transactions.map(tx => `
        <div class="transaction-item">
          <div class="tx-left">
            <div class="tx-icon ${tx.type}">
              ${categoryEmoji[tx.category] || '📦'}
            </div>
            <div>
              <div class="tx-name">${tx.name}</div>
              <div class="tx-meta">${tx.category} · ${formatDate(tx.date)}</div>
            </div>
          </div>
          <div class="tx-right">
            <div class="tx-amount ${tx.type}">
              ${tx.type === 'income' ? '+' : '−'} ${formatMoney(tx.amount)}
            </div>
            <button class="btn-delete" onclick="deleteTransaction(${tx.id})" title="Удалить">×</button>
          </div>
        </div>
      `).join('');
    }

    // ─── ЗАПУСКАЕМ ───
    render();
  </script>

</body>
</html>
