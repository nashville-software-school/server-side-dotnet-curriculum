# Update a Campsite's Details

Add this endpoint: 
``` csharp
app.MapPut("/api/campsites/{id}", (CreekRiverDbContext db, int id, Campsite campsite) =>
{
    Campsite campsiteToUpdate = db.Campsites.SingleOrDefault(campsite => campsite.Id == id);
    if (campsiteToUpdate == null)
    {
        return Results.NotFound();
    }
    campsiteToUpdate.Nickname = campsite.Nickname;
    campsiteToUpdate.CampsiteTypeId = campsite.CampsiteTypeId;
    campsiteToUpdate.ImageUrl = campsite.ImageUrl;

    db.SaveChanges();
    return Results.NoContent();
});
```
This endpoint handler finds the matching campsite, updates each of its properties with the data from the incoming campsite, saves that change to the database, and returns `NoContent` because there isn't any data other than a success message that the endpoint needs to return. 

Up Next: [Getting Reservations](./creek-river-get-reservations.md)