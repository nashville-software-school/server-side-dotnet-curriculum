# Create the `DbContext` Class
In this chapter we will create the class that is able to access a PostgreSQL database and seed the database with data. We will then create the database using the dotnet cli migration tool. 

## Adding a DbContext class
Add a file to the main directory of the project called `CreekRiverDbContext.cs`. paste the following code in it:
``` csharp
using Microsoft.EntityFrameworkCore;
using CreekRiver.Models;

public class CreekRiverDbContext : DbContext
{

    public DbSet<Reservation> Reservations { get; set; }
    public DbSet<UserProfile> UserProfiles { get; set; }
    public DbSet<Campsite> Campsites { get; set; }
    public DbSet<CampsiteType> CampsiteTypes { get; set; }

    public CreekRiverDbContext(DbContextOptions<CreekRiverDbContext> context) : base(context)
    {

    }
}
```
- `DbContext` is a class that comes from EF Core that represents our database as .NET object that we can access. Up until now, you have mostly encountered classes that represent database entities. However, in most .NET projects classes also provide an important way to organize code into abstracted objects that represent different parts of the application, each taking on a specific role. `DbContext` is just such a class. But this class is actually called `CreekRiverDbContext`, and after its name you see `: DbContext`. This is the way that _inheritance_ is indicated in C#. Inheritance means that a class inherits all of the properties, fields, and methods of another class. In this case, we want our `CreekRiverDbContext` class to be a `DbContext` as well. Inheritance indicates an "is-a" relationship between two types. All of the properties of `DbContext` allow this class to connect to the database with no other code that you have to write. 
- The properties on the `CreekRiverDbContext` class are, obviously, the collections corresponding to the tables in our database. By adding them to this class, we are telling EF Core which classes represent our database entities. `DbSet` is another class from EF Core, which is like other collections such as `List` and `Array`, in that we can write Linq queries to get data from them. What is special about `DbSet` is that our Linq queries will be transformed into a SQL query, which will be run against the database to get the data for which we are querying.  
- Finally, there is something that looks like a method called `CreekRiverDbContext`. This is a _constructor_, which is a method-like member of a class that allows us to write extra logic to configure the class, so that it is ready for use when it is created. You can always tell that something is a constructor in a class when: 1. It is public, 2. has the same name as the class itself, and 3. has no return type. In this case, our `CreekRiverDbContext` class actually doesn't need any special setup, but the `DbContext` class does. `DbContext` is our class's _base class_, and it requires an options object to set itself up properly. We use the `base` keyword to pass that object down to `DbContext` when ASP.NET creates the `CreekRiverDbContext` class. 

