# Using Swagger to test your APIs
The Swagger tools are a set of tools for designing, building, documenting, and testing APIs. The `Swashbuckle` package is a dotnet package that allows users to auto-generate API documentation based on the endpoints found in a .NET web API. In addition, `Swashbuckle` can generate a web UI for viewing and testing the API endpoints. The templates that the dotnet sdk generates for a web API come with `Swashbuckle` pre-configured for us. Many students find the Swagger UI faster and more intuitive to work with than Postman, and that is great. However, Postman is still an important tool to have at your disposal, so don't discard it entirely in favor of Swagger. 

## Getting the Swagger UI to open in a browser on startup
The Swagger UI is already running when we start up Honey Rae's API, but we want a browser window to open it at startup as well. Follow these steps:
1. Open `launch.json` in the `.vscode` folder. Add this property to the `serverReadyAction` section: `"uriFormat": "%s/swagger/index.html"`.
1. Start the debugger for the API. When the server starts, a browser tab should open with the Swagger UI in it. Depending on your browser (especially Firefox), it may notify you of a security issue. This is because of a self-signed SSL certificate, which most browsers reject by default. The browser should give you options, if you are not sure what to do at this point, ask an instructor to help. 
1. Once the Swagger UI is open, take a look at the endpoints listed. They have an HTTP method along with a route, so you can tell what each will do.
1. Use the down arrow to open the `/servicetickets` GET endpoint. Click on `Try it out` and then `Execute`, and you will see the `200` Status code and the JSON output below.
1. Click on the down arrow for the `/servicetickets/{id}` GET endpoint. 
1. Click on `Try it out` and you will see an input to provide a value for `id`. Input `1` there and you will see a `200` response with the JSON for that service ticket below
1. Finally, open the `/servicetickets` POST endpoint. After clicking `Try it out` you will see a text input to provide the JSON for a new service ticket. You can erase the template code the UI provides, and add the JSON data for a new service ticket. Click `Execute`, and then check the GET endpoint to see that the ticket was added. 

That's it! Try out some of the other endpoints to get more comfortable with the tool. 