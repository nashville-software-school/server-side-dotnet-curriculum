# :potato: Tuber Treats
Famously, it can take a while to bake a potato. But you want one _now_! Fortunately, Tuber Treats delivers delicious baked potato lunches on demand. To compete with their archrival, Spud Hub, they've asked you to build a new API to help coordinate deliveries. 

## Project Setup
1. Request the Github Classroom link for the assignment from your instructor.
1. Clone your new repo that Github classroom created to your local computer and open it in VS Code.
1. For the rest of these instructions, the files will be referring to files inside the `TuberTreats` folder. You can look at the `TuberTreats.Tests` folder if you wish, but you do not need to modify any code there. 

## Entities
The database should have the following entities:
1. `TuberOrder` - represents an order for a single spud
    - Should start with these properties: `Id`, `OrderPlacedOnDate`, `CustomerId`, `TuberDriverId` (a nullable integer), `DeliveredOnDate`, and `Toppings` (a `List` of `Topping` objects for this order).
1. `Topping` - represents possible toppings to add to an order
    - Has these properties: `Id`, `Name`
1. `TuberTopping` -  represents toppings added to specific orders
    - Has these properties: `Id`, `TuberOrderId`, `ToppingId`
1. `TuberDriver` - employees that can be assigned to deliver the potato
    - Has these properties: `Id`, `Name`, `TuberDeliveries`
1. `Customer` - Customers can make many orders 
    - Has these properties: `Id`, `Name`, `Address`, `TuberOrders`

<br>

1. Create an ERD for this system. Share with an instructor to make sure you understand the relationships. 

1. After your ERD is approved, create classes to represent these entities in the `Models` folder of the project.

1. Create a database with at least three drivers, five customers, five toppings, and three orders (make sure some orders have toppings). 

## Endpoints
Implement all of the following:

### `/tuberorders`
1. Get all orders
1. Get an order by id (must include customer data as well as driver and toppings data, if applicable). 
1. Submit a new order (the API should add an `OrderPlacedOnDate`). Return the new order so the client can see the new `Id`. 
1. Assign a driver to an order (`PUT` to `/tuberorders/{id}`)
1. Complete an order (`POST` to `/tuberorders/{id}/complete`)

### `/toppings`
1. Get all toppings
1. Get topping by id

### `/tubertoppings`
1. Get all TuberToppings
1. Add a topping to a `TuberOrder` (return the new TuberTopping object to the client)
1. Remove a topping from a `TuberOrder`

### `/customers`
1. Get all Customers
1. Get a customer by id, with their orders
1. Add a Customer (return the new customer)
1. Delete a Customer

### `/tuberdrivers`
1. Get all employees
1. Get an employee by id with their deliveries
