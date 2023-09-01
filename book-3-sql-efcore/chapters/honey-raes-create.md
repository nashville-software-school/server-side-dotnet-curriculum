# Creating and Updating an Employee
In this chapter we will cover adding and updating an employee in the database. These endpoints were not included in the exercises for Book 2, so unless you made them on your own, you can add this code to `Program.cs` rather than replacing anything. 

## Creating an Employee

Add this endpoint to the API:
``` csharp
app.MapPost("/employees", (Employee employee) =>
{
    using NpgsqlConnection connection = new NpgsqlConnection(connectionString);
    connection.Open();
    using NpgsqlCommand command = connection.CreateCommand();
    command.CommandText = @"
        INSERT INTO Employee (Name, Specialty)
        VALUES (@name, @specialty)
        RETURNING Id
    ";
    command.Parameters.AddWithValue("@name", employee.Name);
    command.Parameters.AddWithValue("@specialty", employee.Specialty);

    //the database will return the new Id for the employee, add it to the C# object
    employee.Id = (int)command.ExecuteScalar();

    return employee;
});
```
Some things to highlight:
1. We add `RETURNING Id` to the end of the query so that we get the new `Id` for the employee back after it has been created. 
1. We use command parameters to add the data from the incoming employee object to the database query. 
1. We don't need a reader object because we don't really need any other data than the new id back from the database. To get this, we call `ExecuteScalar` instead of `ExecuteReader`, which returns the value of the first column of the first row. 
1. Because C# doesn't know what type to expect from `ExecuteScalar` (after all, it could be anything), we add `(int)` in front of the method call to tell the compiler that the value should be an integer. This is called _type casting_. There is an explorer chapter in Book 1 about these which is worthwhile to go back to. 

Test out the endpoint in Postman. Make sure you use the `POST` method, and add a JSON body with one object. You should only need `name` and `specialty` properties on that object. 

## Updating an Employee

Add this endpoint to `Program.cs`:
``` csharp
app.MapPut("/employees/{id}", (int id, Employee employee) =>
{
    if (id != employee.Id)
    {
        return Results.BadRequest();
    }
    using NpgsqlConnection connection = new NpgsqlConnection(connectionString);
    connection.Open();
    using NpgsqlCommand command = connection.CreateCommand();
    command.CommandText = @"
        UPDATE Employee 
        SET Name = @name,
            Specialty = @specialty
        WHERE Id = @id
    ";
    command.Parameters.AddWithValue("@name", employee.Name);
    command.Parameters.AddWithValue("@specialty", employee.Specialty);
    command.Parameters.AddWithValue("@id", id);

    command.ExecuteNonQuery();
    return Results.NoContent();
});
```
Some comments on this endpoint:
1. `ExecuteNonQuery` is used for data changes when you do not need or expect any data back from the database after the query. In this case, as long as the query runs correctly, we do not need any information from the database. 
1. We need to provide the id to the database to make sure we only update one employee. Without the `WHERE`, it would update every row with this data. 
1. For the same reason, we return a 204 response `No Content` back to the client, because it is also not going to learn anything new from the response. 

Test this endpoint as well, and check the database with pgAdmin to ensure that the data was updated in the database as you expect. 

Up Next: [Deleting an Employee](./honey-raes-delete.md)