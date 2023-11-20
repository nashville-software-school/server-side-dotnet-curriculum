# Deleting a `ServiceTicket`

Try to write this one yourself! 
1. Use the `MapDelete` method to create the endpoint
1. use a route parameter with `{id}` to allow the endpoint to capture the id of the service ticket to delete
1. In the handler's logic, remove the service ticket from the database that matches that id. There are a number of ways to do this with Linq or List methods. 
1. The handler doesn't need to return anything, or you can use `return Results.NoContent();` to return a 204 response. 
1. Test the endpoint in Postman (don't forget to use the `DELETE` method and specify an `id` in the URL!).
1. Try getting all of the service tickets after that to confirm that the one you were trying to delete is not in the database anymore.  
1. You can also try the `GET` endpoint in Postman to get the specific service ticket you deleted. That request should now get a `404` response. 

Up Next: [Assigning and Completing Tickets](./honey-rae-put.md)