# Basic CarBuilder Endpoints
In this chapter you will create the basic endpoints needed for the functionality of the car builder app. 

Implement the following:

## `/wheels`, `/technologies`, `/interiors`, `/paintcolors`
Each of these options collections requires a `GET` endpoint to fetch all of the choices for that option. Add an endpoint for each that returns the whole collection.

## `/orders`
1. Create an endpoint that gets all orders
    - This endpoint should add the related data from each of the options that are related to it. You might have to add more properties to the `Order` class so that there is a place to store those options on the order. 
1. Create another endpoint that creates an order and adds it to the orders collection. 
    - It needs to use the `POST` HTTP method, and should expect an order object in the JSON body of the HTTP request
    - Configure this endpoint to add the `Timestamp` to the order on the server-side using `DateTime.Now`. 
    - Include the code that creates a new Id for the new `Order` object. 
1. Test all of these endpoints before moving on. 

Up Next: [Making Requests](./car-builder-client-requests-cors.md)