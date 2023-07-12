# Delete a campsite

Add this endpoint to the project:
``` csharp
app.MapDelete("/api/campsites/{id}", (CreekRiverDbContext db, int id) =>
{
    Campsite campsite = db.Campsites.SingleOrDefault(campsite => campsite.Id == id);
    if (campsite == null)
    {
        return Results.NotFound();
    }
    db.Campsites.Remove(campsite);
    db.SaveChanges();
    return Results.NoContent();

});
```
There isn't much to add here, except that `SingleOrDefault` works, as you might expect, similarly to `FirstOrDefault`. The `Remove` method, followed by `SaveChanges` executes a SQL query to delete the campsite by Id. 

> :bulb: You might be wondering what happens when you delete a campsite that has reservations associated with it. By default, EF Core will configure the related reservations rows to be deleted as well (this is the behavior that you are used to from JSON Server). This is called a _cascade delete_. If you want the database to do something other than cascade, you have to specify that in the configuration.

Up Next: [Update a campsite's details](./creek-river-campsite-update.md)