# More Features for House Rules

## Updating a Chore
Convert the `ChoreDetails` component into a form to update the `Chore`. After submitting the form, the handle should trigger a navigation back to the chores list view. 

## Highlighting Overdue Chores
Once a chore has not been completed for as many days as its `ChoreFrequencyDays` it is time to do the chore again! Update the application so that overdue chores' names are red in the `ChoresList` so that users will be aware that they need attending to. To complete this feature, you may find it useful to:
1. Create a calculated property that determines whether the most recent completion of a chore was completed more than the `ChoreFrequencyDays` ago. To do this you may find the `Max` Linq method useful, as well as using `Include` to get `ChoreCompletions` when retrieving chores. The `AddDays` methods will also come in handy, along with `DateTime.Today`
1. Use the calculated property in the ChoresList to conditionally render each chore name in the default color or in red. 

## My Chores
Create a page that is visible to all authenticated users that shows the user their assigned chores, allowing them to mark their chores as complete. If the chore does not currently need to be completed, do not display it. You will need to make use of the calculated property you made for the previous task. 

___
This is the end of the HouseRules project. Below is a link to an unrelated exercise about Interfaces that you should complete before moving on to column 3. 

Up Next: [Interfaces](https://github.com/nashville-software-school/bangazon-inc/blob/server-side-curriculum/book-1-orientation/chapters/INTERFACES_INTRO.md)
