# Calculating Total Nights and Total Cost
In this chapter we will explore how to use calculated properties to add useful data to the Reservation model. 

## Calculated Properties
You have seen these in previous books, but calculated properties can also be used on our data models to provide calculated values so that they don't have to be calculated client-side. 

These properties _will not_ be stored in the database. In order for EF Core to create a column for it in the database, a property has to have both `get` and `set`. So properties that only have `get`, or which use the lambda expression syntax (`=>`) will not be created as columns in the database. However, they _will_ get serialized in the JSON response as long as they are marked as `public`. Let's see an example of how this works

## `TotalNights`
Add this property to the `Reservation` model:
``` csharp
public int TotalNights => (CheckoutDate - CheckinDate).Days;
```
This is essentially the same as this:
``` csharp
public int TotalNights
{
    get
    {
        return (CheckoutDate - CheckinDate).Days;
    }
}
```

As you can see, the property only has a `get`, not a `set`, so it will not correspond to a column in the database. However, when creating the JSON (serializing) from the C# objects, ASP.NET will evaluate the calculated properties and add them to the JSON. 

Test this out by hitting the `/api/reservations` endpoint. Now you will see a calculated value for the total number of nights for a reservation. You can imagine that it would be handy to have this value on the client side to display, and keeps the front end code cleaner by moving this business logic to the server side. This allows the client to focus on _how to display data_, not _how to calculate the data to display_. 

> :bulb: You might ask - why not store `TotalNights` as a column in the database? The answer is that we would then have two sources of truth for how many nights a reservation has. Every time an update changes the `CheckinDate` or `CheckoutDate` value for a reservation, you would have to remember to update the `TotalNights` column accordingly. This is easy to forget. _However_ - our SQL database could generate this value for us in a query like this: <br>
    `SELECT *, "CheckoutDate" - "CheckinDate" As "TotalNights" FROM "Reservations";` <br> Working with this in EF Core is somewhat more complex, though, so stick with what we have in the code example above for now. 

> :bulb: You might also ask - What's wrong with calculating this on the front end? In the first part of the course, most of the _business logic_ of your application - the code that contained logic specific to the domain for which your app provided a service (dog walking, shoe organization, calorie tracking, etc...) was in the front end because you did not have an API to handle more of this logic for you. Now, however, things are different. You can allow the code in your UI to focus on logic around how things should look, and let the API worry about the specific business rules of the application. Like everything with software development, there are _lots_ of reasonable exceptions to this rule, and sometimes it is unavoidable to have business logic on the front end.  

## `TotalCost`
Add this line to the `Reservation` class:
``` csharp
private static readonly decimal _reservationBaseFee = 10M;
```
This is a small line, but there's a lot going on:
1. This class member is called a _field_. Fields do not have `get` or `set`, which is how you know it is not a _property_.
1. It is marked as `private`. This is another _access modifier_, like (`public` and `protected`). It prevents this field from being accessed outside of code that is contained in the class (most fields are either `private` or `protected`). One of the results of this is that it will be ignored in the JSON, so it won't appear in HTTP responses. 
1. It is also marked as `static`. This means that the value for this field will be shared across _all instances_ of Reservation
1. It is also marked as `readonly`, so that once set, its value cannot change. 
1. This is not exactly new, but just as a reminder, to indicate that a numeric value is a `decimal` and not an `int` or `double`, there is an `M` at the end of the number.
1. By convention, private fields have a prepended `_`.   

The purpose of this field is to store the base reservation cost, which is charged no matter how many nights are reserved. It is the same for every reservation, so we want all reservation instances to share it, but we don't need to send it back with every reservation in the JSON. 

Add this property to the `Reservation` model:
``` csharp
public decimal? TotalCost
    {
        get
        {
            if (Campsite?.CampsiteType != null)
            {
                return Campsite.CampsiteType.FeePerNight * TotalNights + _reservationBaseFee;
            }
            return null;
        }
    }
```
This property multiplies the fee per night for this campsite by the total nights, and then adds the base reservation fee to calculate the total cost. Because the `Campsite` and `Campsite.CampsiteType` properties may not always be set on this model, the `get` first has to check to make sure that they have values. 

Fortunately, the endpoint to get reservations already includes `CampsiteType`, so test it again to see the `totalCost` values appear in the JSON response for each reservation. 

## Summary
This is the end of the walk-through for Entity Framework Core. Before moving to the second column, do the "Up Next" Chapter on Inheritance. After finishing the other columns, check out the explorer chapters for this project.

Up Next: [Inheritance](https://github.com/nashville-software-school/bangazon-inc/blob/server-side-curriculum/book-1-orientation/chapters/INHERITANCE_INTRO.md) - this is from another repo, come back to this repo to start [Loncotes County Library](./loncotes-setup.md) after finishing the inheritance chapter.

## 🔍 Additional Materials
1. [Access Modifiers](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/access-modifiers)
1. [Fields](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/fields)