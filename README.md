# Attendance-Panel
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zellbury Cutting Depart - Attendance</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Sherilyn&display=swap');

        body { font-family: Arial, sans-serif; text-align: center; background-color: #f4f4f4; }
        h2 {
            font-family: 'Sherilyn', cursive;
            font-size: 36px; 
            font-weight: bold; 
            color: darkyellow;
        }
        table { width: 100%; border-collapse: collapse; background: white; margin-bottom: 20px; }
        th, td { border: 1px solid black; padding: 8px; text-align: center; }
        th { background-color: #4CAF50; color: white; }
        .present { color: green; font-weight: bold; cursor: pointer; }
        .absent { color: red; font-weight: bold; cursor: pointer; }
        .popup { display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); background: white; padding: 20px; border-radius: 5px; box-shadow: 0px 0px 10px gray; }
        .popup input { display: block; margin: 10px 0; }
        .overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.5); display: none; }
        button { padding: 10px; cursor: pointer; background: #008CBA; color: white; border: none; margin-top: 10px; }
        .number { color: black; font-weight: normal; } /* Make the numbers black */
    </style>
</head>
<body>
    <h2>Zellbury Cutting Depart</h2>
    <div class="controls">
        <label for="month">Select Month:</label>
        <select id="month"></select>
        <button onclick="markAttendance()">Mark Attendance</button>
    </div>
    <br> <!-- Added a line break to create space between the "Mark Attendance" button and the sheet -->
    <table>
        <thead>
            <tr>
                <th>Name</th>
                <th colspan="31">Attendance</th>
            </tr>
        </thead>
        <tbody id="attendance-body"></tbody>
    </table>
    <div class="overlay" id="overlay" onclick="closePopup()"></div>
    <div class="popup" id="popup">
        <h3>Edit Attendance Details</h3>
        <label>Entry Time:</label>
        <input type="time" id="entry-time">
        <label>Exit Time:</label>
        <input type="time" id="exit-time">
        <label>Overtime Hours:</label>
        <input type="number" id="overtime" min="0" step="0.5">
        <button onclick="saveDetails()">Save</button>
    </div>
    <div class="controls">
        <button onclick="addName()">Add Name</button>
        <button onclick="removeName()">Remove Name</button>
    </div>
    <script>
        let names = ["Shayan Aas", "Rashid Aas", "Luqman", "Umar", "Ashan", "Basit", "Farhan", "Salman"];
        let editable = false;
        let selectedCell = null;

        function markAttendance() { 
            editable = true; 
            alert("Attendance marked!"); 
        }

        function toggleAttendance(cell) {
            if (!editable) return;
            cell.textContent = cell.textContent === 'P' ? 'A' : 'P';
            cell.className = cell.textContent === 'P' ? 'present' : 'absent';
            selectedCell = cell;
            document.getElementById("overlay").style.display = "block";
            document.getElementById("popup").style.display = "block";
        }

        function closePopup() {
            document.getElementById("overlay").style.display = "none";
            document.getElementById("popup").style.display = "none";
        }

        function saveDetails() {
            closePopup();
        }

        function generateTable() {
            let tbody = document.getElementById("attendance-body");

            // Add an extra row with numbers 1 to 31
            let row = document.createElement("tr");
            let emptyNameCell = document.createElement("td");
            row.appendChild(emptyNameCell);  // Empty cell for the name
            for (let i = 1; i <= 31; i++) {
                let cell = document.createElement("td");
                cell.textContent = i; // Adding number to the slot
                cell.classList.add("number");  // Styling number
                row.appendChild(cell);
            }
            tbody.appendChild(row);

            // Add rows for each person
            names.forEach(name => {
                let row = document.createElement("tr");
                let nameCell = document.createElement("td");
                nameCell.textContent = name;
                row.appendChild(nameCell);
                for (let i = 1; i <= 31; i++) {
                    let cell = document.createElement("td");
                    cell.textContent = "-";
                    cell.onclick = function() { toggleAttendance(cell); };
                    row.appendChild(cell);
                }
                tbody.appendChild(row);
            });
        }

        function addName() {
            let newName = prompt("Enter new name:");
            if (newName) {
                names.push(newName);
                document.getElementById("attendance-body").innerHTML = "";
                generateTable();
            }
        }

        function removeName() {
            let nameToRemove = prompt("Enter name to remove:");
            if (names.includes(nameToRemove)) {
                names = names.filter(n => n !== nameToRemove);
                document.getElementById("attendance-body").innerHTML = "";
                generateTable();
            } else {
                alert("Name not found");
            }
        }

        function populateMonths() {
            let monthSelect = document.getElementById("month");
            let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
            months.forEach(month => {
                let option = document.createElement("option");
                option.value = month;
                option.textContent = month;
                monthSelect.appendChild(option);
            });
        }

        populateMonths();
        generateTable();
    </script>
</body>
</html>
