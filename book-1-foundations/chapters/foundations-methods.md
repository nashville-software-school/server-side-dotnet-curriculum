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
    if (choice == "1")
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
    if (choice == "1")
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

