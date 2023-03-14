# Implementing the Main Menu Options
In this chapter you will implement the logic for each of the options in the menu. For each option, create a method to contain all of the logic for it to work. Then, you can replace the `throw new NotImplementedException()` for each by calling the method that performs that options behavior (you're now _implementing_ the option, so you don't need to throw the warning exception anymore). When you finish each feature, test it before moving on to the next one. 

## Display all Plants
When the user chooses this option, all of the plants in the database should be listed one per-line, with the following format: 
```
<Number>. <Name of Plant> in <City> <is available/was sold> for <Price> dollars
Examples: 
1. A Ficus in Pasadena was sold for 15 dollars
2. A Hydrangea in Walla Walla is available for 25 dollars"
```
> 💡 Algorithmic reasoning: <br> When you have conditional logic which has _only two options_, sometimes the ternary conditional operator can be a cleaner way to choose one path or another than using `if`/`else`. <br> <br> For example, in this feature, you need to either display "is available" or "was sold" based on whether the plant is sold or not. <br> <br> We can do this with a ternary like this: <br> `plant.Sold ? "was sold" : "is available"` <br><br> This expression will return what's on the left side of the `:` if `plant.Sold` is `true`, and what's on the right side of the `:` if it is false. You can use a ternary inside the `{}` with string interpolation to implement this feature.

## Post a Plant
When a user chooses this option, they should be able to input a plant species, light needs, asking price, city, and zip, and add it to the plant database. When a user posts a plant, it should be available by default. Collect the user's answers, and create a new `Plant` object with those responses. Then, add that object to the plants List.

## Adopt a Plant
When a user chooses this option, they should be presented with a list of only the plants that are available (not sold).

> 💡 Algorithmic reasoning: <br> You will have to find a way to connect the user's choice to a particular plant in the database. Try letting the user choose the index of the plant in the plant list when they choose a plant to adopt. 

When the user choose a plant, change its `Sold` property to be `true`. 

## Delist a plant
When a user chooses this option, present them with a list of all plants. When they choose a plant, remove it from the list of plants. Investigate the List `RemoveAt` method for how to remove a List item at a particular index. 
 
Up Next: [Plant of the Day](./extravert-plant-of-day.md)