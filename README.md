<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trading Calculators</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            text-align: center;
            background: #f4f4f4;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 400px;
            margin: 30px auto;
            padding: 20px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            position: relative;
        }
        input, button {
            width: 90%;
            padding: 8px;
            margin: 5px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 14px;
        }
        button { cursor: pointer; }
        .btn-primary { background: #007bff; color: white; }
        .btn-danger { background: #dc3545; color: white; }
        .btn-reset { background: #28a745; color: white; }
        .highlight {
            font-size: 18px;
            font-weight: bold;
            color: #ff5722;
            margin-top: 10px;
        }
        table {
            width: 100%;
            margin-top: 15px;
            font-size: 12px;
            border-collapse: collapse;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 5px;
            text-align: center;
        }
        th {
            background-color: #007bff;
            color: white;
        }
        .footer {
            margin-top: 20px;
            font-size: 12px;
            color: #333;
        }
        .footer img {
            width: 25px;
            margin-top: 5px;
        }
        #moneyManagement { display: none; }
        .switch-button {
            position: fixed;
            top: 10px;
            right: 10px;
            font-size: 16px;
            cursor: pointer;
            background: #ff9800;
            padding: 5px;
            border-radius: 5px;
        }
    </style>
</head>
<body>

    <div class="container" id="compoundingCalculator">
        <h3>Trading Compounding Calculator</h3>
        
        <label>Starting Capital:</label>
        <input type="number" id="startCapital" placeholder="Enter starting amount">
        
        <label>Daily Growth Target (%):</label>
        <input type="number" id="dailyRate" placeholder="Enter daily growth rate">

        <button class="btn-primary" onclick="startCalculation()">Start</button>
        <button class="btn-reset" onclick="resetCalculator()">Reset</button>

        <h4 id="currentTarget" class="highlight"></h4>

        <label>Enter Profit:</label>
        <input type="number" id="profitAmount" placeholder="Enter profit amount">
        <button class="btn-primary" onclick="submitProfit()">Submit Profit</button>

        <label>Enter Loss:</label>
        <input type="number" id="lossAmount" placeholder="Enter loss amount">
        <button class="btn-danger" onclick="submitLoss()">Submit Loss</button>

        <table>
            <thead>
                <tr>
                    <th>Day</th>
                    <th>Capital</th>
                    <th>Target</th>
                    <th>Profit</th>
                    <th>Loss</th>
                    <th>New Capital</th>
                </tr>
            </thead>
            <tbody id="logTable"></tbody>
        </table>
    </div>

    <div class="switch-button" onclick="toggleMoneyManagement()">🖐️</div>

    <div class="container" id="moneyManagement">
        <h3>Money Management Calculator</h3>

        <label>Max Trades Per Day:</label>
        <input type="number" id="maxTrades" placeholder="Enter max trades">

        <label>Total Amount:</label>
        <input type="number" id="totalAmount" placeholder="Enter total amount">

        <button onclick="calculateTrades()">Calculate</button>

        <h4 id="result"></h4>

        <button class="btn-reset" onclick="toggleMoneyManagement()">Back</button>
    </div>

    <div class="footer">
        By <b>tuhin_trader</b>
        <br><br>
        <a href="https://www.instagram.com/trader_is_tuhin?igsh=NTFqc2g0bDlibG8=" target="_blank">
            <img src="https://upload.wikimedia.org/wikipedia/commons/a/a5/Instagram_icon.png" alt="Instagram">
        </a>
<br>
       
    </div>

    <script>
        let currentCapital = 0, dailyRate = 0, day = 0;

        function startCalculation() {
            currentCapital = parseFloat(document.getElementById("startCapital").value);
            dailyRate = parseFloat(document.getElementById("dailyRate").value) / 100;

            if (isNaN(currentCapital) || isNaN(dailyRate) || currentCapital <= 0 || dailyRate <= 0) {
                alert("Please enter valid values!");
                return;
            }

            day = 1;
            updateTarget();
        }

        function updateTarget() {
            let target = (currentCapital * dailyRate).toFixed(2);
            document.getElementById("currentTarget").innerHTML = `Target Amount: ₹${target}`;
            addToTable(day, currentCapital, target, "-", "-", currentCapital);
        }

        function submitProfit() {
            let profit = parseFloat(document.getElementById("profitAmount").value);
            if (isNaN(profit) || profit < 0) {
                alert("Enter a valid profit amount!");
                return;
            }
            currentCapital += profit;
            nextDay(profit, "-");
        }

        function submitLoss() {
            let loss = parseFloat(document.getElementById("lossAmount").value);
            if (isNaN(loss) || loss < 0 || loss > currentCapital) {
                alert("Enter a valid loss amount!");
                return;
            }
            currentCapital -= loss;
            nextDay("-", loss);
        }

        function nextDay(profit, loss) {
            day++;
            updateTarget();
            addToTable(day, currentCapital, (currentCapital * dailyRate).toFixed(2), profit, loss, currentCapital);
        }

        function addToTable(day, capital, target, profit, loss, newCapital) {
            let table = document.getElementById("logTable");
            let row = `<tr><td>${day}</td><td>₹${capital}</td><td>₹${target}</td><td>₹${profit}</td><td>₹${loss}</td><td>₹${newCapital}</td></tr>`;
            table.innerHTML += row;
        }

        function resetCalculator() {
            document.getElementById("logTable").innerHTML = "";
            document.getElementById("currentTarget").innerHTML = "";
        }

        function toggleMoneyManagement() {
            let compounding = document.getElementById("compoundingCalculator");
            let moneyManagement = document.getElementById("moneyManagement");

            if (moneyManagement.style.display === "none") {
                compounding.style.display = "none";
                moneyManagement.style.display = "block";
            } else {
                compounding.style.display = "block";
                moneyManagement.style.display = "none";
            }
        }
    </script>

</body>
</html>
