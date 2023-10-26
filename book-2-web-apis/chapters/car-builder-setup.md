# 🚙🚗 Car Builder Setup
The main goal of this project is to help you think about how the APIs you are going to build from this point in the course forward fit into a larger full-stack application architecture. This column assumes you have completed the front-end Car Builder project.

## Project Setup
1. Use the [instructions](./web-api-setup.md) from the first column to create a webapi called `CarBuilder` in your csharp directory. 
1. Add a `Models` folder, and create the classes required for CarBuilder with their properties:
    - `PaintColor`
        - `Id`
        - `Price`
        - `Color`
    - `Interior`
        - `Id`
        - `Price`
        - `Material`
    - `Technology`
        - `Id`
        - `Price`
        - `Package`
    - `Wheels`
        - `Id`
        - `Price`
        - `Style`
    - `Order`
        - `Id`
        - `Timestamp`
        - `WheelId`
        - `TechnologyId`
        - `PaintId`
        - `InteriorId`
1. Create a DTOs folder inside the Models folder, and add DTOs for each of the models. They can have identical properties as their corresponding Model classes for now. 
1. Delete the weather-forecast related code.  
1. Create collections at the top of `Program.cs` as the database for this project. Make sure that you have populated those collections with the data they need (you can leave the orders collection empty). If you don't want to make up your own options data, you can use these:
    ### Paint Color

    Customer should be able to choose one of the following paint colors. You set the price for each one.

    1. Silver
    1. Midnight Blue
    1. Firebrick Red
    1. Spring Green

    ### Interior

    Customer can choose from the follow options for interior seat types. You set the price for each one.

    1. Beige Fabric
    1. Charcoal Fabric
    1. White Leather
    1. Black Leather

    ### Technology

    Customer can choose from the follow options for the tech capabilities of the car dashboard. You set the price for each one.

    1. Basic Package _(basic sound system)_
    1. Navigation Package _(includes integrated navigation controls)_
    1. Visibility Package _(includes side and rear cameras)_
    1. Ultra Package _(includes navigation and visibility packages)_

    ### Wheels

    Customer can choose from the follow options for wheels. You set the price for each one.

    1. 17-inch Pair Radial
    1. 17-inch Pair Radial Black
    1. 18-inch Pair Spoke Silver
    1. 18-inch Pair Spoke Black

Up Next: [Basic Endpoints](./car-builder-basic-endpoints.md)