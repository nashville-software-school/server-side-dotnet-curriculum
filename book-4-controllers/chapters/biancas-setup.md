# :bike: Bianca's Bike Shop
Bianca's Bike Shop has asked us to build an application that Bianca, her mechanics, and bike owners can use to manage bicycle repairs.

## Setup
1. Use [this](https://github.com/nashville-software-school/dotnet-biancas-template) template repository to create your own repo, and clone the code on your machine.
1. In the project directory, run `dotnet restore` to install dependencies.
1. Run this: 
    ``` bash
    dotnet user-secrets init
    ```
1. Run this (replace `<your password>` with your postgres password) to save the connection string to user secrets:
    ``` bash
    dotnet user-secrets set BiancasBikesDbConnectionString "Host=localhost;Port=5432;Username=postgres;Password=<your password>;Database=BiancasBikes"
    ```
1. Run this to save the app's admin user password in user secrets:
    ``` bash
    dotnet user-secrets set AdminPassword password
    ```
1. Create the database migration:
    ``` bash 
    dotnet ef migrations add InitialCreate
    ```
1. Create the database:
    ``` bash
    dotnet ef database update
    ```
1. In the `client` directory inside the project directory, run `npm install`
1. Start the C# debugger
1. In the client folder, run `npm run dev`. 

## Logging In
The React client should show the login view when it starts. Look for the email address of the `IdentityUser` already seeded in the database in the `OnModelCreating` method of the `BiancasBikesDbContext` class. Login with that email address and the password you saved to the user-secrets as `AdminPassword`. (If you just used "password", as it is in the bash command above, use "password"). After logging in, you should see an error (we will fix that in a future chapter).  

Up Next: [Bianca's Bike Tour](./biancas-tour.md)