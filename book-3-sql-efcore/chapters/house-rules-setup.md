# :broom: :soap: House Rules
House Rules is an app to manage household members and their chores. 

## Project Setup
This project does not include template code, so there is some additional setup required that was not covered in Bianca's Bikes:

1. create a new api project with controllers in your `workspace/csharp` directory with this command:
    ``` bash 
    dotnet new webapi -o HouseRules
    ```
    - Note that `-minimal` is not included so that there will be controllers
1. `cd` into the `HouseRules` and run:
    ``` bash 
    dotnet new gitignore
    ```
1. Initialize a git repo:
    ``` bash
    git init
    ```
1. Create the client:
    ``` bash
    mkdir client && cd $_
    npx create-react-app .
    ```
1. Install `react-router-dom`:
    ``` bash
    npm install --save react-router-dom
    ```
1. Install `bootstrap` and `reactstrap`:
    ``` bash
    npm install --save bootstrap reactstrap
    ```
1. Create `components` and `managers` folders in `src`
1. In the `managers` folder, create an `authManager` and copy all of the contents into it from the `authManager` in Bianca's Bikes. 
1. Copy the entire `auth` folder from Bianca's bikes into the `components` folder. 
1. In the `HouseRules` directory, install the required dependencies:
    ``` bash
    dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore -v 6.0

    dotnet add package Microsoft.EntityFrameworkCore.Design -v 6.0

    dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL -v 6.0
    ```
1. Copy the contents of `Program.cs` from Bianca's Bikes into the House Rules `Program.cs`. Replace `BiancasBikes` with `HouseRules` everywhere it appears in the app (HINT: there are ways to use your code editor to do this to make sure you don't miss any).
1. Create a `Data` folder. Add a file called `HouseRulesDbContext.cs`. Copy the content from `BiancasBikesDbContext.cs`. Change all references to `BiancasBikes` to `HouseRules`. Remove all of the `DbSet` properties _except for UserProfiles_. In the `OnModelCreating` method, remove all of data seeding for owners, bikes, bike types, and work orders. 
1. Create a `Models` folder, and put a `UserProfile.cs` file in it. Copy the content from Bianca's Bikes' `UserProfile` class. Update the `namespace` to be `HouseRules.Models`. Remove the `WorkOrders` property (this was specifically for Bianca's Bikes).  