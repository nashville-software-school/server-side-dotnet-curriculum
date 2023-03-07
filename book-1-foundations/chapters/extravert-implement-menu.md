# Implementing the Main Menu Options
In this chapter you will implement the logic for each of the options in the menu. For each option, create a method to contain all of the logic for it to work. Then, you can replace the `throw new NotImplementedException()` for each by calling the method that performs that options behavior (you're now _implementing_ the option, so you don't need to throw the warning exception anymore)

## Display all Plants
When the user chooses this option, all of the plants in the database should be listed one per-line, with the following format: 
```
<Number>. <Name of Plant> in <City> <is available/was sold> for <Price> dollars
Examples: 
1. Ficus in Pasadena was sold for 15 dollars"
2. Hydrangea in Walla Walla is available for 25 dollars"
```
> 💡 Algorithmic reasoning: <br> When you have boolean logic which has only two options, sometimes the ternary conditional operator can be a cleaner way to choose one path or another than using `if`/`else`. <br> <br> For example, in this feature, you need to either display "is available" or "was sold" based on whether the plant is sold or not. <br> <br> We can do this with a ternary like this: <br> `plant.Sold ? "was sold" : "is available"` <br><br> This expression will return what's on the left side of the `:` if `plant.Sold` is `true`, and what's on the right side of the `:` if it is false. You can put this logic inside a string template with `{}` to implement this feature. Try it out!

Up Next: [Plant of the Day](./extravert-plant-of-day.md)