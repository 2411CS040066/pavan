<!DOCTYPE html>
<html>
<head>
  <title>Simple Product CRUD</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f4;
      padding: 20px;
    }

    h2, h3 {
      text-align: center;
    }

    form {
      background: #ffffff;
      padding: 15px;
      border: 1px solid #ccc;
      max-width: 400px;
      margin: 0 auto 20px;
    }

    label {
      display: block;
      margin-top: 10px;
    }

    input[type="text"],
    input[type="number"] {
      width: 100%;
      padding: 8px;
      margin-top: 5px;
      border: 1px solid #ccc;
    }

    button {
      margin-top: 15px;
      padding: 8px 12px;
      background-color: #4285f4;
      color: white;
      border: none;
      cursor: pointer;
    }

    button:hover {
      background-color: #3367d6;
    }

    table {
      width: 80%;
      margin: 0 auto;
      border-collapse: collapse;
      background: #fff;
      border: 1px solid #ccc;
    }

    th, td {
      padding: 10px;
      border: 1px solid #ccc;
      text-align: left;
    }

    th {
      background-color: #e0e0e0;
    }

    td button {
      padding: 5px 10px;
      margin-right: 5px;
      border: none;
      cursor: pointer;
    }

    td button:first-child {
      background-color: #4caf50;
      color: white;
    }

    td button:last-child {
      background-color: #f44336;
      color: white;
    }

    td button:hover {
      opacity: 0.8;
    }
  </style>
</head>
<body>
  <h2>Product CRUD</h2>
  <form id="productForm">
    <input type="hidden" id="productId">
    <label>Product Name:</label>
    <input type="text" id="productName" required>

    <label>Price:</label>
    <input type="number" id="productPrice" required>

    <button type="submit">Submit</button>
  </form>

  <h3>Product List</h3>
  <table id="productTable">
    <thead>
      <tr><th>ID</th><th>Product</th><th>Price</th><th>Actions</th></tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    let products = [];
    let productIdCounter = 1;

    document.getElementById("productForm").addEventListener("submit", function(e) {
      e.preventDefault();
      const id = document.getElementById("productId").value;
      const name = document.getElementById("productName").value;
      const price = parseFloat(document.getElementById("productPrice").value);

      if (id) {
        const product = products.find(p => p.id == id);
        if (product) {
          product.name = name;
          product.price = price;
          console.log("Updated product ID " + id + ":", product);
        }
      } else {
        const newProduct = { id: productIdCounter++, name, price };
        products.push(newProduct);
        console.log("Created new product:", newProduct);
      }

      resetForm();
      renderTable();
    });

    function renderTable() {
      const tbody = document.querySelector("#productTable tbody");
      tbody.innerHTML = "";
      products.forEach(product => {
        const row = document.createElement("tr");
        row.innerHTML = `
          <td>${product.id}</td>
          <td>${product.name}</td>
          <td>₹${product.price.toFixed(2)}</td>
          <td>
            <button onclick="editProduct(${product.id})">Edit</button>
            <button onclick="deleteProduct(${product.id})">Delete</button>
          </td>
        `;
        tbody.appendChild(row);
      });
    }

    function editProduct(id) {
      const product = products.find(p => p.id === id);
      if (product) {
        document.getElementById("productId").value = product.id;
        document.getElementById("productName").value = product.name;
        document.getElementById("productPrice").value = product.price;
        console.log("Editing product ID " + id + ":", product);
      }
    }

    function deleteProduct(id) {
      const product = products.find(p => p.id === id);
      products = products.filter(p => p.id !== id);
      console.log("Deleted product ID " + id + ":", product);
      renderTable();
    }

    function resetForm() {
      document.getElementById("productForm").reset();
      document.getElementById("productId").value = "";
    }
  </script>
</body>
</html>
