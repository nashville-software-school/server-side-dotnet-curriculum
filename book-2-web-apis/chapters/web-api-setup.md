# Creating a new Web API project
In this chapter we will go over the steps for creating a new .NET Web API using the Minimal APIs feature. You can use this setup guide for all of the projects in this book. 

## Instructions

1. navigate to `~/workspace/csharp` in your terminal
1. Run this command: `dotnet new webapi -o HoneyRaesAPI -minimal`
1. Run `cd HoneyRaesAPI`
1. Run `dotnet new gitignore`
1. Run `dotnet run`

When you've finished these instructions, you should see something like this in your terminal, that indicates that the API is running:
```
Building...
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://localhost:5256
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\Path\To\HoneyRaesAPI
```
Type `Ctrl+C` to shut the API down now. 

6. After this, use `code .` to open the project you just created in VS Code. 

6. You should see this dialog from VS Code (click Yes):
![build and debug assets confirmation](../../assets/honey-raes-assets-confirm.png)

## A tour of the scaffolded project

