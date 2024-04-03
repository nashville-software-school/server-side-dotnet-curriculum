# Creating a new ASP.NET MVC Web Application

In this chapter you'll create a new MVC project to start the Nashville dog walking application, DogGo.

## Getting Started
1. Create a new MVC project <br/>
    - ```dotnet new mvc -o DogGo```
1. Use `cd` to navigate to the `DogGo` directory that was created. Build the project to ensure everything was scaffolded properly
    - ```dotnet build```
1. Trust the HTTPS development certificate by running the following command:
    - ```dotnet dev-certs https --trust```
    - Select Yes to agree to trust the HTTPS certificate
1. Open your new MVC project in VS Code

    Take a look around at the project files that come out of the box with a new ASP.NET MVC project. It already has folders for Models, Views, and Controllers. It has a `wwwroot` folder which contains some static assets like javascript and css files. This is similar to the `public` folder in the front-end React projects you are already familiar with
1. Launch the App with the debugger. If you see a welcome landing page, the template is set up correctly.

## Configuration
You will need a connection string in order to create and modify your postgres database.  Let's do that now.

1. Initialize user-secrets
    - ```dotnet user-secrets init```
1. Add your connection string
    - ```dotnet user-secrets set DogGoDbConnectionString "Host=localhost;Port=5432;Database=DogGo;Username=postgres;Password=<your password>"```

## Install the necessary tools

1. One advantage of using MVC for starting a new project is that there are tools that let you scaffold starter code for the view files for your project. You need to install the dotnet-aspnet-codegenerator tool, which provides scaffolding capabilities. You can install it using the following commands in your terminal:

    - ```dotnet tool uninstall --global dotnet-aspnet-codegenerator```
    - ```dotnet tool install --global dotnet-aspnet-codegenerator --version 8.0```
    - ```dotnet tool uninstall --global dotnet-ef```
    - ```dotnet tool install --global dotnet-ef --version 8.0```
    - ```dotnet add package Microsoft.EntityFrameworkCore.Design --version 8.0```
    - ```dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 8.0```
    - ```dotnet add package Npgsql --version 8.0```
    - ```dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL --version 8.0```
    <br/>
    <br/>
    - The Preceding Commands add:
        - The aspnet-dotnet-codegenerator scaffolding tool 
        - The CLI for EF Core
        - and the packages needed for scaffolding: 
            - Microsoft.VisualStudio.Web.CodeGeneration.Design and Microsoft.EntityFrameworkCore.SqlServer
    - This should install all of the tooling and packages that you need in order to create your postgres database and create controllers and views for you mvc application.

## Models

Create a `Neighborhood.cs`, `Walker.cs`, `Owner.cs`, `Dog.cs` file in the Models folder and add the following code.

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

> Walk.cs
``` csharp
namespace DogGo.Models
{
    public class Walk
    {
        public int Id { get; set; }
        public int DogId { get; set; }
        public Dog Dog { get; set; }
        public int WalkerId {get; set; }
        public Walker Walker { get; set;}
        public int Duration { get; set; }
    }
}
```

## Create DbContext class
- Create a new class in the main directory of the project called `DogGoDbContext.cs`, at this point you should be familiar with the DbContext class. Create a new DbContext given the classes provided above. Make sure that your class inherites from `DbContext`

