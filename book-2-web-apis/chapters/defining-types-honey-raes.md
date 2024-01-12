# Defining types for `HoneyRaesAPI`
In this chapter we will use classes to define the application-specific types for HoneyRaesAPI, and learn how to use namespaces to better organize our code. 

## Data Models and Namespaces

For the rest of the course starting with this chapter, we are going to keep all of the data type files we create in a folder called `Models`. Go ahead and create that folder in VS Code or the terminal. It should be in the same directory as `Program.cs` and the `csproj` file. 

### Namespaces

Namespaces are another concept in C# that you didn't learn about in your introduction to programming in the front end part of the course. A `namespace`, most simply stated, is a space for names. Names are things like class definition names. Within a `namespace`, those names have to be unique (for example, you can't have two classes both named `AwesomeClass` within the same namespace). If you are familiar with the concept of _scope_ from Javascript, a namespace creates a scope for classes, even across multiple files if those files use the same namespace. 

In Javascript, you got used to different files creating different _modules_ that you could import from one to another. But a C# project is different. In a C# project, the source code files will be treated as if they are all in the same namespace unless you separate the files into different namespaces. In Book 1, because we didn't use namespaces, all of the code in all of the files of our project was in one global namespace. 

You can see how class names are recognized across files in your program like this: 

1. Open `ThrownForALoop` in VS Code
1. At the bottom of `Program.cs`, add this line: `class Product {}`
1. You should see a compiler error that says `The namespace '<global namespace>' already contains a definition for 'Product' [ThrownForALoop]`. 
1. See? The compiler doesn't see `Product.cs` as a different "module" than what's in `Program.cs`. 
1. You can remove the extra code and close `ThrownForALoop`

### Namespace for our Models

1. In the Models folder, create three files: `Customer.cs`, `Employee.cs`, and `ServiceTicket.cs`
1. At the top of each file, add this line `namespace HoneyRaesAPI.Models;`
1. In each file create a `public` `class` that matches the file names underneath the namespace declaration. 
1. Now, to illustrate how this is different from having the files all in the global namespace, go back to `Program.cs`. Add the line `class Customer { }` to the very bottom of the file, underneath the `WeatherForecast` record. See? there is no compiler error because the two `Customer` classes are in different _namespaces_ now. You can remove this last line of code, it was for demonstration only. 

Why `HoneyRaesAPI.Models`? While this isn't enforced by the language, it is very common to have namespaces be nested the same way that the folder structure of the project is nested. Think of `HoneyRaesAPI` as the top-level namespace for the project. Because `Models` is a folder nested inside the project, we give it its own _nested_ namespace, and put all of the classes declared inside it in that namespace. Later in the course we will be adding more nested namespaces to our projects as they get bigger. 

From now on for this course, you won't declare any classes inside `Program.cs`, so you don't need to give it a namespace, it can stay in the global namespace.

This intro is just getting you started with namespaces, there is a lot more to learn. See the extra material below, but don't get hung up on it right now. 

### Finish the type definitions for our data models

Add the appropriate properties for the data models:
1. `Customer` should have `Id`, `Name` and `Address` properties
1. `Employee` should have `Id`, `Name`, and `Specialty` properties
1. `ServiceTicket` should have `Id`, `CustomerId`, `EmployeeId`, `Description`, `Emergency` (this is a boolean), and `DateCompleted` properties. 

## DTOs (Data Transfer Objects)
Now that we have created the entities that represent our database models, we need to create some more classes that represent the way that the data will be presented to the client (as JSON). A common term that is used to describe such classes is DTO, or _data transfer object_. Sometimes these classes will be identical or nearly identical to the database models that we have already made, but their purpose is to allow the way that you present data to the client to evolve independently of the way the data is modeled and stored in the database. 
You can also use DTOs to create things like calculated properties to further modify data from the way that it is stored in the database. 

1. Create a folder inside the `Models` folder called `DTOs`.
1. Create three files in that folder called `CustomerDTO.cs`, `EmployeeDTO.cs`, and `ServiceTicketDTO.cs`.
1. The properties for the classes that will go into these files will be the same as those for their comparable data model class. Here is an example for `EmployeeDTO`:
    ``` csharp
    namespace HoneyRaesAPI.Models.DTOs;

    public class EmployeeDTO
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Specialty { get; set; }
    }
   ```
1. Create the other classes in the same way. Notice that we append `DTO` to the end of the class name to distinguish it from the data model class.

    
Up Next: [Implementing Get All/Get One Service Ticket(s)](./honey-raes-get-tickets.md)

## 🔍 Additional Materials
1. [namespace keyword](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/namespace)
    - Note in this documentation, some namespaces are declared with a pair of `{}`, and the classes in that namespace are inside the curly braces. In older code this was the only way that you could create namespaces. 
    - C# recently added the ability to declare a "file scoped namespace declaration". This is what we did in HoneyRaesAPI, and is handy because it removes a level of curly brace nesting on the left-hand side of the code files. It puts all of the types declared in a file in that namespace, which is what we want!
1. [More about DTOs](https://learn.microsoft.com/en-us/aspnet/web-api/overview/data/using-web-api-with-entity-framework/part-5)