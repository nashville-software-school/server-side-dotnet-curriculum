# :tent: More Data Validation For Reservations
In this chapter you will add logic to the reservation create handler to validate the reservation data

## CheckinDate is after CheckoutDate
[Time only moves forward](https://youtu.be/yKbJ9leUNDE), so it doesn't make sense for the CheckinDate to be after the CheckoutDate. Let's add some logic to our handler to make sure such a reservation could never be made. Add this before the `try` in :
``` csharp 
// Check if reservation checkout is before or the same day as checkin
if (newRes.CheckoutDate <= newRes.CheckinDate)
{
    return Results.BadRequest("Reservation checkout must be at least one day after checkin");
}
```

Test the endpoint now by trying to add a reservation with a CheckinDate later than the CheckoutDate

## Reservation is too long
The `CampsiteType` table tells us what the maximum number of nights a campsite may be reserved, depending on what kind of site it is. Let's make sure a reservation that is too long for the campsite can't be added to the database:
``` csharp
// check if reservation is too long
Campsite campsite = db.Campsites.Include(c => c.CampsiteType).Single(c => c.Id == newRes.CampsiteId);
if (campsite != null && newRes.TotalNights > campsite.CampsiteType.MaxReservationDays)
{
    return Results.BadRequest("Reservation exceeds maximum reservation days for this campsite type");
}
```

This code gets the campsite for the reservation from the database with the campsite type. Then we use the `TotalNights` property, and make sure it's not longer than the `CampsiteType`'s `MaxReservationDays`. 

## Challenges
1. Add logic to the handler to check that the reservation does not conflict with another reservation for that campsite that already exists (a new Checkin is allowed to happen on the same day as an existing Checkout). 
1. A reservation cannot be made for a same-day checkin or (more obviously) a check-in in the past. Prevent this with logic in the handler
1. Open-ended reservations are not allowed. Make sure that the reservation has both a CheckinDate and a CheckoutDate

