# Calculated Properties

In this chapter you will add a `DateStocked` to the `Product` class to store when the product was added to the inventory. You will also add a _calculated property_ that will allow the user to easily query how long the product has been on the shelf.

## What is going on with `get` and `set`?

So far you have been adding properties to your classes that look like this (by the way, go ahead and add this property to your `Product` class):

```csharp
Datetime DateStocked { get; set;}
```

The example property above is called an _auto-implemented_ property. This means that the _getter_ and _setter_ for `DateStocked` are created by the compiler so that you don't have to write them by hand.

But what are `get`ters and `set`ters? A `get`ter for a property is essentially a function whose result is bound to the property name on a class. Instead of auto-implementing a property, we could write the `get` ourselves. A classic example is creating a property to retrieve a full name for a person when we are already storing their first and last names separately. That looks like this:

```csharp
public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string FullName
    {
        get
        {
            return FirstName + " " + LastName;
        }
    }
}
```

You can see that `LastName` is defined as a string, so the `get` has to return a value that is of type `string` as well. The property concatenates the `FirstName` and `LastName` properties together with a space. Notice that there is no `set` at all. This is because we don't want code to ever set the `FullName` property. We can determine that from the values of `FirstName` and `LastName` instead. So we can do this:

```csharp
const sarah = new Person()
{
    FirstName = "Sarah",
    LastName = "Brandt"
};

Console.WriteLine(sarah.FullName);
// result: "Sarah Brandt"

sarah.FullName = "Sarah Brandt-Smith"; // Won't compile!
//compiler error: Property or indexer 'Person.FullName' cannot be assigned to -- it is read only
```

When the code accesses the `FullName` property, the `get` function returns the first name, plus a space, plus the last name as a string. The `FullName` property is _calculated_, in the sense that it is determined by other values, and cannot be set directly. It is _read-only_.

The `set` function takes a value as an input (accessed with the keyword `value` inside the setter) to be processed or saved as needed. It is rare to implement a `set` in comparison to `get` as there are fewer use cases for it, none of which we'll cover in this course. The most you will interact with it is to include it in auto-implemented properties, like `FirstName`, or exclude it, as in calculated properties like `FullName` to make them read-only.

## Adding a `DaysOnShelf` property to the `Product` class

Red and Abe want to quickly be able to identify what products are sitting on the shelves too long, so they want you to support easily seeing the number of days since a product was stocked.

1. If you haven't added the `DateStocked` property above into your class already do that.

1. Add this property to the `Product` class:

```csharp
public int DaysOnShelf
{
    get
    {
        TimeSpan timeOnShelf = DateTime.Now - DateStocked;
        return timeOnShelf.Days;
    }
}
```

3. Add `DateStocked` DateTimes to all of the products in your database.

4. Now, where the products are being listed, display the days that the product has been on the shelf along with the rest of the product details by accessing the `DaysOnShelf` property.

> Why are we doing this? In Thrown for a Loop, we calculated a TimeSpan in our code without using a class property, and that worked fine. The disadvantage of what we did in Thrown For A Loop is that the code outside of the class needed to know _how_ to use the properties of the Product class to calculate the age of the product. The code needed to know all of the implementation details. Here, we are keeping all of those details in the `Product` class itself, so that all the code that uses the class needs to know is that there is a property on the class `DaysOnShelf` that can be accessed. This is an example of the first pillar of Object-Oriented Programming (OOP) - _abstraction_.

Up Next: [Querying our Data with Linq]()

## 🔍 Additional Materials

[.NET OOP tutorial](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/tutorials/oop)
