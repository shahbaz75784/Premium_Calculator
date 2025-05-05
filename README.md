<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Calculator by Shahbaz</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(to bottom right, #5f00ba, #8a2be2);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }

    .calculator {
      background-color: #111;
      padding: 20px;
      border-radius: 25px;
      box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
      width: 320px;
    }

    .display {
      background-color: #000;
      color: #fff;
      font-size: 2.5em;
      padding: 15px;
      text-align: right;
      border-radius: 15px;
      margin-bottom: 20px;
      overflow-x: auto;
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
    }

    button {
      padding: 20px;
      font-size: 1.3em;
      border: none;
      border-radius: 15px;
      cursor: pointer;
      font-weight: bold;
    }

    .number {
      background-color: #6a67ce;
      color: white;
    }

    .operator,
    .special {
      background-color: #f7cd2e;
      color: black;
    }

    .history {
      background-color: orange;
      color: white;
    }

    .equal {
      background-color: #f7cd2e;
      color: black;
      grid-column: span 2;
    }

    .footer {
      margin-top: 15px;
      color: white;
      font-weight: bold;
    }

    .history-panel {
      margin-top: 10px;
      background: white;
      border-radius: 10px;
      padding: 10px;
      width: 300px;
      max-height: 150px;
      overflow-y: auto;
      display: none;
    }

    .history-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 5px;
      margin-bottom: 4px;
      border-bottom: 1px solid #ccc;
      cursor: pointer;
    }

    .history-text {
      flex: 1;
      color: #333;
    }

    .delete-btn {
      background: red;
      color: white;
      border: none;
      padding: 4px 8px;
      border-radius: 8px;
      font-size: 0.9em;
      cursor: pointer;
      margin-left: 8px;
    }
  </style>
</head>
<body>

  <div class="calculator">
    <div class="display" id="display">0</div>
    <div class="buttons">
      <button class="number">7</button>
      <button class="number">8</button>
      <button class="number">9</button>
      <button class="operator">+</button>

      <button class="number">4</button>
      <button class="number">5</button>
      <button class="number">6</button>
      <button class="operator">-</button>

      <button class="number">1</button>
      <button class="number">2</button>
      <button class="number">3</button>
      <button class="operator">*</button>

      <button class="number">0</button>
      <button class="special">.</button>
      <button class="history">History</button>
      <button class="operator">/</button>

      <button class="special">C</button>
      <button class="special">AC</button>
      <button class="equal">=</button>
    </div>
  </div>

  <div class="history-panel" id="historyPanel"></div>
  <div class="footer">Created by Shahbaz</div>

  <script>
    const display = document.getElementById('display');
    const buttons = document.querySelectorAll('button');
    const historyPanel = document.getElementById('historyPanel');
    let currentInput = '';
    let history = [];

    function renderHistory() {
      historyPanel.innerHTML = '';
      history.forEach((item, index) => {
        const div = document.createElement('div');
        div.className = 'history-item';

        const span = document.createElement('span');
        span.className = 'history-text';
        span.textContent = item;

        const del = document.createElement('button');
        del.className = 'delete-btn';
        del.textContent = 'Delete';
        del.onclick = (e) => {
          e.stopPropagation();
          history.splice(index, 1);
          renderHistory();
        };

        div.appendChild(span);
        div.appendChild(del);

        div.onclick = () => {
          currentInput = item.split('=')[0].trim();
          display.textContent = currentInput;
        };

        historyPanel.appendChild(div);
      });
    }

    buttons.forEach(button => {
      button.addEventListener('click', () => {
        const value = button.innerText;

        if (button.classList.contains('number') || button.classList.contains('operator') || value === '.') {
          if (display.innerText === '0' && value !== '.') {
            currentInput = value;
          } else {
            currentInput += value;
          }
          display.innerText = currentInput;
        } else if (value === 'AC') {
          currentInput = '';
          display.innerText = '0';
        } else if (value === 'C') {
          currentInput = currentInput.slice(0, -1);
          display.innerText = currentInput || '0';
        } else if (value === '=') {
          try {
            const result = eval(currentInput);
            history.push(`${currentInput} = ${result}`);
            currentInput = result.toString();
            display.textContent = currentInput;
            renderHistory();
          } catch {
            display.textContent = 'Error';
            currentInput = '';
          }
        } else if (value === 'History') {
          historyPanel.style.display = historyPanel.style.display === 'block' ? 'none' : 'block';
        }
      });
    });
  </script>

</body>
</html>
