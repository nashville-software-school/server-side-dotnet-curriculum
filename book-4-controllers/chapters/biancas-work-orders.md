# Viewing Work Orders
In this chapter we will add an API endpoint to get work orders and a component in the front end to view them. 

## Create a WorkOrder List in the UI
Add a folder to the `components` folder of the React code called `workorders`. Add a file to the new folder called `WorkOrderList.jsx` and paste the following code:
>WorkOrderList.jsx
``` javascript
import { useEffect, useState } from "react";
import { Table } from "reactstrap";

const testWorkOrders = [
  {
    id: 1,
    description: "bent fork",
    dateInitiated: "2023-07-12T00:00:00",
    dateCompleted: null,
    userProfile: null,
    userProfileId: null,
    bikeId: 1,
    bike: {
      id: 1,
      brand: "Huffy",
      color: "red",
      ownerId: 1,
      bikeTypeId: 1,
      bikeType: {
        id: 1,
        name: "Mountain",
      },
      owner: {
        id: 1,
        name: "bob",
        address: "101 main street",
        email: "bob@bob.comx",
        telephone: "123-456-7890",
      },
    },
  },
  {
    id: 2,
    description: "broken brakes",
    dateInitiated: "2023-07-15T00:00:00",
    dateCompleted: null,
    userProfile: {
      id: 1,
      firstName: "Tony",
      lastName: "The Tiger",
      address: "102 fourth street",
      email: "tony@thetiger.comx",
      roles: ["Admin"],
      identityUserId: "asdgfasdfvousdag",
    },
    userProfileId: 1,
    bikeId: 2,
    bike: {
      id: 2,
      brand: "Schwinn",
      color: "blue",
      ownerId: 2,
      bikeTypeId: 1,
      bikeType: {
        id: 1,
        name: "Mountain",
      },
      owner: {
        id: 2,
        name: "andy",
        address: "101 main street",
        email: "andy@bob.comx",
        telephone: "123-456-7890",
      },
    },
  },
];

export default function WorkOrderList({ loggedInUser }) {
  const [workOrders, setWorkOrders] = useState([]);

  useEffect(() => {
    setWorkOrders(testWorkOrders);
  }, []);

  return (
    <>
      <h2>Open Work Orders</h2>
      <Table>
        <thead>
          <tr>
            <th>Owner</th>
            <th>Brand</th>
            <th>Color</th>
            <th>Description</th>
            <th>DateSubmitted</th>
            <th>Mechanic</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {workOrders.map((wo) => (
            <tr key={wo.id}>
              <th scope="row">{wo.bike.owner.name}</th>
              <td>{wo.bike.brand}</td>
              <td>{wo.bike.color}</td>
              <td>{wo.description}</td>
              <td>{new Date(wo.dateInitiated).toLocaleDateString()}</td>
              <td>
                {wo.userProfile
                  ? `${wo.userProfile.firstName} ${wo.userProfile.lastName}`
                  : "unassigned"}
              </td>
              <td></td>
            </tr>
          ))}
        </tbody>
      </Table>
    </>
  );
}
```
- This component has some test data in it. You can do this when you are building components that don't have endpoints yet. If you just want to work on the UI, create some fake data!

Replace the current `workorders` endpoint in `ApplicationViews.jsx with this one:
``` jsx
<Route
    path="workorders"
    element={
    <AuthorizedRoute loggedInUser={loggedInUser}>
        <WorkOrderList />
    </AuthorizedRoute>
    }
/>
```
You should be able to navigate to `workorders` in the react application and see the layout of the component using the test data. 


## Creating a New Controller
Because work orders are their own resource in our data model, they should get their own controller to contain the methods for getting those resources. Add a file to the `Controllers` directory called `WorkOrderController.cs`, and paste in the following code:
> WorkOrderController.cs
``` csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.EntityFrameworkCore;
using Microsoft.AspNetCore.Mvc;
using BiancasBikes.Data;

namespace BiancasBikes.Controllers;

[ApiController]
[Route("api/[controller]")]
public class WorkOrderController : ControllerBase
{
    private BiancasBikesDbContext _dbContext;

    public WorkOrderController(BiancasBikesDbContext context)
    {
        _dbContext = context;
    }

    [HttpGet("incomplete")]
    [Authorize]
    public IActionResult GetIncompleteWorkOrders()
    {
        return Ok(_dbContext.WorkOrders
        .Include(wo => wo.Bike)
        .ThenInclude(b => b.Owner)
        .Include(wo => wo.Bike)
        .ThenInclude(b => b.BikeType)
        .Include(wo => wo.UserProfile)
        .Where(wo => wo.DateCompleted == null)
        .OrderBy(wo => wo.DateInitiated)
        .ThenByDescending(wo => wo.UserProfileId == null).ToList());
    }
}
```
- This controller, like the others, inherits from `ControllerBase` and has the `ApiController` attribute. 
- The route for this controller will be `/api/workorder`. 
- the `WorkOrderController` constructor is _injecting_ an instance of the `BiancasBikesDbContext` class to use to access the database. 
- There is one endpoint in the class, `GetIncompleteWorkOrders`. 
- The query in the method uses `OrderBy` and `ThenByDescending` to order the work orders first by when they were created, so that the oldest appear first. Then they are further sorted by whether an employee has been assigned to them or not. If the work order does not have a `UserProfileId`, it will appear before one that does. 
- notice that we had to use `Include` twice for `Bike`. Once, to be able to call `ThenInclude` for `Owner`, and a second time to be able to call `ThenInclude` for `BikeType`. 

## Connect the Endpoint and Component
Add a `workOrderManager.js` file to the `managers` folders with this code:
``` javascript
const _apiUrl = "/api/workorder";

export const getIncompleteWorkOrders = () => {
  return fetch(_apiUrl + "/incomplete").then((res) => res.json());
};
```
Finally, update the component to use the `getIncompleteWorkOrders` function:
>WorkOrderList.jsx
``` javascript
useEffect(() => {
    getIncompleteWorkOrders().then(setWorkOrders);
}, []);
```

## Test the component
Make sure that the component works with the API. Once you have tested the component, you can remove the `testWorkOrders` data from the file. 


Up Next: [Creating a Work Order](./biancas-create-work-order.md)