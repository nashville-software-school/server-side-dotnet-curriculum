# Getting campsites
In this chapter you will use EF Core to query the PostgreSQL database for campsite, as we as get a single campsite by `Id` with its related data. 

## Getting all campsites
1. Remove all of the weather forecast related code from `Program.cs` as we have done in previous projects
1. Add this endpoint to `Program.cs`: 
    ``` csharp
   app.MapGet("/api/campsites", (CreekRiverDbContext db) =>
    {
        return db.Campsites.ToList();
    });
    ```
1. Start debugging the project, and test the endpoint. That's right! It should work with no other code. A few things to notice about the endpoint:
    - Linq methods can be chained to `db.Campsites`, like `ToList`. Underneath this seemingly simple line of code, EF Core, is turning this method chain into a SQL query: `SELECT Id, Nickname, ImageUrl, CampsiteTypeId FROM "Campsites";`, and then turning the tabular data that comes back from the database into .NET objects! ASP.NET is _serializing_ those .NET objects into JSON to send back to the client. 
    - We provide the endpoint access to the `DbContext` for our database by adding another param to the handler. This is a rudimentary form of _dependency injection_, where the framework sees a dependency that the handler requires, and passes in an instance of it as an arg so that the handler can use it.

## Getting a `Campsite` by Id with its `CampsiteType`
Add this endpoint to `Program.cs`:
``` csharp
app.MapGet("/api/campsites/{id}", (CreekRiverDbContext db, int id) =>
{
    return db.Campsites.Include(c => c.CampsiteType).Single(c => c.Id == id);
});
```
A few more things to note:
1. `Include` is a method that will add related data to an entity. Because our campsite has a `CampsiteType` property where we can store that data, `Include` will add a `JOIN` in the underlying SQL query to the `CampsiteTypes` table. This is the same functionality that JSON Server provided with the `_expand` query string param.  
1. `Single` is like `First` in that it looks for one matching item based on the expression, but unlike `First` will throw an Exception if it finds _more_ than one that matches. For something like a primary key, where there is definitely only one row that should match the query, `Single` is a good way to express that. 
1. Test the endpoints with the following scenarios:
    - with an id for campsite that exists
        - Notice that the campsite JSON that is returned includes the campsite type data. 
    - with an id for a campsite that does not exist
        - This should break in the debugger because `Single` throws an Exception. Let the code continue to see the `500` response in Postman (or Swagger, if that's what you're using). Try to add error handling to this endpoint to prevent the `500` response, and send a `404` instead when a non-existent campsite is requested. 

Up Next: [Create a Campsite](./creek-river-create-campsite.md)