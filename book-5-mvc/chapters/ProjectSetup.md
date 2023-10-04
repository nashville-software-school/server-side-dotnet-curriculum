# Creating a new ASP.NET MVC Web Application

In this chapter you'll create a new MVC project to start the Nashville dog walking application, DogGo.

## Getting Started
1. Create a new MVC project <br/>
    - ```dotnet mvc -o DogGo```
1. Build the project to ensure everything was scaffolded properly
    - ```dotnet build```
1. Trust the HTTPS development certificate by running the following command:
    - ```dotnet dev-certs https --trust```
    - Select Yes to agree to trust the HTTPS certificate
1. Open your new MVC project in VS Code
    - ```code DogGo```

    Take a look around at the project files that come out of the box with a new ASP.NET MVC project. It already has folders for Models, Views, and Controllers. It has a `wwwroot` folder which contains some static assets like javascript and css files.

1. Launch the App without debugging by selecting Ctrl+F5
    - Visual Studio Code:
        - Starts Kestrel
        - Launches a browser
        - Navigats to https://localhost:<port#>
    - Launching the app without debugging by selecting Ctrl+F5 allows you to:
        - Make code changes
        - Save the file
        - Quickly refresh the browser and see the code changes






## Configuration
You will need a connection string in order to create and modify your postgres database.  Let's do that now.

1. Initialize user-secrets
    - ```dotnet user-secrets init```
1. Add your connection string
    - ```dotnet user-secrets set DogGoDbConnectionString "Host=localhost;Database=DogGo;Username=postgres;Password=<your password>"```

## Install the necessary tools

1. Neither VS Code or the .NET CLI support scaffolding directly. Therefore, you need to install the dotnet-aspnet-codegenerator tool, which provides scaffolding capabilities. You can install it using the following commands in your terminal:

    - ```dotnet tool uninstall --global dotnet-aspnet-codegenerator```
    - ```dotnet tool install --global dotnet-aspnet-codegenerator --version 6.0.0```
    - ```dotnet tool uninstall --global dotnet-ef```
    - ```dotnet tool install --global dotnet-ef --version 6.0.0```
    - ```dotnet add package Microsoft.EntityFrameworkCore.Design --version 6.0.0```
    - ```dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 6.0.0```
    - ```dotnet add package Npgsql --version 6.0.0```
    <br/>
    <br/>
    - The Preceding Commands add:
        - The aspnet-dotnet-codegenerator scaffolding tool 
        - The CLI for EF Core
        - and the packages needed for scaffolding: 
            - Microsoft.VisualStudio.Web.CodeGeneration.Design and Microsoft.EntityFrameworkCore.SqlServer
    - This should install all of the tooling and packages that you need in order to create your postgres database and create controllers and views for you mvc application.

## Models

Create a `Neighborhood.cs`, `Walker.cs`, `Owner.cs`, `Dog.cs` file in the Models folder and add the following code

> Neighborhood.cs
```csharp
namespace DogGo.Models
{
    public class Neighborhood
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }
}
```

> Walker.cs
```csharp
namespace DogGo.Models
{
    public class Walker
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int NeighborhoodId { get; set; }
        public string ImageUrl { get; set; }
        public Neighborhood Neighborhood { get; set; }
    }
}
```

> Owner.cs
```csharp
namespace DogGo.Models
{
    public class Owner
    {
        public int Id { get; set; }
        public string Email { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public int NeighborhoodId { get; set; }
        public string Phone { get; set; }
        public List<Dog> Dogs { get;set; }
    }
}
```

> Dog.cs
```csharp
namespace DogGo.Models
{
    public class Dog
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int OwnerId { get; set; }
        public String Breed { get; set; }
        public string Notes { get; set; }
        public string ImageUrl { get; set; }
        public Owner DogOwner { get; set; }
    }
}
```




