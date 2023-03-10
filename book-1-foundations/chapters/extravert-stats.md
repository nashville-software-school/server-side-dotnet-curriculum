# Statistics for the ExtraVert App

In this chapter we will allow the user to view some statistics about the plants that are listed on the app.

## Feature requirements

1. add an option to the main menu that allows the user to view app statistics.
1. The following statistics should be displayed in the stats view:

| Stats                                  |
| -------------------------------------- |
| Lowest price plant name                |
| Number of Plants Available             |
| Name of plant with highest light needs |
| Average light needs                    |
| Percentage of plants adopted           |

You will need to use loops to collect this data from the plants `List`, and use arithmetic to get the results. For the statistics that require division, see the the section below.

## Working with different number types when doing arithmetic

When doing arithmetic operations on two integers, their result will always also be an integer. This is a little surprising coming from Javascript, because JS only has one actual number type: `number`.

Doing math with integers in C# will sometimes give you surprising results:

```csharp
int half = 1 / 2
Console.WriteLine(half);
//output: 0
int twoAndAHalf = 5 / 2
Console.WriteLine(twoAndAHalf);
//output: 2
```

The result you will get when dividing integers that result in a non-whole number will be rounded down (which is why 1/2 will give you 0). This is called _floor division_.

The good news is that you can _cast_ a value as another type. This is what that syntax looks like:

```csharp
int firstNumber = 1;
double firstNumberAsDouble = (double)firstNumber;
double half = firstNumber / 2;
Console.WriteLine(half);
//result: 0.5
```

Notice that we only had to turn one of the numbers into a `double` for this to work. If you do arithmetic on two numbers of different types, C# will implicitly try to give a result with the more specific type. Also take note that `firstNumber` is still an `int` with a value of `1`. The type system rules are still in place. We just took the _value_ of `1` and converted it to a double, so it gets its own variable `firstNumberAsDouble` that has the `double` type.

You can try to cast any variable as any other type, but you will get a runtime `Exception` if the conversion is not possible.

Up Next: [Handling Unexpected Data](./extravert-exceptions.md)
