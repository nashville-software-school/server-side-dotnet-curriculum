# Create a Work Order
In this chapter we will add the ability to create a new work order

## Building the UI
### Creating the Form Component
Add another file to the `components/workorders` folder called `CreateWorkOrder.jsx` with the following code:
``` javascript
import { useEffect, useState } from "react";
import { getBikes } from "../../managers/bikeManager";
import { Button, Form, FormGroup, Input, Label } from "reactstrap";

export default function CreateWorkOrder({ loggedInUser }) {
  const [description, setDescription] = useState("");
  const [bikeId, setBikeId] = useState(0);
  const [bikes, setBikes] = useState([]);

  const handleSubmit = (e) => {
    e.preventDefault();
    const newWorkOrder = {
      bikeId,
      description,
    };

    console.log(
      `new work order submitted: ${newWorkOrder.description}, bikeId: ${newWorkOrder.bikeId}`,
    );
  };

  useEffect(() => {
    getBikes().then(setBikes);
  }, []);

  return (
    <>
      <h2>Open a Work Order</h2>
      <Form>
        <FormGroup>
          <Label>Description</Label>
          <Input
            type="text"
            value={description}
            onChange={(e) => {
              setDescription(e.target.value);
            }}
          />
        </FormGroup>
        <FormGroup>
          <Label>Bike</Label>
          <Input
            type="select"
            value={bikeId}
            onChange={(e) => {
              setBikeId(parseInt(e.target.value));
            }}
          >
            <option value={0}>Choose a Bike</option>
            {bikes.map((b) => (
              <option
                key={b.id}
                value={b.id}
              >{`${b.owner.name} - ${b.brand} - ${b.color}`}</option>
            ))}
          </Input>
        </FormGroup>
        <Button onClick={handleSubmit} color="primary">
          Submit
        </Button>
      </Form>
    </>
  );
}

```
- This component logs the form data to the console rather than submitting it to the database. we'll add the logic for submitting to the API later. 

### Routing
Replace the `workorders` route in `ApplicationViews.jsx` with these routes:
``` jsx
<Route path="workorders">
    <Route
    index
    element={
        <AuthorizedRoute loggedInUser={loggedInUser}>
            <WorkOrderList />
        </AuthorizedRoute>
    }
    />
    <Route
    path="create"
    element={
        <AuthorizedRoute loggedInUser={loggedInUser}>
            <CreateWorkOrder />
        </AuthorizedRoute>
    }
    />
</Route>
```
- The Route group create two routes for `workorders`. The route marked `index` will match to `workorders` with no extra url segments. The create route will match `/workorders/create`. 

Let's add a link to the form in the list component:
>WorkOrderList.jsx
``` javascript
<h2>Open Work Orders</h2>
<Link to="/workorders/create">New Work Order</Link>
//... rest of component omitted
```
You should be able to test navigating to the form and filling it out. Check the console for confirmation that the form is collecting data. 

## Creating the Endpoint
Add this method to the `WorkOrderController` class:
``` csharp
[HttpPost]
[Authorize]
public IActionResult CreateWorkOrder(WorkOrder workOrder)
{
    workOrder.DateInitiated = DateTime.Now;
    _dbContext.WorkOrders.Add(workOrder);
    _dbContext.SaveChanges();
    return Created($"/api/workorder/{workOrder.Id}", workOrder);
}
```
- This endpoint will map to a `POST` request with the url `/api/workorder`. 

## Using the Endpoint in the UI
Add another function to the `workOrderManager`:
>workOrderManager.js
``` javascript
export const createWorkOrder = (workOrder) => {
  return fetch(_apiUrl, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(workOrder),
  }).then((res) => res.json);
};
```

Finally, use that API access function in the `CreateWorkOrder` component:
> CreateWorkOrder.jsx
``` javascript
const navigate = useNavigate();

  const handleSubmit = (e) => {
    e.preventDefault();
    const newWorkOrder = {
      bikeId,
      description,
    };

    createWorkOrder(newWorkOrder).then(() => {
      navigate("/workorders");
    });
  };
```

There is nothing new here. Go ahead and test the create feature to see that the new work order gets added to the list of work orders. 

Up Next: [Assigning and Completing Work Orders](./biancas-update-work-orders.md)