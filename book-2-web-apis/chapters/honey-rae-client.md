# Adding a Client to Honey Rae's API
Follow the setup instructions in the README of [this repo](https://github.com/nss-group-projects/dotnet-honey-rae-client) to get starter code for the React client app. 

### Testing the `/servicetickets` endpoint
Make sure that both the API and the React client are running. In the React client, click on the Service Tickets option in the nav bar, and you should see all of the tickets from the API displayed in the client. If you see that, you know that both projects are configured correctly. If you don't, go over the instructions to make sure you didn't miss a step, and find an instructor if you still can't get them running. 

### Implement seeing a service ticket's details
There is already a component to view a service ticket's details. Create the correct function in the `serviceTicketData` module to get one ticket from the API, and use it in a `useEffect` in the details component. 

### Details and List views for customers and employees
Implement the same views for customers and employees

### Creating a service ticket (Challenge)
There is an empty component for a form to create a service ticket. Create the form and the proper function in the `serviceTicketData` module to send service ticket data to the API to create a new ticket. Remember that you will need to get all of the customers and employees to list them for the user to choose to get their ids for the serviceTicket data.

### Delete button
Add a delete button to the service tickets list rows to remove a service ticket. After the ticket is deleted, _dynamically_ update the array of service tickets _without refreshing the page_. 

### Complete a ticket
Add a button next to the delete button to mark a request as complete, and correctly update the API database with the right HTTP request when it is clicked. The Complete button should only appear when the request is 1. assigned and 2. not complete. 

### Assigning an employee (challenge)
1. Add an `Assign` button to the service ticket details routes the user to the following component: 
1. Create a component that allows the user to choose an employee to assign to a service ticket. The component should submit the ticket data to the API so that the database can be updated.  After submission, the app should navigate back to the details for that ticket. 
1. The assign button should appear in the "Employee" column instead of "Unassigned", otherwise display the assigned employee name

