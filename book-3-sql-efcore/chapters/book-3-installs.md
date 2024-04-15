# Installing PostgreSQL and pgAdmin
In this chapter you will install two important applications, the PostgreSQL server, as well as pgAdmin, an app that you can use to interact with the PostgreSQL server. 

1. Go to [this link](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads) and choose the latest **version 16** installation for your OS (If by chance you are a Linux user, ask an instructor about your options).  
1. Once the file is downloaded, open it to run it. 
1. You should be able to use all of the default settings for the installer.
1. Once you have clicked through the installer and the installation process begins, it is normal for it to take several minutes to finish. 
1. When the installation finished, it will ask you if you want to start Stack Builder on exit. You can uncheck the box and simply exit without starting that application. 
1.  You will be prompted during the installation process to choose a password for the server's default postgres user. Make sure that you save that password, as you will need it later. 

# Installing the Entity Framework Core dotnet cli tool

Run this command in your terminal:
``` bash
dotnet tool install --global dotnet-ef --framework net8.0
```