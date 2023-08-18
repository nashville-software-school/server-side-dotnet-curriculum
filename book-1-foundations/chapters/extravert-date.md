# Adding an "Available Until" Date

This chapter will add the ability to provide an expiration date for a plant post, after which time the plant will be unavailable even if it is not sold.

## Feature requirements

1. Add a property to the `Plant` class called `AvailableUntil` with the appropriate type to hold this data.
1. In the interface for adding a plant, the user should be prompted to enter a Year, Month, and Day that the post will expire.
1. Use `new DateTime()`, passing in the year, month and day to create the date.
1. Set the `AvailableUntil` property on the new `Plant` with the date created in the previous step.

## Refactoring the app to use `AvailableUntil`
Update the view that shows the plants available for adoption to only include plants that are not sold _and_ are still available. You will have to compare the plant's `AvailableUntil` property to `DateTime.Now` to check this.

Up Next: [App Statistics](./extravert-stats.md)
