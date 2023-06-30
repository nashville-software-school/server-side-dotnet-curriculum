# Getting an Employee with their ServiceTickets
In this chapter we will cover getting an employee by id with their service tickets, which come from a separate table.

## Replacing the `/employees/{id}` endpoint
Replace your current endpoint with this:
``` csharp
app.MapGet("/employees/{id}", (int id) =>
{
    Employee employee = null;
    using NpgsqlConnection connection = new NpgsqlConnection(connectionString);
    connection.Open();
    using NpgsqlCommand command = connection.CreateCommand();
    command.CommandText = "SELECT * FROM Employee WHERE Id = @id";
    // use command parameters to add the specific Id we are looking for to the query
    command.Parameters.AddWithValue("@id", id);
    using NpgsqlDataReader reader = command.ExecuteReader();
    // We are only expecting one row back, so we don't need a loop!
    if (reader.Read())
    {
        employee = new Employee
        {
            Id = reader.GetInt32(reader.GetOrdinal("Id")),
            Name = reader.GetString(reader.GetOrdinal("Name")),
            Specialty = reader.GetString(reader.GetOrdinal("Specialty"))
        };
    }
    return employee;
});
```
### Comparing this endpoint with  `GET` `/employees`
This code block largely follows the same patterns as the previous endpoint. We need to create a connection, open it, and create a command. 

The command text is different, because we are using a `WHERE` clause to find only one employee. But each time this endpoint receives a request, there could be a different `id` passed into the URL. Because of this, we need to use _parameters_ to allow us to put a different `id` in the SQL query each time. `@id` is the parameter we put in the query as a placeholder for the eventual value that will go there before the query is executed. The SQL command object has a `Parameters` property, and there is a method that let's us set the value for a parameter by name called `AddWithValue`. Finally, we use an `if` instead of `while` to check for data, because we only need the data reader to check for one row (because this query can only return a maximum of one row - ids are unique, so the `WHERE` clause can only return a maximum of one employee).  

### Adding `ServiceTicket`s to the `Employee` object
Test the endpoint in Postman. It should work, but the `serviceTickets` is `null`. Let's fix that!

1. First, we need to change our SQL query. Make this the command text:
```csharp
 command.CommandText = @"
        SELECT 
            e.Id,
            e.Name, 
            e.Specialty, 
            st.Id AS serviceTicketId, 
            st.CustomerId,
            st.Description,
            st.Emergency,
            st.DateCompleted 
        FROM Employee e
        LEFT JOIN ServiceTicket st ON st.EmployeeId = e.Id
        WHERE e.Id = @id";
```
Notice we are using `@` in front of the string to allow a multi-line string. Second, we are using a `LEFT JOIN` to get `ServiceTicket`s with the same `EmployeeId` as the employee. We are also using an alias to distinguish between the `Id` of an `Employee` and the `Id` of a `ServiceTicket`.

2. Run the query in pgAdmin. Replace `@id` in the pgAdmin query with the id of an employee that has more than one service ticket. Notice that we now have multiple rows. The employee data is duplicated in each row, and the service ticket data is unique. 

3. If, potentially, we will have more than one row in the results, we need to replace the `if` block with a `while` loop:
``` csharp
while (reader.Read())
{
    if (employee == null)
    {
        employee = new Employee
        {
            Id = reader.GetInt32(reader.GetOrdinal("Id")),
            Name = reader.GetString(reader.GetOrdinal("Name")),
            Specialty = reader.GetString(reader.GetOrdinal("Specialty")),
            ServiceTickets = new List<ServiceTicket>() //empty List to add service tickets to
        };
    }
}
```
Inside the loop, we have an `if` to catch whether we've already set the employee data or not (basically, is this the first row or not). Remember, every row in these results will have the same employee data, which we only want to save once, the first time we come across it. 

Second, when we create the employee object, we add an empty `List<ServiceTicket>` as the value of `ServiceTickets` so that we have a place to store all of the service ticket data in each row.

