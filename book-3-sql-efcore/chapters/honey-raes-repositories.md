
# Organizing Data Access with Repositories
Once you have added all of the endpoints - using Npgsql - to the Honey Rae's API, you will notice that `Program.cs` has gotten very long. In addition, each endpoint is now not only responsible for deciding what data an endpoint should return or mutate, but also has all of the logic for working with the database.

We can use a rudimentary version of the repository pattern to keep the logic for data access separate (and reusable!) from the rest of the business logic in the endpoints.

## Setup
1. We are going to be creating multiple classes across the application that all need to access the connection string. `appsettings.json` holds configuration data we can access using _dependency injection_. Change `appsettings.json` so that it looks like this:
    ``` json
    {
    "Logging": {
        "LogLevel": {
        "Default": "Information",
        "Microsoft.AspNetCore": "Warning"
        }
    },
    "AllowedHosts": "*",
    "ConnectionStrings": {
        "HoneyRaesDbConnectionString": "Host=localhost;Port=5432;Username=postgres;Password=<your postgres password>;Database=HoneyRaes"
    }
    }
    ```
1. Add a folder to the Project called `Repositories`
1. Inside that folder, add three file, `EmployeeRepository.cs`, `CustomerRepository.cs`, and `ServiceTicketRepository.cs`.
1. In `Employee.cs`, add the following code:
    > Employee.cs
    ``` csharp
    using HoneyRaesAPI.Models;
    using Npgsql;

    namespace HoneyRaesAPI.Repositories;

    public class EmployeeRepository
    {
        private string _connectionString;

        private NpgsqlConnection Connection => new NpgsqlConnection(_connectionString);

        public EmployeeRepository(IConfiguration config)
        {
            _connectionString = config.GetConnectionString("HoneyRaesDbConnectionString");
        }

        public List<Employee> GetEmployees()
        {
            List<Employee> employees = new List<Employee>();
            // get a new connection from the class's Connection property
            using NpgsqlConnection connection = Connection;

            connection.Open();
            using var command = connection.CreateCommand();
            command.CommandText = "SELECT * FROM Employee";
            using var reader = command.ExecuteReader();
            while (reader.Read())
            {
                employees.Add(new Employee
                {
                    Id = reader.GetInt32(reader.GetOrdinal("Id")),
                    Name = reader.GetString(reader.GetOrdinal("Name")),
                    Specialty = reader.GetString(reader.GetOrdinal("Speciality"))
                });
            }
            return employees;
        }
    }
    ```
    - the class first has a `private` _field_ called `_connectionString`. It is private rather than public so that it can only be accessed inside the class methods (our endpoints don't need to and shouldn't access or change this value.)
    - `Connection` is a calculated property that creates a new NpgsqlConnection with the `_connectionString` and returns it. 
    - the member that looks like a method `EmployeeRepository` is called a _constructor_. It runs every time an `EmployeeRepository` is created. The code inside it can contain logic to set the class up so that it is ready for use (in this case, getting the connection string out of the configuration - which is loaded from `appsettings.json`). 
    - Finally, we have an actual method, which has all the code to get employees. Most of this is identical to the code you currently have in the endpoint. The main difference is that it is getting its `NpgsqlConnection` from the `Connection` property in this class. 
1. Add this line to `Program.cs` after `var builder = WebApplication.CreateBuilder(args);`
    ``` csharp
    builder.Services.AddTransient<EmployeeRepository>();
    ```
    - This line registers the `EmployeeRepository` as a service that can be used in our project's endpoints. 
1. Now we can update the endpoint that gets all employees to use the `GetEmployees` method from the `EmployeeRepository` to get the data.
    ``` csharp
    app.MapGet("/employees", (EmployeeRepository repo) =>
    {
        List<Employee> employees = repo.GetEmployees();
        return employees;
    });
    ```
1. Test this endpoint to make sure that it still works. 
1. Add the following methods to the `EmployeeRepository`:
    - `GetEmployeeById(int id)` - returns an Employee
    - `AddEmployee(Employee employee)` - returns the new Employee
    - `UpdateEmployee(Employee employee)` - returns `void`
    - `DeleteEmployee(int id)` - returns `void`
    > Note: make sure that the methods are `public` so that they are available to use in the endpoints. 
1. Transfer the code from the matching endpoints into the Repository methods. 
1. Update the endpoints to use the repository methods from `EmployeeRepository`

## Creating more repositories
See if you can create the `CustomerRepository` and `ServiceTicketRepository` on your own. Remember to register them with `AddTransient` in `Program.cs`. When you are done you will have much cleaner and easier to read endpoints, and repository methods that are reusable. 