- Seed the Database by copy pasting the code below into your DbContext class
```csharp
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
    // seed data with campsite types
        modelBuilder.Entity<Neighborhood>().HasData(new Neighborhood[]
        {
            new Neighborhood {Id = 1, Name = "East Nashville"},
            new Neighborhood {Id = 2, Name = "Antioch"},
            new Neighborhood {Id = 3, Name = "Berry Hill"},
            new Neighborhood {Id = 4, Name = "Germantown"},
            new Neighborhood {Id = 5, Name = "The Gulch"},
            new Neighborhood {Id = 6, Name = "Downtown"},
            new Neighborhood {Id = 7, Name = "Music Row"},
            new Neighborhood {Id = 8, Name = "Hermitage"},
            new Neighborhood {Id = 9, Name = "Madison"},
            new Neighborhood {Id = 10, Name = "Green Hills"},
            new Neighborhood {Id = 11, Name = "Midtown"},
            new Neighborhood {Id = 12, Name = "West Nashville"},
            new Neighborhood {Id = 13, Name = "Donelson"},
            new Neighborhood {Id = 14, Name = "North Nashville"},
            new Neighborhood {Id = 15, Name = "Belmont-Hillsboro"}
        });

        modelBuilder.Entity<Owner>().HasData(new Owner[]
        {
            new Owner {Id = 1, Name="John Sanchez", Email="john@gmail.com", Address="355 Main St", Phone="(615)-553-2456",NeighborhoodId= 1},
            new Owner {Id = 2, Name="Patricia Young", Email="patty@gmail.com", Address="355 Main St", Phone="(615)-553-2456",NeighborhoodId= 2},
            new Owner {Id = 3, Name="Robert Brown", Email="robert@gmail.com", Address="355 Main St", Phone="(615)-553-2456",NeighborhoodId= 3},
            new Owner {Id = 4, Name="Jennifer Wilson", Email="jennifer@gmail.com", Address="355 Main St", Phone="(615)-553-2456",NeighborhoodId= 1},
            new Owner {Id = 5, Name="Michael Moore", Email="michael@gmail.com", Address="355 Main St", Phone="(615)-553-2456",NeighborhoodId= 2},
            new Owner {Id = 6, Name="Linda Green", Email="linda@gmail.com", Address="355 Main St", Phone="(615)-553-2456",NeighborhoodId= 3},
            new Owner {Id = 7, Name="William Anderson", Email="william@gmail.com", Address="355 Main St", Phone="(615)-553-2456",NeighborhoodId= 1}
        }); 

        modelBuilder.Entity<Dog>().HasData( new Dog[] 
        {
            new Dog {Id = 1, Name="Ninni", OwnerId = 1, Breed="Rottweiler"},
            new Dog {Id = 2, Name="Kuma", OwnerId = 1, Breed="Rottweiler"},
            new Dog {Id = 3, Name="Remy", OwnerId = 2, Breed="Greyhound"},
            new Dog {Id = 4, Name="Xyla", OwnerId = 3, Breed="Dalmation"},
            new Dog {Id = 5, Name="Chewy", OwnerId = 3, Breed="Beagle"},
            new Dog {Id = 6, Name="Groucho", OwnerId = 4, Breed="Dalmation"},
            new Dog {Id = 7, Name="Finley", OwnerId = 5, Breed="Golden Retriever"},
            new Dog {Id = 8, Name="Casper", OwnerId = 6, Breed="Golden Retriever"},
            new Dog {Id = 9, Name="Bubba", OwnerId = 7, Breed="English Bulldog"},
            new Dog {Id = 10, Name="Zeus", OwnerId = 7, Breed="Schnauzer"}
        });

        modelBuilder.Entity<Walker>().HasData( new Walker[] 
        {
            new Walker {Id=1, Name="Claudelle", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=15},
            new Walker {Id=2, Name="Roi", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=9},
            new Walker {Id=3, Name="Shena", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=10},
            new Walker {Id=4, Name="Gibb", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=8},
            new Walker {Id=5, Name="Tammy", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=6},
            new Walker {Id=6, Name="Rufe", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=11},
            new Walker {Id=7, Name="Cassandry", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=12},
            new Walker {Id=8, Name="Cully", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=4},
            new Walker {Id=9, Name="Cully", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=14},
            new Walker {Id=10, Name="Agnesse", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=1},
            new Walker {Id=11, Name="Koo", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=7},
            new Walker {Id=12, Name="Diana", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=6},
            new Walker {Id=13, Name="Moreen", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=7},
            new Walker {Id=14, Name="Sonni", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=13},
            new Walker {Id=15, Name="Nadiya", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=9},
            new Walker {Id=16, Name="Olag", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=12},
            new Walker {Id=17, Name="Alasdair", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=12},
            new Walker {Id=18, Name="Jo ann", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=12},
            new Walker {Id=19, Name="Ewen", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=6},
            new Walker {Id=20, Name="Andee", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=5},
            new Walker {Id=21, Name="Sada", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=12},
            new Walker {Id=22, Name="Tasia", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=3},
            new Walker {Id=23, Name="Sherry", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=5},
            new Walker {Id=24, Name="Witty", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=6},
            new Walker {Id=25, Name="Ezekiel", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=5},
            new Walker {Id=26, Name="Emmeline", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=1},
            new Walker {Id=27, Name="Darrick", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=9},
            new Walker {Id=28, Name="Redford", ImageUrl="https://avatars.dicebear.com/v2/female/c117aa483c649ecbc46c6d65172bf6e6.svg", NeighborhoodId=14}
        });

}
```


## Configure the App to use EF Core
1. Open the program.cs file in VS Code
    - Add the following Using Statements:
    ```csharp
        using DogGo.Models;
        using Microsoft.EntityFrameworkCore;
        using System.Text.Json.Serialization;
        using Microsoft.AspNetCore.Http.Json;
    ```
    - Add the following code right above var app = builder.Build(); in Program.cs:
    ```csharp
        // allows passing datetimes without time zone data 
        AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);

        // allows our api endpoints to access the database through Entity Framework Core
        builder.Services.AddNpgsql<DogGoDbContext>(builder.Configuration["DogGoDbConnectionString"]);

        // Set the JSON serializer options
        builder.Services.Configure<JsonOptions>(options =>
        {
            options.SerializerOptions.ReferenceHandler = ReferenceHandler.IgnoreCycles;
        });
    ```




