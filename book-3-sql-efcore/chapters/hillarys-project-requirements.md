# :haircut: Hillary's Hair Care
In this project you will create and implement a plan for building an application for Hillary, the owner of a hair salon. Hillary has hired us to create an application to manage appointments with her stylists at Hillary's Hair Care. As a reminder, learning how to break down projects into manageable units of work and modeling data with ERDs are essential tasks for a professional software developer, so do not skip this opportunity to practice these core professional disciplines.  

## Technical Requirements
1. The app will be built with a ASP.NET Minimal APIs web api, using Postgresql and Entity Framework Core for data storage and access. 
1. The front-end application will be a React client. The code for the client should be in a directory called `client` inside the .NET project directory. See instructions below for creating that client. 
1. The following are the entities you should use in the project:
    - `Stylist`
    - `Customer`
    - `Appointment`
    - `Service`
## Notes from Meeting with Hillary
Hillary would like her application to allow her to schedule appointments between a customer and a stylist. During an appointment, Hillary mentioned that there are a range of services that can be provided, like a haircut, coloring, and beard trims, etc. Customers will often order more than one service per appointment. Appointments happen on the hour, and are scheduled to be an hour long. The customers are charged per service, and Hillary needs to keep track of the total cost for the appointment. 

Customers often cancel appointments, so she will need to be able to make this change in the app. They occasionally change the services that will be provided at the appointment as well, so the ability to make this change in the app is an essential feature. 

Hillary needs the ability to add new customers and stylists to her system. Occasionally stylists move on to other jobs, and she needs to deactivate them so that new appointments are not accidentally made with them. However, she does not want them completely removed from the system, because she needs to keep records of appointments from former stylists even if she no longer employs them.

## Planning 
1. Start by creating some wireframes for this application. Hillary didn't mention how the app should look, or how to organize it and navigate through it. This is up to you. She is hoping you will build a clean, professional interface that is easy to use.

1. Use the notes above to create user stories for the features that you will need to build in this application. Use the format that you were introduced to in DeShawn's Dog Walking in the last book.

1. Create a Github repository for this project:
    - create an empty repo (don't add a README or gitignore)
    - Add all of the user stories as issues to the repo
    - create a GH project board, and add all of the issues to it. 

1. Create an ERD using the entities provided in the technical requirements. Remember that a many-to-many relationship will need a table in addition to the entities listed above. After your first draft of the ERD, make sure that the data model you have created will support the features listed in the user stories. 

## Setting up the codebase
1. Follow the [steps](./creek-river-setup.md) for creating an api in column one (don't forget to create a gitignore file) 
1. Follow the [steps for creating the database](./creek-river-db-context.md) from column one. 
1. run `git init`
1. run `git remote add origin` with the SSH address of the GH repo you created with the issue tickets
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
1. In the `client` folder, you will need to run `npm install --save react-router-dom` to install the router. 
1. At this point, you may want to add, commit, and push this initial code to the remote repository.    

## Starting to Code
Once you have finished planning, and set up the code base, it's time to start implementing features! Order the tickets on your project board so that they are in the order you want to complete them. Move the top ticket into the "In Progress" column, and checkout a new branch from main. Use pull requests to merge features into the main branch, and make sure that the ticket moves to the "Done" column. Then start the process again with a new ticket until you have completed the project. 

> Note: You should not build the entire API, and then build the entire client. Add endpoints to the API as you need them for the feature you are currently working on. 
