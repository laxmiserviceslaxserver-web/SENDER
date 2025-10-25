# Attandance 
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Attendance Tracker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            padding: 20px;
        }
        .container {
            max-width: 600px;
            margin: auto;
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 0px 10px #aaa;
        }
        input, button {
            padding: 10px;
            margin: 5px 0;
        }
        button {
            cursor: pointer;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }
        th, td {
            border: 1px solid #000;
            padding: 8px;
            text-align: center;
        }
        th {
            background-color: #f2f2f2;
        }
        .download-btn {
            margin-top: 10px;
            padding: 8px 12px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
            border-radius: 5px;
        }
        .download-btn:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Attendance Tracker</h1>
        <form id="attendanceForm">
            <input type="text" id="name" placeholder="Enter Name" required>
            <button type="submit">Mark Attendance</button>
        </form>
        <button class="download-btn" id="downloadBtn">Download CSV</button>
        <h2>Attendance List</h2>
        <table id="attendanceTable">
            <tr><th>Name</th><th>Date & Time</th></tr>
        </table>
    </div>
    <script>
        const form = document.getElementById('attendanceForm');
        const table = document.getElementById('attendanceTable');
        const downloadBtn = document.getElementById('downloadBtn');

        // Load attendance from localStorage
        let attendanceList = JSON.parse(localStorage.getItem('attendance')) || [];

        // Function to display attendance
        function displayAttendance() {
            table.innerHTML = "<tr><th>Name</th><th>Date & Time</th></tr>";
            attendanceList.forEach(entry => {
                const row = table.insertRow();
                row.insertCell(0).innerText = entry.name;
                row.insertCell(1).innerText = entry.datetime;
            });
        }

        // Display on page load
        displayAttendance();

        // Add attendance
        form.addEventListener('submit', function(e) {
            e.preventDefault();
            const name = document.getElementById('name').value;
            const datetime = new Date().toLocaleString();
            attendanceList.push({ name, datetime });
            localStorage.setItem('attendance', JSON.stringify(attendanceList));
            displayAttendance();
            form.reset();
        });

        // Download CSV
        downloadBtn.addEventListener('click', function() {
            let csv = "Name,Date & Time\n";
            attendanceList.forEach(entry => {
                csv += `${entry.name},${entry.datetime}\n`;
            });
            const blob = new Blob([csv], { type: 'text/csv' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'attendance.csv';
            a.click();
            URL.revokeObjectURL(url);
        });
    </script>
</body>
</html>