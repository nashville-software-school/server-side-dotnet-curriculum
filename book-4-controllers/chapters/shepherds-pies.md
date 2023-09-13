# :pizza: Shepherd's Pies
Giuseppe Shepherd learned how to make the perfect pizza as a child from his nonna. Since then it's been his dream to open his own restaurant, but he needs help creating the order management system for his new business.

> NOTE: This project is a good example of a fairly ambitious capstone project that more than fulfills the basic requirements for graduation. As you start planning your capstone project, keep this in mind when thinking about the size and scope of the application you are planning to build.    



## Project Description

### Orders
Giuseppe's (Joe, to his friends) restaurant has a dining room and delivery service available, and an order can either be placed for a particular table number or for delivery. Each order at the restaurant can have multiple pizzas on it.

Each order can potentially have two employees assigned to it for different purposes - Joe can tell if an order is for delivery if an employee has been assigned as the deliverer for that order. Orders are also _always_ associated with the employee that took the order (at a table or over the phone).  

Joe needs to see what the total cost for the order will be based on the total cost of all of the pizzas on that order. He also needs to see if the customer left a tip. Joe's restaurant is located in a magical place with no sales tax. For record-keeping purposes, Joe also needs to know the date and time an order was placed.    

### Pizza
Eventually there will be other items available, but for the restaurant opening Joe is going to only serve pizzas. The pizzas come in three sizes - small, medium, and large. Each pizza on an order can have a cheese type, a sauce type, and then any number of toppings chosen from a list on the menu.

Joe provided us with a draft menu to help build our data model:

---
### :pizza::tomato: Shepherd's Pies :tomato: :pizza:
### Pizzas
- small (10") - $10.00
- medium (14") - $12.00
- large (18") - $15.00

### Choose a Cheese Option
- Buffalo Mozzarella
- Four Cheese
- Vegan
- None (cheeseless)

### Choose a Sauce Option
- Marinara
- Arrabbiata
- Garlic White
- None (sauceless pizza)

### Toppings ($0.50 each)
- sausage
- pepperoni
- mushroom
- onion
- green pepper
- black olive
- basil
- extra cheese

**Delivery surcharge: $5.00/order**

---

## Application Requirements
Only employees will use this system as logged on users. They need to be able to:
1. Create an order (there are lots of design questions here - should the user be able to add pizzas on this view, or only after the order has been created?)
1. View all orders (should be filtered by day, with today being the default when first viewing the list), ordered by order datetime (newest first).
1. View an order's details (including a list of pizzas)
1. Update an order
    - Add a pizza to an order ( linked from order details view)
    - Remove a pizza from an order
    - Assign an employee to deliver the pizza (see chapters below for correctly setting up the data model for this feature)
1. Update a pizza that is on an order
    - change cheese or sauce type, or size
    - add and remove toppings
1. Cancel an order (delete from the system)

## Project Planning
You will not be able to complete this project efficiently without proper planning ahead of time. 

1. Create an ERD for the system. The description above and the requirements should be enough information to create a good data model for this application
1. Create wireframes of the different views that the UI will need in order to fulfill requirements. These can be very simple, but make sure you have an idea of how the user is going to access each view, and how any forms will need to look to allow the user to input data. 
1. Use the project requirements to create user stories. Make issues on Github once you have a repo, and add those issues to a Github project. 
1. Once you have started coding, create a new branch for each feature you work on. Merge that branch into main before you start working on the next feature. 

[Multiple Foreign Key Properties](./shepherds-pies-foreign-keys.md)