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
CREATE TABLE genre (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL
);
```

Click the triangle execute button (or hit `F5`). Postgres will create a table in the MusicHistory database called `genre`, and that table will have two columns, `id` and `name`. `SERIAL` is a special integer type in PostgreSQL that will auto-increment uniquely every time we add a new row. `TEXT` is the type you should use for most character columns in PostgreSQL in this course.

Open another query window by clicking on the "Query Tool" button in the Object Explorer window again. In that query window paste the following:
``` sql
INSERT INTO genre (name) VALUES ('Soul');
INSERT INTO genre (name) VALUES ('Rock');
INSERT INTO genre (name) VALUES ('Blues');
INSERT INTO genre (name) VALUES ('Jazz');
INSERT INTO genre (name) VALUES ('Heavy Metal');
INSERT INTO genre (name) VALUES ('R&B');
INSERT INTO genre (name) VALUES ('Pop');
INSERT INTO genre (name) VALUES ('Bluegrass');
INSERT INTO genre (name) VALUES ('Punk');
INSERT INTO genre (name) VALUES ('Classical');
INSERT INTO genre (name) VALUES ('Country');
INSERT INTO genre (name) VALUES ('Latin');
INSERT INTO genre (name) VALUES ('Rap');
INSERT INTO genre (name) VALUES ('Electronic');
```
Click the Execute button (or hit `F5`) to insert all of the genres for this database. 

You can confirm that the insert statements worked by by querying for them in another window. Open a third query window and run: 
``` sql
SELECT * FROM genre;
```
Execute this query. You will see the results in the Data Output window on the bottom right of the screen. Notice we did not need to provide `id` values, because the `SERIAL` column auto-incremented this value for each row when they were created. 

Go back to the first query window and add the rest of the tables for the database:
```sql
CREATE TABLE artist (
    id SERIAL PRIMARY KEY,
    artist_name TEXT NOT NULL,
    year_established INTEGER NOT NULL
);

CREATE TABLE album (
    id SERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    release_date TEXT NOT NULL,
    album_length INTEGER NOT NULL,
    label TEXT NOT NULL,
    artist_id INTEGER NOT NULL REFERENCES artist (id),
    genre_id INTEGER REFERENCES genre (id)
);

CREATE TABLE song (
    id SERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    song_length INTEGER NOT NULL,
    release_date TEXT NOT NULL,
    genre_id INTEGER REFERENCES genre (id),
    artist_id INTEGER NOT NULL REFERENCES artist (id),
    album_id INTEGER NOT NULL REFERENCES album (id)
);
```

Highlight these three `CREATE TABLE` statements (exclude the genre table which we already created) and execute the query. Highlighting text in a query window before executing will limit the query that is executed to the highlighted text. This is very handy if you have multiple statements in a script, but don't want to run them all.

Take a look at the script we just ran. All of the column definitions that include the keyword `REFERENCES` define those columns as _foreign keys_ on other tables.  For example, on the `album` table, this line: 
```sql 
artist_id INTEGER NOT NULL REFERENCES artist (id),
```
says that an album will have a required (NOT NULL) column called `artist_id`. That integer value will _reference_ an `id` on one of the rows in the `artist` table.

## Adding the rest of the data to the database

Go to the second query window with the genre `INSERT` statements in it. Add all of this code to the query:
```sql 
INSERT INTO artist (artist_name, year_established) VALUES ('Judas Priest', 1969);
INSERT INTO artist (artist_name, year_established) VALUES ('Def Leppard', 1977);
INSERT INTO artist (artist_name, year_established) VALUES ('ZZTop', 1969);
INSERT INTO artist (artist_name, year_established) VALUES ('Genesis', 1967);
INSERT INTO artist (artist_name, year_established) VALUES ('Journey', 1973);
INSERT INTO artist (artist_name, year_established) VALUES ('Beatles', 1960);
INSERT INTO artist (artist_name, year_established) VALUES ('Howlin'' Wolf', 1959);
INSERT INTO artist (artist_name, year_established) VALUES ('Black Flag', 1981);
INSERT INTO artist (artist_name, year_established) VALUES ('Porcupine Tree', 1987);
INSERT INTO artist (artist_name, year_established) VALUES ('Grateful Dead', 1965);
INSERT INTO artist (artist_name, year_established) VALUES ('The Shins', 1996);
INSERT INTO artist (artist_name, year_established) VALUES ('Rush', 1968);
INSERT INTO artist (artist_name, year_established) VALUES ('The Features', 1998);
INSERT INTO artist (artist_name, year_established) VALUES ('Squeeze', 1977);
INSERT INTO artist (artist_name, year_established) VALUES ('Midnight Oil', 1976);
INSERT INTO artist (artist_name, year_established) VALUES ('Dire Straits', 1977);
INSERT INTO artist (artist_name, year_established) VALUES ('Hoodoo Gurus', 1981);
INSERT INTO artist (artist_name, year_established) VALUES ('U2', 1976);
INSERT INTO artist (artist_name, year_established) VALUES ('Mayer Hawthorne', 2009);
INSERT INTO artist (artist_name, year_established) VALUES ('David Bowie', 1968);
INSERT INTO artist (artist_name, year_established) VALUES ('Sigur Ros', 1997);
INSERT INTO artist (artist_name, year_established) VALUES ('deadmau5', 2006);
INSERT INTO artist (artist_name, year_established) VALUES ('Justice', 2007);
INSERT INTO artist (artist_name, year_established) VALUES ('Miles Davis', 1948);
INSERT INTO artist (artist_name, year_established) VALUES ('The Sheepdogs', 2006);
INSERT INTO artist (artist_name, year_established) VALUES ('Coheed & Cambria', 2001);
INSERT INTO artist (artist_name, year_established) VALUES ('Jay Z', 1986);

INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('The Black Album', '11/14/2003', 2268, 'Def Jam', 27, 13);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('A Strange Arrangement', '09/08/2009', 2082, 'Stones Throw Records', 19, 1);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('How Do You Do', '10/11/2011', 2357, 'Stones Throw Records', 19, 1);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Where Does This Door Go', '06/16/2013', 3114, 'Stones Throw Records', 19, 1);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Revolver', '08/05/1966', 2083, 'Parlophone', 6, 2);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Sgt. Pepper''s Lonely Hearts Club Band', '06/01/1967', 2392, 'Stones Throw Records', 6, 2);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Magical Mystery Tour', '11/27/1967', 1148, 'Stones Throw Records', 6, 2);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Screaming for Vengeance', '06/17/1982', 2322, 'Columbia', 1, 2);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Point of Entry', '02/26/1981', 2262, 'Columbia', 1, 2);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Defenders of the Faith', '01/04/1984', 2383, 'Columbia', 1, 2);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Round About Midnight', '03/06/1957', 2327, 'Columbia', 24, 4);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Miles Ahead', '10/21/1957', 2132, 'Columbia', 24, 4);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Milestones', '09/02/1958', 2856, 'Columbia', 24, 4);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Moanin'' in the Moonlight', '05/14/1959', 2033, 'Chess', 7, 3);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Howlin'' Wolf', '10/21/1957', 1917, 'Chess', 7, 3);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('The Howlin'' Wolf Album', '09/02/1969', 2459, 'Chess', 7, 3);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Eliminator', '3/23/1983', 2668, 'Warner Bros.', 3, 2);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Afterburner', '1/1/2011', 417, 'Warner Bros.', 3, 2);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Tres Hombres', '6/14/1979', 321, 'Warner Bros.', 3, 2);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Damaged', '12/05/1981', 2098, 'SST', 8, 9);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('TV Party', '07/12/1982', 409, 'SST', 8, 9);
INSERT INTO album (title, release_date, album_length, label,artist_id, genre_id) VALUES ('Everything Went Black', '12/03/1982', 3718, 'SST', 8, 9);

INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Revenge', 61, '12/03/1982', 9, 8, 22);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('White Minority', 69, '12/03/1982', 9, 8, 22);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Gimme Gimme Gimme', 120, '12/03/1982', 9, 8, 22);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('No Values', 118, '12/03/1982', 9, 8, 22);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('TV Party', 232, '06/12/1982', 9, 8, 21);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('I''ve Got To Run', 105, '06/12/1982', 9, 8, 21);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('My Rules', 71, '06/12/1982', 9, 8, 21);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Gimme All Your Lovin', 507, '3/23/1983', 2, 3, 17);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Sharp Dressed Man', 419, '3/23/1983', 2, 3, 17);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Legs', 309, '3/23/1983', 2, 3, 17);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('I Need You Tonight', 470, '3/23/1983', 2, 3, 17);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('TV Dinners', 539, '3/23/1983', 2, 3, 17);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Thug', 925, '3/23/1983', 2, 3, 17);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Taxman', 714, '8/5/1966', 7, 6, 5);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Eleanor Rigby', 807, '8/5/1966', 7, 6, 5);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Good Day Sunshine', 350, '8/5/1966', 7, 6, 5);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Got To Get You Into My Life', 574, '8/5/1966', 7, 6, 5);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Interlude', 237, '12/03/1982', 13, 27, 1);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('What More Can I Say', 150, '12/03/1982', 13, 27, 1);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Encore', 260, '12/03/1982', 13, 27, 1);
INSERT INTO song (title, song_length, release_date, genre_id,artist_id, album_id) VALUES ('Dirt Off Your Shoulder', 851, '12/03/1982', 13, 27, 1);
```

Highlight all but the genre inserts, and execute the script statements. Your database is now ready for the exercise!

## Saving SQL scripts for later use
It is usually a good idea to save SQL scripts like the ones we wrote today for future use. For example, if you needed to recreate this database, being able to load these queries from a file can save you a lot of time. SQL scripts can be saved as text files with the `.sql` extension. Let's practice that now:
1. In your workspace folder, create a `sql` folder, and a `MusicHistory` folder inside that. 
1. In pgAdmin, click on the query window that has the `CREATE TABLE` statements.
1. Click the Save button, and use the dialog to find the `MusicHistory` directory we just created. In the file name field, input `01_MusicHistory_Create.sql`. We prefix the file with `01` to indicate that this script needs to run before `02`, which will be the other script. Click, save, and you will see that the title of the query window has changed to the file name
1. Click on the second query window with the `INSERT` statements. Save this query as `02_MusicHistory_Seed.sql` in the same directory. 
1. Now, if you ever need to recreate the database, you can refer to these scripts, and run them in order on a new database. You will learn more about how to use these scripts in upcoming chapters.

## ✍️ Reflections

1. Go back to the foreign key definitions in the `CREATE TABLE` statements using `REFERENCES`. See if you can write down an explanation of what each of those column definitions means. For example: "The x_id column is of type y, and is required/not required. It is a foreign key that references the x table's id column". Being able to use this kind of vocabulary in this context is important. You can write these down and share with an instructor during a 1-on-1 if you are uncertain about your understanding. 

Up Next: [Music History Practice Queries](./music-history-practice.md)