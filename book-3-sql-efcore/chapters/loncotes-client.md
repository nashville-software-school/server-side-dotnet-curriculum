# Loncotes React Client
The library has asked us to build a new client application. Initially, this will just be for librarian usage. Another developer on the team has already gotten started on the project, and you will pick it up from here. Go to [this](https://github.com/nashville-software-school/dotnet-loncotes-client) repo to get the template code to start the project. Follow the directions in the README to get it set up. The repo uses [`reactstrap`](https://reactstrap.github.io/?path=/story/home-installation--page), a library of Bootstrap-styled React components. If you wish, you can use it to add the new components you are going to build. It is also fine to not use it. Some of the styles from Bootstrap will be applied even if you don't use any `reactstrap` components. Reactstrap (and other component libraries) are worth getting familiar with, but you do not have to do so right away.  

## Exploring the existing repo
Examine the existing codebase and how it is structured. Pay particular attention to the following areas:
1. the `data` directory with the various managers. Try to use these manager modules to keep your `fetch` calls out of your components. 
1. `index.jsx` which holds the router and routes. 
1. the `components` directory, which is further separated into directories for different resources. There is only one, `materials` directory holding components related to that resource. 

If you have any questions about how the repo works, ask an instructor for help. 

## Currently implemented features
The first developer was able complete the following features:
1. display all circulating materials. 
1. create a new material
1. display a material's details

## Features to implement:
1. Add a component to allow the librarians to see all patrons, including their active status.  
1. Add a component to allow the librarians to see the details for a patron, including their late fees, if they have any. 
1. Add a form component to allow librarians to edit the address or email of a patron. 
1. Add a button labeled "Deactivate" to each of the patrons _that are active_ in the patrons list to deactivate a patron. Use `onClick` with a handler to deactivate the user. After the user has been deactivated, the patrons list should be updated, and the deactivated user should not have a button next to their name anymore.  
1. Add a button to each item in the materials list component to remove that material from circulation. Use `onClick` with a handler to make the appropriate request to the API. When the button is clicked, the material should be removed from circulation, and the list of materials should be updated. 
1. Add a menu item to view checkouts, that links to a component to list all of the checkouts. 
1. Add a button to each of the currently checked out items to return the item. Use the click handler for that item to make a request to the correct API endpoint, and update the list to reflect that the item has been returned.
1. Add a menu item to the nav bar called "Browse" that links the user to a list of all of the available (not checked out, and in circulation) materials.
1. Add a button to the material items in the Browse component labeled "Check out". This button should navigate to a form component that allows the user to input a patron's id. Add a submit button to this component which send a request to the API to create a new checkout for that material (HINT: you will need to use a URL param and `useParams` in the form component to know which material the checkout is for). 
1. Add a menu item to the navbar called "Overdue Checkouts" that links to a component that shows all of the overdue checkouts with the patron that has the item. 
 


### Challenges:
1. The librarians want to be able to filter the materials in the front-end application by genre and material type. Add inputs to the materials list to allow a user to do that, and update the function that makes the api request to use the `genreId` and `materialTypeId` query string params. 
1. When a user is deactivated, instead of a `Deactivate` button, there should be a `Reactivate` button in the materials list component. Add an API endpoint to handle the reactivation on the server side, and make a request to that endpoint in the click handler for the `Reactivate` button. When they are reactivated, the user should once again have a `Deactivate` button. 

Up Next: [Advanced Linq](https://github.com/nashville-software-school/bangazon-inc/blob/server-side-curriculum/book-1-orientation/chapters/LINQ_INTRO.md) (this is another practice chapter from a different curriculum. Start on column 3 in this curriculum when you have finished the advanced linq practice)