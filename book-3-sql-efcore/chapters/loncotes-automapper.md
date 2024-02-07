 Mapping from Model to DTO with Automapper
 ===============================================
> Automapper > Manually Mapping from Model to DTO?

### Why use automapper?
By using Automapper, developers can avoid writing boilerplate mapping code, reduce errors from manual mapping, and make the codebase cleaner and easier to understand. It's important to note that Automapper should not be used to perform business logic and should only be responsible for mapping data between objects

### How does automapper work?
Automapper operates based on conventions and reflection to map properties between objects without requiring extensive configuration. It can automatically match properties with similar names and types, and it can flatten complex object models into simpler DTOs. This means that if you have a domain model with associations to other entities, AutoMapper can map this to a DTO with properties that combine the associated entity's properties

To use Automapper, you would typically define mappings in a configuration file, often during the application startup. These mappings tell Automapper how to convert from one type to another. Once configured, you can use the IMapper interface to perform the actual mapping within your application layers, such as controllers in a Web API project. This involves calling the Map method with the source object and the desired target type, which returns a new instance of the target type with properties copied over from the source

### Performance Impact
Yes, there can be a performance impact when using Automapper for mapping between models and DTOs. AutoMapper uses *<b>reflection</b>*, which inherently adds overhead to the process of mapping because reflection operations are generally slower compared to direct assignments.

#### The performance cost can manifest itself in two main areas:

##### Initial Setup: Configuring AutoMapper, especially during application startup, can introduce a slight delay. This is because the mapping configurations are being established, which includes creating mapping profiles and setting up rules for property matching.

##### Runtime Processing: When the mapping occurs during the execution of the application, the performance cost is also present. Complex mappings, including those involving nested objects or conditional logic, can slow down the mapping process. This is where keeping the mappings as simple and straightforward as possible is beneficial 25.

It's worth noting that the performance impact might not be significant for smaller projects or when the mapping operations are not performed frequently. However, for larger projects or high-throughput scenarios, the impact could become noticeable. In such cases, it's recommended to measure the performance costs and optimize the usage of AutoMapper accordingly

## Setup
1. Install AutoMapper using 
```
dotnet add package Automapper
```
```
dotnet add package AutoMapper.Extensions.Microsoft.DependencyInjection
```
2. Create Automapper Profiles and configure

  *Create AutoMapperProfiles.cs class
```csharp
using System.Runtime.CompilerServices;
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
3.Register Automapper *Dependency Injection* in program.cs

* 
```csharp
using AutoMapper;

//Configure AutoMapper
builder.Services.AddAutoMapper(typeof(AutoMapperProfiles));
```

4. Inject mapper into minimal API Endpoints and use Automapper.QueryableExtensions to map from the model to the DTO.
* The automapper.queryableextensions library gives us access to the ProjectTo<T> extension method so that we can map 
```csharp
using AutoMapper.QueryableExtensions;

app.MapGet("/materialtypes", async (LoncotesLibraryDbContext db, IMapper mapper) =>
{
    return await db.MaterialTypes.ProjectTo<MaterialDTO>(mapper.ConfigurationProvider).ToListAsync();
});

// //Get Genres
app.MapGet("/genres", async (LoncotesLibraryDbContext db, IMapper mapper) =>
{
    return await db.Genres.ProjectTo<GenreDTO>(mapper.ConfigurationProvider).ToListAsync();
});

// //Get Patrons
app.MapGet("/patrons", async (LoncotesLibraryDbContext db, IMapper mapper) =>
{
    return await db.Patrons.ProjectTo<PatronDTO>(mapper.ConfigurationProvider).ToListAsync();
});
```

5. Perform mapping on the remaining api endpoints that call for it
6. Test Your API