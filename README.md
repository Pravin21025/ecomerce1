<!DOCTYPE html>
<html>
<head>
    <title>Product List</title>
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
    <script src = "https://cdnjs.cloudflare.com/ajax/libs/axios/1.6.0/axios.min.js"></script>

    <script>
        let products = [];

        function addProduct() {
            const productNameInput = document.getElementById("productName");
            const sellingPriceInput = document.getElementById("sellingPrice");

            const productName = productNameInput.value;
            const sellingPrice = parseFloat(sellingPriceInput.value);

            if (productName && !isNaN(sellingPrice)) {
                products.push({ productName, sellingPrice });
                productNameInput.value = "";
                sellingPriceInput.value = "";
                displayProductList();
            }
            axios.post("https://crudcrud.com/api/4cd5f212d8cc478297ee6fbb630ff2ee/appointmentData",obj);
            .then[(response) =>{
                showNewUserOnScreen(respond.data)

                console.log(response)
            }]
            .catch((err))=>{
                console.log(err)
            }
            
        }

        function displayProductList() {
            const productList = document.getElementById("product-list");
            productList.innerHTML = '';

            products.forEach(product => {
                const listItem = document.createElement("li");
                listItem.textContent = `${product.productName} - $${product.sellingPrice.toFixed(2)}`;
                productList.appendChild(listItem);
            });
        }
    </script>
</body>
</html>

