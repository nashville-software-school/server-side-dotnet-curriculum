# Creating a `ServiceTicket`
In this chapter we will add an endpoint to post new service tickets to the database.

## Creating the endpoint

Add the following endpoint to `Program.cs`:
``` csharp
app.MapPost("/servicetickets", (ServiceTicket serviceTicket) =>
{
    // creates a new id (When we get to it later, our SQL database will do this for us like JSON Server did!)
    serviceTicket.Id = serviceTickets.Max(st => st.Id) + 1;
    serviceTickets.Add(serviceTicket);
    return serviceTicket;
});
```

Some things to notice:

1. We are using the `MapPost` method to create this endpoint, because it should be triggered when a `POST` request is made to the API. 
1. The route for this endpoint is the same as for getting all service tickets. That's fine because they have different HTTP methods (`GET` and `POST`)
1. The handler for this endpoint accepts a `ServiceTicket` parameter. If there is a JSON object in the request body, ASP.NET will try to convert that JSON object into a C# `ServiceTicket` object if it can (this is like `JSON.parse`, but for C#!). 
1. The rest of the handler is fairly straightforward. The first line creates a new id for the new service ticket, and adds it to the object that was sent from the client. Finally we add the new object to the database. 

## Test the new endpoint
1. Start the debugger in VS Code or restart if it is already running.
1. In Postman, open a new request with the `+` button. Choose the `POST` method in the dropdown to the left of the URL, and put `http://localhost:5293/servicetickets` in the URL bar.
1. Before we send this request, we need to add the data that we're going to post to the body of the request. Click on the Body tab,choose the `raw`  radio button, and then `JSON` from the dropdown. 

Add this as the body:
```json
{
    "customerId": 1,
    "description": "Garage door won't open",
    "emergency": false
}
```

We're leaving the `employeeId` out as well as the `dateCompleted`, because a new service ticket doesn't need to be assigned and isn't done yet, and we will create the `id` for the service in the API itself. 

Hit send!

If you didn't get any errors you can confirm that the endpoint is working by looking at the response. You should see the same object in the response body, but it will have an `id` on it created by the API. This is one of the reasons it can be helpful to send the new object back. You can also see that the object is in the database by changing the method in Postman to `GET`. Hit send again, and you should see the new service ticket that was posted in the list of service tickets in the response. We've now confirmed two things: a new service ticket is created, and it is successfully saved to the database collection.

Up Next: [Delete a Service Ticket](./honey-raes-delete.md)
