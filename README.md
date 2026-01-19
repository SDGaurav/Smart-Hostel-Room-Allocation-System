# Smart-Hostel-Room-Allocation-System
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Smart Hostel Room Allocation System</title>
<style>
body {
    font-family: Verdana, sans-serif;
    background-color: #e9ecf1;
    margin: 0;
}

header {
    background-color: #222;
    color: white;
    padding: 15px;
    text-align: center;
}

.section {
    display: flex;
    justify-content: center;
    gap: 40px;
    margin: 30px;
    flex-wrap: wrap;
}

.panel {
    background: white;
    padding: 20px;
    width: 300px;
    border-radius: 10px;
    box-shadow: 0 5px 10px rgba(0,0,0,0.2);
}

.panel h2 {
    margin-bottom: 10px;
}

input[type="number"] {
    width: 100%;
    padding: 7px;
    margin-bottom: 10px;
}

label {
    display: block;
    margin: 5px 0;
}

button {
    width: 100%;
    padding: 8px;
    background: #28a745;
    border: none;
    color: white;
    border-radius: 6px;
    cursor: pointer;
}

button:hover {
    background: #1e7e34;
}

table {
    width: 85%;
    margin: auto;
    border-collapse: collapse;
    background: white;
}

th, td {
    border: 1px solid #bbb;
    padding: 8px;
    text-align: center;
}

th {
    background: #28a745;
    color: white;
}

.center {
    text-align: center;
}

#result {
    margin-top: 10px;
    font-weight: bold;
}
</style>
</head>
<body>

<header>
    <h1>Smart Hostel Room Allocation System</h1>
</header>

<section class="section">

    <div class="panel">
        <h2>Add New Room</h2>
        <input type="number" id="rNo" placeholder="Room Number">
        <input type="number" id="rCap" placeholder="Room Capacity">

        <label><input type="checkbox" id="rAC"> AC Available</label>
        <label><input type="checkbox" id="rWash"> Attached Washroom</label>

        <button onclick="createRoom()">Save Room</button>
    </div>

    <div class="panel">
        <h2>Allocate Room</h2>
        <input type="number" id="stuCount" placeholder="Number of Students">

        <label><input type="checkbox" id="reqAC"> Need AC</label>
        <label><input type="checkbox" id="reqWash"> Need Washroom</label>

        <button onclick="findRoom()">Allocate</button>

        <div id="result"></div>
    </div>

</section>

<section>
    <h2 class="center">Available Rooms</h2>
    <table>
        <thead>
            <tr>
                <th>Room</th>
                <th>Capacity</th>
                <th>AC</th>
                <th>Washroom</th>
            </tr>
        </thead>
        <tbody id="roomList"></tbody>
    </table>
</section>

<script>
let hostelRooms = [];

// CREATE ROOM
function createRoom() {
    const roomNo = rNo.value;
    const capacity = rCap.value;

    if (!roomNo || capacity <= 0) {
        alert("Invalid room details");
        return;
    }

    if (hostelRooms.find(r => r.roomNo == roomNo)) {
        alert("Room already exists");
        return;
    }

    hostelRooms.push({
        roomNo: roomNo,
        capacity: Number(capacity),
        ac: rAC.checked,
        washroom: rWash.checked
    });

    showRooms();
    rNo.value = '';
    rCap.value = '';
    rAC.checked = false;
    rWash.checked = false;
}

// DISPLAY ROOMS
function showRooms() {
    roomList.innerHTML = "";
    hostelRooms.forEach(room => {
        roomList.innerHTML += `
            <tr>
                <td>${room.roomNo}</td>
                <td>${room.capacity}</td>
                <td>${room.ac ? "Yes" : "No"}</td>
                <td>${room.washroom ? "Yes" : "No"}</td>
            </tr>
        `;
    });
}

// FIND & ALLOCATE ROOM
function findRoom() {
    const students = Number(stuCount.value);
    if (students <= 0) {
        alert("Enter valid student count");
        return;
    }

    let filtered = hostelRooms.filter(room => {
        if (room.capacity < students) return false;
        if (reqAC.checked && !room.ac) return false;
        if (reqWash.checked && !room.washroom) return false;
        return true;
    });

    filtered.sort((x, y) => x.capacity - y.capacity);

    if (filtered.length === 0) {
        result.innerHTML = "❌ No room available";
    } else {
        let r = filtered[0];
        result.innerHTML = `
            ✅ Room Allocated<br>
            Room No: ${r.roomNo}<br>
            Capacity: ${r.capacity}<br>
            AC: ${r.ac ? "Yes" : "No"}<br>
            Washroom: ${r.washroom ? "Yes" : "No"}
        `;
    }

    stuCount.value = '';
    reqAC.checked = false;
    reqWash.checked = false;
}
</script>

</body>
</html>

