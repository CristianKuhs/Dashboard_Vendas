<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dashboard de Vendas Avançado</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
html, body {
  height: 100%;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
body {
  background: #0f172a;
  color: #e2e8f0;
  padding: 20px;
  transition: background 0.3s, color 0.3s;
}
body.light {
  background: #f9fafb;
  color: #1e293b;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 20px;
  background: #1e293b;
  border-radius: 12px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.4);
  margin-bottom: 20px;
  flex-wrap: wrap;
}
body.light header {
  background: #ffffff;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}
header h1 {
  font-size: 1.8rem;
  font-weight: 600;
}
header button, button {
  background: linear-gradient(135deg, #3b82f6, #2563eb);
  border: none;
  color: #fff;
  padding: 8px 16px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.95rem;
  font-weight: 500;
  transition: transform 0.2s, background 0.3s;
}
button:hover {
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  transform: scale(1.05);
}
button.action {
  background: #ef4444;
  font-size: 0.8rem;
  padding: 5px 12px;
}
button.action:hover {
  background: #dc2626;
}
.container {
  display: grid;
  gap: 20px;
}
.form-container {
  background: #1e293b;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.4);
  transition: background 0.3s;
}
body.light .form-container {
  background: #ffffff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}
h2 {
  font-size: 1.3rem;
  margin-bottom: 12px;
  color: #93c5fd;
}
body.light h2 {
  color: #2563eb;
}
input, select {
  width: 100%;
  padding: 10px;
  margin: 8px 0;
  border: 1px solid #334155;
  border-radius: 8px;
  font-size: 1rem;
  background: #0f172a;
  color: #f1f5f9;
  transition: border 0.3s, background 0.3s;
}
input:focus, select:focus {
  border-color: #3b82f6;
  outline: none;
}
body.light input, body.light select {
  background: #f1f5f9;
  color: #1e293b;
  border: 1px solid #cbd5e1;
}
#userList {
  list-style: none;
  padding: 0;
  margin: 10px 0 0;
}
#userList li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 6px 10px;
  margin-bottom: 5px;
  border-radius: 6px;
  background: #0f172a;
  font-size: 0.95rem;
}
body.light #userList li {
  background: #f1f5f9;
}
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
  font-size: 0.95rem;
}
th, td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #334155;
}
th {
  background: #1e293b;
  color: #93c5fd;
}
body.light th {
  background: #e2e8f0;
  color: #1e293b;
}
body.light td {
  border-bottom: 1px solid #cbd5e1;
}
.summary {
  display: flex;
  justify-content: space-between;
  background: #1e293b;
  padding: 15px;
  border-radius: 12px;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.4);
  text-align: center;
}
.summary div {
  flex: 1;
}
body.light .summary {
  background: #ffffff;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}
.charts {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}
canvas {
  background: #0f172a;
  padding: 10px;
  border-radius: 12px;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.4);
}
body.light canvas {
  background: #ffffff;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}
@media (max-width: 768px) {
  header {
    flex-direction: column;
    align-items: flex-start;
    gap: 10px;
  }
  .summary {
    flex-direction: column;
    gap: 10px;
  }
}
    }
  </style>
