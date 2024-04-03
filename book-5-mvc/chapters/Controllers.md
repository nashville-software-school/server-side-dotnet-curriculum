# Scaffolding Controllers

## Controller

We can use the dotnet-aspnet-codegenerator to scaffold the skeleton of a controller. Give it the name `WalkersController`

1. 
    ```
        dotnet aspnet-codegenerator controller -name WalkersController -m Walker -outDir Controllers
    ```
    - This command generates a WalkersController in the Controllers directory. The -m Home option specifies the model to be used with this controller. If you don't have a model yet, you can omit this option.

dotnet-aspnet-codegenerator kindly just created a whole bunch of code for us.

When you use the dotnet-aspnet-codegenerator tool to create a new MVC controller, it will also generate all CRUD (Create, Read, Update, Delete) actions in the controller by default. Moreover, it will also generate the associated views for each of these actions. This process is also known as scaffolding.

Specifically, the generated controller will include the following methods:

- Index(): Lists all items.
- Details(int? id): Shows details of a specific item.
- Create(): Renders a form to create a new item.
- Create([Bind] YourModel model): Handles the POST request to create a new item.
- Edit(int? id): Renders a form to edit an existing item.
- Edit(int id, [Bind] YourModel model): Handles the POST request to update an existing item.
- Delete(int? id): Renders a confirmation view for deleting an existing item.
- DeleteConfirmed(int id): Handles the POST request to delete an existing item.

The associated views for these actions will also be created in the Views/YourControllerName directory. The views will be named according to the action methods, for example, Index.cshtml, Details.cshtml, Create.cshtml, Edit.cshtml, and Delete.cshtml.

Remember to replace YourModel and YourControllerName with the actual name of your model and controller respectively.

### A Route Invokes a Controller Method

In the context of ASP<span>.NET</span>, each of the public methods in the controllers is considered an **Action**. When our application receives incoming HTTP requests, The ASP<span>.NET</span> framework is smart enough to know which controller Action to invoke.  

How does it do this? Take a look at the bottom of the `Startup.cs` class

```csharp
endpoints.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

ASP<span>.</span>NET will inspect the parts of the url route. If a request comes in at `localhost:5001/Walkers/Index`, the framework will look for an `Index` action on the `Walker` controller and invoke it. If a request comes in at `localhost:5001/Walkers/Details/5`, The framework will look for a `Details` action in the `Walkers` controller and invoke it while passing in the parameter `5`. You'll also notice in the code above that some defaults have been set up in the routes. If the url of the request does not contain an action, the framework will invoke the Index action by default--meaning `localhost:5001/Walkers` would still trigger the `Index` action in the `Walkers` controller. Likewise, if the url doesn't contain a controller, i.e. `localhost:5001/`, the framework will assume the `Home` controller and the `Index` action. You are of course welcome to change these defaults.