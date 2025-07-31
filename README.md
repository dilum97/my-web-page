<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Seettu Contributions</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    /* your existing styles here */
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: #f2f5f7;
      margin: 0;
      padding: 20px;
    }

    h2 {
      text-align: center;
      color: #333;
      margin-bottom: 30px;
    }

    .table-container {
      max-width: 800px;
      margin: 0 auto;
      background: #fff;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      overflow-x: auto;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 16px;
    }

    th,
    td {
      padding: 15px;
      text-align: center;
    }

    th {
      background-color: #007bff;
      color: white;
    }

    tr:nth-child(even) {
      background-color: #f9f9f9;
    }

    tr:hover {
      background-color: #eef4ff;
    }

    .paid-true {
      color: green;
      font-weight: bold;
    }

    .paid-false {
      color: red;
      font-weight: bold;
    }

    .subtotal {
      font-weight: bold;
      font-size: 18px;
      color: #007bff;
      text-align: right;
      margin: 15px 0;
      max-width: 800px;
      margin-left: auto;
      margin-right: auto;
    }

    @media (max-width: 600px) {
      table {
        font-size: 14px;
      }

      th,
      td {
        padding: 10px;
      }
    }
  </style>
</head>
<body>
  <h2>Seettu Contribution Summary</h2>
  <div class="table-container" id="subForm">Loading...</div>
  <div class="subtotal" id="subtotalPaid">Subtotal Paid: Rs. 0</div>

  <script>
    function loadData() {
      fetch(
        'https://opensheet.elk.sh/1cHxU-k8NwTy7l6LsICNFKcJXieNgpXDl3BIA471L-_8/Sheet1'
      )
        .then((res) => res.json())
        .then((data) => {
          let html = '<table>';
          html += '<tr><th>Name</th><th>Amount (Rs)</th><th>Status</th></tr>';

          let subtotalPaid = 0;

          data.forEach((row) => {
            const paid = row.Paid === 'TRUE';
            const paidIcon = paid ? '✅' : '❌';
            const paidClass = paid ? 'paid-true' : 'paid-false';

            const amount = parseFloat(row.Amount) || 0;

            if (paid) {
              subtotalPaid += amount;
            }

            html += `<tr>
                      <td>${row.Name}</td>
                      <td>Rs. ${amount.toLocaleString()}</td>
                      <td class="${paidClass}">${paidIcon}</td>
                    </tr>`;
          });

          html += '</table>';
          document.getElementById('subForm').innerHTML = html;

          document.getElementById(
            'subtotalPaid'
          ).textContent = `Subtotal Paid: Rs. ${subtotalPaid.toLocaleString()}`;
        })
        .catch((err) => {
          document.getElementById('subForm').innerHTML =
            "<p style='color:red;text-align:center;'>Error loading data.</p>";
          console.error(err);
        });
    }

    window.onload = loadData;
  </script>
</body>
</html>
