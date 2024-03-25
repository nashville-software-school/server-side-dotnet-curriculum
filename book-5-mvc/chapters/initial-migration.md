# EF CORE Initial Migration

## Run the following commands
1. Run the following command in the main project directory:
    - ```dotnet ef migrations add InitialCreate```

1. This command will create a Migrations folder with a number of C# files in it in the project directory. When it finishes, run this:
    - ```dotnet ef database update```

1. If you look at pgAdmin you should now have a new database called `DogGo` with the models that you created as tables.