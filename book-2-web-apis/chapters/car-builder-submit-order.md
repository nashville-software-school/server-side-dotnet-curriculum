# Submitting a New Order
In this chapter we will update Car Builder to submit a new order to the API and retrieve orders from the API. 

## Submitting orders
Currently the submit function to add an order should look something like this:
``` javascript
export const addCustomOrder = () => {
    const newOrder = {...database.orderBuilder}
    newOrder.timestamp = new Date().toLocaleDateString("en-US")
    newOrder.id = database.customOrders[database.customOrders.length - 1].id + 1
    database.customOrders.push(newOrder)

    database.orderBuilder = {}
    document.dispatchEvent(new CustomEvent("stateChanged"))
}
```
This function does three things that our API can now do for us:
1. The API will create a timestamp for us
1. The API will now create the new Id for the order
1. The API can now store the new order so it persists even if the front end app refreshes. 

To refactor do the following (try to do this before looking at the solution):
1. mark the function as `async`
1. remove the code we no longer need and replace with a `fetch` call from the API. `await` the response so that the operation finishes before clearing out the order builder and dispatching the state change event. 

    <details>
    <summary>The solution:</summary>

    ``` javascript
    export const addCustomOrder = async () => {
    const newOrder = { ...database.orderBuilder };
    await fetch(`https://localhost:7094/orders`, {
        method: "POST",
        headers: {
        "Content-Type": "application/json",
        },
        body: JSON.stringify(newOrder),
    });
    database.orderBuilder = {};
    document.dispatchEvent(new CustomEvent("stateChanged"));
    };
    ```
    </details>

1. Use Postman to get all of the orders to ensure that your order was created. 

## Fetching Orders from the database
1. The client is still getting the list of orders from the database in the front end. Update the `getOrders` function to be `async` and get its data from the API
1. You will notice that now that we are using `await` inside the `Orders` function, it must also be `async` (we have to get the orders _inside_ the `Orders` function, otherwise the orders won't refresh when we re-render them).
1. This means that we also need to `await` a call to the `Orders` function in `CarBuilder`:
    ``` javascript
    <article class="customOrders">
            <h2>Custom Car Orders</h2>
            ${await Orders()}
    </article>
    ```
1. This means that the `CarBuilder` function must also be `async`! (because you can only use `await` inside an `async` function):
    ```javascript
    export const CarBuilder = async () => {...
    ```
1. And finally, this means that the render function in `main.js` must also be `async`, in order to `await` `CarBuilder`:
    ``` javascript
    const renderAllHTML = async () => {
        mainContainer.innerHTML = await CarBuilder();
    };
    ```
1. As you can see, the `async`/`await` syntax is simpler than `.then`, but requires any functions that call an `async` function to also be `async`. This is fine! 
1. Now test the client to see that the list of orders updates when you submit an order.

Up Next: [Related Data and Calculated Values](./car-builder-related-data.md)

