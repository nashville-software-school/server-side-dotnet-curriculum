# Implementing the rest of the GET endpoints
In this chapter we will:
1. add the GET endpoints for employees and customers
1. deal with unexpected user input
1. include related data from other entities in data output

## Finish the GET endpoints
1. Add endpoints to get all employees and get an employee by id
1. Add endpoints to get all customers and get a customer by id
1. Make sure you test each endpoint as you add it to confirm that it works as expected.

## Dealing with unexpected input
When testing your endpoints, did you try to provide an id for an employee or customer that doesn't exist? Luckily, because we used `FirstOrDefault` for the endpoints that get by `Id`, that method will just return `null` to the client when an employee or customer is not found. We can provide a better response, however, if we use HTTP status codes...

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

5. Update the service ticket endpoint accordingly.

## Including related data
One of the principles of Object Oriented Programming is the concept of _composition_. This is the idea that we can use many object types to compose the properties of another object type. For example, in `HoneyRaesAPI` we have a `ServiceTicket` class that has an `EmployeeId` property on it that is supposed to match the `Id` property on an instance of the `Employee` class. From that perspective, we can say that the `ServiceTicket` _has an_ `Employee`. Because this is a many-to-one relationship, even though there is no property on the `Employee` class to indicate this, any given employee can have many `ServiceTicket`s. 

How could we represent this in the class definition for `Employee`? We can add a property to the Employee class like this:
``` csharp
public List<ServiceTicket> ServiceTickets { get; set; }
```
Now there is a place on the `Employee` class where we can store the `ServiceTicket` objects that belong to any given employee.

> :bulb: Notice that we did not have to use a `using` directive to have the `ServiceTicket` class in the `Employee.cs` file. This is because both classes are in the `HoneyRaesAPI.Models` namespace, so there is no need to "import" one into other!

### Include service tickets in employee details

Let's update the get-by-id endpoint for employee to include the service tickets to which the employee is assigned. Add this line to that endpoint right before returning the `Ok` result:
```csharp
employee.ServiceTickets = serviceTickets.Where(st => st.EmployeeId == id).ToList();
```

This line uses Linq to find the service tickets that match the employee's id, and then set's the value of that employee's `ServiceTickets` property to whatever tickets are found. 


Restart your app, and then using an employee id that has service tickets assigned to them, send a request to get one employee. In Postman, you should now see another property in the JSON (`serviceTickets` - yes, ASP.NET automatically converts the C# PascalCase properties to camelCase for Javascript), which will be an array of service ticket objects.  

### Include the employee's data in the service ticket details
Just like an `Employee` has many `ServiceTicket`s, a `ServiceTicket` has at most one `Employee` assigned to it. We already have an `EmployeeId` in the class, but if we want to include the whole `Employee` object, we can do that too, by adding a property to the `ServiceTicket` class:
```csharp
public Employee Employee { get; set; }
```
Yes, it's perfectly fine for the property name to be the same as its type; in fact you will see this all over C# codebases. Here is what the updated endpoint for getting a service ticket should look like:
```csharp
app.MapGet("/servicetickets/{id}", (int id) =>
{
    ServiceTicket serviceTicket = serviceTickets.FirstOrDefault(st => st.Id == id);
    if (serviceTicket == null)
    {
        return Results.NotFound();
    }
    serviceTicket.Employee = employees.FirstOrDefault(e => e.Id == serviceTicket.EmployeeId);
    return Results.Ok(serviceTicket);
});
```
Test this endpoint to see that you get a service ticket that has an `employee` object on it with the correct data. 

### `null` values
What happens when you get a service ticket that doesn't have an employee assigned yet? Try it out! You should see that the `employee` property in the JSON has a value of `null`, as you would expect, but the `employeeId` is `0`! This is because some types in C# are not allowed to be `null` and `int` is one of them. `Int`'s default value is `0` if one is not provided. In order to fix this, change the type of `EmployeeId` to be a nullable `int`, like this:

``` csharp
public int? EmployeeId { get; set; }
```

Restart the API, and test the endpoint again. `employeeId` should now be `null`, as we would prefer. 


## Adding more related data
1. Add the customer data to the service ticket when sending a request to the get-by-id endpoint
1. Add the customer's service tickets to the customer object when getting one customer by id
1. Be sure to test each endpoint as you complete it with Postman. The testing is as important a part of the exercise as writing the code.

## ✍️ Reflections
1. <small>By now you've probably noticed that the way we are coding is very different. _Most_ of the functionality of the application is abstracted away from us by a few lines of code in `Program.cs` that end with `app.Run()`. We add a few line here and there, and suddenly we have a new endpoint to get data from our database. We didn't have to think about starting a server to listen on a port, or how to start the application that will call the functions we are writing to handle responses. We don't have to think about how to send the responses back either! This is the power of a framework like ASP.NET: taking care of common but complicated needs so we can focus on the business logic. This comes with frustrations too, though: it feels like you understand less and less of what's going on (a lot of it is "magic"). This is normal! As you work with these tools more, you can start learning more about _how_ they work, but for now focus on learning how to _implement_ the parts of the framework you need to do the work you need to do. Are there other questions you have about what the framework is doing for us? Write them down and ask an instructor, and they can help you figure out what you need to understand now, and what can wait until later! </small>
1. <small> The framework takes a lot of work out of our hands, but we actually now have a lot _more_ control than we did when using JSON Server. In this chapter, we added the same functionality that we would have had to use `_embed` and `_expand` to do with JSON Server. Now though, our client can just make a request to `/employees/1` instead of `/employees/1?_embed=servicetickets` to get the same data. Can you think of other types of more tailor-made `GET` endpoints that `HoneyRaesAPI` could use (like, for example, getting all open service tickets)? </small>

Up Next: [Creating a Service Ticket](./honey-raes-create.md)

