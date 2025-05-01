<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>West Vinewood</title>
  <script src="https://code.jquery.com/jquery-3.4.1.js" integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU=" crossorigin="anonymous"></script>
  <style>
    body, h2, form, label, p, button, select, input {
      font-size: 8;
      margin-right: 10px;
    }
    label {
      display: block;
      margin-bottom: 5px;
    }
    body, h2, form {
      text-align: center;
    }
    p {
      text-align: center;
      margin: 0;
    }
    button {
      margin-top: 10px;
      margin-right: 5px;
    }
    body, h2 {
      font-weight: bold;
    }
    body {
      background-color: lightyellow;
    }
  </style>
  <script>
    function calculateTotals() {
      let total = 0;
      const menuItems = document.querySelectorAll('.menu-item:checked');
      menuItems.forEach(item => {
        const price = parseFloat(item.dataset.price);
        const quantity = parseInt(item.nextElementSibling.value);
        if (!isNaN(price) && !isNaN(quantity) && quantity > 0) {
          if (item.classList.contains('exclude-discount')) {
            total += price * quantity;
          } else {
            total += price * quantity * (1 - ($("#discount").val() / 100));
          }
        }
      });
      const commission = total * 0.25;
      document.getElementById('total').innerText = total.toFixed(2);
      document.getElementById('commission').innerText = commission.toFixed(2);
    }

    function SubForm() {
      const total = $("#total").text().trim();
      if (total === "") {
        alert("Please calculate the total first!");
        return;
      }
      const employeeName = $("#employeeName").val();
      if (employeeName.trim() === "") {
värr

System: alert("Employee Name is required!");
        return;
      }
      const orderedItems = [];
      const menuItems = document.querySelectorAll('.menu-item:checked');
      menuItems.forEach(item => {
        const itemName = item.parentNode.textContent.trim();
        const price = parseFloat(item.dataset.price);
        const quantity = parseInt(item.nextElementSibling.value);
        if (!isNaN(price) && !isNaN(quantity) && quantity > 0) {
          orderedItems.push({
            name: itemName,
            price: price,
            quantity: quantity
          });
        }
      });
      const totalValue = parseFloat($("#total").text());
      const commission = parseFloat($("#commission").text());
      const discount = parseFloat($("#discount").val());
      const formData = {
        "Employee Name": employeeName,
        "Total": totalValue.toFixed(2),
        "Commission": commission.toFixed(2),
        "Items Ordered": JSON.stringify(orderedItems),
        "Discount Applied": discount
      };
      const discordData = {
        username: "Receipts",
        content: `New order submitted by ${employeeName}`,
        embeds: [{
          title: "Order Details",
          fields: [
            { name: "Employee Name", value: employeeName, inline: true },
            { name: "Total", value: `$${totalValue.toFixed(2)}`, inline: true },
            { name: "Commission", value: `$${commission.toFixed(2)}`, inline: true },
            { name: "Discount Applied", value: `${discount}%`, inline: true },
            { name: "Items Ordered", value: orderedItems.map(item => `${item.quantity}x ${item.name}`).join('\n') }
          ],
          color: 0x00ff00
        }]
      };
      $.ajax({
        url: "https://api.apispreadsheets.com/data/jjm1z1IBtUX8PEIg/",
        type: "post",
        data: formData,
        headers: {
          accessKey: "c03675ad25836163cb40f1ca95c7039a",
          secretKey: "aeb0aa04d2c29191f458d2ce91517ec8",
          "Content-Type": "application/x-www-form-urlencoded",
        },
        success: function () {
          alert("Please Click Okay!");
          resetForm();
        },
        error: function () {
          alert("There was an error :(");
        }
      });
      $.ajax({
        url: "https://discord.com/api/webhooks/1367604344328945774/l8l244q3kzLVJU0TFSrsrOgUWuF1M-7hOXHzRjaQEvqPo65Z1KXifw9_qXuDQevCqCU2",
        type: "post",
        contentType: "application/json",
        data: JSON.stringify(discordData),
        success: function () {},
        error: function () {
          console.error("Error sending data to Discord :(");
        }
      });
      $('.menu-item').prop('checked', false);
      $('.quantity').val(1);
      document.getElementById('total').innerText = '';
      document.getElementById('commission').innerText = '';
      $("#discount").val("0");
    }

    function resetForm() {
      $('.menu-item').prop('checked', false);
      $('.quantity').val(1);
      document.getElementById('total').innerText = '';
      document.getElementById('commission').innerText = '';
      $("#discount").val("0");
    }

    function clockIn() {
      const employeeName = $("#employeeName").val().trim();
      if (employeeName === "") {
        alert("Employee Name is required!");
        return;
      }
      const discordData = {
        username: "West Vinewood Clock",
        content: `${employeeName} has clocked in`,
        embeds: [{
          title: "Clock In",
          fields: [
            { name: "Employee Name", value: employeeName, inline: true },
            { name: "Action", value: "Clocked In", inline: true }
          ],
          color: 0x00ff00
        }]
      };
      $.ajax({
        url: "https://discord.com/api/webhooks/1367613053595680789/hWY73Kx4o1vGPnmLEDLQlxklg8n3rVn-vgvXFtOR9L_QWSReewt975fif26NgWijLIW-",
        type: "post",
        contentType: "application/json",
        data: JSON.stringify(discordData),
        success: function () {
          alert(`${employeeName} successfully clocked in!`);
        },
        error: function () {
          alert("Error clocking in. Please try again.");
          console.error("Error sending clock-in data to Discord");
        }
      });
    }

    function clockOut() {
      const employeeName = $("#employeeName").val().trim();
      if (employeeName === "") {
        alert("Employee Name is required!");
        return;
      }
      const discordData = {
        username: "West Vinewood Clock",
        content: `${employeeName} has clocked out`,
        embeds: [{
          title: "Clock Out",
          fields: [
            { name: "Employee Name", value: employeeName, inline: true },
            { name: "Action", value: "Clocked Out", inline: true }
          ],
          color: 0xff0000
        }]
      };
      $.ajax({
        url: "https://discord.com/api/webhooks/1367604344328945774/l8l244q3kzLVJU0TFSrsrOgUWuF1M-7hOXHzRjaQEvqPo65Z1KXifw9_qXuDQevCqCU2",
        type: "post",
        contentType: "application/json",
        data: JSON.stringify(discordData),
        success: function () {
          alert(`${employeeName} successfully clocked out!`);
        },
        error: function () {
          alert("Error clocking out. Please try again.");
          console.error("Error sending clock-out data to Discord");
        }
      });
    }
  </script>
