# Brass & Poem Console Application

## Introduction

In this self-assessment, you will be working on a console application named "Brass & Poem." The application represents a business that sells poetry books and brass musical instruments. Your task is to write the business logic in Program.cs and create two modules, Product.cs and ProductType.cs, with their respective class definitions. The Product class will define the properties for a product, while the ProductType class will define the properties for a product type. Additionally, you will implement a menu system in Program.cs to interact with the application.

## Instructions

### Create the Project

a. Open your preferred Integrated Development Environment (IDE) for C# development.
b. Create a new console application project named "BrassAndPoem."

## Define the ProductType Class
a. Create a new file named ProductType.cs.
b. Inside the ProductType.cs file, create a class named ProductType.
c. Add the following property to the ProductType class:

title (string): Represents the title of the product type.

### Define the Product Class
a. Create a new file named Product.cs.
b. Inside the Product.cs file, create a class named Product.
c. Add the following properties to the Product class:

* name (string): Represents the name of the product.
* price (decimal): Represents the price of the product.
* type (ProductType): Represents the composed, related product type for this product

###Implement the Menu System in Program.cs

a. Open the Program.cs file.
b. Inside the Program class, implement the following methods:

DisplayMenu: Displays the main menu with options for interacting with the application.

DisplayAllProducts: Displays all the available products in the console.

DeleteProduct: Deletes a product from the list of available products

AddProduct: Adds a new product to the list of available products.

UpdateProduct: Updates the properties of an existing product.



### Implement the DisplayMenu Method

1. Inside the Program class, implement the DisplayMenu method.
1. The DisplayMenu method should display the following options to the console:

   ```sh
   1. Display all products
   2. Delete a product
   3. Add a new product
   4. Update product properties
   5. Exit
   ```

1. Prompt the user to enter the number corresponding to their desired option.
1. Based on the user's input, call the corresponding method to perform the desired action.

### Implement the DisplayAllProducts Method

1. Inside the Program class, implement the DisplayAllProducts method.
1. Retrieve the **List** of **Product** objects from wherever you choose to store them.
1. Iterate over the products and display each product's name and price on a new line in the console.

### Implement the DeleteProduct Method

1. Inside the Program class, implement the DeleteProduct method.
1. Prompt the user to enter the name of the product they want to delete.
1. Find the product with the provided name and remove it from the list of products.

### Implement the AddProduct Method

1. Inside the Program class, implement the AddProduct method.
1. Prompt the user to enter the name and price of the new product.
1. Create a new instance of the Product class using the provided information.
1. Add the newly created product to the list of products.

### Implement the UpdateProduct Method

1. Inside the Program class, implement the UpdateProduct method.
1. Prompt the user to enter the name of the product they want to update.
1. Find the product with the provided name and retrieve its reference.
1. Prompt the user to enter the updated name and price for the product.
1. Update the product's name and price properties with the provided information.

## Main Logic

1. Create a list of products
2. Display menu
3. Use a loop to keep the application running until the user chooses the "Exit" option.

## Test Your Application

1. Compile and run your application.
1. Test each menu option to ensure they perform the expected actions.
1. Verify that the product list is updated correctly after adding, deleting, or updating products.
