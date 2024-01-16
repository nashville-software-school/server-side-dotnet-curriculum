# :broom: :soap: House Rules
House Rules is an app to manage household members and their chores. 

## Project Setup
This project does not include template code, so there is some additional setup required that was not covered in Bianca's Bikes. This is another opportunity to see how the apps in this book, using auth and controllers, are different from the apps that we built in books 1-3:

1. Create a new api project with controllers in your `workspace/csharp` directory with this command:
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
1. create a `client` directory inside the project
1. `cd` into that directory, and run `npm create vite@latest . -- --template react` (Don't forget the `.`). 
1. In `Properties/launchSettings.json` in the .NET project folder, change the urls for the API to be `5001` for https and `5000` for http (make sure you are changing this on the `https` and `http` profiles, not the profile labled "IIS" or "IIS Express"). 
1. Replace all of the code in `vite.config.js` in the client folder with this:
    ``` js
    import { defineConfig } from "vite";
    import react from "@vitejs/plugin-react";

    export default defineConfig(() => {
    return {
        server: {
        open: true,
        proxy: {
            "/api": {
            target: "https://localhost:5001",
            changeOrigin: true,
            secure: false,
            },
        },
        },
        build: {
        outDir: "build",
        },
        plugins: [react()],
    };
    });
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
1. In `index.jsx`, replace the `React.StrictMode` tags with `BrowserRouter` tags.
1. Replace the code in `App.jsx` with the code from Bianca's Bikes
1. Copy `ApplicationViews.jsx` from Bianca's Bikes to the House Rules `components` directory. Remove the `bikes`, `workorders`, and `employees` routes. 
1. Create a `NavBar.jsx` component in the `components` directory with the following code:
    ``` javascript
    import { useState } from "react";
    import { NavLink as RRNavLink } from "react-router-dom";
    import {
    Button,
    Collapse,
    Nav,
    NavLink,
    NavItem,
    Navbar,
    NavbarBrand,
    NavbarToggler,
    } from "reactstrap";
    import { logout } from "../managers/authManager";

    export default function NavBar({ loggedInUser, setLoggedInUser }) {
    const [open, setOpen] = useState(false);

    const toggleNavbar = () => setOpen(!open);

    return (
        <div>
        <Navbar color="light" light fixed="true" expand="lg">
            <NavbarBrand className="mr-auto" tag={RRNavLink} to="/">
            🧹🧼House Rules
            </NavbarBrand>
            {loggedInUser ? (
            <>
                <NavbarToggler onClick={toggleNavbar} />
                <Collapse isOpen={open} navbar>
                <Nav navbar></Nav>
                </Collapse>
                <Button
                color="primary"
                onClick={(e) => {
                    e.preventDefault();
                    setOpen(false);
                    logout().then(() => {
                    setLoggedInUser(null);
                    setOpen(false);
                    });
                }}
                >
                Logout
                </Button>
            </>
            ) : (
            <Nav navbar>
                <NavItem>
                <NavLink tag={RRNavLink} to="/login">
                    <Button color="primary">Login</Button>
                </NavLink>
                </NavItem>
            </Nav>
            )}
        </Navbar>
        </div>
    );
    }
    ```
1. In the `HouseRules` directory, install the required dependencies:
    ``` bash
    dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore -v 8.0

    dotnet add package Microsoft.EntityFrameworkCore.Design -v 8.0

    dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL -v 8.0
    ```
1. Copy the contents of `Program.cs` from Bianca's Bikes into the House Rules `Program.cs`. Replace `BiancasBikes` with `HouseRules` everywhere it appears in the app (HINT: there are ways to use your code editor to do this to make sure you don't miss any).
1. Create a `Data` folder. Add a file called `HouseRulesDbContext.cs`. Copy the content from `BiancasBikesDbContext.cs`. Change all references to `BiancasBikes` to `HouseRules`. Remove all of the `DbSet` properties _except for UserProfiles_. In the `OnModelCreating` method, remove all of data seeding for owners, bikes, bike types, and work orders. 
1. Create a `Models` folder, and put a `UserProfile.cs` file in it. Copy the content from Bianca's Bikes' `UserProfile` class. Update the `namespace` to be `HouseRules.Models`. Remove the `WorkOrders` property (this was specifically for Bianca's Bikes). Create a `Dtos` folder and copy `RegistrationDto.cs` and `UserProfileDto.cs` over from Bianca's Bikes as well.
1. Add a controller to the Controllers folder called `AuthController.cs`. Copy the corresponding code from Bianca's Bikes into this file. Change all references to `BiancasBikes` to `HouseRules`. 
1. Initialize user secrets for this project
1. Save the connection string for a database called `HouseRules` to the user secret `HouseRulesDbConnectionString`.
1. Save the password (you can choose it) for the admin user of the app to the `AdminPassword` user secret. 

Up Next: [House Rules Data Model](./house-rules-data-model.md)