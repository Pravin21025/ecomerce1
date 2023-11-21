<!DOCTYPE html>
<html>
<head>
    <title>Product List</title>
    <!-- Include Axios script -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/axios/1.6.2/axios.min.js"></script>
</head>
<body>
    <h1>Product List</h1>

    <h2>Add Product</h2>
    <form id="add-product-form">
        <label for="productName">Product Name:</label>
        <input type="text" id="productName" required>
        <label for="sellingPrice">Selling Price:</label>
        <input type="number" step="0.01" id="sellingPrice" required>
        <button type="button" onclick="addProduct()">Add Product</button>
    </form>

    <h2>Product List</h2>
    <ul id="product-list">
        <!-- Product list will be displayed here -->
    </ul>

    <h2>Update Product</h2>
    <form id="update-product-form">
        <label for="updateProductName">Product Name:</label>
        <input type="text" id="updateProductName" required>
        <label for="updateSellingPrice">Selling Price:</label>
        <input type="number" step="0.01" id="updateSellingPrice" required>
        <button type="button" onclick="updateProduct()">Update Product</button>
    </form>

    <h2>User Input</h2>
    <div id="user-input">
        <!-- User input will be displayed here -->
    </div>

    <script>
        let products = [];
        let selectedIndex = -1; // Track the selected index for updating

        // Check if product data is available in local storage
        if (localStorage.getItem('products')) {
            products = JSON.parse(localStorage.getItem('products'));
            displayProductList();
        }

        function addProduct() {
            const productNameInput = document.getElementById("productName");
            const sellingPriceInput = document.getElementById("sellingPrice");

            const productName = productNameInput.value;
            const sellingPrice = parseFloat(sellingPriceInput.value);

            if (productName && !isNaN(sellingPrice)) {
                // Push the product to the local array for display
                products.push({ productName, sellingPrice });

                // Save the product data in local storage
                localStorage.setItem('products', JSON.stringify(products));

                // Clear input fields
                productNameInput.value = "";
                sellingPriceInput.value = "";

                // Display the product list with the new item
                displayProductList();

                // Store the product data in the database using Axios
                storeProductData({ productName, sellingPrice });
            }
        }

        function updateProduct() {
            const updateProductNameInput = document.getElementById("updateProductName");
            const updateSellingPriceInput = document.getElementById("updateSellingPrice");

            const updatedProductName = updateProductNameInput.value;
            const updatedSellingPrice = parseFloat(updateSellingPriceInput.value);

            if (updatedProductName && !isNaN(updatedSellingPrice) && selectedIndex !== -1) {
                // Update the product data at the selected index
                products[selectedIndex] = { productName: updatedProductName, sellingPrice: updatedSellingPrice };

                // Save the updated product data in local storage
                localStorage.setItem('products', JSON.stringify(products));

                // Clear input fields
                updateProductNameInput.value = "";
                updateSellingPriceInput.value = "";

                // Display the updated product list
                displayProductList();

                // Clear the selected index for updating
                selectedIndex = -1;
            }
        }

        function deleteProduct(index) {
            if (index >= 0 && index < products.length) {
                // Remove the product from the list
                products.splice(index, 1);

                // Save the updated product list to local storage
                localStorage.setItem('products', JSON.stringify(products));

                // Display the updated product list
                displayProductList();

                // Delete the product data from the database using Axios
                deleteProductData(index);
            }
        }

        function displayProductList() {
            const productList = document.getElementById("product-list");
            productList.innerHTML = '';

            products.forEach((product, index) => {
                const listItem = document.createElement("li");
                listItem.textContent = `${product.productName} - $${product.sellingPrice.toFixed(2)}`;

                // Add an "Edit" button for each product
                const editButton = document.createElement("button");
                editButton.textContent = "Edit";
                editButton.onclick = () => editProduct(index);

                // Add a "Delete" button for each product
                const deleteButton = document.createElement("button");
                deleteButton.textContent = "Delete";
                deleteButton.onclick = () => deleteProduct(index);

                listItem.appendChild(editButton);
                listItem.appendChild(deleteButton);
                productList.appendChild(listItem);
            });
        }

        // Function to store product data in the database using Axios
        function storeProductData(productData) {
            axios.post("https://crudcrud.com/api/d2c17aba3c0242eab3edecb294428246/apointmentData", productData)
                .then((response) => {
                    console.log('Data stored successfully:', response.data);
                })
                .catch((error) => {
                    console.error('Error storing data:', error);
                });
        }

        // Function to delete product data from the database using Axios
        function deleteProductData(index) {
            axios.delete(`https://crudcrud.com/api/d2c17aba3c0242eab3edecb294428246/apointmentData/${index}`)
                .then((response) => {
                    console.log('Data deleted successfully:', response.data);
                })
                .catch((error) => {
                    console.error('Error deleting data:', error);
                });
        }
    </script>
</body>
</html>
