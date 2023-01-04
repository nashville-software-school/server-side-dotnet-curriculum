# Lists
Learning Objectives:
1. Declaring and initializing variables with the `List<T>` type
1. Adding items to a List
1. Accessing a List member by index
1. Iterating through a List with `for` and `foreach`

## Declaring variables
Our interaction with the type system is about to get more complicated, so let's review what we've seen so far about declaring variables with different types. 

This is what it looks like to declare `string`, `int` and `bool` type variables:
![type declarations](../../assets/type-declarations-example.png)

We can reassign the values of these variables, but only values that are the same type that the variable was declared to hold. So...
``` csharp
name = "Grace Hopper"; //totally fine
name = null; //totally fine - null is the default value for strings
name = 586; //compiler error. 586 is an int, not a string. 

age = 42; //totally fine
age = null // compiler error! a regular int cannot be null.
age = "ten"; //compiler error - "ten" is a string

ready = true; //totally fine
ready = "true"; //compiler error, "true" is a string, not a bool
ready = null; //compiler error - bools cannot be null.
```
If you haven't already, this is a good time to notice that many types in C# are not nullable by default, that is, `null` is not a valid value for them. 

## Initializing a List
The `List` type in C# is the type you will use where you would have used an `Array` in Javascript. Even though they have different method and property names, they overlap significantly in functionality. 
This is an example of creating a new List:
``` csharp
List<string> names = new List<string>();
``` 
Suddenly, we've departed significantly from JS in syntax, but if we break down this statement it is the same as the other types above,with the same pieces:
![list initialization](../../assets/list-initialization.png)

You've probably noticed that there are two types in the type definition for the `names` variable, `List`, and `string`. The type inside the `<>` angle braces indicates what type all of the items in the List will be. This names list will only have strings in it. 

On the right side of the equals sign we have something that looks very similar to the type, but with the `new` keyword in front of it, and `()` after it. `new` followed by a type creates a new _instance_ of that type. The parentheses after the type name are there because `new` actually calls a special kind of method (called a constructor) to make a new one of the type it's creating. Sometimes we will pass arguments into those parentheses, but more often we won't. The equivalent JS of the above code is:
``` javascript
let names = [];
```
Yes, yes, I know that seems a lot cleaner and easier to read, but our verbose C# will come in handy soon enough. 

### Initialize a List with values
We can also create a new List, and immediately add items to it with the _collection initializer_. This is a pair of curly braces that comes after the parenthenses, like this:
``` csharp
List<int> years = new List<int>()
{
    1985, 
    2022,
    1999,
    1976
};
```
The equivalent JS is:
``` javascript
let years = [
  1985, 
  2022,
  1999,
  1976
];
```



## 🔍 Additional Materials
1. [Default values for C# types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/default-values)