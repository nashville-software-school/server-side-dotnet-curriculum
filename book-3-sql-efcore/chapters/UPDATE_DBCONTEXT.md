Add the following code right above the existing `OnModelCreating` method in your DB Context module.

```cs
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder.ConfigureWarnings(warnings => warnings.Ignore(WarningCodes.PendingModelChangesWarning));
}
```

Then run `dotnet ef database update`. The build should succeed and update the database.
