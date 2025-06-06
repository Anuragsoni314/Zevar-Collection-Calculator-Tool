<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zevar Collection – Jewellery Price Guide</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            margin: 20px;
            background: linear-gradient(135deg, #ffefba, #ffffff);
            color: #333;
        }
        .container {
            max-width: 500px;
            margin: auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            border: 3px solid gold;
        }
        h2 {
            text-align: center;
            font-weight: 600;
            color: #b8860b;
            margin-bottom: 20px;
        }
        label {
            display: block;
            margin-top: 10px;
            font-weight: 600;
        }
        input, select, button {
            width: 100%;
            padding: 10px;
            margin-top: 5px;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 16px;
        }
        button {
            background: gold;
            border: none;
            cursor: pointer;
            font-size: 18px;
            font-weight: bold;
            transition: 0.3s;
        }
        button:hover {
            background: darkgoldenrod;
            color: white;
        }
        #result {
            margin-top: 20px;
            font-weight: bold;
            text-align: center;
            padding: 10px;
            background: #fff8dc;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Zevar Collection – Jewellery Price Guide</h2>
        <label>Metal Type:</label>
        <select id="metalType" onchange="updateMakingCharges()">
            <option value="gold">Gold</option>
            <option value="silver">Silver</option>
        </select>

        <label>Live Rate (per 10 gm in ₹):</label>
        <input type="number" id="liveRate" placeholder="Enter current rate for 10 gm" required>

        <label>Weight (in grams):</label>
        <input type="number" id="weight" placeholder="Enter weight" required>

        <label>Karat:</label>
        <select id="karat">
            <option value="22">22K (91.6%)</option>
            <option value="18">18K (75%)</option>
            <option value="15.6">15.6K (650 Silver)</option>
        </select>

        <label>Making Charge per gram:</label>
        <div id="makingChargeOptions">
            <input type="radio" name="makingCharge" value="999" checked> ₹999
            <input type="radio" name="makingCharge" value="799"> ₹799
        </div>

        <label>Making Charge Discount (%):</label>
        <input type="number" id="discount" placeholder="Enter discount %" required>

        <label>GST (%):</label>
        <input type="number" id="gst" value="3" required>

        <button onclick="calculatePrice()">Calculate Price</button>
        <div id="result"></div>
    </div>

    <script>
        function updateMakingCharges() {
            let metalType = document.getElementById("metalType").value;
            let makingChargeDiv = document.getElementById("makingChargeOptions");
            let karatSelect = document.getElementById("karat");
            
            if (metalType === "gold") {
                makingChargeDiv.innerHTML = `
                    <input type="radio" name="makingCharge" value="999" checked> ₹999
                    <input type="radio" name="makingCharge" value="799"> ₹799
                `;
                karatSelect.innerHTML = `
                    <option value="22">22K (91.6%)</option>
                    <option value="18">18K (75%)</option>
                `;
            } else {
                makingChargeDiv.innerHTML = `
                    <input type="radio" name="makingCharge" value="25" checked> ₹25
                    <input type="radio" name="makingCharge" value="35"> ₹35
                    <input type="radio" name="makingCharge" value="45"> ₹45
                `;
                karatSelect.innerHTML = `
                    <option value="15.6">15.6K (650 Silver)</option>
                `;
            }
        }

        function calculatePrice() {
            let metalType = document.getElementById("metalType").value;
            let liveRatePer10gm = parseFloat(document.getElementById("liveRate").value);
            let weight = parseFloat(document.getElementById("weight").value);
            let karat = parseFloat(document.getElementById("karat").value);
            let makingCharge = parseFloat(document.querySelector('input[name="makingCharge"]:checked').value);
            let discount = parseFloat(document.getElementById("discount").value);
            let gst = parseFloat(document.getElementById("gst").value);

            if (isNaN(liveRatePer10gm) || isNaN(weight) || isNaN(gst) || isNaN(discount)) {
                document.getElementById("result").innerHTML = "Please enter all values correctly.";
                return;
            }

            let liveRatePerGram = liveRatePer10gm / 10;
            let purityFactor = (metalType === "gold") ? (karat / 24) : (karat === 15.6 ? 0.65 : 1);
            let basePrice = weight * liveRatePerGram * purityFactor;
            let metalLabel = metalType === "gold" ? "Gold Value" : "Silver Value";
            
            let makingCost = weight * makingCharge;
            let discountAmount = (makingCost * discount) / 100;
            let finalMakingCharge = makingCost - discountAmount;

            let subtotal = basePrice + finalMakingCharge;
            let gstAmount = (subtotal * gst) / 100;
            let finalPrice = subtotal + gstAmount;

            document.getElementById("result").innerHTML = `
                <strong>${metalLabel}: ₹${basePrice.toFixed(2)}</strong><br>
                Making Charges: ₹${makingCost.toFixed(2)}<br>
                Discount on Making: ₹${discountAmount.toFixed(2)}<br>
                GST (${gst}%): ₹${gstAmount.toFixed(2)}<br>
                <strong>Final Price: ₹${finalPrice.toFixed(2)}</strong>
            `;
        }
    </script>
</body>
</html>
