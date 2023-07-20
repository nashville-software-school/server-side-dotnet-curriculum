# :potted_plant: Creating Custom Exceptions
In this chapter you will learn how to create custom exceptions to more precisely target issues in your application. 

## Inheriting the Exception class
You can use the Exception class as the base for the new type you're creating. We will cover inheritance more in depth later, but it basically allows you to take all of the members of one class and apply them to another:
``` csharp
public class TooLongException : Exception
{
    public TooLongException(string message) : base(message)
    {

    }
}
``` 
We need to be able to pass in a message when we throw an exception, so we add a _constructor_ to the `TooLongException` type that can pass that message to the constructor of `Exception`. Don't worry too much about constructors yet, we will cover them more in depth later. 

Now we can use that Exception as if it were any other:
```csharp
try
{
    Person person = CreatePerson();
    Console.WriteLine("Person created!");
} 
catch (TooLongException ex)
{
    Console.WriteLine(ex.Message);
}



Person CreatePerson()
{
    Console.Write("Name, please: ");
    var name = Console.ReadLine();
    if (name.Length > 100)
    {
        throw new TooLongException("Name is too long");
    }
    else
    {
        return new Person {Name = name};
    }
}

public class Person
{
    public string Name { get; set; }
}

public class TooLongException : Exception
{
    public TooLongException(string message) : base(message)
    {

    }
}
```
Run this whole program in a console application to see how it works. Try using both a short name, and a too-long one.  

## Adding a `ValidationException` class to ExtraVert

At the bottom of `Program.cs`, add the following class:
``` csharp
public class ValidationException : Exception
{
    public TooLongException(string message) : base(message)
    {

    }
}
```
In the method for posting a plant, try to think of ways in which the user could provide bad input (in addition to the input we handled in the [bug report chapter](./extravert-exceptions.md)). Some of these won't actually throw Exceptions unless you explicitly throw them (for example, the ZIP should not have more than 5 numbers). Throw the `ValidationException` with a message about what's wrong for any of these other validation issues. See if you can figure out where to `catch` them in the application. As always, ask a colleague or an instructor if you get stuck. 
