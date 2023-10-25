# Assigning and Completing Tickets
In this chapter we will add an endpoint to update a service ticket, as well as create a separate endpoint for completing a ticket.

## Updating a service ticket

Add this endpoint to allow for updating a service ticket:
```csharp
app.MapPut("/servicetickets/{id}", (int id, ServiceTicket serviceTicket) =>
{
    ServiceTicket ticketToUpdate = serviceTickets.FirstOrDefault(st => st.Id == id);

    if (ticketToUpdate == null)
    {
        return Results.NotFound();
    }
    if (id != serviceTicket.Id)
    {
        return Results.BadRequest();
    }

    ticketToUpdate.CustomerId = serviceTicket.CustomerId;
    ticketToUpdate.EmployeeId = serviceTicket.EmployeeId;
    ticketToUpdate.Description = serviceTicket.Description;
    ticketToUpdate.Emergency = serviceTicket.Emergency;
    ticketToUpdate.DateCompleted = serviceTicket.DateCompleted;

    return Results.NoContent();
});
```

1. Restart the debugger and submit a `PUT` request through Postman to the correct endpoint. In the body of the request, put a json service ticket that adds an employeeId to a currently unassigned ticket. Submit the request. 
1. If everything succeeds, switch the method to `GET` in Postman, and submit that request. Check the response body to make sure that the ticket was updated correctly. You should also see the details of the employee that was assigned!

### Algorithmic reasoning  ✍️ 
The explanation of how this particular endpoint works has been omitted intentionally. Write an explanation of how it works, step by step, for yourself. You can look at the other explanations in this project for inspiration.  Once you think you're done, ask an instructor to go over your explanation with you. 

## Creating a custom endpoint to complete a ticket

One of the advantages of building our own API is that we can create custom endpoints that add functionality that is specific to our app's needs. In this case, we want to be able to mark a task as complete with a timestamp of when it was completed. 

Let's start with an empty endpoint:
``` csharp
app.MapPost("/servicetickets/{id}", () =>
{

});
```
We can't use this route for a `POST`, because we already have one for creating a new service ticket. Let's change the route to this:
``` csharp
"/servicetickets/{id}/complete"
```

Now our route will be unique, and accurately describes what it's going to do

We need the handler to be able to use the `id` route parameter, so let's add it:
``` csharp
app.MapPost("/servicetickets/{id}/complete", (int id) =>
...
```

Next, get the service ticket from the database that needs to be marked as complete:
``` csharp
ServiceTicket ticketToComplete = serviceTickets.FirstOrDefault(st => st.Id == id);
```
Finally, set the service ticket's `DateCompleted` to today:
``` csharp
ticketToComplete.DateCompleted = DateTime.Today;
```

Pick an incomplete ticket, and use Postman to make a request to this endpoint, and then check the data with a `GET` to confirm that it works!

## Additional Challenges
This is the end of the Honey Rae's API walk-through. If you would like to explore using this API to serve data to a front-end client, look in the explorer chapters. There is also a chapter there to add additional endpoints to this API that require more complex logic. In addition to practicing with endpoints, they are good algorithmic practice. 


