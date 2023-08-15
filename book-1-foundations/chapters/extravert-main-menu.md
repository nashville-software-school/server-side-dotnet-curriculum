# Adding a main menu to ExtraVert
By the end of this chapter, your app should be able to:
1. Show a greeting
1. Display a menu of choices for the user to pick
1. Once the user has picked a choice, the main menu should be redisplayed for another choice until the user exits. 

> <small>You might be tempted to try to code this project all at once without testing it at any of the intermediate steps. Though some or most of you may succeed in producing a solution with this method, it is a bad habit. Eventually, the solutions you will need to provide (on the job, as well as at NSS) will be too complex to use that strategy and succeed. This column of exercises models what it is like to break a larger task down into pieces, testing the functionality each step of the way. That is the habit you want to get into! Don't skip steps.</small> 

## Adding a Main Menu with Options
1. You don't need to test that your `List` of `Plant` objects works anymore, so you can take away the code that writes them all to the console. 
2. After the initial greeting, create logic to display a menu with the following options: <br>
    a. Display all plants <br>
    b. Post a plant to be adopted <br>
    c. Adopt a plant <br>
    d. Delist a plant
3. Add logic to perform an action for each option that could be chosen. You can use `if`/`else if`/`else` to accomplish this like we did in Thrown for a Loop, or if you're feeling more adventurous maybe try a `switch` statement ([see documentation](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/selection-statements#the-switch-statement)). For now, you can use `throw new NotImplementedException();` as the logic for each of the options. `NotImplementedException` is an `Exception` type that comes with the .NET libraries included in every .NET project. It is a signal to a developer or tester of a project that a provided feature has not actually been built. This is true for all of the features of our app currently, so you can run that line of could for every option. Here's an example: 
``` csharp
if (choice == "1")
{
    throw new NotImplementedException("Display all plants");
}
```
4. The user should also be presented with an option to exit the program. If they choose it, the program should end. Provide a suitable salutation to let them know the program is ending. 
5. If the user doesn't choose to exit, the main menu should be presented again to allow the user to choose another option. 

> Stop here and test your code. What behavior are you testing for? Look at the requirements and instructions above to come up with a list of things to test. How can you test each of those things? Given that all of the options raise an `Exception` (which exits the program), how can you test the last requirement (Step 5) above?

## Handling Bad User Input

If you thoroughly tested the code that you wrote above, you noticed that we still have some work to do. If the user enters something that isn't an option, the app doesn't give them any indication of that. Add logic to notify the user when they give input that doesn't correspond to one of the options. (add to your `switch` or `if`/`else if`/`else`)

## Cleaning up the Console

You'll notice that the Console is filling up with a wall of input and feedback, which is cluttered and hard to read. Just like in JS, `Console.Clear();` will empty the console before running the next line of code. See if you can use it strategically in the program to make the display less confusing. Sometimes you want the program to wait before it clears the console because the console has information displayed that the user should be able to examine (have you ever had a program ask you to `press any key to continue`?). `Console.ReadKey()` will pause the program and wait for the user to hit any key.  

Up Next: [Implementing the Menu Options](./extravert-implement-menu.md)

