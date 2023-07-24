# Adding functionality: Completing a Build
In this chapter you will add a button to the custom orders displayed in the front end to mark the order as fulfilled. This chapter's main learning objective is to understand the workflow for adding a feature to a full-stack project (where you have both a front end app and a a custom web API).

## Adding a button to the order items
Add a button to the `section` tag that holds an order in the `Orders` component: 
``` html
<input type="button" name="complete" id="${order.id}" value="Complete">
```
Add an event listener to the module that will call the database function:
``` javascript
document.addEventListener("click", (event) => {
  const { name, id } = event.target;
  if (name === "complete") {
    completeOrder(id);
  }
});
```
Add the `completeOrder` function to the `database` module:
``` javascript
export const completeOrder = async (orderId) => {
  await fetch(`https://localhost:<port>/orders/${orderId}/fulfill`, {
    method: "POST",
  });
  document.dispatchEvent(new CustomEvent("stateChanged"));
};
```

## Creating the endpoint in the API
Your task is to create an endpoint in the API that:
1. marks the order matching the provided `orderId` as fulfilled. You will have to add a property to the `Order` class to store this value. 
1. will be triggered by the client-side code above. pay attention to the URL, the request method, and whether any data is being sent with the request. 

Test the endpoint to make sure that it works. How will you confirm that the endpoint did what you expected it to do?

## Filtering the Orders
Finally, update the `Get` `/orders` endpoint to only return orders that are unfulfilled using the `Where` Linq method, and test your solution. When you complete an order, you should see it disappear from the browser without refreshing. 


Up Next: [Kandy Korner](./kandy-corner-setup.md)
