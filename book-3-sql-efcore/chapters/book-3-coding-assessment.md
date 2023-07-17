# :convenience_store: EF Corner Store
Ernie Fairchild has hired us to build a new point-of-sale system for his convenience store. 

## Setup
1. Request the Github Classroom link for the assignment from your instructor.
1. Clone your new repo that Github classroom created to your local computer and open it in VS Code.
1. For the rest of these instructions, the files will be referring to files inside the `CornerStore` folder. You can look at the `CornerStore.Tests` folder if you wish, but you do not need to modify any code there.
1. The project code is already configured to use EF Core and Npgsql. Running `dotnet restore` in the project directory will resolve all missing dependencies.
1. Initialize user secrets for this project, and add a secret called `CornerStoreDbConnectionString` with the connection string for a local database called `CornerStore`. Be sure to include you username and password for your local Postgresql `postgres` user. 
1. There is a `client` folder with a React project ready to use. run `npm install` before running `npm start` to run the project server. 

## Entities
Add the following classes with their properties to the `Models` folder. Use Data Annotations to ensure that properties that should not be nullable or ignored in the database are configured correctly.

1. Cashier
    - Id (primary key)
    - FirstName (not nullable)
    - LastName (not nullable)
    - FullName (computed)
1. Product
    - Id (primary key)
    - ProductName (not nullable)
    - Price (not nullable)
    - Brand (not nullable)
    - CategoryId (not nullable, foreign key)
    - Quantity (not used in database, will be calculated)
1. Category
    - Id (primary key)
    - CategoryName (not nullable)
1. Order 
    - Id (primary key)
    - CashierId (not nullable, foreign key)
    - Total (computed - add the logic to sum the product prices for the order)
    - PaidOnDate (nullable DateTime to record when the transaction was completed)

In addition to the properties above that correspond to database columns, add other properties that reflect these relationships that will allow related data to be nested:
1. A cashier can have many orders, an order is only handled by one cashier
1. A product belongs in one category. A category can be associated with many products
1. An order may have many associated products, and a product may be purchased in many orders. If the same product is ordered twice in the same order, they will be represented as two or more _separate_ products on the order (in other words, we do _not_ need to track the quantity for a given product being ordered).
1. Make an ERD for this database

## Seeding the database
1. Use the `OnModelCreating` method in the `CornerStoreDbContext` class to add cashiers, products, categories, and orders. Also ensure that orders have products associated with them. 
1. Use the migration tool to create your database

## Endpoints
Implement the following endpoints in the API:

### `/cashiers`
1. Add a cashier
1. Get a cashier (include their orders).

### `/products`
1. Get all products. If the `search` query string param is present, return only products whose names include the `search` value (ignore case).
1. `/products/popular` - Get the most popular products (check for a query string param called `amount` that says how many products to return. Return ten by default).
1. Add a product
1. Update a product
### `/orders`
1. Get an order details, including the cashier and products on the order with their category. Group the products together so that any given product is only listed once, and the `Quantity` property reflects how many times the product appears on the order. 
1. Get all orders. Check for a query string param `orderDate` that only returns orders from a particular day. If it is not present, return all orders. 
1. Delete an order
1. Create an Order (with products!)

### Integrate API with client 
1. Use the API to provide all the necessary data to the included client 

When you have completed all of the tasks, push the result to the main branch (you can push more often, just make sure that the final working result is pushed as well). 

