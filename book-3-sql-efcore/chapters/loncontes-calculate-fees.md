# Calculating Late Fees
In this chapter you will use calculated properties to calculate the late fee for a returned material

## Instructions
1. Add a private static decimal field to the `Checkout` class called `_lateFeePerDay`,  and set it equal to `.50M`. This will store the amount a patron is charged per day a material is late (50 cents). 
1. Add a calculated property to the `Checkout` class called `LateFee`. Its type should be a nullable decimal. For the logic for this property, see the algorithm below

## Algorithm
1. If there is a `ReturnDate`, and the due date was before the `ReturnDate`, there will be a fee. Otherwise, the value should be null:
    ``` 
    if (ReturnDate != null &&  dueDate < ReturnDate)
    {
        //do logic to return fee...
    }
    return null;
    ```
1. To calculate the due date, you need to add the number of days that the item can be checked out to the checkout date:
    ```
    DateTime dueDate = CheckoutDate.AddDays(Material.MaterialType.CheckoutDays);
    ```
1. To calculate the fee, you need to find out the number of days between the ReturnDate and the dueDate
    ```
    int daysLate = (ReturnDate - dueDate).Days;
    ```
1. The actual late fee will be the product of `daysLate` and the `_lateFeePerDay`:
    ``` 
    decimal fee = daysLate * _lateFeePerDay;
1.  Now all that's left is to put it all together:
    ```
    DateTime dueDate = CheckoutDate.AddDays(Material.MaterialType.CheckoutDays);
    if (ReturnDate != null && dueDate < ReturnDate)
    {
       int daysLate = ((DateTime)ReturnDate - dueDate).Days;
       decimal fee = daysLate * _lateFeePerDay;
       return fee;
    }
    return null;
    ``` 
    We have to _cast_ `ReturnDate` as a `DateTime`, because it could possibly be `null`. By casting `ReturnDate` as a `DateTime` (instead of `DateTime?`), we are telling the compiler to trust us that `ReturnDate` does indeed have a `DateTime` value. This is quite safe in this case, because this code is inside an `if` whose condition begins with `ReturnDate != null`.  
1. Add this logic to the calculated property. 

## Try it for yourself!
Add another calculated property to the `Patron` class called `Balance` that totals up the unpaid fines that a patron owes. Add a `Paid` property of type `bool` to the `Checkout` class that indicates whether a fee has been paid or not. Try to use the method in this and the previous chapters to break the problem down. Start with a description of the problem, and then pseudo-code. Slowly turn parts of the pseudo-code into actual code as you figure out how to solve each piece. 