# Completing and Assigning Chores
All logged in users should be able to indicate that they have completed a chore, and admin users should be able to assign and unassign chores. In this chapter we will add those features to HouseRules

## Complete a Chore
In the `ChoresList` component, add a button to each chore labeled `Complete`. Add an event listener to this button that creates a new `ChoreCompletion` for that chore. The user should be able to complete a chore even if they aren't assigned to it. Get the `userId` for the completion from `loggedInUser`. Make sure to add a function to the `choreManager` module rather than making the fetch call directly in your component!

## Assign and Unassign Chores
Let's enhance the Chore Details page so that an admin can assign a chore to different users.

### Feature requirement
1. The Chore Details page no longer needs to display a list of currently assigned users
1. Instead, the component should display a list of the names of all the users with checkboxes to the left of their names
1. If a user is currently assigned to the chore, that user's checkbox should be checked when the component displays.
1. When an admin user clicks on a checkbox:
    - If the box is getting checked, the database should be updated to assign the chore to the user
    - If the box is getting unchecked, the database should updated so that the user is not assigned to the chore. 
1. In either case, after the response to the assignment request comes back from the API, the component should re-fetch the chore to update the current assignments for that chore. 

### Implementing the Feature
Some helpful tips for implementing this feature:
1. The state of the `ChoreDetails` component will need to include all users in addition to the chore data. 
1. Use the `checked` prop on the checkbox `input` elements to set the checked state for each one. The checkbox should be checked if the corresponding user's id matches the `userProfileId` property of any of the chore's `choreAssignments`. 
1. You can store the user id for each checkbox in the `value` prop of the input so that you can retrieve it later in the handler. 
1. Create a handler function for the checkbox elements that, `onChange`:
    - Checks to see if the box is getting checked or unchecked
    - Gets the user id associated with that checkbox, and then either:
        - Assigns the chore to that user or
        - unassigns that chore
    - Finally, re-fetch the chore with its assignments from the database to update the state. 

Up Next: [Server-Side Data Validation](./house-rules-data-annotations.md)