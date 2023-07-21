# Making Client Requests to the Car Builder API
In this chapter you will refactor the Car Builder client you build in the front end course to use the new .NET API. You will also learn one strategy for dealing with CORS. 

## Refactoring the client
> You will find it more difficult, but also much more satisfying and fun to update the client you built in the front end. However, if your cohort did not do this project, or you didn't get to it, you can use [this](https://github.com/nashville-software-school/car-builder) template to complete this part of the project. 

### Technologies
Currently, the function to get technologies in `database.js` looks something like this:
``` javascript
export const getTechnologies = () => {
    return [...database.technologies]
}
```
This function gets data from the database variable, copies it, and returns it. Let's refactor it to use the fetch API to make a request to the .NET API (you will have to add the port that your API is using) We will need to use `await` to wait for the data. `await` is similar to `.then` in that it allows the code to wait for an asynchronous operation to finish. It looks like this:
``` javascript
export const getTechnologies = async () => {
  const res = await fetch("https://localhost:<port>/technologies");
  const data = await res.json();
  return data;
};
```

>`await` can only be used inside functions marked `async` or at the top-level of a module, as we will see below.

Here is an example of what the `Technologies` component currently looks like:
``` javascript
const techs = getTechnologies()

export const Technologies = () => {
    return `<h2>Technologies</h2>
    <select id="tech">
        <option value="0">Select a technology package</option>
        ${
            techs.map(
                (tech) => {
                    return `<option value="${tech.id}">${tech.package}</option>`
                }
            ).join("")
        }
    </select>`
}
```

However, we are now getting technologies asynchronously over the network, so we need to `await` them as well (this `await` is ok because it is at the top level of the module):

```javascript
const techs = await getTechnologies();

export const Technologies = () => {

  return `<h2>Technologies</h2>
    <select id="tech">
        <option value="0">Select a technology package</option>
        ${techs
          .map((tech) => {
            return `<option value="${tech.id}">${tech.package}</option>`;
          })
          .join("")}
    </select>`;
};
```
We need to similarly update the `Order` component to `await` the request for technologies:
```javascript

const paints = getPaints()
const interiors = getInteriors()
const techs = await getTechnologies()
const wheels = getWheels()
export const Orders = () => {
    //omitted for brevity...
}
```

## Testing the API and client - CORS
We're ready to try the new version of getting technologies!

1. Start the API in debug mode
1. use `serve` to start the front end application
1. Open the front end app in your browser, and open your dev tools. 

Uh oh. You are probably seeing a blank browser window. Take a look at the console (you might have to refresh to see the error). You should see an error like this:


``` 
Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://localhost:<port>/. (Reason: CORS header ‘Access-Control-Allow-Origin’ missing). Status code: 404.
```
What does this error mean? CORS stands for *Cross-Origin Resource Sharing*. This request is cross-origin because the ports that the client and server-side apps are served from are different, and therefore they have different _origins_. The code in the front end is trying to make a request to a different origin, and the browser Your browser, by default, will block any client from making requests to origins different than the client's. This is to help protect the browser from malicious apps using browser data to access data they shouldn't on other sites. 

Normally, with a deployed application, that's exactly what we would want to happen. However, the server where the data is being requested can indicate to the browser that it's safe for the browser to access the API from a different origin (or in our case, from any origin). When we are developing locally on two different ports, this is the behavior we want. Let's configure the API to allow requests from any origin: 

In `Program.cs` of your API code, add the following line below `builder.Services.AddSwaggerGen();`
``` csharp
builder.Services.AddCors();
```
Update the code that starts with `app.Environment.IsDevelopment())` with this code block:
``` csharp
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
    app.UseCors(options =>
                {
                    options.AllowAnyOrigin();
                    options.AllowAnyMethod();
                    options.AllowAnyHeader();
                });
}
```
Restart the API, and refresh the browser. You should now see Car Builder loading in the browser. Our API is now telling the browser that it is safe to request the data from any origin.  

## Refactoring the Other Components
1. Refactor the database functions for getting paints, interiors, and wheels to fetch from the API, and make them `async` so you can `await` the results of the fetch
1. Update the `Paints`, `Interiors`, `Wheels` and `Orders` components to `await all of those functions that fetch from the API. 
1. Test the app to make sure all of the `select` elements are getting populated with the right data. 


Up Next: [Submitting an Order](./car-builder-submit-order.md)
## 🔍  Extra Materials
1. [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
1. [await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
1. [async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)