# Using Npgsql to connect to PostgreSQL from the Web API
In this chapter we will connect to our PostgreSQL database programmatically inside the HoneyRaesAPI project, and create the query to get all Employees inside the handler for that endpoint.

## Adding the `Npgsql` dependency to the project
The .NET runtime does not include by default a library for connecting to PostgreSQL, so we will have to add a package to the project that does that for use. We can download just such a package (called Npgsql) from the Nuget package registry (kind of like the `npm install ...` of .NET). 

Run this command in the directory of HoneyRaesAPI that includes the `csproj` file:
``` bash
dotnet add package Npgsql --version 8.0.1
```
Now we will have access to the classes in that package that we will use to access our SQL database. 

## Using SQL to get all employees

1. Add this line to the top of Program.cs: `using Npgsql;`
1. Add this line right after the `using` directives: 
```
var connectionString = "Host=localhost;Port=5432;Username=postgres;Password=<your_postgresql_password>;Database=HoneyRaes";
``` 
(Note: put your own postgres user's password in place of `<your_postgresql_password>`)

3. Replace the endpoint that gets all employees with this one:
``` csharp
app.MapGet("/employees", () =>
{
    // create an empty list of employees to add to. 
    List<Employee> employees = new List<Employee>();
    //make a connection to the PostgreSQL database using the connection string
    using NpgsqlConnection connection = new NpgsqlConnection(connectionString);
    //open the connection
    connection.Open();
    // create a sql command to send to the database
    using NpgsqlCommand command = connection.CreateCommand();
    command.CommandText = "SELECT * FROM Employee";
    //send the command. 
    using NpgsqlDataReader reader = command.ExecuteReader();
    //read the results of the command row by row
    while (reader.Read()) // reader.Read() returns a boolean, to say whether there is a row or not, it also advances down to that row if it's there. 
    {
        //This code adds a new C# employee object with the data in the current row of the data reader 
        employees.Add(new Employee
        {
            Id = reader.GetInt32(reader.GetOrdinal("Id")), //find what position the Id column is in, then get the integer stored at that position
            Name = reader.GetString(reader.GetOrdinal("Name")),
            Specialty = reader.GetString(reader.GetOrdinal("Specialty"))
        });
    }
    //once all the rows have been read, send the list of employees back to the client as JSON
    return employees;
});
```

### Wait, what now?
Yes, that is a lot of new code. It has been commented extensively to explain each line, but there is a larger context that you need to understand what is going on here. 

First, to get this out of the way, you don't need to understand how your application gets and maintains a connection to the SQL database server. It just does. Think of an `NpgsqlConnection` object as a tunnel to send messages back and forth between the database server and your web API. The _connection string_ is very similar to a URL: it is an address where the SQL connection can find the database, just like a URL lets us find a website. The connection string also provides credentials to restrict access to users. 

> IMPORTANT: As you can see, connections strings can contain sensitive data like passwords. This is the last time in this course you will see a connection string in a file that you would push to Github. You will learn how to keep these secrets out of your code later in this book! 

One of the things you can do with connections is create SQL commands. The text of those commands will look just like a SQL query. One of the things you can do with commands is _execute_ them with the `ExecuteReader` method. that method returns a `DataReader` object, which gives you access to all the data that the query produced. 

The hardest part to understand here is `reader.Read()`. The data that comes back from the database is in a tabular format. You have seen a graphical representation of it in pgAdmin. There is a table of results with columns and rows. The reader only reads _one row of this table at a time_. Every time the `Read` method is called, it moves down a row. It then examines whether there is a row to read or not (if not, that means the end of the results have been reached), and returns a _boolean_ to say so. `while (reader.Read()) {}` says "Move down a row. If there is data there, do whatever is in the braces. Otherwise, stop."

Inside the braces of the `while` loop, we use other methods to access the data in that row, and then create a new C# `Employee` object with it. `GetOrdinal` finds the position in the table of a column with the name that's passed in. The columns are in a particular order in the table. So if `Id` is the first column in the table, `reader.GetOrdinal("Id")` would return 0. `GetString` or `GetInt32` gets that data type value out of whatever number column in that row is passed in. `reader.GetInt32(0)` will return whatever integer value is stored in the first column of the current row. We use these values to set the properties of our new `Employee` objects.

Finally, this code block uses `using` statements for creating the connection, command, and reader objects. `using` here is different from a `using` _directive_, which is how we import classes from another namespace. Here, `using` indicates that some special logic needs to happen before discarding this object in memory. If you think about a connection to a sql database as a phone call, `using` tells your application to properly disconnect the call before putting the phone down.  

Ok, that still might not make sense yet. Read through the code and the explanation a few times. After working through the rest of this book, this syntax will feel less bewildering. If it's still confusing then, ask an instructor for help!

Up Next: [Getting Related Data](./honey-raes-related-data.md)