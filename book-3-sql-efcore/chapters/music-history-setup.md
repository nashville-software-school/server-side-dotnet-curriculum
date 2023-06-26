# Creating a Database with pgAdmin

There are a number of ways you will learn to create new databases in this course. In this chapter we will go over creating a database using the pgAdmin UI. This process is useful for exercises like the next chapter, and you will find it useful if you want to practice PostgreSQL on its own, without a web application. 

## Instructions for `MusicHistory` database
1. Open pgAdmin
1. In the Object Explorer panel, you will see a Servers node. Expand the node, and you should see `PostgreSQL <version number>` with an :x: next to it. The application _may_ open the connection dialog automatically, but if not click on the node to open it. 
1. You will be presented with a dialog to enter the password for the superuser for the database (you should have saved this password when you installed PostgreSQL). 
1. Enter the password (you can check the box to save the password if you would like), and click `OK`.
1. Once connected, you will now see more nodes in the `PostgreSQL <version number>` node, including one called "Databases" with a count of the databases in the server. There should only be one
1. Expand the databases node, and you will see one database called `postgres`. This the default database created when a server is created. For the most part, _leave this database alone unless explicitly instructed otherwise_. 
1. Right-click on the Databases nodes, then choose `Create` -> `Database...`
1. Input `MusicHistory` in the Database field, leave everything else the same, and click `Save`. 
1. In the Object Explorer, you will now see two databases, `postgres`, and `MusicHistory`. 

## Creating the tables for `MusicHistory`

Click on the `MusicHistory` node in the Object Explorer so that it is highlighted. Find the Query Tool button at the top of the Object Explorer and click it (You may also hit `Alt`+`Shift`+`Q`). 

To the right you should now see a tab where you can write SQL queries for this database. Currently our database has no tables, so there is no data to query! 

Paste this SQL into the Query window:
```sql
CREATE TABLE Genre (
    Id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    Name TEXT NOT NULL
);
```

Click the triangle execute button (or hit `F5`). Postgres will create a table in the MusicHistory database called `Genre`, and that table will have two columns, `id` and `Name`. The keywords `GENERATED ALWAYS AS IDENTITY` ensure that the database will provide a new incremented identity value. You should use this syntax for all of your `PRIMARY KEY` `Id` columns in this course. `TEXT` is the type you should use for most character columns in PostgreSQL in this course.

Open another query window by clicking on the "Query Tool" button in the Object Explorer window again. In that query window paste the following:
``` sql
INSERT INTO Genre (Name) VALUES ('Soul');
INSERT INTO Genre (Name) VALUES ('Rock');
INSERT INTO Genre (Name) VALUES ('Blues');
INSERT INTO Genre (Name) VALUES ('Jazz');
INSERT INTO Genre (Name) VALUES ('Heavy Metal');
INSERT INTO Genre (Name) VALUES ('R&B');
INSERT INTO Genre (Name) VALUES ('Pop');
INSERT INTO Genre (Name) VALUES ('Bluegrass');
INSERT INTO Genre (Name) VALUES ('Punk');
INSERT INTO Genre (Name) VALUES ('Classical');
INSERT INTO Genre (Name) VALUES ('Country');
INSERT INTO Genre (Name) VALUES ('Latin');
INSERT INTO Genre (Name) VALUES ('Rap');
INSERT INTO Genre (Name) VALUES ('Electronic');
```
Click the Execute button (or hit `F5`) to insert all of the Genres for this database. 

You can confirm that the insert statements worked by by querying for them in another window. Open a third query window and run: 
``` sql
SELECT * FROM Genre;
```
Execute this query. You will see the results in the Data Output window on the bottom right of the screen. Notice we did not need to provide `Id` values, because the `SERIAL` column auto-incremented this value for each row when they were created. 

Go back to the first query window and add the rest of the tables for the database:
```sql
CREATE TABLE Artist (
    Id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    ArtistName TEXT NOT NULL,
    YearEstablished INTEGER NOT NULL
);

CREATE TABLE Album (
    Id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    Title TEXT NOT NULL,
    ReleaseDate TEXT NOT NULL,
    AlbumLength INTEGER NOT NULL,
    Label TEXT NOT NULL,
    ArtistId INTEGER NOT NULL REFERENCES Artist (Id),
    GenreId INTEGER REFERENCES Genre (Id)
);

CREATE TABLE Song (
    Id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    Title TEXT NOT NULL,
    SongLength INTEGER NOT NULL,
    ReleaseDate TEXT NOT NULL,
    GenreId INTEGER REFERENCES Genre (Id),
    ArtistId INTEGER NOT NULL REFERENCES Artist (Id),
    AlbumId INTEGER NOT NULL REFERENCES Album (Id)
);
```

Highlight these three `CREATE TABLE` statements (exclude the Genre table which we already created) and execute the query. Highlighting text in a query window before executing will limit the query that is executed to the highlighted text. This is very handy if you have multiple statements in a script, but don't want to run them all.

Take a look at the script we just ran. All of the column definitions that include the keyword `REFERENCES` define those columns as _foreign keys_ on other tables.  For example, on the `Album` table, this line: 
```sql 
ArtistId INTEGER NOT NULL REFERENCES Artist (id),
```
says that an Album will have a required (NOT NULL) column called `ArtistId`. That integer value will _reference_ an `Id` on one of the rows in the `Artist` table.

## Adding the rest of the data to the database