4. Finally, we need to check to see if there is service ticket data in any given row (if the employee has no service tickets, we will get back one row with `NULL`s for the service ticket columns), and then add a service ticket to the employee's `ServiceTickets` collection. The whole endpoint will look like this: 

``` csharp
app.MapGet("/employees/{id}", (int id) =>
{
    Employee employee = null;
    using NpgsqlConnection connection = new NpgsqlConnection(connectionString);
    connection.Open();
    using NpgsqlCommand command = connection.CreateCommand();
    command.CommandText = @"
        SELECT 
            e.Id,
            e.Name, 
            e.Specialty, 
            st.Id AS serviceTicketId, 
            st.CustomerId,
            st.Description,
            st.Emergency,
            st.DateCompleted 
        FROM Employee e
        LEFT JOIN ServiceTicket st ON st.EmployeeId = e.Id
        WHERE e.Id = @id";
    // use command parameters to add the specific Id we are looking for to the query
    command.Parameters.AddWithValue("@id", id);
    using NpgsqlDataReader reader = command.ExecuteReader();
    // We are only expecting one row back, so we don't need a loop!
    while (reader.Read())
    {
        if (employee == null)
        {
            employee = new Employee
            {
                Id = reader.GetInt32(reader.GetOrdinal("Id")),
                Name = reader.GetString(reader.GetOrdinal("Name")),
                Specialty = reader.GetString(reader.GetOrdinal("Specialty")),
                ServiceTickets = new List<ServiceTicket>() //empty List to add service tickets to
            };
        }
        // reader.IsDBNull checks if a column in a particular position is null
        if (!reader.IsDBNull(reader.GetOrdinal("serviceTicketId")))
        {
            employee.ServiceTickets.Add(new ServiceTicket
            {
                Id = reader.GetInt32(reader.GetOrdinal("serviceTicketId")),
                CustomerId = reader.GetInt32(reader.GetOrdinal("CustomerId")),
                //we don't need to get this from the database, we already know it
                EmployeeId = id,
                Description = reader.GetString(reader.GetOrdinal("Description")),
                Emergency = reader.GetBoolean(reader.GetOrdinal("Emergency")),
                // Npgsql can't automatically convert NULL in the database to C# null, so we have to check whether it's null before trying to get it
                DateCompleted = reader.IsDBNull(reader.GetOrdinal("DateCompleted")) ? null : reader.GetDateTime(reader.GetOrdinal("DateCompleted"))
            });
        }
    }
     // Return 404 if the employee is never set (meaning, that reader.Read() immediately returned false because the id did not match an employee)
    // otherwise 200 with the employee data
    return employee == null ? Results.NotFound() : Results.Ok(employee);
});
```

Test the endpoint to make sure that you get the employee along with their service tickets in the results. 

The second `if` in the `while` loop is checking to see whether that row includes `ServiceTicket` data. We could choose any required column to check, but the service ticket `Id` column is as good as any to see if the row contains ticket data. If we find that there is data, we add a service ticket to the employee's `ServiceTickets` List.

## Summary
As you can see, this is quite a lot of code inside the handler. But it's important to note all the things that this code is doing:
- Managing a connection to the database (over the network)
- sending a SQL query to get data
- _parsing_ that tabular data into strongly-typed .NET objects that our .NET API understands
- returning that data in the HTTP response as JSON.  

If you are having a hard time understanding _why_ the `while` and `if` statements are necessary, take a look at the results of this query in pgAdmin again. the reader object will go through the table of results row-by-row, one row per iteration of the `while` loop. Think about what needs to be done with each row, and try to map that to the code in the handler above. 

Up Next: [Creating and Updating an Employee](./honey-raes-create.md)

## 🔍 Additional Materials

1. [`using` statement](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/using)
1. [database connection](https://en.wikipedia.org/wiki/Database_connection)
1. [Npgsql documentation](https://www.npgsql.org/index.html)