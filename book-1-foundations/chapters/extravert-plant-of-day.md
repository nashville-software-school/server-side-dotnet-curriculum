# Plant of the Day
In this chapter you will add a feature to let the user see a random "Plant of the Day" to highlight for users of the app. 

## Getting a Random Number
You can use a `Random` object to generate random numbers for you in a given range. Creating such an object is fairly straightforward:
``` csharp
Random random = new Random();
```
Remember that the first `Random` is declaring the type of the variable named `random`, and we are initializing that variable with a `new` `Random` object

The `Random` type has a method called `Next`. You can use it to get a random number with a certain range, like this: 
``` csharp
int randomInteger = random.Next(1, 11);
```
This will generate a random integer between 1 and 11, _not including_ 11. The second number is _exclusive_, so the highest number we can expect to possibly see as a value of `randomInteger` will be 10. 

## Getting a random plant
1. At the top of the application, after where the database is declared, create a `Random` object and use it to generate a random index of one of the plants in the plant List. As a reminder, you can use `.Count` to get the length of the List, rather than hard-coding a number. 
2. Add an option to the main menu to display the details of the plant at the randomly selected index. Display the plant's species, location, light needs, and price.
3. Test the feature by running the program a few times to ensure that it gets a different plant each time.  

## Only showcase plants that are available
We only want to showcase plants that are still available for adoption. When getting the random index, you need to check to make sure that index is holding an available plant. If not, we need to keep trying random indexes until we get an available plant. 

> 💡 Algorithmic reasoning: <br> Potentially the program will need to try to get a random index many times, but you don't know _how many times_ it will take the program to find a suitable index. When you need to run logic multiple times, but don't know how many times, you should use a `while` loop with a condition that says when you don't need to run the loop anymore. In this case, the loop can stop running when you've found an available plant. 

You can call the `Next` method as many times as you need on the the object stored in `random` to get more random numbers. You can use a variable outside of the loop to store a suitable plant. Break out of the loop when that variable has a value. 

Update your app and then test this feature to see that you get a different, available plant each time you run the app.

Up Next: [Searching for Sunlight](./extravert-search.md)

## 🔍 Additional Materials

[.NET Random class](https://learn.microsoft.com/en-us/dotnet/api/system.random?view=net-8.0)