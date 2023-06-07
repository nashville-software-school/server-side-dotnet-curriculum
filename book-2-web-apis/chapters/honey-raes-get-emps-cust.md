# Implementing the rest of the GET endpoints
In this chapter we will:
1. add the GET endpoints for employees and customers
1. deal with unexpected user input
1. include related data from other entities in data output

## Finished the GET endpoints
1. Add endpoints to get all employees and get an employee by id
1. Add endpoints to get all customers and get a customer by id
1. Make sure you test each endpoint as you add it to confirm that it works as expected.

## Dealing with unexpected input
When testing your endpoints, did you try to provide an id for an employee or customer that doesn't exist? Luckily, because we used `FirstOrDefault` for the endpoints that get by `Id`, that method will just return `null` to the client when an employee or customer is not found. 

### HTTP status codes
You probably already encountered HTTP status codes in the front end part of the course. They are included in the HTTP response to indicate something about what happened with the request. `200` means `OK`, `404` means `Not Found`, and `500` means `Internal Server Error`. By default, an endpoint will give a `200` status code if no errors are thrown in the handler. If an error does occur, the endpoint will end up causing a `500` response from the server. You can test this by:
1. Changing `FirstOrDefault` to `First` in an endpoint that gets an item by id and
1. making a request to that endpoint with an id that doesn't exist for that collection. 
1. You should see a runtime error in VS Code: `System.InvalidOperationException: Sequence contains no matching element`
1. Click the continue button on the debugger controls
1. Look in Postman. In the response, in the top right you should see a status code of `500`. Notice that in the response body, you will see the same runtime error that we saw in VS Code. This feature of the development mode for running your API will be very useful later on.

### Sending back a different status code programmatically
The ASP.NET Core framework allows us to send back whatever status code we want (assuming that nothing catastrophic occurred that results in a `500`). 

1. Replace the endpoint for getting an employee by id with this:
``` csharp
app.MapGet("/employees/{id}", (int id) =>
{
    Employee employee = employees.FirstOrDefault(e => e.Id == id);
    if (employee == null)
    {
        return Results.NotFound();
    }
    return Results.Ok(employee);
});
```

2. If you hover over the `NotFound` method, you'll see that it creates a `404` response for the endpoint. We return that if no employee is found with a matching `Id`. Otherwise, we return a `200` response with the employee data in the body (created by the call to `Ok()`, and passing in the employee object as an argument). 

3. Test the endpoint and make sure you get a `404` response in Postman when querying for an employee that doesn't exist in the database. 

4. Finally, implement this change in the endpoint for getting a customer by `Id`. 