# User Interaction in the Console

Learning Objectives:
1. `Console` methods: `WriteLine` and `ReadLine`
1. declaring variables with strong types
1. string interpolation
### Hello, World!
Take a look at the code that was created by the template:

``` csharp
Console.WriteLine("Hello World!");
```
When we ran this code in the last chapter, the program printed "Hello World!" to the terminal and ended the program (console and terminal mean the same thing for these purposes). You've interacted with the console in your browser when you ran JS code in it, and this is the same, but your terminal is the console for this program, because the program is running in your terminal, not in the browser. 

1. Change the program so that it prints "Welcome to Thrown for a Loop!" to the terminal instead of "Hello World!". 
1. Go back to your terminal and run `dotnet run`.  You should run this command from whatever directory the `ThrownForALoop.csproj` file is in (just like NPM commands need to be issued from whatever directory `package.json` is in)
1. You should have to wait a short time before the program runs, because the code is getting _recompiled_ before it runs. This happens when we make a change since the last time we ran the program. If you don't make changes between runs, the program starts right away with the built code that was created the last time it ran. 
1. Use another `Console.WriteLine` to write a second line to the terminal that says "Please choose an option: ", and run the program again. 

> You must put a semicolon at the end of every statement when writing C#. It does not mean there will be one at the end of every line, rather whenever you have a "complete thought", like a period for a sentence. 

### Declaring and initializing variables
Because JS is a _dynamically_ typed language, one variable can hold many data type values. Therefore, this code is perfectly valid:
``` javascript
let theMeaningOfLife = 42
theMeaningOfLife = 'forty-two'
console.log(theMeaningOfLife)
```
`theMeaningOfLife` is _declared_ with the `let` keyword, and then _initialized_ with a value of `42`. This code says "create a changeable variable called `theMeaningOfLife` and give it an initial value of `42`. Then, change the value of `theMeaningOfLife` to `'forty-two'`, and print that to the console." Javascript does not care that the first value of `theMeaningOfLife` was a number, but was changed to be a string. 
<br>

Let's look at some similar C# code:
``` csharp
string theMeaningOfLife = "forty-two";
Console.WriteLine(theMeaningOfLife);
```
What's different? Instead of using `let` or `const`, we declare a variable with a type - in this case, `string`. This tells the compiler that `theMeaningOfLife` will only ever hold `string` values, or `null` (the default value for a string - more about default values later). As a result, we cannot reassign `theMeaningOfLife` with a value of `42`, because `42` is an integer. This is what it means for C# to be strongly typed. Variables are assigned a type when they are declared, and that type does not change. Also note that strings in C# must be inside double-quotes (`"`).

Add a line at the top of `Program.cs` that stores the program greeting in a variable. Then, _reference_ that variable in the call to `Console.WriteLine`:
```csharp
string greeting = "Welcome to Thrown For a Loop!";
Console.WriteLine(greeting);
```

Note that when we reference `greeting` in the second line above, we don't put the type in front of the variable name because we are referencing a variable that we already declared above. The compiler already knows the type for `greeting`. 

### Making our program interactive
If we want to make a string multi-line, we can prepend the string with an `@`. This is called a _verbatim string_, but its other features are not important right now.

Let's make our greeting a bit more descriptive:
```csharp
string greeting = @"Welcome to Thrown For a Loop
Your one-stop shop for used sporting equipment";
Console.WriteLine(greeting);
Console.WriteLine("Please enter a product name: ");
```
We've asked the user to provide input, but we haven't collected any yet. To do that, we can use `Console.ReadLine`. Add this line at the bottom of `Program.cs`:

```csharp
string response = Console.ReadLine();
```
When `ReadLine` is called, the program will wait for the user to type text and press `Enter`. When the user presses enter, the `ReadLine` method (which is a kind of function) will _return_ whatever text the user typed (the data type of this content will be, unsurprisingly, a `string`). This means that the program will be "paused" on this line until the use presses `Enter`, and when they do, `response` will hold the text response that the user typed. 

Let's let the user know what they just gave as input. We can do _string interpolation_ in C#, but the syntax is slightly different than JS. Add another line:

```csharp
Console.WriteLine($"You chose: {response}");
```
Run the program to see what it does! You'll notice that the program stops and waits for your input. You can type whatever you want at that point and press `Enter`, and after that the program continues until it is done. 

It is possible to use the `@` and `$` symbols together to make a multi-line interpolated string, like this:

```csharp
Console.WriteLine(@$"You chose: {response}. 
Thank you for your input!");
```

Try that as well, and see how it works by running the program.  

## ✍️ Reflections
1. A number of terms related to variables were reviewed here, included _declare_, _initialize_, and _reference_. What does it mean to declare a variable? What does it mean to initialize it with a value? What does it mean to make a reference to a variable? Write down some answers to these questions. If you're unsure of your understanding, ask a teammate or an instructor. These are key concepts for your progress as a software developer. 
1. `Console.WriteLine` is fairly straightforward to compare to `console.log` in JS, but `Console.ReadLine` is quite different. In introducing this method, we reviewed the idea of a method (a special kind of function) _returning_ a value. What does it mean for a function to return a value, and how is that working in the line  `string response = Console.ReadLine();`? Again, ask a teammate or instructor if you're unsure of your understanding. 

Up Next: [Control Flow and more on strings](./control-flow.md)
