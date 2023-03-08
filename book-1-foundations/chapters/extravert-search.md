# Search for Plants by Light Needs
In this chapter you will add a feature to ExtraVert that allows a user to search for plants by light needs in case they have a darker apartment and need plants that will do well in low light.

## Add an option 
Add an option to the menu that allows the user to search for plants. That option, when chosen, should call a method that will hold the logic for this feature.

## Filtering the List
Inside your new method, you will need to:
1. Prompt the user to enter a maximum light needs number (between 1 and 5).
1. Use a loop and a new List inside your method to collect all of the plants that have lighting needs at the level the user chose or lower. 
1. Display all of those plants to the user before returning to the main menu

> 💡 Algorithmic reasoning: <br> If you need a loop that considers each member of a collection, like a `List`, use a `foreach` loop. Later in this book we will examine List methods that are similar to JS array methods that will make some of these loops unnecessary. 

Up Next: [ExtraVert Stats]()