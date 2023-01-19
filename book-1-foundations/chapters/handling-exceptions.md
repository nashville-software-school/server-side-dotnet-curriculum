# Handling Exceptions
In this chapter we will handle a runtime error that is thrown when a user gives input that can't be used by the program.

Learning Objectives:
1. The difference between compiler errors and runtime Exceptions
1. using `try`/`catch` to handle potential Exceptions
1. capturing the Exception value with a variable (`Exception ex`)

## Compiler errors vs. runtime Exceptions
Chances are you have already encountered both compiler errors and runtime exceptions since you have started this part of the course. Practically, you can easily tell the difference between them: compiler errors will show up highlighted in your editor with squiggles under them. There is a program called OmniSharp that is running in the background, and using the C# compiler to check your code for these errors (it's part of the C# extension you installed for VS Code). You can also see a list of compiler errors in the terminal if you run `dotnet build` or `dotnet run` and your program has errors in it.  These are usually syntax or type errors, like missing semicolons, or assigning a `string` as the value of an `int` variable. 
![Compiler Errors](../../assets/compiler-errors.png)

Runtime exceptions occur when a problem happens with the code that isn't obvious from the syntax of the code. For applications with users and user input, this often happens because some unanticipated input is provided by the user, and it causes the code to not be able to move forward. The classic example is dividing by zero. Let's say we have a calculator program to divide two numbers. The divide program looks like this:
```csharp
Console.WriteLine("Please input divisor:");
int divisor = int.Parse(Console.ReadLine()); 
Console.WriteLine("Please input dividend:");
int dividend = int.Parse(Console.ReadLine());
Console.WriteLine($"{dividend} / {divisor} = {dividend /  divisor}");
```

This program will work fine unless the user provides `0` for the input. Because the compiler can't anticipate this from the syntax, the code is perfectly valid. But, given that input from the user,  the program will _throw_ a runtime exception that looks like this in the terminal:
```
Unhandled exception. System.DivideByZeroException: Attempted to divide by zero.
   at Program.<Main>$(String[] args) in C:\Users\jimbob\workspace\csharp\Divider\Program.cs:line 5
```
Let's break this message down:
1. The first part of this message says `Unhandled exception`. It says _unhandled_ because the code has not anticipated the situation.  The big problem with an exception being unhandled is that it causes the program to stop running. That's not such a problem for this little program, but imagine an app that many users were using on the web. Depending on how it is configured, it might suddenly stop working for everyone!
1. The second part `System.DivideByZeroException` gives the specific type of exception that was thrown. Yes, even the errors have types in C#!  Knowing what type of error was thrown is often helpful in googling a solution, but as we'll see below, also very helpful in deciding what to do when the error happens. 
1. after the colon, we have `Attempted to divide by zero.`  This is a human-readable message that explains what the most immediate concern is. In this case, it is pretty obvious what the problem is. 
1. Finally, starting with the word `at`, we have the _stacktrace_. This is very helpful, because as you can see at the end of the line, the trace tells us what line threw the error, and in which file. When you start using the debugger, you might put a breakpoint here to see what the program's state was right before the error is thrown. IMPORTANT NOTE: In more complicated programs, the stacktrace will have more lines in it than this. Most of the time, the top line of the trace (and the file name and line number in it) are going to be the most (or only) relevant information for solving your problem. 

> It is very common for beginners to be bewildered by exception messages. That's ok! They can look intimidating. However, try to break each exception message you see down to these four pieces and mine them for data to help solve your problem. Eventually, exception messages will be your best friend in solving problems with your code. 