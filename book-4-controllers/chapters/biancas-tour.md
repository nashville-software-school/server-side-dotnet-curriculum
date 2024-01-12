# A Tour of Bianca's Bike Shop
In this chapter we will introduce the concept of a controller and how to add controller methods to create endpoints with routes. Depending on how you like to work, you will read this carefully now, or skim it. Some like to have all the details up front, and others like to learn them later after working through the exercises. After you finish the exercises in this book (all three columns), come back to these tour chapters again to solidify your conceptual understanding. 

## `Program.cs`
This file is generally very different from previous books, and this is partially because of all of the authentication logic that wasn't there before (we'll look at that in the next chapter.) Notice, however, that there are no endpoints. Instead, that is all replaced with one statement:
``` csharp
app.MapControllers();
```
This tells ASP.NET that we want to use controllers to create endpoints, and the framework is able to discover those controllers in our code. 

## What's a controller? 
First, a controller is a _class_. A controller class contains methods that are the _handlers_ for the endpoints of the API.  In web APIs the controllers _inherit_ many of the properties and methods we will use from the `ControllerBase` class. Take a look at the `BikeController`:
> BikeController.cs
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
This class has a number of _attributes_ on it and on the methods that you haven't seen before:
1. All controllers in your program should be _decorated_ with the `ApiController` attribute, and should _inherit_ `ControllerBase`. 
1. A controller contains all of the endpoints for a specific resource. In this case, all endpoints related to the Bikes resource. Usually you will have one controller for each resource available in the API. 
1. The `BikeController` class also has a `Route` attribute. This tells the framework what route segment should be associated with all of the endpoints in the controller. In this case, `"api/[controller]"` tells the framework to use the first part of the controller name ("Bike") to create the route. So all of the endpoints in this controller will have URLs that start with `"/api/bike"` (it is case insensitive)
1. Finally, the `Get` method is an endpoint. The `Ok` method that gets called inside `Get` will create an HTTP response with a status of `200`, as well as the data that's passed in. It is decorated with the `HttpGet` attribute to mark it as a `GET` endpoint, but is technically unnecessary as `GET` is the default. `Authorize` will be covered in the next chapter. 

Up Next: [Bianca's Tour II: Auth](./biancas-auth.md)

## 🔍 Additional Materials
1. [ControllerBase class](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controllerbase?view=aspnetcore-8.0)
1. [ApiController Attribute](https://learn.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-8.0#apicontroller-attribute-1)