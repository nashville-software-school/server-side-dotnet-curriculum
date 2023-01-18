# Doubles and Decimals
In this chapter we're going to (1) change the Price property to use the `decimal` type, (2) add a `Condition` property to the `Product`s to rate the wear-and-tear on them, and (3) display the total value of the inventory to the user.

Learning Objectives:
1. Declaring and initializing `double` and `decimal`
1. Understanding the difference between these two types and integers
1. Performing calculations with number types. 
1. interating through a `List` with `foreach`.

## Adding a `Condition` property to `Product`
The manger of Thrown For a Loop would like to record the condition of the used equipment items that are in stock. The condition is graded on a scale of 1-5, and the manager wants to be able to use non-integer values (3.4, 4.8, etc...)

Add another property to the `Product` class called `Condition`:
```csharp
public double Condition { get; set; }
```
In your products list, add floating point values for the condition of of the items in the products list. Here's one:
``` csharp
new Product() 
{ 
    Name = "Football",
    Price = 15,
    Sold = false,
    StockDate = new DateTime(2022, 10, 1),
    ManufactureYear = 2010, 
    Condition = 4.2
}
```
Add to the string at the bottom of the program to also display the condition of the product when showing its details. 

## Representing prices as decimals

Not all of the prices of our items will be integers, so let's change the type of `Price` to `decimal` in `Product.cs`:
```csharp
public decimal Price { get; set; }
```
Now, we can represent our prices like this:
```csharp
new Product()
{
    Name = "Football",
    Price = 15.00,
    Sold = false,
    StockDate = new DateTime(2022, 10, 1),
    ManufactureYear = 2010, 
    Condition = 4.2
}
```

Uh oh. What happens when we do that? You should see a compiler error (hover over a number with a red squiggle under it) looking something like this:
![Decimal Compiler Error](../../assets/decimal-compiler-error.png)
The reason this happens is that the C# compiler reads that `15.00` as a `double` value, and doesn't allow you to convert from one to the other (the reason for that is something you don't need to know right now). The message tells us to add an `M` at the end of the value to indicate to the compiler that our number is a `decimal`, and not a `double`, so let's do that:
``` csharp
new Product()
{
    Name = "Football",
    Price = 15.00M,
    Sold = false,
    StockDate = new DateTime(2022, 10, 1),
    ManufactureYear = 2010, 
    Condition = 4.2
}
```

Now our compiler errors should go away! Update all of the prices in the products list and run the program to make sure it still works. 



