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
.firstPart {
    background: rgb(75, 222, 222);
    padding: 20px;
    width: 450px;
    border-radius: 10px;
    box-shadow: 0 5px 10px rgba(0,0,0,0.2);
}
input[type="number"] {
    width: 100%;
    padding: 7px;
    margin-bottom: 10px;
}
label {
    display: block;
    margin: 10px 0;
}
button {
    width: 100%;
    padding: 8px;
    background: #3e68e4;
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
.items {
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

<div class="firstPart">
    <h2>Add New Room</h2>
    <input type="number" id="rNo" placeholder="Room Number">
    <input type="number" id="rCap" placeholder="Room Capacity">

    <label><input type="checkbox" id="rAC"> AC Available</label>
    <label><input type="checkbox" id="rWash"> Attached Washroom</label>

    <button onclick="createRoom()">Save Room</button>
</div>

<div class="firstPart">
    <h2>Allocate Room</h2>
    <input type="number" id="stuCount" placeholder="Number of Students">

    <label><input type="checkbox" id="reqAC"> Need AC</label>
    <label><input type="checkbox" id="reqWash"> Need Washroom</label>

    <button onclick="findRoom()">Allocate</button>

    <div id="result"></div>
</div>

</section>

<section>
    <h2 class="items">Available Rooms</h2>
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
/* ROOM DATA (Local Storage) */
let hostelRooms = JSON.parse(localStorage.getItem("rooms")) || [];

/* LOAD ROOMS ON PAGE LOAD */
window.onload = loadRooms;

/* ADD ROOM */
function createRoom() {
    if (!rNo.value || rCap.value <= 0) {
        alert("Invalid room details");
        return;
    }

    hostelRooms.push({
        roomNo: rNo.value,
        capacity: Number(rCap.value),
        ac: rAC.checked,
        washroom: rWash.checked
    });

    localStorage.setItem("rooms", JSON.stringify(hostelRooms));
    loadRooms();

    rNo.value = "";
    rCap.value = "";
    rAC.checked = false;
    rWash.checked = false;
}

/* DISPLAY ROOMS */
function loadRooms() {
    roomList.innerHTML = "";
    hostelRooms.forEach(room => {
        roomList.innerHTML += `
        <tr>
            <td>${room.roomNo}</td>
            <td>${room.capacity}</td>
            <td>${room.ac ? "Yes" : "No"}</td>
            <td>${room.washroom ? "Yes" : "No"}</td>
        </tr>`;
    });
}

/* ALLOCATE ROOM */
function findRoom() {
    let students = Number(stuCount.value);
    let needAC = reqAC.checked;
    let needWash = reqWash.checked;

    let room = hostelRooms.find(r =>
        r.capacity >= students &&
        (!needAC || r.ac) &&
        (!needWash || r.washroom)
    );

    if (!room) {
        result.innerHTML = "❌ No room available";
    } else {
        result.innerHTML = `
        ✅ Room Allocated <br>
        Room No: ${room.roomNo} <br>
        Capacity: ${room.capacity} <br>
        AC: ${room.ac ? "Yes" : "No"} <br>
        Washroom: ${room.washroom ? "Yes" : "No"}
        `;
    }

    stuCount.value = "";
    reqAC.checked = false;
    reqWash.checked = false;
}
</script>

</body>
</html>
