# Getting Reservations with Related Data

Add this endpoint to the project:
``` csharp
app.MapGet("/api/reservations", (CreekRiverDbContext db) =>
{
    return db.Reservations
        .Include(r => r.UserProfile)
        .Include(r => r.Campsite)
        .ThenInclude(c => c.CampsiteType)
        .OrderBy(res => res.CheckinDate)
        .ToList();
});
```

Here it might be helpful to imagine the SQL query that this method chain will produce:
1. The first `Include` will `JOIN` the `UserProfiles` table
1. The second `Include` will further `JOIN` the `Campsites` table
1. `ThenInclude` is called when you want to `JOIN` to a table that is not the original table. It must immediately follow the `Include` call that `JOIN`s the table that you wish to `JOIN` to. In this case, because we can only get to `CampsiteTypes` from `Campsites`, and not the original `Reservations` table, we call `ThenInclude` to add `CampsiteTypes` data to the `Campsites` data right after the `Include` call to `JOIN` `Campsites`
1. `OrderBy` is a regular Linq method (see the explorer chapter on advanced Linq!), but corresponds directly to the `ORDER BY` keywords in SQL. 

The full SQL query this generates will be something like this:
``` sql
SELECT r.Id,
       r.CampsiteId,
       r.UserProfileId,
       r.CheckinDate,
       r.CheckoutDate,
       up.Id,
       up.FirstName,
       up.LastName,
       up.Email,
       c.Id, 
       c.Nickname
       c.ImageUrl,
       c.CampsiteTypeId, 
       ct.Id,
       ct.CampsiteTypeName,
       ct.MaxReservationDays,
       ct.FeePerNight
FROM Reservations r
LEFT JOIN UserProfiles up ON up.Id = r.UserProfileId
LEFT JOIN Campsites c ON c.Id = r.CampsiteId
LEFT JOIN CampsiteTypes ct ON ct.Id = c.CampsiteTypeId
ORDER BY r.CheckinDate;
```
Here you can see that we need `ThenInclude` for `CampsiteType` because the `JOIN` for it is not on a `Reservations` column but a `Campsites` column, and `ThenInclude` must be chained right after the `Include` that `JOIN`s `Campsites`, so that `ThenInclude` can determine which table it is joining from.  

## Testing the endpoint
Try the endpoint out. You will notice that if a campsite has reservations associated with it, EF Core will also populate the reservation data for those campsites nested in the `reservations` property. This is ok, and is called "navigation property fix-up". For the most part, you can ignore this extra data, but occasionally you might find it useful.

Up Next: [Booking Reservations](./creek-river-book-reservation.md)

