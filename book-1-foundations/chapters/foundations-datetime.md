# Working with the `DateTime` type
In this chapter we're going to add some time-related data to our products to determine their age and how long they've been in stock in the Thrown For a Loop store. 

Learning objectives:
1. Creating an instance of `DateTime`
1. `DateTime.Now`, `DateTime.Today`
1. comparing `DateTime`s

## Adding stock and manufacture dates to our `Product`s
The inventory manager at Thrown For a Loop would like to see how old and item is, and how long it has been in the store's inventory. For that we can use the `DateTime` type. 

Like classes, you can create a new instance of `DateTime` with the `new` keyword, like this:
```csharp
DateTime newYears2023 = new DateTime(2023, 1, 1);
```
This is very similar to the JS code for doing the same thing. The major difference is that January is represented by `1` in C#, but the months are zero-indexed in Javascript:
``` javascript
const newYears2023 = new Date(2023, 0, 1);
```

You can also add time information to your `DateTime`:
``` csharp
DateTime noonOnJanuaryFirst = new DateTime(2023, 1, 1, 12, 0, 0);
```
There are other ways and more data that you can use to create a `DateTime`, but this is more than enough for now. 

We can also add `DateTime` type properties to our classes. Let's do that with the `Product` class in `ThrownForALoop`.  Add these properties to the `Product` class in `Product.cs`:
``` csharp
public DateTime StockDate { get; set; }
public int ManufactureYear { get; set; }
```

Now, when we create `Product` instances, we can specify values for these properties. Here's the first one:
``` csharp
new Product()
{ 
    Name = "Football", 
    Price = 15, 
    Sold = false,
    StockDate = new DateTime(2022, 10, 20),
    ManufactureYear = 2010
}
```

Add `StockDate`s and `ManufactureYear`s to the rest of the products in your data. 

## Displaying product age and time in stock
It is common to capture the current date and time as part of the logic of the program. Accessing the `Now` property of the `DateTime` type will return a new instance of DateTime set to the moment it was called. Let's add that to our program right before the final line of the program:
``` csharp
DateTime now = DateTime.Now;
```

It is possible to retrieve the various pieces of data in the datetime using properties of the `DateTime` type. Let's replace the string that prints the details of the product to include the product's age and time in stock: 
``` csharp
TimeSpan timeInStock = now - chosenProduct.StockDate;
Console.WriteLine(@$"You chose: 
{chosenProduct.Name}, which costs {chosenProduct.Price} dollars.
It is {now.Year - chosenProduct.ManufactureYear} years old. 
It {(chosenProduct.Sold ? "is not available." : $"has been in stock for {timeInStock.Days} days.")}");
```

We are calculating the time that the item has been in stock by subtracting the `StockDate` of our product from `now`. When you do math on two `DateTime`s, the result is a `TimeSpan`. `TimeSpan`s also have a `Days` property, which represents the total number of days in that time span. We are accessing the `Year` property on `now` to get the current year, and subtracting the `Manufacture` year of the chosen product to give us the product's current age. In the ternary, we are checking to see if the product is sold. If it is, we display "not available". Otherwise, we access the `timeInStock` variable's `Days` property to get the total number of days between the stock date and now. We also added `@` to the string to allow formatting the message on multiple lines. 

Test your code with all of the products that you have in your list, and see how it works!

We have just scratched the surface of what `DateTime` can do for us, but this chapter covers some of the most common uses of the type that you will see in the course. We will take a look at one more in the next chapter. 

Up Next: [Working with other number types](./doubles-and-decimals.md)

## 🔍 Additional Materials
[DateTime documentation](https://learn.microsoft.com/en-us/dotnet/api/system.datetime?view=net-8.0)





