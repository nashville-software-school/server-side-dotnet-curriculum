# :football: :curly_loop: Datetimes in Depth
In this explorer chapter, we will add more features to Thrown For a Loop relating to Datetimes. This chapter assumes you have already learned the new concepts in Reductio & Absurdum, so make sure you finish the main table before coming to this chapter. 

## Adding more data properties to the `Product` class

1. Change the `Sold` property to a `Datetime` property called `SoldOnDate`. Add `?` at the end of the type (it will look like this -  `Datetime?`) so that the `Datetime` will be _nullable_. (Datetimes can't normally have a value of `null`). We want it to be nullable because not all products are already sold. 
1. Now that you have covered _calculated properties_ and have added the `SoldOnDate` property to the `Product` class, we can create a calculated `TimeInStock` property

### `TimeInStock`

#### Algorithm
1. The time in stock property needs to subtract the products last day on the shelf from its first day
1. If the product is sold, its last day was the day it was sold. 
1. Otherwise, its last day is today (because our app can't time travel - _yet_. )
#### Implementation
1. Add the `TimeInStock` property with only a `get`:
    ```csharp
    public TimeSpan TimeInStock 
    {
        get
        {

        }
    }
    ```
    - We use the `TimeSpan` type because this data will represent the difference between two moments in time. 
1. First, we need to determine the last day on the shelf. According to our algorithm, we need to first see whether the product is sold and set the last day based on that:
   ```csharp
    public TimeSpan TimeInStock 
    {
        get
        {
            DateTime lastDay;
            if (SoldOnDate != null)
            {
                lastDay = (DateTime)SoldOnDate;
            } else
            {
                lastDay = Datetime.Now;
            }
        }
    }
    ```
    - We have to convert `SoldOnDate` to a `DateTime` because `DateTime` and `DateTime?` are not the same type! `lastDay` has to be a `DateTime`. (See the other explorer chapter on type conversion).  
1. Finally, we subtract the last day from the `StockDate` to get the `TimeInStock` and return it:
   ```csharp
    public TimeSpan TimeInStock 
    {
        get
        {
            DateTime lastDay;
            if (SoldOnDate != null)
            {
                lastDay = (DateTime)SoldOnDate;
            } else
            {
                lastDay = DateTime.Now;
            }
            return lastDay - StockDate;
        }
    }
    ```
1. We can use a ternary to make this code a little cleaner:
    ``` csharp
    public TimeSpan TimeInStock
    {
        get
        {
            DateTime lastDay = SoldOnDate != null ? (DateTime)SoldOnDate : DateTime.Now;
            return lastDay - StockDate;
        }
    }
    ```
#### Refactor `Program.cs`
Now that you have updated the product class, update the code in `Program.cs` to use the new properties that are available in `Product`. Add `SoldOnDate`s to some of the `Product`s in the `products` collection, and update (in most cases you can simplify!) the code in each of the methods. 

## Monthly Sales Report
This feature will be easier to test if you add more products to the products collection with `SoldOnDate`s from the same month and year.

1. Add a `MonthlySalesReport` method to `Program.cs` and an option in the main menu for using it. 

1. In the method, prompt the user to input a year and a month

1. Use `Where` to find products that were sold in that month and year (use the `Year` and `Month` properties on the `DateTime` to get the `SoldOnDate`s year and month)

1. Add all of the prices of the products sold in that month. Print a friendly message with the year, month, and the sales total. (you can use a loop to add all of the prices together, or investigate the `Sum` Linq method). 

## Add and Update a Product
1. Add a method to add a new unsold Product to the collection. Use `DateTime.Now` to automatically set the `StockDate` when adding a `Product`. 
1. Add a method to mark a product as sold, again using `DateTime.Now` to automatically set that date. 
1. CHALLENGE: make this second feature available after showing the product details, rather than from the main menu.   

## More Stats
Implement the following features:

1. Allow the user to see the average time that currently stocked products have been on the shelf (in days).
1. Allow the user to see the average amount of time the sold products were on the shelf before sale. 
1. CHALLENGE: Use the `Hours` property of the `DateTime` class to discover the busiest sales hours at the store. Display each hour in the day with their sales numbers. You will need to minimally add hours, minutes, and seconds data for each of the sold products.     