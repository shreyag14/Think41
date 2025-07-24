<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Browser History Simulator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f9f9f9;
      margin: 40px;
      padding: 0;
    }
    h1 {
      color: #2c3e50;
    }
    .container {
      max-width: 600px;
      margin: auto;
      background: #fff;
      padding: 25px;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }
    input, button {
      padding: 10px;
      margin: 5px 5px 15px 0;
      font-size: 16px;
    }
    input[type="text"], input[type="number"] {
      width: calc(100% - 22px);
    }
    ul {
      list-style: none;
      padding: 0;
    }
    li {
      background: #ecf0f1;
      margin-bottom: 10px;
      padding: 10px;
      border-left: 4px solid #3498db;
    }
    .buttons {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>🕘 Browser History UI</h1>

    <input type="text" id="urlInput" placeholder="Enter URL here (e.g., https://example.com)" />
    
    <div class="buttons">
      <button onclick="visitPage()">Visit Page</button>
      <input type="number" id="historyLimit" min="1" value="5" onchange="renderHistory()" />
      <button onclick="clearHistory()">Clear History</button>
    </div>

    <h2>Recent Visits</h2>
    <ul id="historyList"></ul>
  </div>

  <script>
    let history = new Map();  // Optimized using Map for O(1) lookups

    function visitPage() {
      const input = document.getElementById("urlInput");
      const url = input.value.trim();
      if (!url) return;

      const now = new Date();
      // If URL already exists, delete and reinsert (to bring it to the front)
      if (history.has(url)) {
        history.delete(url);
      }
      history.set(url, now);  // Maintain insertion order and timestamp

      input.value = "";
      renderHistory();
    }

    function renderHistory() {
      const limit = parseInt(document.getElementById("historyLimit").value) || 5;
      const list = document.getElementById("historyList");
      list.innerHTML = "";

      const recent = Array.from(history.entries())
                          .slice(-limit) // Get last N visited URLs
                          .reverse();    // Most recent first

      for (const [url, timestamp] of recent) {
        const item = document.createElement("li");
        item.textContent = `${url} — visited at ${timestamp.toLocaleString()}`;
        list.appendChild(item);
      }
    }

    function clearHistory() {
      history.clear();
      renderHistory();
    }
  </script>

</body>
</html>
# Think41
