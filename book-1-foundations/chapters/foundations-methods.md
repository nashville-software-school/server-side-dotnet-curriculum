# Introduction to Methods
In this chapter we will wrap our current code in a method (like a function) so that we can run it multiple times, and will add two other methods to implement looking up the latest additions to the stock. 

Learning objectives: 

1. Creating a method, understanding the pieces of a  _method signature_
1. Using methods to create reusable pieces of code

## Allowing the user to perform more than one action
Right now, the app only allows a user to view a single product and then exits. Let's keep the app running so that the user can look at more than one product. 

First, let's create a method at the bottom of `Program.cs`:
``` csharp
void ViewProductDetails()
{
    
}
```
This basic method doesn't do anything right now, because there is no code in it! Move all of the code after the greeting is written to the console inside the curly braces of the new method we just added. 

Now, right below the greeting, you can _call_ the method, like you would call a function in Javascript:
```csharp
string greeting = @"Welcome to Thrown For a Loop
Your one-stop shop for used sporting equipment";

Console.WriteLine(greeting);
ViewProductDetails(); // Add this line right here
```
Run your app, and you will see that it works just like it did before. This isn't particularly helpful yet, because we're just calling the method once. Let's create a menu of options that the user can choose from. Replace the line you added in the last step with this code:
``` csharp
string choice = null;
while (choice != "0")
{
    Console.WriteLine(@"Choose an option:
                        0. Exit
                        1. View Product Details");
    choice = Console.ReadLine();
    if (choice == "0")
    {
        Console.WriteLine("Goodbye!");
    }
    else if (choice == "1")
    {
        ViewProductDetails();
    }
}
```
Now our application will let us choose an action to perform (right now, either look at a product or exit the program), and once that action is finished, will go back to the main menu if we don't choose `0`. Try it out!

The `ViewProductDetails` method is useful because we can call it multiple times, and the `while` loop with the main menu doesn't have to have all of that code inside it. 

Let's add another method at the very bottom of `Program.cs`:
```csharp
void ListProducts()
{
    decimal totalValue = 0.0M;
    foreach (Product product in products)
    {
        if (!product.Sold)
        {
            totalValue += product.Price;
        }
    }
    Console.WriteLine($"Total inventory value: ${totalValue}");
    Console.WriteLine("Products:");
    for (int i = 0; i < products.Count; i++)
    {
        Console.WriteLine($"{i + 1}. {products[i].Name}");
    }
}
```

Notice, this is all of the code that is in `ViewProductDetails` for displaying the products. You can replace that code in `ViewProductDetails` with a call to `ListProducts`:

```csharp
void ViewProductDetails()
{
    ListProducts(); 
    // the rest of the method is omitted for brevity
```
We can also add another option to our menu to view all products:
``` csharp
string choice = null;
while (choice != "0")
{
    Console.WriteLine(@"Choose an option:
                        0. Exit
                        1. View All Products
                        2. View Product Details");
    choice = Console.ReadLine();
    if (choice == "0")
    {
        Console.WriteLine("Goodbye!");
    }
    else if (choice == "1")
    {
        ListProducts();
    }
    else if (choice == "2")
    {
        ViewProductDetails();
    }
}
```
Run the application, and try viewing all products as well as listing the details for a few products, then testing to make sure the app exits when you choose `0`.


## View Latest Additions

The store manager would like an option to view the latest additions to the store. These products should be still available, and added to the stock within the last 90 days. First, you will need to make sure you have a few `Product`s in `products` that have a  `StockDate` more recent than 90 days ago. To cover our `edge cases`, we should make sure that at least one of the newer products is `Sold`, so that we can test whether our algorithm filters the right products. Go ahead and add those now. 

Now let's write the method to get and display those products:
``` csharp
void ViewLatestProducts()
{
    // create a new empty List to store the latest products
    List<Product> latestProducts = new List<Product>();
    // Calculate a DateTime 90 days in the past
    DateTime threeMonthsAgo = DateTime.Now - TimeSpan.FromDays(90);
    //loop through the products
    foreach (Product product in products)
    {
        //Add a product to latestProducts if it fits the criteria
        if (product.StockDate > threeMonthsAgo && !product.Sold)
        {
            latestProducts.Add(product);
        }
    }
    // print out the latest products to the console 
    for (int i = 0; i < latestProducts.Count; i++)
    {
        Console.WriteLine($"{i + 1}. {latestProducts[i].Name}");
    }
}
```
Spend a little time with the code above. If there are some types or code in it that you don't understand, try searching the web for them. 

We also need to add the option to the main menu. Add the option to the menu string, and then add this to the end of the `if`/`else`:
``` csharp
else if (choice == "3")
{
    ViewLatestProducts();
}
```

Run the app and try all of the options. Check to make sure that only the unsold, recent products appear when you select `3`.

## `void`
You'll have noticed that all of the methods we have written so far start with the word `void`. All methods are declared with a _return type_, indicating what the type of data the output of the method will be. Our methods we wrote in this chapter don't return anything in particular (as you can see, none of them have the `return` keyword inside of them). When a method doesn't use the `return` keyword anywhere, we say that its return type is `void`. In Javascript, the return type of a function without `return` in it is `undefined`. In later projects in this book, we will introduce methods that return other types besides `void`. 
## ✍️ Reflections
1. Take a look at the methods (again, methods are like functions) that we added to our project. Try to think of a few reasons why these functions were helpful to us in making this program. Share your thoughts with your team, see what your colleagues thought as well!
1. We're finishing up with Thrown For a Loop. If you have time, it can be instructive to try to rewrite this program in Javascript. Notice where it isn't a 1-to-1 translation.  Do you know of JS features that would make some of this code shorter and more readable? C# has many of those features too, and we will be introducing some more in the next projects. 

Up Next: [Introducing ExtraVert](./extravert-intro.md)


