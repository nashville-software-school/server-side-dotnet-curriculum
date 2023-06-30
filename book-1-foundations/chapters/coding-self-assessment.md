# :trumpet: :page_with_curl: Brass & Poem Console Application

## Introduction

In this self-assessment, you will be working on a console application named "Brass & Poem." The application represents a business that sells poetry books and brass musical instruments. Your task is to write the business logic in Program.cs and create two modules, Product.cs and ProductType.cs, with their respective class definitions. The Product class will define the properties for a product, while the ProductType class will define the properties for a product type. Additionally, you will implement a menu system in Program.cs to interact with the application.

## Instructions

### Create the Project
1. Request the Github Classroom link for the assignment from your instructor.
1. Clone your new repo that Github classroom created to your local computer and open it in VS Code.
1. For the rest of these instructions, the files will be referring to files inside the `BrassAndPoem` folder. You can look at the `BrassAndPoem.Tests` folder if you wish, but you do not need to modify any code there. 

## Define the ProductType Class
a. Inside the ProductType.cs file, create a class named ProductType.
b. Add the following property to the ProductType class:

* Title (string): Represents the title of the product type.
* Id (int) : Represents a unique Id for the product type

### Define the Product Class
a. Inside the Product.cs file, create a class named Product.
b. Add the following properties to the Product class:
* Name (string): Represents the name of the product.
* Price (decimal): Represents the price of the product.
* ProductTypeId (int): Represents the id for the product's product type

### Create the program loop
1. Declare a list of product types and a list of products. When creating the lists, add at least two product types and five products.
1. Display a welcome message for the application
1. Create a loop that asks the user to choose an option, and will continue running until the use selects `5`, at which point the program will finish. 

### Implement the Menu System in Program.cs
Inside the `Program.cs` file , you will implement the following methods underneath the loop (detailed instructions for each below). To test whether these methods work, add logic to the program loop to call the appropriate method when a user chooses an option:

1. `DisplayMenu`

1. `DisplayAllProducts`

1. `DeleteProduct`

1. `AddProduct`

1. `UpdateProduct`

### Implement the `DisplayMenu` Method

1. The DisplayMenu method should display the following options to the console:

   ```sh
   1. Display all products
   2. Delete a product
   3. Add a new product
   4. Update product properties
   5. Exit
   ```

### Implement the DisplayAllProducts Method

1. Iterate over the products and display each product's name and price on a new line in the console. Start the line with the index of that product in the `List`.
1. Add the product type title to the product display. HINT: You will need to use a Linq method to get the product type for each product. 

### Implement the DeleteProduct Method

1. Display the products and prompt the user to enter the index of the product they want to delete.
1. Find the product with the provided index and remove it from the list of products.

### Implement the AddProduct Method

1. Prompt the user to enter the name and price of the new product (in this order).
1. Display the `ProductType`s and prompt the user to choose a type for the new product. 
1. Create a new instance of the Product class using the provided information.
1. Add the newly created product to the list of products.

### Implement the UpdateProduct Method

1. Display the products and prompt the user to enter the index of the product they want to update.
1. Find the product with the provided index and retrieve its reference.
1. Prompt the user to enter the updated name, price and product type for the product (in that order). If the user presses enter without typing anything, leave the property unchanged. 
1. Update the product's properties with the provided information.
