# Completing Honey Rae's with Npgsql
In this chapter you will replace the rest of the endpoints from the Honey Rae's API that you built in Book 2 with endpoints that hit the Postgresql database you created in Book 3

## Using `IsDBNull` to check for nullable columns
One issue with `null` values in C# is that they cannot be directly from the database's NULL. This is because in the database, `NULL` represents an absence of value, rather than a value of `null`, as it is in C#. This means that you need to check for NULL database values before trying to get a string or other type out of a column which is nullable. Here is an example of this with `EmployeeId` and `DateCompleted` (both nullable columns in the `ServiceTicket` table):
``` csharp
app.MapGet("/servicetickets", () =>
{
    List<ServiceTicket> serviceTickets = new List<ServiceTicket>();
    using NpgsqlConnection connection = new NpgsqlConnection(connectionString);
    connection.Open();
    using NpgsqlCommand command = connection.CreateCommand();
    command.CommandText = @"
    SELECT * FROM ServiceTicket
    ";
    using NpgsqlDataReader reader = command.ExecuteReader();
    while (reader.Read())
    {
        serviceTickets.Add(new ServiceTicket
        {
            Id = reader.GetInt32(reader.GetOrdinal("Id")),
            CustomerId = reader.GetInt32(reader.GetOrdinal("CustomerId")),
            EmployeeId = reader.IsDBNull(reader.GetOrdinal("EmployeeId")) ? null : reader.GetInt32(reader.GetOrdinal("EmployeeId")),
            Description = reader.GetString(reader.GetOrdinal("Description")),
            Emergency = reader.GetBoolean(reader.GetOrdinal("Emergency")),
            DateCompleted = reader.IsDBNull(reader.GetOrdinal("DateCompleted")) ?
                null : reader.GetDateTime(reader.GetOrdinal("DateCompleted"))
        });
    }
    return serviceTickets;
});
```   
As you can see above, this endpoint uses ternaries and `IsDBNull` to either set the property as `null` if it is `NULL` in the database, otherwise, it proceeds to get the value with `GetString` or `GetDateTime`. 

## Getting One Service Ticket 
This endpoint requires using `JOIN`s to get the `Employee` and `Customer` data along with the ticket. Because the EmployeeId and CustomerId are required, and there is only one of each for the service ticket, this query will return a maximum of one row. Because of that, you can just check for that row with `if (reader.Read())` rather than using a `while` loop. See if you can write this one on your own!

## Deleting a service ticket
No other table depends on service tickets for data, so you can safely delete a service ticket without worrying about cascades. This endpoint will look similar to the delete endpoint for an employee.

## Complete a Service Ticket
Even though this endpoint uses the `POST` HTTP method, in the database it is an update of a service ticket. Unlike a normal update, you are not getting any data from the request, only a service ticket id in the URL. Use the id as a param in the SQL query to update the `DateCompleted` column to be today. You can use `DateTime.Today` to do this, or you can do it directly in the update SQL query with `DateCompleted = LOCALTIMESTAMP(0)`. 

## More endpoints
The following endpoints will look similar to their counterparts for the `Employee` table:
- update a service ticket
- get customers
- get one customer with service tickets
- create a service ticket