</head>
<body>
  <header>
    <h1>Dashboard de Vendas</h1>
    <div>
      <button id="toggleTheme">🌙 Tema</button>
      <button onclick="exportCSV()">⬇ Exportar CSV</button>
    </div>
  </header>
  <div class="container">
    <div class="summary">
      <div><strong>Total de Vendas:</strong> R$ <span id="totalSales">0.00</span></div>
      <div><strong>Usuários Cadastrados:</strong> <span id="totalUsers">0</span></div>
    </div>
    <div class="form-container">
      <h2>Gerenciar Usuários</h2>
      <input type="text" id="userName" placeholder="Nome do usuário">
      <button onclick="addUser()">Adicionar Usuário</button>
      <ul id="userList"></ul>
    </div>
    <!-- Cadastro de Vendas -->
    <div class="form-container">
      <h2>Registrar Venda</h2>
      <select id="userSelect"></select>
      <input type="number" id="saleAmount" placeholder="Valor da venda (R$)">
      <input type="date" id="saleDate">
      <button onclick="addSale()">Adicionar Venda</button>
    </div>
    <div class="form-container">
      <h2>Filtrar Vendas</h2>
      <select id="filterUser" onchange="renderSales()">
        <option value="">Todos os Usuários</option>
      </select>
      <input type="date" id="filterStart" onchange="renderSales()">
      <input type="date" id="filterEnd" onchange="renderSales()">
    </div>
    <div class="form-container">
      <h2>Vendas Registradas</h2>
      <table id="salesTable">
        <thead>
          <tr>
            <th>Usuário</th>
            <th>Valor (R$)</th>
            <th>Data</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </div>
    <div class="charts">
      <canvas id="salesChart"></canvas>
      <canvas id="userPieChart"></canvas>
    </div>
  </div>
  <script>
    let users = JSON.parse(localStorage.getItem('users')) || [];
    let sales = JSON.parse(localStorage.getItem('sales')) || [];
    const userSelect = document.getElementById('userSelect');
    const filterUser = document.getElementById('filterUser');
    const salesTable = document.getElementById('salesTable').querySelector('tbody');
    const totalSalesEl = document.getElementById('totalSales');
    const totalUsersEl = document.getElementById('totalUsers');
    const userList = document.getElementById('userList');
    function saveData() {
      localStorage.setItem('users', JSON.stringify(users));
      localStorage.setItem('sales', JSON.stringify(sales));
    }
    function addUser() {
      const name = document.getElementById('userName').value.trim();
      if (!name) return alert('Digite um nome.');
      users.push(name);
      document.getElementById('userName').value = '';
      saveData();
      updateUserSelect();
    }
    function deleteUser(index) {
      if (!confirm('Deseja excluir este usuário? As vendas dele permanecerão.')) return;
      users.splice(index, 1);
      saveData();
      updateUserSelect();
    }
    function updateUserSelect() {
      userSelect.innerHTML = '';
      filterUser.innerHTML = '<option value="">Todos os Usuários</option>';
      userList.innerHTML = '';
      users.forEach((user, index) => {
        const opt1 = document.createElement('option');
        opt1.value = index;
        opt1.textContent = user;
        userSelect.appendChild(opt1);
        const opt2 = document.createElement('option');
        opt2.value = user;
        opt2.textContent = user;
        filterUser.appendChild(opt2);
        const li = document.createElement('li');
        li.textContent = user;
        li.innerHTML += ` <button class="action" onclick="deleteUser(${index})">Excluir</button>`;
        userList.appendChild(li);
      });
      totalUsersEl.innerText = users.length;
    }
    function addSale() {
      const userIndex = userSelect.value;
      const amount = parseFloat(document.getElementById('saleAmount').value);
      const date = document.getElementById('saleDate').value || new Date().toISOString().slice(0,10);
      if (userIndex === '' || isNaN(amount) || amount <= 0) return alert('Preencha os campos corretamente.');
      sales.push({ user: users[userIndex], value: amount, date });
      document.getElementById('saleAmount').value = '';
      document.getElementById('saleDate').value = '';
      saveData();
      renderSales();
    }
    function deleteSale(index) {
      if (!confirm('Excluir esta venda?')) return;
      sales.splice(index, 1);
      saveData();
      renderSales();
    }
    function renderSales() {
      salesTable.innerHTML = '';
      let total = 0;
      const filterU = filterUser.value;
      const filterStart = document.getElementById('filterStart').value;
      const filterEnd = document.getElementById('filterEnd').value;
      const filteredSales = sales.filter(s => {
        if (filterU && s.user !== filterU) return false;
        if (filterStart && s.date < filterStart) return false;
        if (filterEnd && s.date > filterEnd) return false;
        return true;
      });
      filteredSales.forEach((sale, index) => {
        total += sale.value;
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${sale.user}</td>
          <td>R$ ${sale.value.toFixed(2)}</td>
          <td>${sale.date}</td>
          <td><button class="action" onclick="deleteSale(${index})">Excluir</button></td>
        `;
        salesTable.appendChild(row);
      });
      totalSalesEl.innerText = total.toFixed(2);
      updateCharts(filteredSales);
    }
    function exportCSV() {
      let csv = 'Usuário,Valor,Data\n';
      sales.forEach(s => {
        csv += `${s.user},${s.value},${s.date}\n`;
      });
      const blob = new Blob([csv], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'vendas.csv';
      a.click();
    }
    const ctxSales = document.getElementById('salesChart').getContext('2d');
    const salesChart = new Chart(ctxSales, {
      type: 'line',
      data: { labels: [], datasets: [{ label: 'Vendas (R$)', data: [], borderColor: '#42a5f5', fill: true, backgroundColor: 'rgba(66,165,245,0.2)', tension: 0.3 }] },
      options: { responsive: true, plugins: { legend: { labels: { color: '#fff' } } }, scales: { x: { ticks: { color: '#fff' } }, y: { ticks: { color: '#fff' } } } }
    });
    const ctxPie = document.getElementById('userPieChart').getContext('2d');
    const userPieChart = new Chart(ctxPie, {
      type: 'pie',
      data: { labels: [], datasets: [{ data: [], backgroundColor: ['#42a5f5','#66bb6a','#ffa726','#ef5350','#ab47bc'] }] },
      options: { plugins: { legend: { labels: { color: '#fff' } } } }
    });
    function updateCharts(filteredSales) {
      const totalPerUser = {};
      filteredSales.forEach(sale => {
        totalPerUser[sale.user] = (totalPerUser[sale.user] || 0) + sale.value;
      });
      salesChart.data.labels = filteredSales.map((s, i) => `Venda ${i + 1}`);
      salesChart.data.datasets[0].data = filteredSales.map(s => s.value);
      salesChart.update();
      userPieChart.data.labels = Object.keys(totalPerUser);
      userPieChart.data.datasets[0].data = Object.values(totalPerUser);
      userPieChart.update();
    }
    document.getElementById('toggleTheme').addEventListener('click', () => {
      document.body.classList.toggle('light');
      const color = document.body.classList.contains('light') ? '#222' : '#fff';
      salesChart.options.plugins.legend.labels.color = color;
      salesChart.options.scales.x.ticks.color = color;
      salesChart.options.scales.y.ticks.color = color;
      userPieChart.options.plugins.legend.labels.color = color;
      salesChart.update();
      userPieChart.update();
    });
    updateUserSelect();
    renderSales();
  </script>
</body>
</html>
