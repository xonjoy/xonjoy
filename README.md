<!DOCTYPE html>
<html>
<head>
    <title>Estimate Bill</title>
    <style>
        table {
            border-collapse: collapse;
        }

        table, th, td {
            border: 1px solid black;
            padding: 5px;
        }

        th {
            background-color: #f2f2f2;
        }

        input[type="text"],
        input[type="number"] {
            width: 100%;
            box-sizing: border-box;
        }
    </style>
</head>
<body>
    <h1>Estimate Bill</h1>
    <form method="post" id="estimate-form">
        <table>
            <tr>
                <th>S/N</th>
                <th>Particulars</th>
                <th>Qty</th>
                <th>Rate</th>
                <th>Amount</th>
            </tr>
            <tbody id="bill-items">
                <tr>
                    <td>1</td>
                    <td><input type="text" name="particulars[]"></td>
                    <td><input type="number" name="qty[]" min="1" value="1" onchange="calculateAmount(this)"></td>
                    <td><input type="number" name="rate[]" step="0.01" min="0.01" value="0.00" onchange="calculateAmount(this)"></td>
                    <td>0.00</td>
                </tr>
            </tbody>
        </table>
        <br>
        <input type="button" value="Add Item" onclick="addNewItem()">
        <input type="submit" value="Calculate Total">
    </form>

    <h2>Total Amount: <span id="totalAmount">0.00</span></h2>

    <script>
        function calculateAmount(inputElement) {
            const tr = inputElement.parentElement.parentElement;
            const qty = parseFloat(tr.querySelector('input[name="qty[]"]').value);
            const rate = parseFloat(tr.querySelector('input[name="rate[]"]').value);
            const amount = (qty * rate).toFixed(2);
            tr.querySelector('td:last-child').textContent = amount;

            updateTotalAmount();
        }

        function updateTotalAmount() {
            const amounts = document.querySelectorAll('#bill-items td:last-child');
            let totalAmount = 0;

            amounts.forEach(amountElement => {
                totalAmount += parseFloat(amountElement.textContent);
            });

            document.getElementById('totalAmount').textContent = totalAmount.toFixed(2);
        }

        function addNewItem() {
            const billItems = document.getElementById('bill-items');
            const itemCount = billItems.children.length + 1;

            const newRow = document.createElement('tr');
            newRow.innerHTML = `
                <td>${itemCount}</td>
                <td><input type="text" name="particulars[]"></td>
                <td><input type="number" name="qty[]" min="1" value="1" onchange="calculateAmount(this)"></td>
                <td><input type="number" name="rate[]" step="0.01" min="0.01" value="0.00" onchange="calculateAmount(this)"></td>
                <td>0.00</td>
            `;

            billItems.appendChild(newRow);
        }
    </script>
</body>
</html>
