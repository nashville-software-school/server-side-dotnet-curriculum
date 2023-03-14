# ExtraVert :potted_plant:
In this project we will build a console app that allows users to browse, list, and buy secondhand plants. As every book in this curriculum progresses, you will notice that there are fewer and fewer explanations and code examples to show you how to do something. In this column of the book, you will get detailed instructions on what code to write, but few code snippets.

You can always refer back to a previous column if you need help with syntax, but try to look up the documentation for the C# language feature you are trying to use as well. Getting used to reading documentation is a big part of being a software developer. The only real way to get better at it is by doing a lot of it. 

## Setup
Refer to the directions in the [first chapter of column one](./setting-up-console-app.md) for creating this project. You can name it ExtraVert.

## Creating the data
Plants in our app will be represented as instances of the `Plant` class. Follow these steps to do that:
1. Create another file in the project called `Plant.cs`
1. add a class definition in that file for a class called `Plant`. 
1. The `Plant` class should have the following _properties_: `Species`,`LightNeeds` (on a scale of 1-5, 1 being "shade", 5 representing "full sun"), `AskingPrice`, `City`, `ZIP`, and `Sold`. Define these properties with the types you think are appropriate. 
1. create a List in `Program.cs` that will hold all of the plants that the app can display. 
1. Use the _collection initializer_ to add five plants to the list. The List should be stored in the variable `plants`. 

## Testing the initial setup

1. Create a greeting that users will see on startup, and write it to the console. 
1. Loop through your plants, and display a numbered list of the plant names 
1. Run the app and make sure the setup works correctly. 

Up Next: [Adding a Main Menu](./extravert-main-menu.md)