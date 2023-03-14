# Handling Unexpected Data

In this chapter will will handle when a user inputs invalid date data

## Bug Report :bug:

The Project Manager for ExtraVert submitted a bug report:

> **#1** **Date entry validation** <br> Steps to Reproduce: Enter an invalid date when creating a post (February 29th, 2023 works) <br> Expected result: The app tells me that the date is invalid and it recovers <br> Actual Result: There is this runtime error and the app crashes: <br> `Unhandled exception. System.ArgumentOutOfRangeException: Year, Month, and Day parameters describe an un-representable DateTime.`

## Squashing the Bug

Use a `try`/`catch` to help the app recover when this error happens.

1. The first step is to try to reproduce the bug locally. Follow the steps to reproduce.
1. Once you see what type of `Exception` you are looking for, use `try` to catch the possible error. Inside the `catch`, provide a message to the user that their input was invalid, and return to the main menu.

Up Next: [Method Parameters](./extravert-methods.md)
