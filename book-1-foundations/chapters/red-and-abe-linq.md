# Querying Data with Linq

Up until now we have been using loops to find and filter data inside collections like `List`s. Fortunately, like array methods in Javascript, .NET includes a library called Linq that provides similar methods for `List`s as well as other iterable collections. We will go into them more in depth in Book 3, but for now you will find three very useful: `Where`, `Select`, and `First` (along with `FirstOrDefault`). These three have nearly identical functionality to `filter`, `map`, and `find` in Javscript

For the next section we will be using the following data:

```csharp
List<string> toppings = new List<string>()
{
    "Cheese",
    "Mushrooms",
    "Peppers",
    "Onions",
    "Pepperoni",
    "Sausage",
    "Pineapple",
    "Canadian Bacon"
};
```

## `First` and `FirstOrDefault`

`First` works like the array method `find` in javascript. As a reminder, this is a basic find method in JS:

```javascript
const toppingThatStartsWithO = toppings.find((t) => t.startsWith("O"));
```

`find` takes an _anonymous function_ definition as an arg to tell `find` what it is looking for. An anonymous function is a function that is not named or assigned to a variable. That anonymous function needs to return a boolean (or something falsy). `find` will iterate through the `toppings` and call that anonymous function for each topping, passing the topping into that function as `t`. The first time that `t.startsWith("O")` is `true`, it will return that topping. In this case, `toppingThatStartsWithO` would equal "Onions" if we were using the JS version of the data above.

Let's look at the same code, but using Linq in C#:

```csharp
string toppingThatStartsWithO = toppings.First(t => t.StartsWith("O"));
```

It's almost the same! Like `find` `First` is expecting a _lambda expression_ as an arg (another term for an anonymous function). Just like `find`, the anonymous function returns a boolean to let `First` know what condition to search for.

There is one major difference: if `find` doesn't find a matching member of the array to fit the condition, it will return `undefined`. If `First` doesn't find a matching item, it will throw a runtime error. Sometimes this is what you want, but if you want the default value instead of throwing an error, `FirstOrDefault` works more like `find`:

```csharp
string toppingThatStartsWithZ = toppings.First(t => t.StartsWith("Z"));
//Unhandled exception. System.InvalidOperationException: Sequence contains no matching element

string toppingThatStartsWithZ = toppings.FirstOrDefault(t => t.StartsWith("Z"));
// toppingThatStartsWithZ will have a value of `null`
```

## `Where`

`Where` works nearly the same as `filter` in JS. Its lambda expression also returns a boolean, but will instead return all instances that meet the condition. So, for example:

```csharp
List<string> toppingsThatStartWithP = toppings.Where(t => t.StartsWith("P")).ToList();

foreach (string topping in toppingsThatStartWithP)
{
    Console.WriteLine(topping);
}
//output:
// Peppers
// Pepperoni
// Pineapple
```

One difference between `filter` in JS and `Where` in .NET is that `Where` does not, by default, return a `List`, but another type called `IEnumerable`. Don't worry about this other type for now, just use the `ToList` method to turn the result into a `List`.

## `Select`

`Select` is similar to the `map` array method in JS. `map` and `Select` both loop through a collection and return something in place of each object in the collection. The lambda expression should return the new thing that will replace the collection member. Consider this code using `Select`:

```csharp
List<string> screamedToppings = toppings.Select(t => t.ToUpper()).ToList();
foreach (string topping in screamedToppings)
{
    Console.WriteLine(topping);
}
// output:
// CHEESE
// MUSHROOMS
// PEPPERS
// ONIONS
// PEPPERONI
// SAUSAGE
// PINEAPPLE
// CANADIAN BACON
```

The lambda expression returns an all-caps version of each topping name. Again, we need to use the `ToList` method to change what `Select` returns back into a `List`. 

## Refactoring Reductio & Absurdum using Linq Methods
Now that you have been introduced to Linq methods, let's refactor some of our code to use them. You probably needed to write a lot of `for` or `foreach` loops in order get the right data for the different features of the app. 

Let's use a new feature as an example:

1. Add an option for viewing all available products to the menu. 
1. Up until now, we would use a `foreach` loop to loop through all of the products, and push the unsold ones to a new `List`, something like this: 

``` csharp
List<Product> unsoldProducts = new List<Product>();
foreach (Product p in products)
{
    if (!p.Sold)
    {
        unsoldProducts.Add(p);
    }
}
```
Using `Where`, we can filter the `products` more concisely like this:
``` csharp
List<Product> unsoldProducts = products.Where(p => !p.Sold).ToList();
```
Much shorter, and easier to read as well. Notice that the condition in the lambda expression is exactly the same as the condition in the `if` of the first block of code. 

**Here are some places in your app you might consider using a Linq method when using loops**:
1. Anywhere that you are searching for an individual item in products or product types. (HINT: you need to use `FirstOrDefault` and search by `product.Name`, if you are looking for an item with a name that you know.) This could be when updating an item, as an example. 
1. To allow a user to search by product type, you need to show them the list of product types, and then filter the list of products based on the user's input. These are good use cases for `Where` (to filter the products by ProductTypeId) and `Select` (to turn the List of ProductTypes into strings to display them). See if you can refactor the code you have for that feature to use these methods instead of loops to do that work. If you're struggling with how to implement this, ask an instructor to talk through it with you.

**Challenge**: Take a look at Extravert to see if you can use some of these methods in the same way. 

Up Next: [Foundations Coding Assessment](./coding-self-assessment.md)