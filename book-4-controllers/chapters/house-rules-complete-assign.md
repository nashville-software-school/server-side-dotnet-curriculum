# Completing and Assigning Chores
All logged in users should be able to indicate that they have completed a chore, and admin users should be able to assign unassign chores. In this chapter we will add those features to HouseRules

## Complete a Chore
In the `ChoresList` component, add a button to each chore labeled `Complete`. Add an event listener to this button that creates a new `ChoreCompletion` for that chore. The user should be able to complete a chore even if they aren't assigned to it. Get the `userId` for the completion from `loggedInUser`. Make sure to add a function to the `choreManager` module rather than making the fetch call directly in your component!

## Assign and Unassign Chores
