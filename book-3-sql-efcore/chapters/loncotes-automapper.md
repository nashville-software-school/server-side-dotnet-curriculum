 # Mapping from Model to DTO with Automapper


### Why use AutoMapper?
After finishing the projects in this book and Book 2, you will have noticed that the use of DTO classes has significantly increased the amount of code you need to write for each endpoint. You will also have noticed that much of this code is repetitive and tedious to write.

What do we do when we have tedious, repetitive code? Make it DRYer! How do we do that? We write reusable code that can be used everywhere that we need to create DTO objects. 

What would this reusable code need to do? It would need to take an object of one type and convert that object to another type. Using Loncotes County Library as an example, if we have an instance of `Material`, we want to be able to turn its data into an instance of `MaterialDto`. If we have an instance of `Genre`, we want to turn it into a `GenreDto`. Aside from the fact that `Material` and `Genre` have different properties, the process is exactly the same (you know this, because you have written a lot of endpoints that map instances of a model class to instances of a DTO class).  

What if we had code that could be told which classes map to which other classes? What if it had methods that would take an object of one type as an input along with a target type, and return a new object of the target type with the data from the input object?

Here is some pseudo-code of the above to help you understand the goal (this is not compilable C# code):
``` csharp
public targetType Map<targetType, inputType>(inputType inputObject)
{
    // create a new instance of the target type
    targetType newObject = new targetType();
    // get the input object's properties
    var properties = GetObjectProperties(inputObj);
    // set each of the new object's properties with the values from the input object
    for (var property of properties)
    {
        newObject.SetProperty(property.Name, property.Value);
    } 
    //return the new object with all of the property values from the old object
    return newObject;
}
```

Here's an example of our theoretical `Map` method in use:
``` csharp
MaterialDto material = Map<MaterialDto, Material>(inputMaterialObject);
```

We could implement code to do this on our own. But someone else already did, when they wrote `AutoMapper`! The rest of this chapter is a demo on implementing AutoMapper in Loncotes County Library.

## Setup
1. Install AutoMapper (run this command in the same directory as the `csproj` file for Loncotes County Library)
    ```
    dotnet add package AutoMapper
    ```
1. Create AutoMapper Profiles and configure.

   - This class tells AutoMapper which classes correspond to which other classes. If the property names match for the classes in question, this is all the configuration you need! It should be noted that you can configure these mappings more specifically.
   

    ```csharp
    using AutoMapper;

    public class AutoMapperProfiles : Profile
    {
        public AutoMapperProfiles()
        {

            CreateMap<Material, MaterialDTO>();
            CreateMap<MaterialDTO, Material>();
            CreateMap<MaterialType, MaterialTypeDTO>();
            CreateMap<MaterialTypeDTO, MaterialType>();
            CreateMap<Genre, GenreDTO>();
            CreateMap<GenreDTO, Genre>();
            CreateMap<Patron, PatronDTO>();
            CreateMap<PatronDTO, Patron>();
            CreateMap<Checkout, CheckoutDTO>();
            CreateMap<CheckoutDTO, Checkout>();
            CreateMap<Checkout, CheckoutWithLateFeeDTO>();
            CreateMap<CheckoutWithLateFeeDTO, Checkout>();
        }
    }
    ```
1. Register AutoMapper using _dependency injection_ in Program.cs

    ```csharp
    using AutoMapper;

    //Configure AutoMapper
    builder.Services.AddAutoMapper(typeof(AutoMapperProfiles));
    ```

1. Inject the mapper into  the minimal API endpoints and use Automapper QueryableExtensions to map from the model to the DTO.
    * The AutoMapper.QueryableExtensions library gives us access to the `ProjectTo<T>` extension method so that we can map an object to the target type `T` as part of our Linq query.
    ```csharp
    using AutoMapper.QueryableExtensions;

    app.MapGet("/materialtypes", (LoncotesLibraryDbContext db, IMapper mapper) =>
    {
        return db.MaterialTypes.ProjectTo<MaterialDTO>(mapper.ConfigurationProvider).ToList();
    });

    // //Get Genres
    app.MapGet("/genres", (LoncotesLibraryDbContext db, IMapper mapper) =>
    {
        return db.Genres.ProjectTo<GenreDTO>(mapper.ConfigurationProvider).ToList();
    });

    //Get Patrons
    app.MapGet("/patrons", (LoncotesLibraryDbContext db, IMapper mapper) =>
    {
        return db.Patrons.ProjectTo<PatronDTO>(mapper.ConfigurationProvider).ToList();
    });
    ```

1. Test these three endpoints to make sure that they work.

## Getting Single Items and Related Data
Replace the endpoint to get a single material with the following code:
``` csharp
app.MapGet("/api/materials/{id}", (IMapper mapper, LoncotesLibraryDbContext db, int id) =>
{
    var material = db.Materials
    .ProjectTo<MaterialDto>(mapper.ConfigurationProvider)
    .SingleOrDefault(m => m.Id == id);

    return material != null ? Results.Ok(material) : Results.NotFound();
});
```
Test the endpoint out and examine the data in the response. Notice that the material data includes the materialType, genre, and checkout data even though this code does not use `Include` to join the data. This is because, by default, `ProjectTo` will try to populate any properties present in the target class (in this case, `MaterialDto`). This means that practically speaking, if you have properties that you want to _exclude_ for a certain query, one of the easiest ways to do that is to create another DTO with exactly the properties that you want that endpoint to return. Often this will mean that you might have up to as many DTO classes as GET endpoints in your application (and that's fine!).

Second, notice that `ProjectTo` comes before `SingleOrDefault` in the method chain. This is because `SingleOrDefault` returns a `Material` instance, which doesn't have access to the `ProjectTo` method.

Try using AutoMapper for the rest of the GET endpoints in Loncotes County Library! 

## 🔍 Additional Materials
1. [AutoMapper docs](https://docs.automapper.org/en/)
1. [Queryable Extensions](https://docs.automapper.org/en/stable/Queryable-Extensions.html)