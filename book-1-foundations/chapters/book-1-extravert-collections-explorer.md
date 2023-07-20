# :potted_plant: Dictionaries and Arrays

## Dictionaries
Dictionaries allow for storing multiple key-value pairs in a collection. They do not feature prominently in this course, but they are sometimes very useful data structures. They are the closest thing to a plain-old javascript object you will encounter in the course. The biggest difference is that it is possible to define a type for both the keys and the values.  

Here is an example:

``` csharp
Dictionary<string, int> vehicleInventory =  new Dictionary<string, int> 
{
    {"Ford", 5}, 
    {"Chevy", 6},
    {"Nissan", 10}
};
```
Adding to a Dictionary is fairly straightforward:

``` csharp
vehicleInventory.Add("Honda", 8);
```

Unlike classes (and JS objects), you cannot look up a dictionary item with dot notation:

``` csharp
var fordNumber = vehicleInventory.Ford; // compiler error
```
Instead, you have to use bracket notation:
``` csharp
var fordNumber = vehicleInventory["Ford"]; // fordNumber is now 5
```
This can be unsafe, however. The compiler cannot tell whether the key you are looking up actually exists:
``` csharp
var ferrariNumber = vehicleInventory["Ferrari"]; // KeyNotFoundException!
```
This can be more safely looked up with `TryGetValue`:
```csharp
int fordNumber;
bool fordNumberSuccess = vehicleInventory.TryGetValue("Ford", out fordNumber)
```
`TryGetValue` will attempt to retrieve the value for a given key. If it succeeds, it returns `true` (the value of `fordNumberSuccess`), and will set `fordNumber` to the retrieved value. Otherwise, it will return `false`, which will be the value of `fordNumberSuccess`. 

It is possible to iterate through a Dictionary:
```csharp
foreach (KeyValuePair<string, int> kv in vehicleInventory)
{
    Console.WriteLine($"Brand: {kv.Key}, Number: {kv.Value}");
}
```
Each item in the loop is a `KeyValuePair`, and will have the same types as the Dictionary's key and value types. You can access each with the `Key` and `Value` properties on the key value pair. 

By know you should have noticed that though dictionaries can be useful, most of the cases in which you would have used an object in Javascript, you will instead create a user-defined class, with all of the benefits that come with it (strong types for each property on the class, dot notation accessibility, safe property access). 

Let's implement a feature in Extravert that is a good use case for a Dictionary: 

### Showing Plant Inventory by Species
1. Add a method to `Program.cs` called `InventoryBySpecies` that returns `void`.
1. Create a Dictionary with `string` as the type for keys, and `int` as the type for the values
1. iterate through the plants. Check the dictionary to see if the species is already recorded in the dictionary. If it is already there, increment the value by 1. 
1. If it is not already there, add a new key value pair with the species name and `1` as the value. 
1. After iterating through the products, iterate through the Dictionary and print out a report of the inventory by species.

> NOTE: There are cleaner ways to accomplish this with methods you have not yet learned about (`GroupBy` and `ToDictionary`). If you have extra time, look them up and see how you might do this another way. Otherwise, don't worry, we will cover GroupBy in a later part of the course. 

## Arrays
The Array type is very commonly used in C#. We do not spend a lot of time with it in this course because the `List` type is much more useful for most of the tasks you are asked to complete while a student. However, it is good to be familiar with Arrays, as you will see them quite a lot on the job. 

Arrays are declared like this:
``` csharp
string[] pizzaToppings =
{    "cheese",
    "pepperoni",
    "sausage",
    "mushrooms",
    "onions"
};
```
Arrays are of a fixed length. Now that the above Array has five items, it will always have five items. If you don't use the initializer to put items in the array, you have to specify its size: 
``` csharp
string[] pizzaToppings = new string[5];
```
Setting or getting items from it is like in JS:

``` csharp
pizzaToppings[0] = "cheese";
Console.WriteLine(pizzaToppings[0]);
```
Accessing an item outside of the range will cause an exception:
``` csharp 
Console.WriteLine(pizzaToppings[5]); //IndexOutOfRangeException
pizzaToppings[5] = "green pepper"; // IndexOutOfRangeException
```
Annoyingly, though you'd think the compiler could tell that this code wouldn't work, it is a runtime Exception rather than a compiler error. 
 
You can iterate through an array:
``` csharp
foreach (string topping in pizzaToppings)
{
    Console.WriteLine(topping);
}
```
But if you need to push new items to an array (and therefore change its size), you need to use `Array.Resize`. When you get to this point, the advantages of using a `List` instead become clear. 

However, Arrays are very good for storing data that won't change much. Let's look at another place in Extravert where we could use this data type:

### Choosing a Plant Type
1. Add a property to the `Plant` class called `PlantType`, of type `string`. 
1. In the method for posting a plant, add this array:
    ``` csharp
    string[] plantTypes =
    {
        "tree",
        "bush",
        "flower", 
        "herb"
    };
    ```
1. When the user is creating a plant, they should be presented with a numbered list of the plant types to choose. Iterate through the plant types to print out that list.
1. When the user chooses a number, use that number to access the plant type string that corresponds to it, and set the `Plant` object's `PlantType` property to that string. 
1. Update the app to display plant type along with the other properties. 