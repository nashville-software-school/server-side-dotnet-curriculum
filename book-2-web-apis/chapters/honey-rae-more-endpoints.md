# Further Development of the Honey Rae's API :weight_lifting:
This chapter lists a number of features for the Honey Rae's API for you to implement. You can do as many of them as you like. Some of the features will require you to build a larger database to properly test their functionality. 

This chapter will require you to think about how to name your routes. To assist you in this, the explorer chapter on [REST principles](./rest-concepts.md) is recommended before this chapter.

If you get stuck, ask for help! These exercises intentionally go beyond what you've learned in the course so far. 

### 1. Emergencies
Create an endpoint to return all of the service tickets that are incomplete and are emergencies

### 2. Unassigned
Create an endpoint to return all currently unassigned service tickets

### 3. Inactive Customers
Create an endpoint to return all of the customers that haven't had a service ticket closed for them in over a year (refer to the explorer chapter in Book 1 on calculating `DateTime`s).

### 4. Available employees
Create an endpoint to return employees not currently assigned to an incomplete service ticket

### 5. Employee's customers
Create an endpoint to return all of the customers for whom a given employee has been assigned to a service ticket (whether completed or not)

### 6. Employee of the month
Create and endpoint to return the employee who has completed the most service tickets last month. 

### 7. Past Ticket review
Create an endpoint to return completed tickets in order of the completion data, oldest first. (This will required a Linq method you haven't learned yet...)

### 8. Prioritized Tickets (challenge)
Create an endpoint to return all tickets that are incomplete, in order first by whether they are emergencies, then by whether they are assigned or not (unassigned first). 