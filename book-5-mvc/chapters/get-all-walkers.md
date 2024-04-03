# Get All Walkers/Single Walker

### Get All Walkers

When a user is on `localhost:5001/Walkers`, we want to show them a view that contains a list of all the walkers in our system. Update the `Index` method to look like the following

```csharp
// GET: Walkers
public ActionResult Index()
{
    List<Walker> walkers = //Use the approprate EF Core method to get all the walkers*

    return View(walkers);
}
```

This code will get all the walkers in the Walker table, convert it to a List and pass it off to the view. 



### Viewing the list of walkers

Go to the `Index` Method of your `WalkersController`, Add in the appropriate EF Core methods where needed to get a collection of all walkers, and pass it off to the view. 
   - Razor Pages can be individually scaffolded by specifying the name of the new page and the template to use. i.e. ` dotnet aspnet-codegenerator view Walker Index List -outDir Views/Walkers`. 
   - The supported templates are:
        - Empty, Create, Edit, Delete, Details, List

   - The generated view creates an html table and iterates over each walker in the list and creates a new row for each one.

### Razor Templates

You'll notice a couple things about the code in the view. For one, it's not in an html file--it's in a cshtml file. This is called a _razor template_. With razor we can write a mix of C# and html code. It's similar to JSX in that it can dynamically create html. Once data gets passed into the view, the razor engine will convert it to an html page that can be returned to the browser. Here's an example of what razor code might look like

```html+razor
<h1>@Model.Name</h1>
```

And here is what the dynamically-created html might look like:

```html
<h1>Mo Silvera<h1>
```

We can also do things in our razor templates like make `if` statements or `foreach` loops to dynamically create html. Notice that any C# code that we want evaluated in the views starts with the `@` symbol. Also notice that the `Model` keyword is a reference to whatever object that the view receives from the controller. Assume in the example below that a controller has just passed the view a `List<Walker>`

```html+razor
<ul>
    @foreach (Walker walker in Model)
    {
        <li>@walker.Name</li>
    }
</ul>
```

Run the application and go to `/walkers/index`. You should see your data driven page.

The view that Visual Studio scaffolded for us is a decent start, but it has a number of flaws with it. For now, lets take care of the image urls. Instead of seeing the actual url, lets replace that with an actual image. Replace the code that say `@Html.DisplayFor(modelItem => item.ImageUrl)` with the following

```html+razor
<img src="@item.ImageUrl" alt="avatar" />
```

Finally, uncomment the the code at the bottom of the view, and instead of using `item.PrimaryKey`, change the code to say `item.Id` on each of the action links.

```html+razor
<td>
    @Html.ActionLink("Edit", "Edit", new { id=item.Id }) |
    @Html.ActionLink("Details", "Details", new { id=item.Id }) |
    @Html.ActionLink("Delete", "Delete", new { id=item.Id })
</td>
```

These action links will generate `<a>` tags at runtime. The first one, for example, is saying that we want an `<a>` tag whose text content says the word "Edit", and we also want it to link to the `Edit` action in the controller. Lastly, it's saying that we want to include whatever `item.Id` is as a route parameter. The generated anchor tag would look something like this

```html
<a href="/Walkers/Edit/5">Edit</a>
```

### Getting A single walker

When our users go to `/walkers/details/3` we want to take them to a page that has the details of the walker with the ID 3. To do this, we need to implement the `Details` action in the `Walkers` controller.

```csharp
// GET: Walkers/Details/5
public ActionResult Details(int id)
{
    Walker walker = //Use the appropriate EF Core method to retrieve a single walker by Id

    if (walker == null)
    {
        return NotFound();
    }

    return View(walker);
}
```

Notice that this method accepts an `id` parameter. When the ASP<span>.NET</span> framework invokes this method for us, it will take whatever value is in the url and pass it to the `Details` method. For example, if the url is `walkers/details/2`, the framework will invoke the Details method and pass in the value `2`. The code looks in the database for a walker with the id of 2. If it finds one, it will return it to the view. If it doesn't the user will be given a 404 Not Found page.


```html
<img class="bg-info" src="@Model.ImageUrl" alt="avatar" />
```

Run the application and go to `/walkers/details/1`. Then go to `/walkers/details/999` to see that we get a 404 response back.