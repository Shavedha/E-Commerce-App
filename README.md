# E-COMMERCE APP USING REACT, SPRINGBOOT AND SQL
## AIM
To create a E-Commerce application using React,SpringBoot and SQL
## ALGORITHM
1. Configure a new project with the required dependencies and files.
2. Initialise the same in IntelliJ and include file Patient.java which contains all the variables like Product ID,name,costumer name and price and their respective constructor,getters and setters.
3. Include Components and service files which configures and works for commands like GET,POST and DELETE.
4. Run the file by connecting to an database by creating the same in psql.
5. Start an react project named E-Commerce-frontend
6. Include necessary functions in App.js
7. Include the necessary .css files for all the three methods.
8. Define separate .js files for viewing the list of products,adding an product,and deleting an productId.
9. Connect with the backend server and view the output user interface in the browser.

## PROGRAM
### SpringBoot Program
### Product.java
```
package com.saveetha.product.pro;
import jakarta.persistence.*;
import java.time.LocalDate;
import java.time.Period;
@Entity
@Table
public class Product {
    @Id
    @SequenceGenerator(
            name="productSequence",
            sequenceName = "productSequence",
            allocationSize = 1
    )
    @GeneratedValue(
            strategy = GenerationType.SEQUENCE,
            generator = "productSequence"
    )
    private Long productID;
    private String productName;
    private String customerName;
    private Long productPrice;

    public Product() {
    }

    public product(String productName,String customerName,Long productPrice) {
        this.productName = productName;
        this.customerName = customerName;
        this.productPrice = productPrice;
    }
    public Long getproductID() {
        return productID;
    }
    public String getproductName() {
        return productName;
    }
    public String getcustomerName() {
        return customerName;
    }
    public Long getproductPrice() {
        return productPrice;
    }

    public void setproductID(Long productID) {
        this.productID = productID;
    }
    public void setproductName(String productName) {
        this.productName = productName;
    }
    public void setcustomerName(Long customerName) {
        this.customerName = customerName;
    }
    public void setproductPrice(Long productPrice) {
        this.productPrice = productPrice;
    }
@Override
    public String toString() {
        return "Product{" +
                "productId=" + productID +
                ", productName='" + productName + '\'' +
                ", customerName=" + customerName +
                ", productPrice=" + productPrice + '\'' +
                '}';
    }
}

```
### ProductController.java
```
package com.saveetha.product.pro;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;
import java.util.List;
@RestController
@RequestMapping(path="/api/v1/product")
public class ProductController {
    private final ProductService productService;
    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }
    @GetMapping("/")
    public List<Product> getProductDetails(){
        return productService.displayProductDetails();
    }
    @PostMapping("/")
    public void postProduct(@RequestBody Product product){
        productService.registerNewProduct(product);
    }
    @DeleteMapping(path={"/{id}"})
    public void deleteProduct(@PathVariable("id") Long productId)
    {
        productService.removeProduct(productId);
    }
}

```
### React Program
### ProductDirectoryComponent.js
```
import React, { useEffect, useState } from 'react';
import './ProductDirectoryComponent.css'; 
function ProductDirectoryComponent() {
  const [product, setproduct] = useState([]);
  useEffect(() => {
    fetchProduct();
  }, []);

  const fetchProduct = async () => {
    try {
      const response = await fetch('http://localhost:8080/api/v1/product/');
      const data = await response.json();
      setProduct(data);
    } catch (error) {
      console.error('Error:', error);
    }
  };
  return (
    <div className="product-list">
      {products.map((product) => (
        <div className="product-card" key={product.productID}>
          <p>Product ID: {product.productID}</p>
          <p>Product Name: {product.productName}</p>
          <p>Customer Name: {product.customerName}</p>
          <p>Product Price: {product.productPrice}</p>
        </div>
      ))}
    </div>
  );
};

export default ProductDirectoryComponent;
```
### ProductRegistrationComponent.js
```
import React, { useState } from 'react';
import './ProductRegistrationComponent.css'

function ProductRegistrationComponent() {
  const [product, setProduct] = useState({
    productName: '',
    customerName: '',
    productPrice: ''
  });

  const handleChange = (event) => {
    const { name, value } = event.target;
    setProduct({ ...product, [name]: value });
  };

  const handleSubmit = async(event) => {
    event.preventDefault();
    await fetch('http://localhost:8080/api/v1/product/',{
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(product)
    })
        .then((response) => {
            if (response.status === 500)
            {
                alert(`Error!`)
            }
            else{
                alert(`Data of ${product.productName} is added successfully`)
                window.location.href = '/'
            }
        })
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Product Name:
        <input
          type="text"
          name="productName"
          value={product.productName}
          onChange={handleChange}
          required
        />
      </label>
      <label>
        Customer Name:
        <input
          type="text"
          name="customerName"
          value={product.customerName}
          onChange={handleChange}
          required
        />
      </label>
      <label>
        Product Price:
        <input
          type="number"
          name="productPrice"
          value={product.productPrice}
          onChange={handleChange}
          required
        />
      </label>
      
      <button type="submit">Submit</button>
    </form>
  );
};

export default ProductRegistrationComponent;
```
### ProductDeletionComponent.js
```
import React, { useState } from 'react';
import './ProductDeletionComponent.css'
function ProductDeletionComponent() {
  const [productID, setProductID] = useState('');
  const handleChange = (event) => {
    setProductID(event.target.value);
  };
  const handleSubmit = async(event) => {
    event.preventDefault();
    await fetch(`http://localhost:8080/api/v1/product/${productID}`,{
      method: 'DELETE'
    })
    .then((response) => {
            if (response.status === 500)
            {
                alert(`Error!`)
            }
            else{
                alert(`Data deleted successfully`)
                window.location.href = '/'
            }
        })
  };
  return (
    <form onSubmit={handleSubmit}>
      <label>
        Product ID:
        <input
          type="number"
          name="productID"
          value={productID}
          onChange={handleChange}
          required
        />
      </label>
      <button type="submit">Submit</button>
    </form>
  );
};
export default ProductDeletionComponent;

```
## OUTPUT
<img width="959" alt="image" src="https://github.com/Shavedha/E-Commerce-App/assets/93427376/c57734a9-a138-4e38-b7c0-0a4121a18527">

<img width="960" alt="image" src="https://github.com/Shavedha/E-Commerce-App/assets/93427376/8acd68d6-5f57-4c98-b37e-e06217422d3b">

## RESULT
Thus an E-Commerce Application is created using React,SpringBoot and SQL.
