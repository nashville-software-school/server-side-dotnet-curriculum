# Creating Controllers
In this chapter we will create the controllers that require basic CRUD operations for Chores and UserProfiles.

It might be helpful to use the app to create a few more users (write down their passwords or use the admin password!) to test the endpoints created in this chapter. 

## `UserProfileController`

Add a file to the Controllers directory called `UserProfileController.cs`. You can use the `UserProfileController` from Bianca's Bikes to set up the class. You should only need to change the namespace reference and the name of the db context class. 

### Additional Endpoint for `UserProfileController`
1. `GET` `/api/userprofile/{id}`
    - This endpoint will get a UserProfile along with its assigned chores (through the `ChoreAssignment` table),as well as the user's completed chores (through the `CompletedChore` table). 
    - It should be accessible to any logged in user, regardless of role. That means using the `Authorize` attribute without specifying a role. 
    - Use `Include` and `ThenInclude` for `ChoreAssignment` and for `CompletedChore` to include the `Chore` data in both cases. 
    - As a reminder, you can test the endpoint in postman without logging in by commenting out the `Authorize` attribute. 
    - If you haven't already, you will need to add collections to the `UserProfile` to store that related data. 

## `ChoreController`
Add a `ChoreController` to the Controllers directory. It will need the `HouseRulesDbContext` as a dependency, so create a private field to hold an instance of it, and use the constructor to set its value (this is the same way it is done in the `UserProfileController`).

### Endpoints
The following endpoints should be accessible to all logged in users:
1. `GET` `/api/chore`
    - This endpoint will return all chores
1. `GET` `/api/chore/{id}`
    - This endpoint will return a chore with the current assignees and all completions (you do not need to include each UserProfile that did the completion)
1. `POST` `/api/chore/{id}/complete`
    - This endpoint will create a new `ChoreCompletion`.
    - Use a query string parameter to indicate the `userId` that will be assigned to the chore matching the id in the URL.
    - Set the `CompletedOn` property in the controller method so that the client doesn't have to pass it in.
    - This endpoint can return a `204 No Content` response once it has created the completion. 

The following endpoints should be accessible to admin users only:
1. `POST` `/api/chore`
    - Post a new chore to be created
1. `PUT` `/api/chore/{id}`
    - This endpoint should allow updating all of the columns of the `Chore` table (except `Id`)
1. `DELETE` `/api/chore/{id}`
    - This endpoint will delete a chore with the matching id
1. `POST` `/api/chore/{id}/assign`
    - This endpoint will assign a chore to a user. 
    - Pass the `userId` in as a query string param, as in the completion endpoint above.
    - This endpoint can return a 204 response. 
1. `POST` `/api/chore/{id}/unassign`
    - This endpoint will unassign a chore to a user. 
    - Pass the `userId` in as a query string param, as in the other endpoints above.  

Up Next: [Client Views](./house-rules-client-views.md)

