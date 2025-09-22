<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Productivity Tracker Dashboard</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      margin: 0;
      background: #f4f6f8;
      color: #333;
    }
    header {
      background: #4a90e2;
      color: white;
      padding: 1rem;
      text-align: center;
    }
    .container {
      display: flex;
      flex-wrap: wrap;
      padding: 1rem;
    }
    .card {
      background: white;
      border-radius: 8px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      margin: 1rem;
      padding: 1rem;
      flex: 1 1 300px;
    }
    h2 {
      margin-top: 0;
    }
    input[type="text"] {
      width: 80%;
      padding: 0.5rem;
      margin-right: 0.5rem;
    }
    button {
      padding: 0.5rem 1rem;
      background: #4a90e2;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    ul {
      list-style: none;
      padding: 0;
    }
    li {
      padding: 0.5rem 0;
      display: flex;
      justify-content: space-between;
    }
    .completed {
      text-decoration: line-through;
      color: gray;
    }
    canvas {
      max-width: 100%;
    }
  </style>
</head>
<body>
  <header>
    <h1>📊 Productivity Tracker Dashboard</h1>
  </header>

  <div class="container">
    <div class="card">
      <h2>✅ Daily Tasks</h2>
      <input type="text" id="taskInput" placeholder="Add a new task..." />
      <button onclick="addTask()">Add</button>
      <ul id="taskList"></ul>
    </div>

    <div class="card">
      <h2>📈 Progress Chart</h2>
      <canvas id="progressChart" width="400" height="200"></canvas>
    </div>

    <div class="card">
      <h2>🎯 Goals</h2>
      <p>Set weekly goals and track completion:</p>
      <input type="text" id="goalInput" placeholder="Add a goal..." />
      <button onclick="addGoal()">Add</button>
      <ul id="goalList"></ul>
    </div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script>
    const taskList = document.getElementById('taskList');
    const goalList = document.getElementById('goalList');
    let completedTasks = 0;
    let totalTasks = 0;

    function addTask() {
      const input = document.getElementById('taskInput');
      const taskText = input.value.trim();
      if (taskText === '') return;

      const li = document.createElement('li');
      li.innerHTML = `<span>${taskText}</span> <button onclick="completeTask(this)">Done</button>`;
      taskList.appendChild(li);
      input.value = '';
      totalTasks++;
      updateChart();
    }

    function completeTask(button) {
      const li = button.parentElement;
      li.querySelector('span').classList.add('completed');
      button.remove();
      completedTasks++;
      updateChart();
    }

    function addGoal() {
      const input = document.getElementById('goalInput');
      const goalText = input.value.trim();
      if (goalText === '') return;

      const li = document.createElement('li');
      li.textContent = goalText;
      goalList.appendChild(li);
      input.value = '';
    }

    const ctx = document.getElementById('progressChart').getContext('2d');
    const progressChart = new Chart(ctx, {
      type: 'doughnut',
      data: {
        labels: ['Completed', 'Remaining'],
        datasets: [{
          data: [completedTasks, totalTasks - completedTasks],
          backgroundColor: ['#4caf50', '#f44336']
        }]
      },
      options: {
        responsive: true,
        plugins: {
          legend: {
            position: 'bottom'
          }
        }
      }
    });

    function updateChart() {
      progressChart.data.datasets[0].data = [completedTasks, totalTasks - completedTasks];
      progressChart.update();
    }
  </script>
</body>
</html>
