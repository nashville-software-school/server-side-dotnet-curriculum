# Calculating the Total Price

In this chapter we will use _composition_ and _calculated properties_ to move the logic for calculating the order total from the front end to the API.

## Car Builder's current logic

In the current front end app, the `Orders` component should look something like this:

```javascript
export const Orders = async () => {
  const orders = await getOrders();
  return `${orders
    .map((order) => {
      const paint = paints.find((p) => p.id === order.paintId);
      const technology = techs.find((t) => t.id === order.technologyId);
      const interior = interiors.find((i) => i.id === order.interiorId);
      const wheel = wheels.find((w) => w.id === order.wheelId);

      return `<section class="order">
                ${paint.color} car with
                ${wheel.style} wheels,
                ${interior.material} interior,
                and the ${technology.package}
                for a total cost of
                ${(
                  paint.price +
                  technology.price +
                  interior.price +
                  wheel.price
                ).toLocaleString("en-US", {
                  style: "currency",
                  currency: "USD",
                })}
            </section>`;
    })
    .join("")}`;
};
```

This component gets each of the options objects from the database directly. Let's update the API so that it sends those objects directly, without having to look them up!

## Adding related data properties to `Order`

1. [Add properties](./honey-raes-get-emps-cust.md#including-related-data) for the various options to the `Order` class. Currently, the model only has, for example, `WheelId`, but you also need a `Wheels` property.
1. [Update the `GET` `/orders` endpoint](./honey-raes-get-emps-cust.md#include-the-employees-data-in-the-service-ticket-details) to look up the various options based on the foreign keys, and add them to the order before returning it.
1. In the `Orders` component on the front end, you can remove the logic that gets all of the options and finds the correct one for each option type. It should now be possible to access those directly on the order. For example:
   ```javascript
   `<section class="order">
       ${order.paint.color} car with
   ... etc.
   ```
   > One _very common_ issue that developers encounter when doing refactors like this is that they will occasionally choose different property names on the client and server side. You may encounter that while working on this project,so keep it in mind while debugging!
1. Test the refactor to make sure it works!

## Calculating the Total

1. Now that the `Order` class can access the prices of each of its options, you can add a calculated property to `Order` that sums all of the prices of the options. Call the property `TotalCost`.
1. In the front end, the app no longer needs to calculate this value. This code:
   ```javascript
   paint.price + technology.price + interior.price + wheel.price;
   ```
   can be replaced by this code:
   ```javascript
   order.totalCost;
   ```
1. Test again to make sure everything is working. Make sure to remove any code that is no longer needed, even if it's not causing errors.

Up Next: [Completing Car Builds](./car-builder-complete-build.md)
