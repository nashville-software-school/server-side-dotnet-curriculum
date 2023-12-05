# Filter Orders By Paint Color
In this chapter we will update the `/orders` endpoint to optionally filter the orders by paint color. 

## Refresher on query string params
Query string params allow us to pass data to an endpoint as part of the url that is sent to the server. A request can have multiple params in it, which always come after the `?` at the end of a url. Consider this request url:
```
https://www.example.com/resource/1?age=20&type=open
```
The above URL has two query string params:
1. `age`, with a value of 20
1. `type`, with a value of "open"

The `1` at the end of the URL is _not_ a query string param. You can tell because it comes before the `?`, not after. It is a _URL param_. Notice that when there are multiple query string params, they are separated by an `&`. Finally, it is important to note that query string params are _always optional_. Regardless of how many params (or none!) are after the question mark, as long as a request is made starting with  `https://www.example.com/resource/1`, all of those requests would hit that endpoint.

> It is important to note that you should generally only use primitive data types like string and numbers as the values for your query params. It is common for beginners to try to pass entire objects as the value of a query string param, but this is generally a bad idea, and there are other ways to solve nearly any problem you are trying to use this strategy to solve.

## Capturing query params in an endpoint
The endpoints in our web APIs are capable of capturing the values passed in through query params by declaring handler params with the same names. Replace the `/orders` GET endpoint with this one:

``` csharp
app.MapGet("/orders", (int? paintId) =>
{
    foreach (Order order in orders)
    {
        order.Wheels = wheels.First(w => w.Id == order.WheelId);
        order.Technology = technologies.First(w => w.Id == order.TechnologyId);
        order.PaintColor = paints.First(w => w.Id == order.PaintId);
        order.Interior = interiors.First(w => w.Id == order.InteriorId);
    }

    List<Order> filteredOrders = orders.Where(o => !o.Fulfilled).ToList();

    // Now, check for the paintId property to see if we should filter by that as well
    if (paintId != null)
    {
        filteredOrders = filteredOrders.Where(order => order.PaintId == paintId).ToList();
    }

    return filteredOrders.Select(o => new OrderDTO
    {
        Id = o.Id,
        Timestamp = o.Timestamp,
        TechnologyId = o.TechnologyId,
        Technology = new TechnologyDTO
        {
            Id = o.Technology.Id,
            Package = o.Technology.Package,
            Price = o.Technology.Price
        },
        WheelId = o.WheelId,
        Wheels = new WheelsDTO
        {
            Id = o.Wheels.Id,
            Style = o.Wheels.Style,
            Price = o.Wheels.Price
        },
        InteriorId = o.InteriorId,
        Interior = new InteriorDTO
        {
            Id = o.Interior.Id,
            Material = o.Interior.Material,
            Price = o.Interior.Price
        },
        PaintId = o.PaintId,
        PaintColor = new PaintColorDTO
        {
            Id = o.PaintColor.Id,
            Color = o.PaintColor.Color,
            Price = o.PaintColor.Price
        },
    }).ToList();
});
```
This endpoint has a parameter called `paintId` that is a nullable integer. It is nullable because the query param is optional. If a request came to the endpoint that did not have the query param (because query params in the url are always optional), and the type of `paintId` were `int`, the value that the framework would pass in as the arg for `paintId` would be `0`. This can be confusing, so it is better to make the param an `int?`, so that if the request does not have the param, the value will be `null`. 

Try the endpoint with the following urls:
1. `/orders`
1. `/orders?paintId=1`

Notice that in the first case the endpoint still returns all of the orders, because the query param is optional, and you can see in the endpoint that we optionally filter the orders if the `paintId` is not null. With the second request, you should only see orders with the `paintId` of `1`. 


Up Next: [DeShawn's Dog Walking](./deshawns-setup.md)