## Seeding the database with data
_Inside the `CreekRiverDbContext` class_, add the following method:
``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    // seed data with campsite types
    modelBuilder.Entity<CampsiteType>().HasData(new CampsiteType[]
    {
        new CampsiteType {Id = 1, CampsiteTypeName = "Tent", FeePerNight = 15.99M, MaxReservationDays = 7},
        new CampsiteType {Id = 2, CampsiteTypeName = "RV", FeePerNight = 26.50M, MaxReservationDays = 14},
        new CampsiteType {Id = 3, CampsiteTypeName = "Primitive", FeePerNight = 10.00M, MaxReservationDays = 3},
        new CampsiteType {Id = 4, CampsiteTypeName = "Hammock", FeePerNight = 12M, MaxReservationDays = 7}
    });
}
```
This method introduces a number of new topics:

1. This is the first use in the course of the _access modifier_ `protected`. Up until now, you have made all of your classes and properties `public`, meaning that they are accessible anywhere in the code that you can reference the class or a property of the class. `protected`, in contrast, means that the method can only be called from code _inside the class itself_, or by any class that inherits it. This is a form of _encapsulation_, which means keeping code that is only safe or useful to use inside a particular context inaccessible to other parts of the program. We will see why this is important later. 
1. `override` indicates that `OnModelCreating` is actually replacing a method of the same name that is inherited from the `DbContext` class. Such methods, like the one in the `DbContext` class, are marked with the `virtual` keyword. See the extra chapters for a deeper dive on `override`. 
1. The rest of the code in this method will check - every time we create or update the database schema - whether this data is in the database or not, and will attempt to add it if it doesn't find it all. This is very useful for seeding the database when it is created for the first time with test data. 

## Adding more data

1. Add this _inside the `OnModelCreating` method_:
    ``` csharp
    modelBuilder.Entity<Campsite>().HasData(new Campsite[]
    {
        new Campsite {Id = 1, CampsiteTypeId = 1, Nickname = "Barred Owl", ImageUrl="https://tnstateparks.com/assets/images/content-images/campgrounds/249/colsp-area2-site73.jpg"},
    }
    ```
1. This adds one campsite to the database. Add at least five more to the `Campsite[]` so that you will have a number of campsites with which to test your code.
1. use the `HasData` method to add one `UserProfile` and at least one `Reservation` to the database as well. In all cases, make sure that the foreign keys you choose match a primary key in the corresponding table. 

## Configure the web API to use EF Core
Add the following using directives at the top of `Program.cs`:
``` csharp
using CreekRiver.Models;
using Microsoft.EntityFrameworkCore;
using System.Text.Json.Serialization;
using Microsoft.AspNetCore.Http.Json;
```

Add the following code right above `var app = builder.Build();` in `Program.cs`:
``` csharp
// allows passing datetimes without time zone data 
AppContext.SetSwitch("Npgsql.EnableLegacyTimestampBehavior", true);

// allows our api endpoints to access the database through Entity Framework Core
builder.Services.AddNpgsql<CreekRiverDbContext>(builder.Configuration["CreekRiverDbConnectionString"]);

// Set the JSON serializer options
builder.Services.Configure<Microsoft.AspNetCore.Http.Json.JsonOptions>(options =>
{
    options.SerializerOptions.ReferenceHandler = ReferenceHandler.IgnoreCycles;
});
```
The middle of these three statements is the most important. It makes an instance of the `CreekRiverDbContext` class available to our endpoints, so that they can query the database. `builder.Configuration["CreekRiverDbConnectionString"]` retrieves the connection string that we stored in the secrets manager so that EF Core can use it to connect to the database. Don't worry about what the others are doing for now. 

## Creating the database
We are ready to actually run the tool that will create our PostgreSQL database from our C# code!

1. Run the following in the main project directory:
    ``` bash
    dotnet ef migrations add InitialCreate
    ```
1. This command will create a `Migrations` folder with a number of C# files in it in the project directory. When it finishes, run this:
    ``` bash
    dotnet ef database update
    ```
1. If everything goes well, you should now have a database ready to query! Open pgAdmin, and take a look at the database that EF Core created.

## ✍️ Reflections
1. We introduced _many_ new terms and concepts related to Object-Oriented Programming (OOP) in this chapter, such as:
    - inheritance
    - constructors
    - access modifiers
    - overrides
    - base class (also called parent class)
    - encapsulation
    
    There are a number of extra chapters with exercises to enhance your understanding of these terms and how they work in C#/.NET. Unlike most explorer chapters in this course, these should be considered high priority material for you to cover. You are responsible for these terms and what they mean. If any of them are unclear after working through the explorer chapter exercises, write down your questions to talk to an instructor about them. 

1. This is another point in the course where it can feel like the code is getting increasingly abstract. More and more of the work is being done for you by yet another framework, Entity Framework Core. Although this chapter required a lot of explanation, the total amount of code written to get it working was fairly minimal.  You only need a high-level understanding about what each piece does. Write down a few sentences describing what each of the following is, and what they do (Googling or querying an LLM may help as well!). If you want, check with a colleague or an instructor to check your own understanding:
    - Entity Framework Core
    - DbContext
    - DbSet
    - dotnet user-secrets
    - connection string

Up Next: [Getting Campsites](./creek-river-get-campsites.md)

## 🔍 Additional Materials
1. [Constructors](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/constructors?source=recommendations)
1. [Inheritance](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/tutorials/inheritance)