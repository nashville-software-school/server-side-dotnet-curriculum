# Foundations Installations

## Setting up a second computer
If you are using a different computer for server side than you did for front end (or setting up Windows with Bootcamp on your Mac), you will need to do all of the front end installations first. They can be found [here](https://github.com/nashville-software-school/client-side-mastery/blob/master/book-0-installations/chapters/GETTING_STARTED_WINDOWS_C_SHARP.md).
## .NET 8 SDK
The .NET SDK includes all of the command line tools you will need to build and run your .NET programs for the first part of the course. <br>
[Download it for Windows here](https://download.visualstudio.microsoft.com/download/pr/cb56b18a-e2a6-4f24-be1d-fc4f023c9cc8/be3822e20b990cf180bb94ea8fbc42fe/dotnet-sdk-8.0.101-win-x64.exez
) <br>
[Download it for M1/M2 Macs here](https://download.visualstudio.microsoft.com/download/pr/4d6fe60e-611f-4db0-8b03-fc15ee03ca7a/e24b834bd82a75fb2a50a59b8a27aed3/dotnet-sdk-8.0.101-osx-arm64.pkg) <br>
[Download it for Intel Macs here](https://download.visualstudio.microsoft.com/download/pr/3b11b408-68e1-4a8f-a0ad-55b21456c4f6/03819d38c79a9aa4fd806f8c7b64130d/dotnet-sdk-8.0.101-osx-x64.pkg
)<br>
When the download completes, run (open) the file downloaded by your browser and click `Install` when prompted to. 

Verify that the install has worked by opening a new terminal and running `dotnet --version`. You should see `8.0.101`, or something similar. If your terminal doesn't recognize the command, ask an instructor to help. 


## C# Extension for VS Code
The official C# extension from Microsoft for VS Code provides a number of helpful features including syntax highlighting, Intellisense (code completion and hints) and tools for debugging in your editor. <br>
Install it by searching for `C#` in the extensions tab (open with Ctrl+Shift+X), select the C# extension by Microsoft and click `Install`.