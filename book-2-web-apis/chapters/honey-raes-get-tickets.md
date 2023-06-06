# Get All `ServiceTicket`s and get one `ServiceTicket`

## Creating a database
Before we can add endpoints for `HoneyRaesAPI` we need to create a database to pull data from. In the next book, we will replace this with a SQL database, but until then we will use C# collections to hold our data. 

Add the following lines to the top of `Program.cs`:

``` csharp
List<HoneyRaesAPI.Models.Customer> customers = new List<HoneyRaesAPI.Models.Customer> {};
List<HoneyRaesAPI.Models.Employee> employees = new List<HoneyRaesAPI.Models.Employee> {};
List<HoneyRaesAPI.Models.ServiceTicket> serviceTickets = new List<HoneyRaesAPI.Models.ServiceTicket> {};
```

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

Add three customers, two employees, and five service tickets to the collections. Make sure that the customer and employee ids in the service tickets match actual ids in the customer and employee collections. Leave some of the service tickets incomplete by not providing a `DateCompleted`. If you want, you can leave some unassigned by not providing an `EmployeeId`. 

