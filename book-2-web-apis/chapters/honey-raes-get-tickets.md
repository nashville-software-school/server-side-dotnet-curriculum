# Get All `ServiceTicket`s and get one `ServiceTicket`

## Creating a database
Before we can add endpoints for `HoneyRaesAPI` we need to create a database to pull data from. In the next book, we will replace this with a SQL database, but until then we will use C# collections to hold our data. 

Add the following lines to the very top of `Program.cs`:

``` csharp
List<HoneyRaesAPI.Models.Customer> customers = new List<HoneyRaesAPI.Models.Customer> {};
List<HoneyRaesAPI.Models.Employee> employees = new List<HoneyRaesAPI.Models.Employee> {};
List<HoneyRaesAPI.Models.ServiceTicket> serviceTickets = new List<HoneyRaesAPI.Models.ServiceTicket> {};
```

### Using directives

Now that we have put our data models into a namespace, we need to use their fully-qualified namespace to use them in `Program.cs`. But this creates really long names! Add this line above the other three:
```csharp
using HoneyRaesAPI.Models;
```
This `using` directive imports all of the names of `HoneyRaesAPI.Models` into this file so that we can refer to them without the whole name. Now you should be able to create your collections like this: 

``` csharp
List<Customer> customers = new List<Customer> { };
List<Employee> employees = new List<Employee> { };
List<ServiceTicket> serviceTickets = new List<ServiceTicket> { };
```

In practice, you will always add these `using` directives to the top of files where you are referencing classes from other namespaces when it is possible. If you want to think about them like JS module imports, that will help for now. 

### Adding data to the collections

Add three customers, two employees, and five service tickets to the collections using the _collection initializers_. Make sure that the customer and employee ids in the service tickets match actual ids in the customer and employee collections. Leave some of the service tickets incomplete by not providing a `DateCompleted`. Leave some unassigned by not providing an `EmployeeId`. 

## Get all `ServiceTicket`s

1. Remove the two endpoints that are in `Program.cs` already, as they are not relevant to HoneyRaesAPI. Also remove the `WeatherForecast` `record` definition at the bottom of the file and the ` var summaries...` declaration.
1. Add this endpoint above `app.Run();`: 
``` csharp
app.MapGet("/servicetickets", () =>
{
    return serviceTickets.Select(t => new ServiceTicketDTO
    {
        Id = t.Id,
        CustomerId = t.CustomerId,
        EmployeeId = t.EmployeeId,
        Description = t.Description,
        Emergency = t.Emergency,
        DateCompleted = t.DateCompleted
    });
});
```
3. Start the debugger for your project using VS Code (refer to the earlier chapter where we cover this if you've forgotten how)
4. In Postman, make a `GET` request to `http://localhost:<port>/servicetickets`. Check to make sure you got the right data back in the response. 

### What did we just do?
This endpoint is fairly simple. When a GET request to "/servicetickets" is made, the handler function just takes all of the service tickets in the database, creates an instance of `ServiceTicketDTO` for each one, and returns all of them. Then ASP.NET code (the framework we are using to create our web API) turns that C# List of objects into JSON text (this is just like `JSON.stringify` in JS), and sends an HTTP response with that data in the body. 

## Get `ServiceTicket` by `Id`
1. Add this endpoint below the first one:
``` csharp
app.MapGet("/servicetickets/{id}", (int id) =>
{
    ServiceTicket serviceTicket = serviceTickets.FirstOrDefault(st => st.Id == id);
  
    return new ServiceTicketDTO
    {
        Id = serviceTicket.Id,
        CustomerId = serviceTicket.CustomerId,
        EmployeeId = serviceTicket.EmployeeId,
        Description = serviceTicket.Description,
        Emergency = serviceTicket.Emergency,
        DateCompleted = serviceTicket.DateCompleted
    };
});
```
2. Use the restart button on the debugger controls to reload the API.
3. When it is running again, make a GET request in Postman to `http://localhost:<port>/servicetickets/1`. 
4. Check the output to confirm that the service ticket with an id of one is in the response body. 

### What did we just do?
This endpoint introduces some complexity. In the route the `{id}` part of the string is called a _route parameter_. They allow us to specify that some variable value will be present in the route. This is very useful, because ASP.NET will pass the value in that parameter as an argument to the matching parameter in the handler. You can see that `id` in the route is the _same name_ as the `id` param in the handler function. _You must make sure that these names match_ so that the framework can match a route parameter to a function parameter. After the route param is passed into the handler, we use that value and a Linq method (`FirstOrDefault`) to find the service ticket with an `Id` of `1`. Finally, the handler returns a new instance of the `ServiceTicketDTO` class with its properties populated from the values of the same properties in the service ticket from the database.

Up Next: [GET endpoints for employees and customers](./honey-raes-get-emps-cust.md)