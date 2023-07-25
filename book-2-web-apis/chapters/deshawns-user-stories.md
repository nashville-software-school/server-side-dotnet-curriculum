# Requirements for Deshawn's
In this chapter you will use _user stories_ to create an ERD and wireframes for DeShawn's Dog Walking before you start building the project. This is an essential step in this project, _do not_ skip it. In the next book you will also be required to write the user stories. Working with user stories, building ERDs, and creating wireframes (or at least implementing wireframes that you are given) are important skills on the job, and will be required to receive approval for your capstone project. 

## User Stories
1. As a user, I want to view all dogs, so that I can see which dogs are in the system.
    - GIVEN a user is navigating to the site
    - WHEN they visit the home page
    - THEN they should see of all dogs
1. As a user, I want to view a dog's details, so that I can confirm that they are correct and see which walker is signed up to walk it. 
    - GIVEN a user is viewing the dog list
    - WHEN they click on a dog's name
    - THEN the dog detail's view is displayed with their current walker (if any)
1. As a user, I want to add a dog to the system, so that I can sign it up with a walker.
    - GIVEN a user is viewing the Dog list
    - WHEN they click the "Add Dog" Button
    - THEN they are presented with a form to add a new dog to the system
    - GIVEN a user has filled out the new dog form
    - WHEN they click the "Submit" button
    - THEN the dog is added to the system, and the new dog's details are displayed
1. As a user, I want to view walkers by city, so that I can find a walker in my city to walk my dog. 
    - GIVEN the user is on any page on the site
    - WHEN they click the "Walkers" Menu button in the nav bar
    - THEN they are presented with a list of walkers, and an optional dropdown to display them by city
    - GIVEN a user is viewing the Walkers list
    - WHEN they choose a city
    - THEN the application will show the most up to date list of all walkers in the chosen city
1. As a user, I want to choose a walker to walk my pet, so that walker will be assigned to my dog. 
    - GIVEN a user is viewing a list of walkers
    - WHEN they click on the "Add Dog" button next to the walker's name
    -THEN they are presented with a list of dogs which are: in the walker's cities, and not already assigned to this walker. 
    - GIVEN a user is viewing the list of dogs possible for that walker
    - WHEN they click on a dog's name,
    - THEN the walker is assigned to that dog, and the user is presented with the dog's details, including the new walker for that dog.  
1. As a user, I want to add a city, so that I can expand service to new areas
    - GIVEN a user is viewing any page on the site
    - WHEN they click on the "Cities" link in the Nav bar
    - THEN they are presented with a list of the current cities
    -  GIVEN a user is viewing the list of cities
    - WHEN they enter a City name in the "Add a city" input and click "Add"
    - THEN the city is added to the list of cities
1. As a user, I want to manage cities for a walker, so that I can accurately show all the cities that they walk in. 
    - GIVEN a user is viewing a list of walkers
    - WHEN they click on the walker's name
    - THEN they are presented with a form with editable inputs for the walkers' details, including checkboxes with the cities that the walker walks in
    - GIVEN the user has made changes to the form
    - WHEN they click the "Update" button
    - THEN the changes are saved to the database, and the user is redirected to the list of walkers 
1. As a user, I want to delete a dog, so that dogs that are no longer in the system do not clutter the application. 
    - GIVEN a user is viewing the dog list 
    - WHEN they click on the "Remove" button next to the dog's name
    - THEN the dog will be removed from the dog list
1. As a user, I want to delete a walker, so that they are not inadvertently assigned to a dog when they are no longer available.
    - GIVEN a user is viewing the walker list
    - WHEN they click on the "Remove" button next to the "Add Dog" button
    - THEN the walker will be removed from the list of walkers
    - AND any dogs previously assigned to that walker will no longer be assigned to a walker

### Data Requirements
#### Walkers
Have a Name and can walk in many cities. Walkers can walk many dogs
#### Cities
Have a Name, and can have many walkers walking in them, and many dogs living in them
#### Dogs
Have a Name, a City, and possibly a Walker (if one is assigned). Dogs are only walked by one walker, in one city.  

## ERD
Create an ERD for this system. After you have built the ERD, check to make sure that the ERD will support the user stories above. At least one user story above is leaving out a major data change that will result from the operation it is defining. Can you find it?

## Wireframes
Create a wireframe for each of the user stories above. Some of them mention more than one view, and of course many of them share views. You can use whatever wireframing tool you are most comfortable with. 

## Github
1. Create a project using the `Projects` tab in the repo (the button should be called `Link a Project`, use the arrow on the right of the button to choose `New Project`). 
1. You will be prompted to choose a template - choose `Board`, and click `Create`.
1. Rename the project `DeShawn's Dog Walking`. 
1. Use the `Issues` tab in your repo in Github to create issue tickets for each use story. You can paste the user story in the description, but give the issue a good title. Link the wireframes relevant to that ticket in the description. On the right, you can choose to add the issue to a project. Choose the DeShawn's project you just created. 
1. Once an issue is created, the project settings on the right allow choosing which column of the project to put the issue into. Choose `Todo`. 
1. Once all of the issues have been created, go back to the project board. Rearrange the issue tickets in the order that you think you should complete them, first on top. 

## Beginning to code
Complete the issues in the order that you arranged them in the project:
1. Move an issue into the `In Progress` column while you are working on it. 
1. Create a branch for each feature that includes the issue number and a brief description of the feature. For example:
    ``` text
    feature/1/view-all-dogs
    ```
    This helps you keep track of what branch has what code in it. 
1. Try to only write code for one feature at a time. If you find yourself throwing other features in because you can, resist the temptation. You might get away with it in a project this size, but as the projects get larger, this bad habit will make your life harder, not easier. 
1. When you have committed and pushed all the code that you need to fulfill the feature, go back and read the user story again. Have you actually fulfilled the requirements? 
1. If so, open a pull request. You can reference the issue in the PR by adding `closes #1` to the description. This will create a link automatically in the description to the issue, and will close the issue automatically when the PR is merged, in addition to moving the item on the project board to the `Done` column. Neat! 
1. When you're done, start the process over with the next feature!

Up Next: [Many-to-Many in DeShawn's](./deshawns-many-to-many.md)
