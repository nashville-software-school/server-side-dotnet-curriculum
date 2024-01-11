# Conditionals and While Loops
Learning Objectives:
1. `if`/`else`, `while`, (`for` and `foreach` will be covered later)
1. `false` in C# vs. 'falsy' in JS
1. string methods: `IsNullOrWhiteSpace`, `IsNullOrEmpty`, `Trim`

### Checking User Input

We want to make sure that our user actually enters input, rather than pressing `Enter` without actually typing anything. If we want to make our program behave differently depending on a condition, we can use `if` and `else` to describe what it should do in each case. The `string` type also has a method called `IsNullOrEmpty` that will check if a string is null or empty. Let's update our program to use those:

```csharp
string greeting = @"Welcome to Thrown For a Loop
Your one-stop shop for used sporting equipment";

Console.WriteLine(greeting);

Console.WriteLine("Please enter a product name: ");

string response = Console.ReadLine();

if (string.IsNullOrEmpty(response))
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

The first thing to notice about the use of `if` here is that it's mostly the same as its use in JS. The code inside the first set of curly braces runs when `string.IsNullOrEmpty(response)` returns `true`. Otherwise, the code in the second set of curly braces runs. This is also a good moment to notice the indentation used in this `if`/`else`, because you will see it a lot in C#. It is the preferred style for this course.

What's different? Well, it is instructive to look at the equivalent idiomatic Javascript:
``` javascript
if (!response) {
    console.log("You didn't choose anything!");
} else {
    console.log(`You chose: ${response}`);
}
```
In Javascript, it is possible to put expressions that evaluate to "falsy" values as conditions for `if`, ternaries, `for` and `while` loops. Values like `null`, `undefined`, `0`, and importantly here, `""`, are treated as `false` for the sake of evaluating the expression.  _This is not true in C#_. 

In C#, the _expression_ inside the parentheses after `if` **must evaluate to `true` or `false`**.  This is one of the reasons why the `IsNullOrEmpty` method is helpful to us, because it _returns_ a `boolean`. The `string` type has many other useful methods, some of which it shares with the string type in JS, but many others which are unique. Check them out in the documentation [here](https://learn.microsoft.com/en-us/dotnet/api/system.string?view=net-8.0#methods). 

### Let the User Try Again
Let's change our program so that if the user doesn't provide input, the program asks for valid input again. To accomplish this, we can use a `while` loop. Just like `if`, while requires an actual `boolean` (`true` or `false`) inside the parentheses. We can replace our `if` with the loop:
``` csharp
while (string.IsNullOrEmpty(response))
{
    Console.WriteLine("You didn't choose anything, try again!");
    response = Console.ReadLine();
}

Console.WriteLine($"You chose: {response}");
```
There are two things to notice here. The first is that we can reassign `response`, similar to a variable declared with `let` in Javascript. For the second, let's run the program a few more times. 
1. The first time, hit `Enter` without entering any text, and ensure that our program asks us for input again. Then, enter any text and hit `Enter`. 
1. The second time, type three spaces and press `Enter`. What happened?

It looks like we might not have handled the edge case of a user entering only spaces. There are a few ways we can handle this problem, and we will explore two now. 

The first is to use `IsNullOrWhiteSpace` instead of `IsNullOrEmpty` like this: 
```
while (string.IsNullOrWhiteSpace(response))
...
```

This would work well, because all-whitespace answers would fail the test. However, we might be better off using `Trim` to remove any whitespace from our response first, because we probably don't want those extra spaces in valid responses either. So we can do this:
``` csharp
string response = Console.ReadLine().Trim();

while (string.IsNullOrEmpty(response))
{
    Console.WriteLine("You didn't choose anything, try again!");
    response = Console.ReadLine().Trim();
}

Console.WriteLine($"You chose: {response}");
```
This will turn any whitespace-only answers into empty answers, and will also trim any valid answers with whitespace on the ends. This is a good opportunity to notice that because `ReadLine` returns a `string`, we can chain `string` methods onto the call to `ReadLine`. 

Test the program again to make sure that it works as expected. What cases do you need to test to know it does what we intend?

Up Next: [Working with Integers](./working-with-integers.md)

## 🔍 Additional Materials
1. [Microsoft string documentation](https://learn.microsoft.com/en-us/dotnet/api/system.string?view=net-8.0)
1. [Falsy in Javascript](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)
1. [Edge Cases](https://en.wikipedia.org/wiki/Edge_case)