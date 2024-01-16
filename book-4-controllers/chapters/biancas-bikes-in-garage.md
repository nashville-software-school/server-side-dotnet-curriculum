# Displaying the Bikes in Garage Count
The navbar of the app includes a count of the bikes that are currently in the garage, but does not yet actually get a count from the database. Let's implement the rest of this feature!

## Creating the API Endpoint
The data that the component needs is fairly simple: an integer representing the number of bikes currently in the garage. It is less obvious _how_ we should get this data from the database. According to Bianca, a bike can be assumed to be in the garage if it has an open work order associated with it. 

### Algorithm for getting Bikes in Garage
1. The component needs bikes
1. with work orders where a work order doesn't have a DateCompleted
1. Instead of returning the bikes, return only a count of those bikes

### Implement the algorithm
1. Step 1:
    ``` csharp
    int inventory = _dbContext.Bikes.ToList();
    ```
1. Step 2: 
    ``` csharp
    int inventory = _dbContext
        .Bikes
        .Where(b => b.WorkOrders.Any(wo => wo.DateCompleted == null))
        .ToList();
    ```
1. Step 3:
    ``` csharp
    int inventory = _dbContext
        .Bikes
        .Where(b => b.WorkOrders.Any(wo => wo.DateCompleted == null))
        .Count();
    ```
### Create the Endpoint
``` csharp
[HttpGet("inventory")]
[Authorize]
public IActionResult Inventory()
{
    int inventory = _dbContext
    .Bikes
    .Where(b => b.WorkOrders.Any(wo => wo.DateCompleted == null))
    .Count();

    return Ok(inventory);
}
```
- the route for this endpoint will be `/api/bike/inventory`
- `Authorize` ensures that this endpoint is only accesible to logged in users

## Accessing the Endpoint in the Client
> bikeManager.js
``` javascript
export const getBikesInShopCount = () => {
  return fetch(`${apiUrl}/inventory`).then((res) => res.json());
};
```
>NavBar.jsx
``` javascript
 const getInventory = () => {
    getBikesInShopCount().then(setInventory);
  };

  useEffect(() => {
    loggedInUser && getInventory();
  }, [loggedInUser]);
```
- We only want to call `getInventory` when there is a logged in user.

Test out the endpoint to make sure it works! If you haven't added any data to the database, there should be only one bike in the garage.

Up Next: [Managing Work Orders](./biancas-work-orders.md)