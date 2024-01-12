# Testing the Web API with Postman

In this chapter you will learn how to test endpoints in an API using Postman and how to add an endpoint to the API.

# Using Postman

Postman is a great tool for exploring APIs. If you want to use Postman to test the endpoints of your locally running API (meaning, if the host is `localhost`), you will need to install the Postman desktop client, you cannot use the web version. 

1. Download it [here](https://www.postman.com/downloads/). There are versions for Windows, Linux, and Intel or Apple chip MacBooks. 
1. You will need to open the file your browser downloaded to set it up.
1. Open the application
1. After opening, click on the plus sign to create a new HTTP request 
1. In your terminal, in the top-level directory for the `HoneyRaesAPI` project (this will be wherever the `csproj` file is), run `dotnet watch run`.
1. In the console, you will once again see the host addresses for the api while it is running. It is serving the `http` only option (this is the default when using `dotnet run`). For Postman, use the `http` host and port. We are going to test the weather forecast endpoint. In `Program.cs` you can see the `url` that goes with that endpoint (`/weatherforecast`). So, in order to make a request to that endpoint, the full address will be `http://localhost:<port>/weatherforecast`. Paste this address into the URL bar in Postman.
1. To the left of the URL bar, you have the option of choosing an HTTP method. GET is chosen by default, which is what we want, because the endpoint in `Program.cs` uses `MapGet`, not `MapPost` (there are others as well...)
1. To the right, there is a Send button. Click it. 
1.  Below you should see the HTTP response. The body should have data similar to this:
``` json
[
    {
        "date": "2023-06-07T10:02:30.6872004-05:00",
        "temperatureC": -20,
        "summary": "Sweltering",
        "temperatureF": -3
    },
    {
        "date": "2023-06-08T10:02:30.6873687-05:00",
        "temperatureC": -5,
        "summary": "Warm",
        "temperatureF": 24
    },
    {
        "date": "2023-06-09T10:02:30.6873722-05:00",
        "temperatureC": -12,
        "summary": "Bracing",
        "temperatureF": 11
    },
    {
        "date": "2023-06-10T10:02:30.6873724-05:00",
        "temperatureC": 49,
        "summary": "Balmy",
        "temperatureF": 120
    },
    {
        "date": "2023-06-11T10:02:30.687373-05:00",
        "temperatureC": 8,
        "summary": "Sweltering",
        "temperatureF": 46
    }
]
```

This data is the result that is returned by the function that is the second argument passed to the `MapGet` method. We can prove it by using breakpoints in VS Code to debug our project...

# Using the VS Code debugger for C#

1. In your terminal type `Ctrl+C` to stop your API. This is a good time to notice that our web API program will continue to run after an endpoint is hit. It wouldn't be very helpful if it could only answer one request and then stop! After the request is handled, the program continues running, and waits for the next request.
1. In the left panel of VS Code, find the Run and Debug icon. Click on it, and then at the top of the panel that opens, click the green play button to start the API with the debugger running. (There is a dropdown next to the button, choose ".NET Core Launch (web)" if it isn't already selected.)
1. The API will start again (this might take a bit longer than before). 
1. Go to `Program.cs` in VS Code. On the line that has `return forecast`, click to the left of the line number, and you should see a red dot appear next to the line. This indicates that there is a breakpoint set on that line. 
1. Go back to Postman and click Send another time. 
1. Instead of immediately getting the response, you should see that the program in VS Code is stopped on the line where you set the breakpoint. 
1. Hover over the variable `forecast`. VS Code should show you the five forecast objects in an array that are about to be sent back to the client (in this case, Postman). 
1. A small panel in VS Code will have a similar set of buttons to those in the browser dev tools that allow you to Step Over, In, Out, etc... Click the "Continue" button (or hit `F5` if you have the option to), and you will see that the program continues, and Postman receives the response. 

> :bug: <small>Debugging Tip: Whenever you are uncertain about what's going on with an endpoint in your API, your first move should be to put a breakpoint somewhere inside the _handler_ for that endpoint. This does a lot of things for you: <br> a. Confirms whether or not the request is being received at that endpoint <br> b. Allows you to confirm that the data being received by that endpoint is correct (if you are sending data in a POST or PUT, for example). <br> c. Allows you to confirm whether the data that method is going to return is valid and correct. <br><br> If you're ever uncertain about where to start debugging a problem in a full stack app, if it's data related, start with the handler!</small>

## Adding another endpoint

1. In `Program.cs`, put this code above `app.Run()`:
``` csharp
app.MapGet("/hello", () =>
{
    return "hello";
});
```
1. Save the file, and in the debugging controls panel, hit the "Restart" button (`Ctrl+Shift+F5` if it's an option for you). 
1. In Postman, replace the `/weatherforecast` end of the address with `/hello`. Click Send again
1. You should see the string `hello` in the response body at the bottom. 

### What did we just do?
By making a second call to `MapGet` in our program, we added another _endpoint_ to our API that is waiting for a `GET` request to `http://localhost:<port>/hello`, because we specified that url in the first argument. When the API receives that request, it will call the function that is the second argument. Whatever that function returns will be the data for the HTTP response back to the client (in this case, Postman). The data that this function returns is just `"hello"`, so that is what is in the response body in Postman. 

## ✍️ Reflections
1. We introduced a number of important terms in this and the last chapters:
    -  _endpoint_ - a specific url at which clients can access resources provided by the API
    - _route_ - the URL for a specific endpoint
    - _handler_ - a function that is called when a request is made to a particular endpoint's URL. This function can accept data from the HTTP request as arguments, and returns data for the HTTP response
    
    These are important concepts, and they will become clearer as you build more APIs. 
1. This is a big leap forward from the last book. We are starting to build applications that interact with network requests. It is worthwhile to take inventory of what you think you already know about HTTP requests, and what you don't know yet or may have forgotten (it happens to us all). Take a look at the introduction to the server side curriculum to review what role this API is playing both in the technology stack that we're building, and its role in the HTTP request/response cycle. If any of that is confusing, ask an instructor to chat about it!

## 🔍 Additional Materials

1. [MDN docs for HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) (save for reference)
1. [ASP.NET Minimal APIs](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/overview?view=aspnetcore-8.0)

Up Next: [Defining types for HoneyRaesAPI](./defining-types-honey-raes.md)