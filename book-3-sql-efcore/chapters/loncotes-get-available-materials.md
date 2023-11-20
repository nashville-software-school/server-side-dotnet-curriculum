# Get Only Available Materials
Let's create an endpoint to get all materials (with their genre and material type) that are currently available (not checked out, and not removed from circulation). A checked out material will have a related checkout that has a `ReturnDate` of `null`. 

## Filtering on Related Data
This is more complicated than a standard use of `Where`, because we are trying to filter on a table that isn't the initial entity that we are querying for, and actually there are potentially _many_ checkouts associated with a material. We need to make sure that _all_ of them have `ReturnDate`s in order to determine that the material is free to be checked out. How do we do that?

### Thinking about the Data Structure
In this case, thinking about this like it was a SQL query is actually _not_ that helpful. Let's use JSON notation to visualize instead. 

A material can have many checkouts:
``` json
{
    "id": 2,
    "materialName": "The History of the Decline and Fall of the Roman Empire",
    "materialTypeId": 3,
    "genreId": 2,
    "checkouts": [
        {
        "id": 1,
        "materialId": 2,
        "patronId": 1,
        "checkoutDate": "2023-07-27T00:00:00",
        "returnDate": null
        },
        {
        "id": 15,
        "materialId": 2,
        "patronId": 3,
        "checkoutDate": "2022-10-27T00:00:00",
        "returnDate": "2022-11-05T00:00:00"
        }
    ]
}
```
The above material is _checked out_, and we know that because it has a checkout without a return date. So for a material to be available, that means that none of its checkouts (if it has any) are not yet returned. This seems obvious, but visualizing it will make the query with Linq (and EF Core) make more sense. 

### Using the `All` Linq method
Start with the same query for getting all circulating materials:
``` csharp
app.MapGet("/materials/available", (LoncotesLibraryDbContext db) =>
{
    return db.Materials
    .Where(m => m.OutOfCirculationSince == null)
    .ToList();
});
```

To this query we need to add another `Where`, to filter out materials that have an unreturned checkout:
```csharp
app.MapGet("/api/materials/available", (LoncotesLibraryDbContext db) =>
{
    return db.Materials
    .Where(m => m.OutOfCirculationSince == null)
    .Where(m => m.Checkouts.All(co => co.ReturnDate != null))
    .Select(material => new MaterialDto
    {
        Id = material.Id,
        MaterialName = material.MaterialName,
        MaterialTypeId = material.MaterialTypeId,
        GenreId = material.GenreId,
        OutOfCirculationSince = material.OutOfCirculationSince
    })
    .ToList();
});
```
The second `Where` says "only return materials where _all_ of the material's checkouts have a value for `ReturnDate`".

Finally, we transform the `Material` objects into `MaterialDTO`s to send back to the client.

Test the endpoint to make sure it works (you will need to have some materials in your database that are checked out currently to be able to properly test.)



Up Next: [Get Overdue Checkouts](./loncontes-overdue-checkouts.md)