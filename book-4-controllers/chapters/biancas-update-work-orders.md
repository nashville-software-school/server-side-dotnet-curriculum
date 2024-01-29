# Assign and Complete Work Orders
In this chapter we will add to the `WorkOrderList` component to allow users to assign work orders to mechanics or mark the work order as complete. 

## Update `WorkOrderList`
The `Action` column is currently empty in the table of work orders. The `Mechanic` column just display a mechanic if there is one. Replace those `td` elements with these:
> WorkOrderList.jsx
``` javascript
<td>
<Input
    type="select"
    onChange={(e) => {
    assignMechanic(wo, parseInt(e.target.value));
    }}
    value={wo.userProfileId || 0}
>
    <option value="0">Choose mechanic</option>
    {mechanics.map((m) => (
    <option
        key={m.id}
        value={m.id}
    >{`${m.firstName} ${m.lastName}`}</option>
    ))}
</Input>
</td>
<td>
{wo.userProfile && (
    <Button
    onClick={() => completeWorkOrder(wo.id)}
    color="success"
    >
    Mark as Complete
    </Button>
)}
</td>
```
Add the following functions to the component:
``` javascript
const assignMechanic = (workOrder, mechanicId) => {
    console.log(`Assigned ${mechanicId} to ${workOrder.id}`);
  };

  const completeWorkOrder = (workOrderId) => {
    console.log(`Completed ${workOrderId}`);
  };
```

### Getting mechanics
The `WorkOrderList` needs to get all of the employees so that `mechanics` above will have a value. Let's create a manager function that we'll use in the `WorkOrderList` component to get all of the mechanics:
>userProfileManager.js
``` javascript
const _apiUrl = "/api/userprofile";

export const getUserProfiles = () => {
  return fetch(_apiUrl).then((res) => res.json());
};
```
>WorkOrderList.jsx
``` javascript
const [mechanics, setMechanics] = useState([]);

useEffect(() => {
    getIncompleteWorkOrders().then(setWorkOrders);
    getUserProfiles().then(setMechanics);
}, []);
```
Use the console to test that the interface for choosing a mechanic works correctly. 

## Create the Endpoint
Add this endpoint to the `WorkOrderController`:
>WorkOrderController.cs
``` csharp
[HttpPut("{id}")]
[Authorize]
public IActionResult UpdateWorkOrder(WorkOrder workOrder, int id)
{
    WorkOrder workOrderToUpdate = _dbContext.WorkOrders.SingleOrDefault(wo => wo.Id == id);
    if (workOrderToUpdate == null)
    {
        return NotFound();
    }
    else if (id != workOrder.Id)
    {
        return BadRequest();
    }

    //These are the only properties that we want to make editable
    workOrderToUpdate.Description = workOrder.Description;
    workOrderToUpdate.UserProfileId = workOrder.UserProfileId;
    workOrderToUpdate.BikeId = workOrder.BikeId;

    _dbContext.SaveChanges();

    return NoContent();
}
```
- There isn't really anything new here. 

## Access the Endpoint in the Client

Add the following to `workOrderManager`:
``` javascript
export const updateWorkOrder = (workOrder) => {
  return fetch(`${_apiUrl}/${workOrder.id}`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(workOrder),
  });
};
```
Finally, update the component to use the above function:
> WorkOrderList.jsx
``` javascript
const assignMechanic = (workOrder, mechanicId) => {
    const clone = structuredClone(workOrder);
    clone.userProfileId = mechanicId || null;
    updateWorkOrder(clone).then(() => {
      getIncompleteWorkOrders().then(setWorkOrders);
    });
};
```

Test the endpoint to make sure it works!

## Complete Work Order
See if you can update the app to make the `Mark As Complete` button work without code examples. You will need to:
1. Create an endpoint that handles a request to complete a work order
1. Create a function in the `workOrderManager` to access this API endpoint
1. Update the `completeWorkOrder` function in the `WorkOrderList` component to use the data access function from `workOrderManager`. 

## Delete Work Order
Add another (red) button to the `Actions` column that deletes an incomplete work order. See if you can get this feature working without instructions! You will need to use the `HttpDelete` attribute on a new endpoint that is meant to handle requests with HTTP `DELETE` methods. 

Up Next: [Managing Employees](./biancas-employee-roles.md)