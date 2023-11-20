# Get Overdue Checkouts
The librarians want to see all overdue checkouts so that they can send emails to the patrons that need to return the books. Let's create an endpoint that does this.

### Start with a Simple Query

Start with what you already know how to do:
``` csharp
app.MapGet("/checkouts/overdue", (LoncotesLibraryDbContext db) =>
{
    return db.Checkouts
    .Include(p => p.Patron)
    .Include(co => co.Material)
    .ThenInclude(m => m.MaterialType)
    .ToList();
});
```

This endpoint gets all checkouts and includes the Patron, Material, and MaterialType data that belong to each Checkout. So far so good, but we don't want to return all checkouts, just those which are overdue. 

### Filter with `Where`
We need to use `Where` again, but logic for which materials are overdue is somewhat more complicated. Before we try to write the code, let's consider the algorithm for this logic:
1. We need to find out whether the number of days a material has been checked out is greater than the number of days the materials is allowed to be checked out. We can express this with the `>` comparison operator like this pseudo-code:
    ``` 
    daysCheckedOut > allowedCheckoutDays
    ```
1. We can figure out the `allowedCheckoutDays` from the material's material type:
    ```
    daysCheckedOut > checkout.Material.MaterialType.CheckoutDays
    ```
1. Days checked out is a little more complex. If we subtract the `CheckoutDate` from `DateTime.Today`, we will get a `TimeSpan` that represents the amount of time the material has been checked out. That `TimeSpan` has a `Days` property that tells us how many days that `TimeSpan` represents:
    ``` 
    (DateTime.Today - checkout.CheckoutDate).Days > checkout.Material.MaterialType.CheckoutDays
    ```
    The subtraction is inside parentheses so that we are accessing `.Days` on the result of the subtraction, not trying to access `Days` on the `CheckoutDate`
1. We also need to check to make sure that the material is still checked out. If it has been returned, it is not overdue:
    ```
    (DateTime.Today - checkout.CheckoutDate).Days > 
    checkout.Material.MaterialType.CheckoutDays && 
    checkout.ReturnDate == null
    ```

### Adding the logic from the algorithm to the query
Our algorithm has produced a usable _expression_ to represent the logic of an overdue material. All that is left is to use that expression as the condition for the `Where` method:

``` csharp
app.MapGet("/api/checkouts/overdue", (LoncotesLibraryDbContext db) =>
{
    return db.Checkouts
    .Include(p => p.Patron)
    .Include(co => co.Material)
    .ThenInclude(m => m.MaterialType)
    .Where(co =>
        (DateTime.Today - co.CheckoutDate).Days >
        co.Material.MaterialType.CheckoutDays &&
        co.ReturnDate == null)
        .Select(co => new CheckoutDto
        {
            Id = co.Id,
            MaterialId = co.MaterialId,
            Material = new MaterialDto
            {
                Id = co.Material.Id,
                MaterialName = co.Material.MaterialName,
                MaterialTypeId = co.Material.MaterialTypeId,
                MaterialType = new MaterialTypeDto
                {
                    Id = co.Material.MaterialTypeId,
                    Name = co.Material.MaterialType.Name,
                    CheckoutDays = co.Material.MaterialType.CheckoutDays
                },
                GenreId = co.Material.GenreId,
                OutOfCirculationSince = co.Material.OutOfCirculationSince
            },
            PatronId = co.PatronId,
            Patron = new PatronDto
            {
                Id = co.Patron.Id,
                FirstName = co.Patron.FirstName,
                LastName = co.Patron.LastName,
                Address = co.Patron.Address,
                Email = co.Patron.Email,
                IsActive = co.Patron.IsActive
            },
            CheckoutDate = co.CheckoutDate,
            ReturnDate = co.ReturnDate
        })
    .ToList();
});
```

Test the endpoint to make sure that it works. Breaking tasks like this one down with an algorithm can make figuring out how to solve new problems easier. We started with the basic logic using variables (`daysCheckedOut` and `allowedCheckoutDays`). Step by step we figured out how to determine the values of each of those.

Up Next: [Late Fees](./loncontes-calculate-fees.md)