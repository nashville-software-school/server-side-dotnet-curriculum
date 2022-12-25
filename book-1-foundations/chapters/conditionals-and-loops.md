# Conditionals and While Loops
Learning Objectives:
1. `if`/`else`, `while`, (`for` and `foreach` will be covered later)
1. `false` in C# vs. 'falsy` in JS
1. string methods: `IsNullOrWhiteSpace`, `IsNullOrEmpty`

### Checking User Input

We want to make sure that our user actually enters input, rather than pressing `Enter` without actually typing anything. If we want to make our program behave differently depending on a condition, we can use `if` and `else` to describe what it should do in each case. The `string` type also has a method called `IsNullOrEmpty` that will check if a string null or empty. Let's update our program to use those:

```csharp
string greeting = @"Welcome to Thrown For a Loop
Your one-stop shop for used sporting equipment";

Console.WriteLine(greeting);

Console.WriteLine("Please enter a product name: ");

string response = Console.ReadLine();

if (string.IsNullOrWhiteSpace(response))
{
    Console.WriteLine("You didn't choose anything!");
}
else
{
    Console.WriteLine($"You chose: {response}");
}
```
Run the updated program, and test all of the cases we have accounted for:
1. The user enters some text and presses `Enter`
1. The user presses `Enter` without entering text

The first thing to notice about the use of `if` here is that it's mostly the same as its use in JS. The code inside the first set of curly braces runs when `string.IsNullOrEmpty(response)` returns `true`. Otherwise, the code in the second set of curly braces runs. This is also good point to notice the identation used in this `if`/`else`, because you will see it a lot in C#. It is the preferred style for this course.

What's different? Well, it is instructive to look at the equivalent idiomatic Javascript:
``` javascript
if (!response)
{
    console.log("You didn't choose anything!");
}
else
{
    console.log(`You chose: ${response}`);
}
```
In Javascript, it is possible to put expressions that evaluate to "falsy" values as conditions for `if`, ternaries, `for` and `while` loops. Values like `null`, `undefined`, `0`, and importantly here, `""`, are treated as `false` for the sake of evaluating the expression.  _This is not true in C#_. 

In C#, the _expression_ inside the parentheses after `if` **must evaluate to `true` or `false`**.  This is one of the reasons why the `IsNullOrEmpty` method is helpful to us, because it _returns_ a `boolean`. the `string` type has many other useful methods, some of which it shares with the string type in JS, but many others which are unique. Check them out in the documentation [here](https://learn.microsoft.com/en-us/dotnet/api/system.string?view=net-6.0#methods). 