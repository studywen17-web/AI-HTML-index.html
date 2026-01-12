<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <title>考試看板</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      margin: 0;
      font-family: "Microsoft JhengHei", Arial, sans-serif;
      background-color: #0f172a;
      color: #ffffff;
    }

    .container {
      padding: 40px;
    }

    h1 {
      text-align: center;
      font-size: 48px;
      margin-bottom: 10px;
    }

    .clock {
      text-align: center;
      font-size: 36px;
      margin-bottom: 40px;
      color: #38bdf8;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 32px;
    }

    th, td {
      border: 2px solid #475569;
      padding: 20px;
      text-align: center;
    }

    th {
      background-color: #1e293b;
      font-size: 34px;
    }

    tr.active {
      background-color: #22c55e;
      color: #000000;
      font-weight: bold;
    }

    tr.break {
      background-color: #facc15;
      color: #000000;
    }
  </style>
</head>
<body>

<div class="container">
  <h1>📘 考試時間看板</h1>
  <div class="clock" id="clock">--:--:--</div>

  <table>
    <thead>
      <tr>
        <th>時間</th>
        <th>內容</th>
      </tr>
    </thead>
    <tbody id="examTable"></tbody>
  </table>
</div>

<script>
  /* ===== 考試時間設定（你要改的只需要這一段） ===== */
  const examData = [
    { subject: "國語", start: "08:50", end: "10:00" },
    { subject: "回教室複習", start: "10:10", end: "10:20", type: "break" },
    { subject: "自然", start: "10:20", end: "11:10" },
    { subject: "英文", start: "11:20", end: "12:00" }
  ];
  /* ============================================ */

  const examTable = document.getElementById("examTable");
  const clockEl = document.getElementById("clock");

  function renderTable() {
    examTable.innerHTML = "";
    examData.forEach(item => {
      const tr = document.createElement("tr");
      tr.dataset.start = item.start;
      tr.dataset.end = item.end;
      if (item.type === "break") tr.classList.add("break");

      tr.innerHTML = `
        <td>${item.start}－${item.end}</td>
        <td>${item.subject}</td>
      `;
      examTable.appendChild(tr);
    });
  }

  function updateClockAndStatus() {
    const now = new Date();
    const hh = String(now.getHours()).padStart(2, "0");
    const mm = String(now.getMinutes()).padStart(2, "0");
    const ss = String(now.getSeconds()).padStart(2, "0");
    clockEl.textContent = `${hh}:${mm}:${ss}`;

    const currentMinutes = now.getHours() * 60 + now.getMinutes();

    document.querySelectorAll("#examTable tr").forEach(tr => {
      tr.classList.remove("active");

      const [sh, sm] = tr.dataset.start.split(":").map(Number);
      const [eh, em] = tr.dataset.end.split(":").map(Number);
      const startM = sh * 60 + sm;
      const endM = eh * 60 + em;

      if (currentMinutes >= startM && currentMinutes < endM) {
        tr.classList.add("active");
      }
    });
  }

  renderTable();
  updateClockAndStatus();
  setInterval(updateClockAndStatus, 1000);
</script>

</body>
</html>
