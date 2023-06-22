# Music History Practice Queries
In this chapter you will write several queries to answer questions about the data in `MusicHistory`. Try to write the most specific query for each question that gets the information requested. 

> <small>It is definitely ok to examine the full tables of data to gut check your answers, _however_: it is not necessarily helpful for your learning to examine all of the data before you write the queries. This database is very small, and it is easy to see, for example, that a genre has a particular id. You might be tempted to search for songs by a particular genre_id. But with a much larger database, it would not be so easy to find that genre name in a table of 1000 genres, and you would have to write a SQL query to find that genre_id. If the request for data only provides you with a genre name to search for songs, write a query that assumes that's the only thing you know about the data you're searching for (in addition to the tables and columns that are available). Your SQL queries themselves are your main window into the data, the sooner you treat them as such the better. </small> 

## Setup
1. Open pgAdmin. 
1. Open a query window on the MusicHistory database
1. Save the query right away in `~/workspace/sql/MusicHistory` as `MusicHistoryPracticeQueries.sql`. Now you can save your work as you make progress (you can click the save button, or `Alt`+`S` whenever you want to save.)
1. Add these exercises to the file:
```text
-- 1. Query all of the entries in the `genre` table
-- 2. Query all the entries in the `artist` table and order by the artist's name. HINT: use the ORDER BY keywords
-- 3. Write a `SELECT` query that lists all the songs in the `song` table and include the artist name
-- 4. Write a `SELECT` query that lists all the artists that have a Pop album
-- 5. Write a `SELECT` query that lists all the artists that have a Jazz or Rock album
-- 6. Write a `SELECT` statement that lists the albums with no songs
-- 7. Using the `INSERT` statement, add one of your favorite artists to the `artist` table.
-- 8. Using the `INSERT` statement, add one, or more, albums by your artist to the `album` table.
-- 9. Using the `INSERT` statement, add some songs that are on that album to the `song` table.
-- 10. Write a `SELECT` query that provides the song titles, album title, and artist name for all of the data you just entered in. Use the [`LEFT JOIN`](https://www.tutorialspoint.com/sql/sql-using-joins.htm) keyword sequence to connect the tables, and the `WHERE` keyword to filter the results to the album and artist you added.
--  > **NOTE:** Direction of join matters. Try the following statements and see the difference in results.
--
--    ```sql
--    SELECT a.title, s.title FROM album a LEFT JOIN song s ON s.album_id = a.id;
--    SELECT a.title, s.title FROM song s LEFT JOIN album a ON s.album_id = a.id;
    ```

-- 11. Write a `SELECT` statement to display how many songs exist for each album. You'll need to use the `COUNT()` function and the `GROUP BY` keyword sequence.
-- 12. Write a `SELECT` statement to display how many songs exist for each artist. You'll need to use the `COUNT()` function and the `GROUP BY` keyword sequence.
-- 13. Write a `SELECT` statement to display how many songs exist for each genre. You'll need to use the `COUNT()` function and the `GROUP BY` keyword sequence.
-- 14. Write a `SELECT` query that lists the artists that have put out records on more than one record label. Hint: When using `GROUP BY` instead of using a `WHERE` clause, use the [`HAVING`](https://www.tutorialspoint.com/sql/sql-having-clause.htm) keyword
-- 15. Using `MAX()` function, write a select statement to find the album with the longest duration. The result should display the album title and the duration.
-- 16. Using `MAX()` function, write a select statement to find the song with the longest duration. The result should display the song title and the duration.
-- 17. Modify the previous query to also display the title of the album.
```

`--` is the character sequence to escape a line (or the rest of a line) in SQL. You can write the correct query for each exercise on the line below the question. Remember to save your queries in that file so that you can access them later. 

Remember that if you have a lot of queries in one file, and only want to run one (or a few) of them at once, just highlight the query that you want to run before clicking the execute button or hitting `F5`. 

Up Next: [Creating Databases with SQL scripts](./poki-setup.md)