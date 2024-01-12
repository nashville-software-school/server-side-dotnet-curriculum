# Creek River Campground
In this project we will build an API for the reservations website of Creek River Campground. Instead directly writing SQL queries, we will use Entity Framework Core to create and interact with the PostgreSQL database. EF Core will analyze the data models of the application, and create a PostgreSQL database for us that matches our data model!

## Creating the Project
1. In the csharp directory of your workspace create the web api project like this: `dotnet new webapi -o CreekRiver -minimal`
1. Inside the `CreekRiver` directory, run `dotnet new gitignore`
1. Install these required dependencies with:
    ``` bash
    dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL --version 8.0
    ```
    and: 
    ``` bash
    dotnet add package Microsoft.EntityFrameworkCore.Design --version 8.0
    ```
1. Run this to be able to store secrets for this app: 
    ``` bash
    dotnet user-secrets init
    ```
1. Add the connection string to the secrets for this app with this (make sure you change `<your_postgresql_password>` to your db password!):
    ``` bash
    dotnet user-secrets set 'CreekRiverDbConnectionString' 'Host=localhost;Port=5432;Username=postgres;Password=<your_postgresql_password>;Database=CreekRiver'
    ```

## Models
Creating the data models for this application will be almost identical to the process for Honey Rae's. The biggest difference is that our model, in addition to defining the types that we'll use in the .NET application, will also be used by Entity Framework to determine what tables to create in the database. This requires a few small additions to the code. 

1. Create a `Models` folder in the project folder
1. Add a `CampsiteType.cs` file to the `Models` folder. Paste the following code in it:
    ``` csharp
    using System.ComponentModel.DataAnnotations;

    namespace CreekRiver.Models;

    public class CampsiteType
    {
        public int Id { get; set; }
        [Required]
        public string CampsiteTypeName { get; set; }
        public int MaxReservationDays { get; set; }
        public decimal FeePerNight { get; set; }
    }
    ```
    - EF Core will automatically make a property called `Id` in C# into the `PRIMARY KEY` column for the corresponding SQL database table. 
    - The `[Required]` code above the `CampsiteTypeName` is called an _attribute_ in C#. Attributes allow you to add metadata about entities in C# that your program can use. In this case, Entity Framework can tell from this attribute that the `CampsiteTypeName` column in the SQL database should have `NOT NULL` when it is created. Entity Framework will assume that any reference type in C# (types that can be null, see the [extra materials](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/reference-types)) will correspond to a nullable column in SQL as well. The other properties in this class should also have `NOT NULL` in the database creation, but because they are all numbers and are not nullable in C#, we don't need to add the `Required` attribute to them. We need to add `using System.ComponentModel.DataAnnotations` to use the `Required` attribute
1.  Add a `Campsite.cs` file to the `Models` folder and paste this code in it:
    ``` csharp
    using System.ComponentModel.DataAnnotations;

    namespace CreekRiver.Models;

    public class Campsite
    {
        public int Id { get; set; }
        [Required]
        public string Nickname { get; set; }
        public string ImageUrl { get; set; }
        public int CampsiteTypeId { get; set; }
        public CampsiteType CampsiteType { get; set; }
        public List<Reservation> Reservations { get; set; }
    }
    ```
    - Notice here that we have a `Reservations` property where we can store reservations associated with a campsite. 
    - Entity Framework will notice that `CampsiteTypeId` has `CampsiteType` in the name before `Id`, and can infer that this is a foreign key to `CampsiteType`. It will create a foreign key constraint on this column when it creates the database.
1. Add a `Reservation.cs` file to the `Models` folder and paste the following code in it:
    ``` csharp
    namespace CreekRiver.Models;

    public class Reservation
    {
        public int Id { get; set; }
        public int CampsiteId { get; set; }
        public Campsite Campsite { get; set; }
        public int UserProfileId { get; set; }
        public UserProfile UserProfile { get; set; }
        public DateTime CheckinDate { get; set; }
        public DateTime CheckoutDate { get; set; }
    }
    ```
    - Here, too, `UserProfileId` and `CampsiteId` will be set up as foreign keys in the database
    - Notice that we can have both `UserProfileId` and `UserProfile` in the model. `UserProfile` will be ignored by EF Core when creating the UserProfile table, because it knows that this type cannot correspond to a separate column on the table, but we can use it to store the `UserProfile` data from the UserProfile table that corresponds to `UserProfileId`. 
1. Finally, add a `UserProfile.cs` file to the `Models` folder and paste the following code:
    ```csharp
    using System.ComponentModel.DataAnnotations;

    namespace CreekRiver.Models;

    public class UserProfile
    {
        public int Id { get; set; }
        [Required]
        public string FirstName { get; set; }
        [Required]
        public string LastName { get; set; }
        [Required]
        public string Email { get; set; }

        public List<Reservation> Reservations { get; set; }

    }
    ```
1. Add the matching DTO classes for these types. They will not need the `Require` attribute anywhere, because the DTOs are describing how the data will be represented to the client, not describing how it is stored in the database. 

In the next chapter we will use these models to configure and provide access to the database. 

Up Next: [DbContext](./creek-river-db-context.md)

## 🔍 Additional Materials

[C# Attributes](https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/reflection-and-attributes/)

[C# Reference Types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/reference-types)