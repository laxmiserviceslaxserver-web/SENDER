# Attandance 
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Attendance Tracker</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Attendance Tracker</h1>
        <form id="attendanceForm">
            <input type="text" id="name" placeholder="Enter Name" required>
            <button type="submit">Mark Attendance</button>
        </form>
        <h2>Attendance List</h2>
        <table id="attendanceTable">
            <tr><th>Name</th><th>Date & Time</th></tr>
        </table>
    </div>
    <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-database-compat.js"></script>
    <script src="script.js"></script>
</body>
</html>
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
// ====== Firebase Config ======
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    databaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
};

firebase.initializeApp(firebaseConfig);
const database = firebase.database();

// ====== DOM Elements ======
const form = document.getElementById('attendanceForm');
const table = document.getElementById('attendanceTable');

// ====== Load Attendance from Firebase ======
function loadAttendance() {
    database.ref('attendance').on('value', function(snapshot){
        // Clear table except header
        table.innerHTML = "<tr><th>Name</th><th>Date & Time</th></tr>";
        snapshot.forEach(function(childSnapshot){
            const data = childSnapshot.val();
            const row = table.insertRow();
            row.insertCell(0).innerText = data.name;
            row.insertCell(1).innerText = data.datetime;
        });
    });
}
loadAttendance();

// ====== Add Attendance ======
form.addEventListener('submit', function(e){
    e.preventDefault();
    const name = document.getElementById('name').value;
    const datetime = new Date().toLocaleString();
    const newAttendance = database.ref('attendance').push();
    newAttendance.set({ name, datetime });
    form.reset();
});