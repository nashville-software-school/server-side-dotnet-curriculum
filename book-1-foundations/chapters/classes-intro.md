# Defining our own Types with Classes
Learning Objectives:

1. Creating abstractions of entities with classes
1. Using `new` to make an instance of a class
1. Identifying and using the _object initializer_ to populate properties of a class. 
1. using classes as data types in a program
1. accessing object properties with dot notation

## Making our data model more complex
Representing our products as simple strings is not terribly helpful. For a used sports equipment sales app, we also want to know things like how much the products cost, whether or not they're still available, etc. 

In Javascript, we would represent each product as an object, like this:
``` csharp
const products = [
    {
        name: "football",
        price: 15,
        sold: false
    },
    {
        name: "hockey stick", 
        price: 12, 
        sold: false
    }
];
```
These objects are easy to write and read, but they don't enforce any types. We could leave off an important property, like `price`, or add an extra property on one object like `brand`. Because JS is dynamically typed, we could even provide the wrong data type for a property, such as providing a `string` value for `price`. 

What we actually want is for all of these product object instances to behave the _same way_ every time, and have the same properties. In C#, we can use the type system to define and enforce the behavior of a `Product` as its own type.

Create another file in the project folder called `Product.cs`
Add this code to that file:
``` csharp
public class Product
{
    public string Name { get; set; }
    public int Price { get; set; }
    public bool Sold { get; set; }
}
```
For now, completely ignore the keyword `public`.  We'll learn more about the importance of this keyword later. For now, always put `public` in front of class definitions and property definitions. Do the same with `{ get; set; }` - add them after every property name for now, but don't worry about what they do.   

What is this `Product` class? Think of it as a template for creating an object. It is not _a_ product, but the blueprint for _creating any product_ in our system. Each one of the lines inside the curly braces defines a _property_ for the `Product` class, so all `Products` in our system will have those three properties (though, of course, they may have different values for them.) Now that we have defined this class is a type in our system, it is available for our use in the program, just like `string`, `int`, and `bool`. Let's do that now!

In `Program.cs`, change the `products` variable to have the type `List<Product>`. This indicates that the `products` variable will hold a `List`, where every item in that `List` is a `Product`. Make the same change after the `new` keyword to create a new `List` that will hold `Product`s. It should look like this:
```csharp
List<Product> products = new List<Product>()
{
};
```
You'll have to remove the strings that were inside this collection before, because they are no longer the correct type to go in the `List`. What goes in there instead? We need to make some `Product`s!  Just like we used the `new` keyword to create a new `List`, we can use it to create a new `Product`.  Here are two:
```csharp
List<Product> products = new List<Product>()
{
    new Product()
    { 
        Name = "Football", 
        Price = 15, 
        Sold = false
    },
    new Product() 
    { 
        Name = "Hockey Stick", 
        Price = 12, 
        Sold = false
    }
};
```
The curly braces below `new Product()` are called the _object initializer_. Like the _collection initializer_ for `List`, the object initializer allows us to set some values in the object right away when we create it. This code is equivalent to the JS above. **Add four more products to the list to get practice with the syntax.** I recommend not copy-pasting to create them. Notice that the different products are comma-separated, just like the strings were before. Each one of these product objects is an _instance_ of the `Product` class.

We have a lot of compiler errors in our program now, because the rest of the program is expecting each item in the `List` to be a `string`.  Let's fix that:

```csharp
Console.WriteLine($"{i + 1}. {products[i].Name}");
```
In the line above, we are adding `.Name` to `products[i]`.  `products[i]`, instead of being a string as it was before, is now an _instance_ of `Product`. Just like a Javascript object, we can access properties of that product object with dot notation. This will be even more helpful when the user chooses a product to view. Replace the last line of the program with the following:

```csharp
Product chosenProduct = products[response - 1];
Console.WriteLine($"You chose: {chosenProduct.Name}, which costs {chosenProduct.Price} dollars and is {(chosenProduct.Sold ? "" : "not ")}sold.");
```
The first line in this new snippet looks up the chosen product by index as before, but notice the type of the variable holding that product is `Product` rather than `string`. This means that we can reference `chosenProduct` to get the property values of that object in the second line (`Name`, `Price`, and `Sold`).  The logic inside the last set of curly braces is a ternary, which you may have encountered in JS already. The rules for ternaries are different in C# than in JS, but the syntax for each is the same. 

> You are probably used to being able to print out the entire object in JS to the console like this: `Console.WriteLine(chosenProduct)`. If you try this in your C# program (and I recommend you do!), you will see that you don't get the same behavior. We will cover how to modify this later.

We should be able to run our program now. Try it and see what it does!

## ✍️ Reflections
1. In this chapter we introduced the idea of a class, and a class _instance_. This can be a hard concept to grasp at first, so it can be helpful to think of metaphors for what it means. We have already mentioned a class as a _blueprint_ or a _template_ for making an object. Another important word we mentioned is _model_ (as in, data model).  This can also be instructive in thinking about what a class is. For example, if I own a particular model of car, like a Honda Accord, many other people also own the same model of car. Each individual car is an _instance_ of the Honda Accord model. We could express creating an Accord for Jim in code like this:
```csharp
HondaAccord jimsAccord = new HondaAccord()
{
    Year = 2012,
    Owner = "Jim"
};
```
The variable `jimsAccord` now holds an object of type `Accord`, and its `Year` and `Owner` properties are set to the specific values for Jim's Accord. 

Are there other metaphors you can think of besides `blueprint`, `model` or `template` that you can come up with to help create a mental model of what a class is? Share them with your colleagues to see if they make sense to them as well, and discuss with an instructor when you meet with one.

2. You may have noticed that this class `Product` looks a lot like an _entity_ in the ERDs that you are already familiar with. That is exactly what it is! Most of the entities in your projects will get their own data model, like `Product`, to describe the type of that data for the C# type system. Practice with class properties by adding another property to our `Product` class. Get creative! You can choose what the property represents and what its data type is. Then, set the value of that property for each of the _instances_ of `Product` in your `products` list. Finally, at the end of the program where we print out the product details, add that new property to the string that describes the product. 

Up Next: [Working with DateTime](./foundations-datetime.md)

