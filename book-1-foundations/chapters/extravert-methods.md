# Method Parameters

Up until now we have only written methods that don't require any parameters. Fortunately, using parameters in C# is very similar to how you use them in JS, with one major difference. Because the parameters are variables, we need to define them with types, just like we would for any other C# variable.

So a JS function like this:

```javascript
const addingMachine = (firstNumber, secondNumber) => {
  return firstNumber + secondNumber;
};
```

would be expressed as a method in C# like this:

```csharp
int AddingMachine(int firstNumber, int secondNumber)
{
    return firstNumber + secondNumber;
}
```

`firstNumber` and `secondNumber` have to be defined with types when you _define the method only_, not when you pass in arguments. So if we were to call this method, that would look like this:

```csharp
int result = AddingMachine(4, 5);
Console.WriteLine(result);
//result: 9
```

You will get a compiler error if you try to pass an arg value in that doesn't match the type in the definition:

```csharp
int result = AddingMachine("four", 5);
// compiler error: cannot convert from 'string' to 'int'
```

Notice that our `AddingMachine` method also _returns_ a value, which is also an `int`. Therefore, instead of `void` in front of the method name, the method has a _return type_ of `int`. Functions have inputs and outputs. Because C# is strongly typed, we can tell the compiler what types to expect for the inputs (the parameters), and what type we can expect the method to output (its return type). All of the information is conveniently available to you right in the method definition (you don't have to search the method for the return keyword to see what the function returns, like you would in Javascript).
So:

```csharp
string result = AddingMachine(4, 5);
//compiler error!
```
and: 
```csharp
int AddingMachine(int firstNumber, int secondNumber)
{
    int result = firstNumber + secondNumber;
    return result.ToString();
    //compiler error!
}
```

`AddingMachine` returns an `int`, and the compiler knows that because the method was defined with `int` as the return type, so we can't save the result in a variable of type `string`. We also can't use `return` in the body of the method to return anything other than an `int`.

Ok, let's put this into practice in ExtraVert!

## A method to format a plant object string

There are a lot of places in the app where we have to print out the details of a plant. Let's create a method to do that for us! It will take a `Plant` object as input, and return a formatted string with the plant's details. Here's some starter code:

```csharp
string PlantDetails(Plant plant)
{
    string plantString = plant.Species;

    return plantString;

}
```

> There is only one parameter in the `PlantDetails` method, `plant`. `Plant` in front of that parameter name is the parameter type. It looks confusing because it's repetitive, but that is only one parameter, not two. This is confusing for beginners, so it may take time for it to look normal. Notice our method returns a `string`, so the type in front of the method name is `string` as well.

Instructions:

1. Add this method to the bottom of the program file.
1. Refactor it so that it returns a string in the same format you are already using in the app. Notice that `plant` has the type of `Plant`, so you can use dot notation to access any of the plant's properties you might need.
1. Use this method everywhere in your app where you were using this format before. Now your code is DRY-er (Don't Repeat Yourself), because you created a method to remove duplicated logic.

Up Next: [Reductio & Absurdum](./red-and-abe-requirements.md)
