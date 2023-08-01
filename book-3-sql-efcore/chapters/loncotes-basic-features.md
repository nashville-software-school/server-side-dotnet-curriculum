# Basic Features for Loncotes Library
Test each endpoint with relevant data before you move on to the next task.

### Get all Materials
The librarians would like to see a list of all the circulating materials. Include the `Genre` and `MaterialType`. Exclude materials that have a `OutOfCirculationSince` value.


### Get Materials by Genre and/or MaterialType
The librarians also like to search for materials by genre and type. Add query string parameters to the above endpoint for `materialTypeId` and `genreId`. Update the logic of the above endpoint to include both, either, or neither of these filters, depending which are passed in. Remember, query string parameters are always optional when making an HTTP request, so you have to account for the possibility that any of them will be missing. 

### Get Material 
The librarians would like to see details for a material. Include the Genre, MaterialType, and Checkouts (as well as the Patron associated with each checkout using `ThenInclude`).

### Add a Material
Materials are often added to the library's collection. Add an endpoint to create a new material

### Remove a Material From Circulation
Add an endpoint that expects an id in the url, which sets the `OutOfCirculationSince` property of the material that matches the material id to `DateTime.Now`. (This is called a _soft delete_, where a row is not deleted from the database, but instead has a flag that says the row is no longer active.)  The endpoint to get all materials should already be filtering these items out. 

### Get MaterialTypes
The librarians will need a form in their app that let's them choose material types. Create an endpoint that retrieves all of the material types to eventually populate that form field

### Get Genres
The librarians will also need form fields that have all of the genres to choose from. Create an endpoint that gets all of the genres. 

### Get Patrons
The librarians want to see a list of library patrons. 

### Get Patron with Checkouts
This endpoint should get a patron and include their checkouts, and further include the materials and their material types. 

### Update Patron
Sometimes patrons move or change their email address. Add an endpoint that updates these properties only. 

### Deactivate Patron
Sometimes patrons move out of the county. Allow the librarians to deactivate a patron (another soft delete example!).

### Checkout a Material
The librarians need to be able to checkout items for patrons. Add an endpoint to create a new Checkout for a material and patron. Automatically set the checkout date to `DateTime.Now`. 

### Return a Material
The librarians need an endpoint to mark a checked out item as returned by item id. Add an endpoint expecting a checkout id, and update the checkout with a return date of `DateTime.Now`. 

Up Next: [Get Available Materials](./loncotes-get-available-materials.md)
