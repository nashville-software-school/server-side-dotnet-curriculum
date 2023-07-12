# Create a Campsite

1. Add the following endpoint to the project:
    ``` csharp
    app.MapPost("/api/campsites", (CreekRiverDbContext db, Campsite campsite) =>
    {
        db.Campsites.Add(campsite);
        db.SaveChanges();
        return Results.Created($"/api/campsites/{campsite.Id}", campsite);
    });
    ```
1. Test the endpoint with a JSON object for a new campsite (do not include an `id`). Remember that the JSON property names should be camelCase, not PascalCase. 

The  `SaveChanges` method (inherited in `CreekRiverDbContext` from the `DbContext` class), actually sends the change to the database. Don't forget to call this method when you are making data changes! Note that it is possible to make multiple changes before calling `SaveChanges`, and the database will get all of those updates at once. 

The `Results.Created` methods creates a `201` response, indicating that a new campsite was created. The first argument in that method call is the URL where the new campsite can be accessed. `Created` will add a "location" header to the HTTP response that the client can use if it needs it. The second argument will be the body of the HTTP response (the new campsite). EF Core _automatically_ adds the new Id to the campsite object.

Up Next: [Delete a Campsite](./creek-river-delete-campsite.md)