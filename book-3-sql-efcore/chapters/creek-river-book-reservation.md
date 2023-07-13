# Booking a Reservation and Cancelling
In this chapter you will add endpoints to create a reservation and cancel a reservation. Additionally, you will explore one way to handle bad input from the user. 

# Creating a Reservation

1. Add this endpoint to the project:
    ``` csharp
    app.MapPost("/api/reservations", (CreekRiverDbContext db, Reservation newRes) =>
    {
        db.Reservations.Add(newRes);
        db.SaveChanges();
        return Results.Created($"/api/reservations/{newRes.Id}", newRes);
    });
    ```
1. Test the endpoint with a JSON reservation object in Postman (or Swagger). If you used valid values for all of the properties, you should get a `201` response with the new reservation in the body. 

1. Test the endpoint again, but provide at least one foreign key value that doesn't exist in the database (`userProfileId` or `campsiteId`). 

1. You should see a debugger message that the program threw a `DbUpdateException` because the update violated a foreign key constraint. If you let the program continue, you will see a `500` response with the same error in the body. 

1. Let's use `try`/`catch` to provide a better response:
    ``` csharp
    try
    {
        db.Reservations.Add(newRes);
        db.SaveChanges();
        return Results.Created($"/api/reservations/{newRes.Id}", newRes);
    }
    catch (DbUpdateException)
    {
        return Results.BadRequest("Invalid data submitted");
    }
    ```
1. Test the endpoint again to make sure that the logic works. We have not yet exhausted the edge cases that could break this endpoint. See if you can come up with some more! Do you have ideas about how to fix them? Try [this explorer chapter](./creek-river-reservation-validation.md) if you want to see some of them. 

## Cancelling a Reservation
See if you can implement this endpoint on your own! You can use the endpoint for deleting a campsite as a reference. 