# 🐕‍🦺 🐩 DeShawn's Dog Walking
In this project you will have the opportunity to build a full stack application with a React client and .NET API. Try to avoid completing the entire API, and only then starting on the React client. We recommend you build the API and the front end concurrently, _feature by feature_, rather than trying to anticipate all of the needs for the API in the beginning. For this reason, (and because it is the column furthest to the right), aside from the setup instructions and the starter code, the main features of the app will only be described in the user stories. Use the tools you have already learned (building an ERD, making wireframes, etc) to plan your work on this project. 

## Setup

1. Use [this](https://github.com/nss-group-projects/dotnet-deshawns-react) template to create your own repo, and clone it locally
1. the React app is in a folder called `client`. Navigate to that directory and run `npm install`
1. Start the API with the VS Code debugger. 
1. in the `client` directory, run `npm run dev` to start the React App. 
1. Explore the codebase to see what is there. Pay particular attention to `index.jsx`, `apiManager.js`, and `App.jsx`. Write down any questions you have so that you can ask a colleague or your instructors. 

## Proxy Settings
This project uses a different method for handling CORS. In `vite.config.js` (for the client app), this code has been added:
``` js
proxy: {
        "/api": {
          target: "https://localhost:5001",
          changeOrigin: true,
          secure: false,
        },
      },
```
This code tells the server that is serving the javascript app to send any unknown requests to the react server  on to `https://localhost:5001`. This is useful for a few reasons: 
1. You can make calls to the API to the _same origin_ that the front-end application is running on. This means that you do not need to provide a domain when specifying URLs in your fetch calls. For example, the fetch in the `apiManager.js` module in the template code looks like this:
    ``` javascript
    export const getGreeting = async () => {
    const res = await fetch("/api/hello");
    return res.json();
    };
    ```
    The URL begins with `/`, omitting the domain. This will result in an HTTP request to `http://localhost:3000/api/hello`. When the dev server that is serving the javascript application doesn't find an asset to return at `/api/hello`, it will pass the request on to `https://localhost:5001/api/hello`. 
1. When deploying the application, the `apiManager.js` module does not need to change if the API and the front-end app are at the same domain. If the React app is at `myawesomeapp.com` and the API is at `myawesomeapp.com/api`, `apiManager` will continue to work, even though the applications are deployed on a completely different architecture than your local machine!
1. Because, as far as the browser is concerned, the API and the React app are at the same domain, we do not need to account for CORS issues at all, because the requests are not cross-origin!
1. This means, that to avoid name clashes, prepend every API route with `/api` for this and the rest of the projects in the course. 

### `launchSettings.json`
This is a good time to point out that in the `Properties` folder of the API project, there is a file called `launchSettings.json`. This file contains the configurations for running the web API. 
There are two profiles in the JSON. This is the one that the debugger is using:
``` JSON
"profiles": {
    "https": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": true,
      "launchUrl": "swagger",
      "applicationUrl": "https://localhost:5001;http://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
```
This is where the URLs for the API are located, as well as environment variables that will be available in the project while it is running. 

Finally, in the `.vscode` folder the `launch.json` file contains more configuration for debugging. This line:
``` json
"uriFormat": "%s/swagger/index.html"
```
in the `serverReadyAction` section automaticaly opens Swagger (a tool for documenting and testing APIs) in a browser tab. See [this](./honey-rae-open-api.md) chapter for more information.  

That's it for the tour of the application! It's time to start coding... oh wait. No! It's time to start planning!

Up Next: [User Stories](./deshawns-user-stories.md)



## 🔍 Additional Materials
1. [Proxy API Requests locally](https://vitejs.dev/config/server-options.html#server-proxy)
