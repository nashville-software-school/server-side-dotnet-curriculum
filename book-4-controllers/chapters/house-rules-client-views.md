# Building Basic Client Views
In this chapter we will create React components that use the endpoints we created in the previous chapters. 

## Home Page
1. Create a component called `Home` that will display a landing page welcoming logged in users. You can design it however you like. 
1. Use the `index` prop on a `Route` inside the `/` `Route`, and make the `element` of that `Route` the `Home` component wrapped in the `AuthorizedRoute` component (to redirect non-authenticated users). 
1. Test the client to see that when you start the app while not authenticated, the app redirects to the `Login` component. After signing in, the user should be redirected to the home page that you just created. 

## UserProfiles
1. Create a component called `UserProfileList` that shows all of the users in the system (you can decide what info about each user profile should be shown). This component should only be viewable by admins. Create a separate route group called `/userprofiles` for this and the other `UserProfile` views. You will need to use the the `roles` prop on the `AuthorizedRoute` component to limit route access to admins. Add a link to the nav bar, conditionally rendered so that only admins will see the link (you will have to check if `loggedInUser`'s roles includes "Admin"). Create a `userProfileManager` module to contain the functions to make fetch calls to the API. 
1. Create a component called `UserProfileDetails` that allows an admin to view a user's profile details (all of the properties in UserProfile except the `Id`), their currently assigned chore names as well as the chores that they have completed (including the completion date).  Add a "Details" link to each of the user profiles in the list component that navigates to the details view for a user profile. Make sure that the route is protected with the `AuthorizedRoute` component, specifying the "Admin" role, so that if a non-admin user navigates there directly from the browser, they will be redirected to the login page. 

## Chores
1. Create a component called `ChoresList` that lists all of the chores in the system. Include the chores' frequencies and difficulties. Add a route group in the `Routes` of the app called `/chores` for all of the views related to chores. Make the `ChoresList` the element of the `index` route of that group. The chores should be viewable by all logged in users, but not logged out users. Each chore should have a delete button that only admins can see that deletes the chore. 
1. Create a component called `ChoreDetails` that shows the details for a chore along with a list of the current assignees and the most recent completion. This component should be viewable only by admins. Add "Details" links to the `ChoresList` items, but conditionally render them based on whether the `loggedInUser` is an Admin (you will have already created the logic to do this for the delete button). 
1. Create a component called `CreateChore` that allows admins to create new chores in the system. The component should contain a form to collect a name, difficulty, and chore frequency. Add a link in the `ChoreList` component to a view using the `CreateChore` component that is only accessible to admins.   

Up Next: [Completing and Assigning Chores](./house-rules-complete-assign.md)