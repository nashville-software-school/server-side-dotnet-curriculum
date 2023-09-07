# :pizza: Shepherd's Pies
Giuseppe Shepherd learned how to make the perfect pizza as a child from his nonna. Since then it's been his dream to open his own restaurant, but he needs help creating the order management system for his new business.

> NOTE: This project is a good example of a fairly ambitious capstone project that more than fulfills the basic requirements for graduation. As you start planning your capstone project, keep this in mind when thinking about the size and scope of the application you are planning to build.    



## Project Description

### Orders
Giuseppe's (Joe, to his friends) restaurant has a dining room and delivery service available, and an order can either be placed for a particular table number or for delivery. Joe can tell if an order is for delivery if an employee has been assigned to deliver it. Orders are also always associated with the employee that took the order (at a table or over the phone). Each order at the restaurant can have multiple pizzas on it. Joe needs to see what the total cost for the order will be based on the total cost of all of the pizzas. He also needs to see if the customer left a tip. Joe's restaurant is located in a magical place with no sales tax. For record-keeping purposes, Joe also needs to know the date and time an order was placed.    

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
1. Create an order (there are lots of design questions here - should the user be able to add pizzas on this view, or only after the order has been created? )
1. View all orders (should be filtered by day, with today being the default), ordered by order datetime (newest first).
1. View an order's details (including a list of pizzas)
1. Update an order
    - Add a pizza to an order ( linked from order details view)
    - Remove a pizza from an order
    - Assign an employee to deliver the pizza (see chapters below for correctly setting up the data model for this feature)
1. Update a pizza that is on an order
    - change cheese or sauce type, or size
    - add and remove toppings
1. Cancel an order (delete from the system)