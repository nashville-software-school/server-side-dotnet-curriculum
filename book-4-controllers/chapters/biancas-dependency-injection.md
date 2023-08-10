# Dependency Injection with Constructors
This chapter will focus on how to use the db context classes inside the controllers using _dependency injection_.

## A closer look at constructors
A constructor is a method-like member of a class that provides extra logic for setting up the class to be ready to use. Constructors always have the same name as the class, are public, and don't have a return type. Here's an example of a simple class with a constructor:
``` csharp
public class Dog
{
    public Dog(string breed, string name, int age, bool hasShots)
    {
        Breed = breed;
        Name = name;
        Age = age;
        HasShots = hasShots;
    }
    public string Breed { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
    public bool HasShots { get; set; }
}
```
You make a new `Dog` with the constructor like this:
``` csharp
Dog spot = new Dog("Beagle", "Spot", 2, true);

Console.WriteLine(spot.Name);
//output: "Spot"
```
This doesn't seem particularly useful right now, because without a constructor we could just make the `Dog` object with the _object initializer_ like this:
``` csharp
Dog spot = new Dog()
{
    Breed = "Beagle",
    Name = "Spot",
    Age = 2,
    HasShots = true
};
```
But what if other code in the class relied on all of those properties being there?
``` csharp
public class Dog
{
    public string Breed { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
    public bool HasShots { get; set; }

    public override string ToString()
    {
        return $"{Name}: {Breed}, {Age} years old, is {(HasShots ? "" : "not ")}vaccinated";
    }
}
```
If we create a `Dog` like this:
``` csharp
Dog spot = new Dog()
{
    Breed = "Beagle",
    Age = 2,
    HasShots = true
    //Name is missing!
};
```
What happens when we do this:
``` csharp
Console.WriteLine(spot);
//Output: : Beagle, 2 years old, and is vaccinated
```
The `ToString` method was relying on a value for `Name` being there. Using a constructor would ensure that a developer would always include required properties so that the class would work properly when it is created (omitting an arg to the constructor results in a compiler error).

## Controller constructors
Let's look at the `BikeController` again:
``` csharp
[ApiController]
[Route("api/[controller]")]
public class BikeController : ControllerBase
{
    private BiancasBikesDbContext _dbContext;

    public BikeController(BiancasBikesDbContext context)
    {
        _dbContext = context;
    }

    [HttpGet]
    [Authorize]
    public IActionResult Get()
    {
        return Ok(_dbContext.Bikes.ToList());
    }
}
```
The `BikeController` constructor requires one param of type `BiancasBikesDbContext` to be passed in when the class instance is created. That `BiancasBikesDbContext` instance is then saved as the value of the _private field_ `_dbContext` so that the `Get` method can use it to get bikes from the database. This field is private because this `_dbContext` variable should only be accessed from inside the class.  

Why not just do the following instead?
``` csharp
private BiancasBikesDbContext _dbContext = new BiancasBikesDbContext();
```
In theory, we could do that! But the problem is that `BiancasBikesDbContext` has a constructor that _also_ requires parameters. Our class doesn't know what values to pass in and shouldn't have to know how to set up `BiancasBikesDbContext`:
``` csharp
private BiancasBikesDbContext _dbContext = new BiancasBikesDbContext(What goes here ??? We don't know...);
```
This is why we use the constructor to _inject_ an already existing instance of the `BiancasBikesDbContext` class for the controller to use. This _decouples_ the dependencies of `BiancasBikesDbContext` from those of `BikeController`. When the framework creates an instance of `BikeController` to handle a bike request, it will pass in an instance of `BiancasBikesDbContext` for the `BikeController` to use.

The `AuthController` has two dependencies:
``` csharp
private BiancasBikesDbContext _dbContext;
private UserManager<IdentityUser> _userManager;

public AuthController(BiancasBikesDbContext context, UserManager<IdentityUser> userManager)
{
    _dbContext = context;
    _userManager = userManager;
}
``` 

Follow this dependency injection pattern when creating new controllers for your APIs. 

Up Next: [Getting Bikes](./biancas-get-bikes.md)