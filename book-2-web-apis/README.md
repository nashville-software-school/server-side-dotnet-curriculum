# Book 2 - An Introduction to Web APIs
The purpose of this book is to provide a conceptual introduction to building Web APIs with ASP.NET Core and modeling data in C#/.NET. Any additional installations for this book are included in the chapter instructions.

## Learning Objectives
1. Debugging C# in VS Code
1. Web API Concepts
    <ul>
        <li>url routes, request handlers, endpoints</li>
        <li>the HTTP request/response cycle revisited</li>
        <li>URL parameters</li>
        <li>data serialization</li>
    </ul>
1. More object-oriented programming
    <ul>
        <li>Creating classes to represent data entities</li>
        <li>using classes to model data relationships between entities (composition)</li>
    </ul>

## Table of Contents

|#|🍯 💻<br>  Honey Rae's Repairs<br> <sub>(guided tour)</sub> |🚙🚗 <br>Car Builder |🐕‍🦺 🐩<br> DeShawn's Dog Walking |
|:-:|:-:|:-:|:-:|
|1|[Setup for Web APIs](./chapters/web-api-setup.md)|[Project Setup](./chapters/car-builder-setup.md)|[Project Setup](./chapters/deshawns-setup.md)<br><sub style="font-size: 0.85rem;">#proxy</sub>|
|2|[Making Requests with Postman](./chapters/testing-web-api.md)  <br><sub style="font-size: 0.85rem;">#debugging #endpoints #routes #handlers</sub>|[Basic Requirements](./chapters/car-builder-basic-endpoints.md)|[User Stories](./chapters/deshawns-user-stories.md)|
|3|[Data Models](./chapters/defining-types-honey-raes.md) <br><sub style="font-size: 0.85rem;">#namespaces #dtos</sub>|[Get Technologies](./chapters/car-builder-client-requests-cors.md)<br><sub style="font-size: 0.85rem;">#cors #await #async</sub>|[Walker Cities](./chapters/deshawns-many-to-many.md)| 
|4|[Get All/Get One Service Ticket(s)](./chapters/honey-raes-get-tickets.md) <br><sub style="font-size: 0.85rem;">#using</sub>|[Submit an Order](./chapters/car-builder-submit-order.md)||
|5|[Adding all Honey Rae's GET endpoints](./chapters/honey-raes-get-emps-cust.md)<br><sub style="font-size: 0.85rem;">#composition #NotFound</sub>|[Calculating Total Price](./chapters/car-builder-related-data.md)||
|6| [Creating a Service Ticket](./chapters/honey-raes-create.md) |[Completing a Build](./chapters/car-builder-complete-build.md)||
|7| [Deleting a Ticket](./chapters/honey-raes-delete.md) <br><sub style="font-size: 0.85rem;">#delete</sub>|[Query String Params](./chapters/car-builder-query-string.md)|
|8| [Assigning a Ticket](./chapters/honey-rae-put.md) <br><sub style="font-size: 0.85rem;">#put</sub>||:potato:[Coding Self-Assessment](./chapters/coding-self-assessment.md)|

|:compass: Explorer Chapters|
|--|
|[REST design principles -naming routes](./chapters/rest-concepts.md)|
|🍯 💻 [Adding More Endpoints to HoneyRae's](./chapters/honey-rae-more-endpoints.md)|
|🍯 💻 [Add a front-end client for HoneyRae's API](./chapters/honey-rae-client.md)|
|🍯 💻 [OpenAPI (Swagger)](./chapters/honey-rae-open-api.md)|