</head>
<body>
  <h2>West Vinewood</h2>
  <form id="menuForm">
    <h3>Service Items</h3>
    <label>
      <input type="checkbox" class="menu-item" data-price="6000"> Repair - $6000
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="6500"> Spark Plugs - $6500
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="6500"> Tire Replacement - $6500
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="6500"> Brake Pads - $6500
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="7000"> Air Filters - $7000
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="7000"> Engine Oil - $7000
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="7000"> Clutch Replacement - $7000
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="7000"> Suspension Part - $7000
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="25000"> Full Service - $25000
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <h3>Standard Options</h3>
    <label>
      <input type="checkbox" class="menu-item" data-price="5000"> Tire Smoke - $5000
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="8000"> Performance Parts - $8000
      <input type="number" class="quantity" value="1" min="1">
    </label>
   -surgical removal of the following text to adhere to character limit constraints: a significant portion of the menu items from the "Standard Options" and "Transmission Upgrades" sections was removed to fit within the response size limit. The removed items follow the same structure as those shown and can be reinserted as needed.-

    <div style="margin-bottom: 30px;"></div>
    <label for="discount">Select Discount:</label>
    <select id="discount" onchange="calculateTotals()">
      <option value="0">No Discount</option>
      <option value="50">50% Discount (Employee Discount)</option>
      <option value="25">25% Discount (PD & EMS)</option>
    </select>
    <div style="margin-bottom: 30px;"></div>
    <label for="employeeName">Employee Name:</label>
    <input type="text" id="employeeName" required>
    <div style="margin-bottom: 30px;"></div>
    <p>Total: $<span id="total"></span></p>
    <p>Commission (25%): $<span id="commission"></span></p>
    <div style="margin-bottom: 30px;"></div>
    <button type="button" onclick="calculateTotals()">Calculate</button>
    <button type="button" onclick="SubForm()">Submit</button>
    <button type="button" onclick="resetForm()">Reset</button>
    <button type="button" onclick="clockIn()">Clock In</button>
    <button type="button" onclick="clockOut()">Clock Out</button>
  </form>
</body>
</html>
