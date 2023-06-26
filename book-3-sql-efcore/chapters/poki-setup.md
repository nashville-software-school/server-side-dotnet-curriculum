# Creating Databases with SQL Scripts
There are a lot reasons why creating a database with pgAdmin (like we did for Music History) is impractical or even inadvisable. A few of them:
1. Trying to recreate the work we did once (with exactly the same data) by hand is error-prone and slow. 
1. Someone else besides you might need to set up the same database (say, a teammate on their local machine)
1. You want an easy way to recreate a clean database when testing and development has created a lot of junk data in the database that might even cause bugs. 

SQL scripts can help with this. A SQL script is just a code file with all of the instructions to create the database. Because you used a `database.json` file in the front-end course that served _as_ your database, there is a common misconception that the SQL script to create your database _is_ your database. But it's not! Think of it as a short program that you run to send instructions to PostgreSQL to create and add data to a database. PostgreSQL stores that data in other files (that you do _not_ need to understand, especially right now). But, it is similar to database.json in the sense that you commit this script to a git repo to save as a record of how to create the database. This is very useful, as we shall soon see. 

## Using `psql` to create the PoemsByKids database

### `PATH` variable 
`psql` is a command line utility that was installed with PostgreSQL. Unfortunately, your `PATH` environment variable may not include the directory that contains `psql`. `PATH` is a variable accessible in your terminal that is a list of directories where executable program files can be found. If you want to run a command, like `ls` or `mv`, your terminal needs to be able to find a program called `ls` or `mv` in one of the directories included in the `PATH` variable. 

You can find out what directory a command's executable file is in with the `which` command. For example:
``` bash
which ls
```
will give this output:
``` bash
/usr/bin/ls
```
This means that the command `ls` runs a program from the `/usr/bin/ls` directory (`/usr/bin` contains many of the utilities that you can use in your shell). 

Try this command:
``` bash
which psql
```
If you see a directory listed, you are already able to run the `psql` command in your terminal. Skip to [here](#download-and-run-the-poems-by-kids-create-script) if this is what you see. 

If not, you will see an output that begins with `which: no psql in ...`. What follows is actually your entire `PATH` variable listed.

What can we do? Add to our `PATH` variable! Depending on your OS and the version of PostgreSQL you installed, the path to `psql` will be different. 

On Windows (using Git Bash) it is usually `/c/Program Files/PostgreSQL/15/bin`.
On macOS it is usually `/Library/PostgreSQL/15/bin`. 

In both cases, the `15` might be different depending which version of PostgreSQL you installed. Use your terminal or file explorer to confirm that the `psql` program is in one of these directories. 

### Adding `psql` to your path
Run this command (replace `</path/to/psql>` with the path to psql on your machine): 
``` bash
echo 'export PATH="</path/to/psql>":$PATH' >> ~/.bashrc
```

This will add a line to your `.bashrc` file that will add that path to your `PATH` variable everytime you start a bash session. You can also run the `.bashrc` file during a session like this: 
``` bash
source ~/.bashrc
```

Finally, run this to confirm that you can now use `psql` in your terminal:
``` bash 
which psql
```
If you see a directory listed (it should be the same one we just added to the `PATH` variable), then you're all set! Otherwise, ask an instructor for help. 

### Download and run the Poems By Kids create script
1. Download the script by: 
    - making a folder called `PoemsByKids` in `~/workspace/sql`
    - clicking on [this](../../assets/pokipostgres.sql) link
    - clicking the "Raw" button in the Github interface
    - right-clicking and choosing "Save As".
    - saving the file in `~/workspace/sql/PoemsByKids` as `poki.sql`
1. open a terminal and navigate to `~/workspace/sql/PoemsByKids`
1. run this this command:
``` bash
psql -U postgres -f poki.sql
```
4. Enter the password for the superuser postgres (this is the password you created when installing PostgreSQL)

5. You should see output logs as PostgreSQL starts running the commands in the script. This database is large, and it will take several minutes for the script to run. 
6. When the script finishes, you will see a command prompt again. Open pgAdmin, and look for the PoemsByKids database in your Object Explorer. If you see the database there, open a query window for it. 
7. Execute the query `SELECT * FROM author`
8. If you see results, your database is set up correctly!