# Deleting an Employee
In this chapter we will cover deleting an employee by id. 

## Adding the endpoint
Add this endpoint to `Program.cs`
``` csharp
app.MapDelete("/employees/{id}", (int id) =>
{
    using NpgsqlConnection connection = new NpgsqlConnection(connectionString);
    connection.Open();
    using NpgsqlCommand command = connection.CreateCommand();
    command.CommandText = @"
        DELETE FROM Employee WHERE Id=@id
    ";
    command.Parameters.AddWithValue("@id", id);
    command.ExecuteNonQuery();
    return Results.NoContent();
});
```
Things to note:
1. We again use `ExecuteNonQuery` because we don't need any information back from the database so long as the delete operation was successful. 
1. The handler returns `204 No Content` because we want to send a success message that is not going to have a body. 

## Testing the endpoint
1. Start the application if it is not running.
1. Create a new employee. Look in the response body to get the id for that employee
1. Test the delete endpoint by deleting the employee that you just created. 
1. Look in pgAdmin to ensure that the delete worked.

> :bulb: What would you expect to happen if you tried to delete an employee that had assigned service tickets? Try it and see what actually happens. If you get an error, look at what it says. Can you figure out what's wrong? Can you think of possible strategies to fix this problem?

## Summary
This is the end of the tutorial on using Npgsql to work with a PostgreSQL database in a .NET web API. This is really only an introduction to this technology, and is meant to show you how much goes into communicating with a database. In the next part, we are going to start using Npgsql with another technology called Entity Framework Core. It will abstract away much of the work that you did in this part. _However_: Knowing how to work with Npgsql (and ADO.NET, the library that Npgsql is based on) is worthwhile, and many developers and codebases use it without Entity Framework Core. If you have time, look at the explorer chapters that go further in depth on Npgsql. It is also worth knowing that other .NET developers may be more familiar with ADO.NET, which is used to connect to Sql Server and Oracle databases, not PostgreSQL. Npgsql works in almost exactly the same way, and uses most of the same method names.   