Go to the second query window with the Genre `INSERT` statements in it. Add all of this code to the query:
```sql 
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Judas Priest', 1969);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Def Leppard', 1977);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('ZZTop', 1969);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Genesis', 1967);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Journey', 1973);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Beatles', 1960);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Howlin'' Wolf', 1959);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Black Flag', 1981);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Porcupine Tree', 1987);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Grateful Dead', 1965);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('The Shins', 1996);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Rush', 1968);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('The Features', 1998);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Squeeze', 1977);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Midnight Oil', 1976);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Dire Straits', 1977);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Hoodoo Gurus', 1981);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('U2', 1976);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Mayer Hawthorne', 2009);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('David Bowie', 1968);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Sigur Ros', 1997);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('deadmau5', 2006);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Justice', 2007);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Miles Davis', 1948);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('The Sheepdogs', 2006);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Coheed & Cambria', 2001);
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Jay Z', 1986);

INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('The Black Album', '11/14/2003', 2268, 'Def Jam', 27, 13);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('A Strange Arrangement', '09/08/2009', 2082, 'Stones Throw Records', 19, 1);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('How Do You Do', '10/11/2011', 2357, 'Stones Throw Records', 19, 1);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Where Does This Door Go', '06/16/2013', 3114, 'Stones Throw Records', 19, 1);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Revolver', '08/05/1966', 2083, 'Parlophone', 6, 2);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Sgt. Pepper''s Lonely Hearts Club Band', '06/01/1967', 2392, 'Stones Throw Records', 6, 2);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Magical Mystery Tour', '11/27/1967', 1148, 'Stones Throw Records', 6, 2);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Screaming for Vengeance', '06/17/1982', 2322, 'Columbia', 1, 2);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Point of Entry', '02/26/1981', 2262, 'Columbia', 1, 2);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Defenders of the Faith', '01/04/1984', 2383, 'Columbia', 1, 2);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Round About Midnight', '03/06/1957', 2327, 'Columbia', 24, 4);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Miles Ahead', '10/21/1957', 2132, 'Columbia', 24, 4);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Milestones', '09/02/1958', 2856, 'Columbia', 24, 4);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Moanin'' in the Moonlight', '05/14/1959', 2033, 'Chess', 7, 3);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Howlin'' Wolf', '10/21/1957', 1917, 'Chess', 7, 3);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('The Howlin'' Wolf Album', '09/02/1969', 2459, 'Chess', 7, 3);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Eliminator', '3/23/1983', 2668, 'Warner Bros.', 3, 2);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Afterburner', '1/1/2011', 417, 'Warner Bros.', 3, 2);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Tres Hombres', '6/14/1979', 321, 'Warner Bros.', 3, 2);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Damaged', '12/05/1981', 2098, 'SST', 8, 9);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('TV Party', '07/12/1982', 409, 'SST', 8, 9);
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Everything Went Black', '12/03/1982', 3718, 'SST', 8, 9);

INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Revenge', 61, '12/03/1982', 9, 8, 22);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('White Minority', 69, '12/03/1982', 9, 8, 22);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Gimme Gimme Gimme', 120, '12/03/1982', 9, 8, 22);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('No Values', 118, '12/03/1982', 9, 8, 22);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('TV Party', 232, '06/12/1982', 9, 8, 21);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('I''ve Got To Run', 105, '06/12/1982', 9, 8, 21);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('My Rules', 71, '06/12/1982', 9, 8, 21);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Gimme All Your Lovin', 507, '3/23/1983', 2, 3, 17);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Sharp Dressed Man', 419, '3/23/1983', 2, 3, 17);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Legs', 309, '3/23/1983', 2, 3, 17);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('I Need You Tonight', 470, '3/23/1983', 2, 3, 17);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('TV Dinners', 539, '3/23/1983', 2, 3, 17);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Thug', 925, '3/23/1983', 2, 3, 17);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Taxman', 714, '8/5/1966', 7, 6, 5);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Eleanor Rigby', 807, '8/5/1966', 7, 6, 5);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Good Day Sunshine', 350, '8/5/1966', 7, 6, 5);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Got To Get You Into My Life', 574, '8/5/1966', 7, 6, 5);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Interlude', 237, '12/03/1982', 13, 27, 1);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('What More Can I Say', 150, '12/03/1982', 13, 27, 1);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Encore', 260, '12/03/1982', 13, 27, 1);
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Dirt Off Your Shoulder', 851, '12/03/1982', 13, 27, 1);
```

Highlight all but the Genre inserts, and execute the script statements. Your database is now ready for the exercise!

## Saving SQL scripts for later use
It is usually a good idea to save SQL scripts like the ones we wrote today for future use. For example, if you needed to recreate this database, being able to load these queries from a file can save you a lot of time. SQL scripts can be saved as text files with the `.sql` extension. Let's practice that now:
1. In your workspace folder, create a `sql` folder, and a `MusicHistory` folder inside that. 
1. In pgAdmin, click on the query window that has the `CREATE TABLE` statements.
1. Click the Save button, and use the dialog to find the `MusicHistory` directory we just created. In the file Name field, input `01_MusicHistory_Create.sql`. We prefix the file with `01` to indicate that this script needs to run before `02`, which will be the other script. Click, save, and you will see that the Title of the query window has changed to the file Name
1. Click on the second query window with the `INSERT` statements. Save this query as `02_MusicHistory_Seed.sql` in the same directory. 
1. Now, if you ever need to recreate the database, you can refer to these scripts, and run them in order on a new database. You will learn more about how to use these scripts in upcoming chapters.

## ✍️ Reflections

1. Go back to the foreign key definitions in the `CREATE TABLE` statements using `REFERENCES`. See if you can write down an explanation of what each of those column definitions means. For example: "The xId column is of type y, and is required/not required. It is a foreign key that references the x table's Id column". Being able to use this kind of vocabulary in this context is important. You can write these down and share with an instructor during a 1-on-1 if you are uncertain about your understanding. 

Up Next: [Music History Practice Queries](./music-history-practice.md)