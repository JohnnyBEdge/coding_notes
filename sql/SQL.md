# SQL (structured query language)

Data in relational database management systems (RDBMS) are stored in objects called **tables.** A table is made up of columns called **fields** and rows called **record**. A column is designed to maintain specific info about every record in the table. A record is every entry that belongs to the table. 

## Syntax

Databases have one or more tables of data. Each table is identified by a name. Below is the Northwind  sample datase: 

![image-20201211102817017](/Users/john_home/Library/Application Support/typora-user-images/image-20201211102817017.png)

It contains 5 records and 7 fields.

### Syntax rules

1. SQL keywords are not case-sensitive, but stands
   1. SELECT = select
   2. check with company's standards and follow rules for capitalizing
2. Some DB require **;** after each statement
   1. this is the standard to seperate each statement when DB systems allow for more than one to run at a time.

### Most common/important SQL statements:

- **SELECT** - extracts data from a database
- **UPDATE** - updates data in a database
- **DELETE** - deletes data from a database
- **INSERT INTO** - inserts new data into a database
- **CREATE DATABASE** - creates a new database
- **ALTER DATABASE** - modifies a database
- **CREATE TABLE** - creates a new table
- **ALTER TABLE** - modifies a table
- **DROP TABLE** - deletes a table
- **CREATE INDEX** - creates an index (search key)
- **DROP INDEX** - deletes an index



## SELECT

used to select data from a DB. The data returned in stores in a result table, called the **result-set**

```sql
SELECT column1, column2
FROM table_name;
```

 Here we are selecting two cols from table_name. But if we want everything, we simply write: 

```sql
SELECT * FROM table_name;
```

### SELECT DISTICT

used to return only distinct (non-reoccuring) values. Maybe you would like a list of countries that represent your customers. Of course you might have customers from the same country, but there would be no need to repeat those values. This is where distinct may be of use.

## WHERE

Where allows you to sort your results being returned. It will check records (rows) for conditions that have been set:

```sql
SELECT column, column
FROM table
WHERE condition
	AND/OR another condition
	AND/OR another contion..;

```

More complexes search can be created usiing the AND or OR keywords. Below are some useful operators that can be used in conjunctions with AND/OR:

![image-20201211123448232](/Users/john_home/Library/Application Support/typora-user-images/image-20201211123448232.png)

By filtering, your results become more manageable, you reduce unnecessary data, and queries run faster.

SQL supports a number of useful operators when querying data contain text: 

![image-20201211124729002](/Users/john_home/Library/Application Support/typora-user-images/image-20201211124729002.png)

**ALL STRINGS SHOULD BE QUOTED SO THE QUERY PARSER CAN DIFFERIENTIATE BETWEEN STRINGS AND SQL**

Examples:

Find all the Toy Story movies:

```sql
SELECT * FROM movies
WHERE title LIKE "%toy story%"
```

Find all the movies directed by John Lasseter:

```sql
SELECT title, director FROM movies 
WHERE director = "John Lasseter";
```

Find all the movies (and director) not directed by John Lasseter:

```sql
SELECT title, director FROM movies 
WHERE director != "John Lasseter";
```

Find all the WALL-* movies:

```sql
SELECT * FROM movies 
WHERE title LIKE "WALL-_";
```



## Filtering and Sorting Query Results

Data in databases are rarely neatly sorted and as a result can be difficult to read. SQL has provided a few ways to sort your results using the **GROUP BY** key words with ascending or descending.

```sql
SELECT column, column, column
FROM table
WHERE conditions(s)
ORDER BY column ASC/DESC;
```

With the use of keyswords **LIMIT** and **OFFSET**, you can limit the results to a particular section of results. LIMIT with reduce the number of rows to return and OFFSET will specify where to begin counting the number rows from. These keywords are generally applied after the other clauses.

```sql
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

Ex:

​	Websites like Reddit, the front page is a list of links sorted by popularity and time, and each subsequent page can be represented by sets of links at different offsets in the DB. Using such clauses allow queries to run faster and only return requested data.

Ex:

1. List all directors of Pixar movies (alphabetically), without duplicates

   1. ```sql
      select distinct director from movies
      order by director asc
      ```

2. List the last four Pixar movies released (ordered from most recent to least)

   ```sql
   select title,year from movies
   order by year desc
   limit 4
   ```

   

3. List the **first** five Pixar movies sorted alphabetically

   ```sql
   select title from movies
   order by title
   limit 5
   ```

   

4. List the **next** five Pixar movies sorted alphabetically

   ```sql
   select title from movies
   order by title asc
   limit 5 offset 5
   ```

   



## Multi Table Queries with JOIN

Real world data tables aren't just one, but broken down into pieces and stored across multiple orthogonal tables using a process called **normalization**. It is usefult because it minimizes suplicate data in any single table and llows data in the DB to grow independently. The trade off is that queries get more complex as they have to search more tables.

Tables that share info about a single entity need to have a **primary key** that identifies that entity uniquely aceross the DB. 

```sql
SELECT column, another_table_column
FROM mytable
INNER JOIN another_table
	ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, ... asc/desc
LIMIT num OFFSET num;
```



**INNER JOIN** is a process that matches rows from the first table and second table which have the same key, as defined by the **ON** keyword. 

**INNER JOIN and JOIN are the same, but INNER JOIN can be easier to read, especially when you start learning other types of JOINs**

EX:

1. Find the domestic and international sales for each movie

   ```sql
   SELECT title, Domestic_sales, International_sales
   FROM Movies
   INNER JOIN BoxOffice
       ON Movies.id = BoxOffice.Movie_id
   ```

2. Show the sales numbers for each movie that did better internationally rather than domestically

   ```sql
   SELECT title, international_sales, domestic_sales
   FROM Movies
   INNER JOIN Boxoffice
       ON Movies.id = Boxoffice.Movie_id
   WHERE international_sales > domestic_sales
   ```

   

3. List all the movies by their ratings in descending order

   ```sql
   SELECT title, rating
   FROM movies
   JOIN boxoffice
       ON movies.id = boxoffice.movie_id
   ORDER BY rating DESC
   ```



### OUTER JOINs

INNER JOINs only join data that exists in both tables (in the examples above, both tables had the same ID attached to each title). When data is entered at different times, it can result in asymmetric data. This is where we can use **LEFT JOIN, RIGHT JOIN, or FULL JOIN**.

```sql
SELECT column, another_column, …
FROM mytable
INNER/LEFT/RIGHT/FULL JOIN another_table 
    ON mytable.id = another_table.matching_id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

Like INNER JOIN, you must specify which column to join data on. When joining table A to table B, a **LEFT JOIN** simply includes rows from A regardless of whether a matching row is found in B. Another definition: LEFT JOIN returns all records from the left table, and the matched records from the right table. If no match, result is null from right table

 The **RIGHT JOIN** is the same, but reversed, keeping rows in B regardless of whether a match is found in A. Another def: returns all records from the right table and matched records from left table. Result is NULL from the left side if not match found. 

Finally, a **FULL JOIN** simply means that rows from both tables are kept, regardless of whether a matching row exists in the other table. The FULL OUTER JOIN keyword returns all matching records from both tables whether the other table matches or not. So, if there are rows in "Customers" that do not have matches in "Orders", or if there are rows in "Orders" that do not have matches in "Customers", those rows will be listed as well.

1. Find the list of all buildings that have employees

   ```sql
   SELECT DISTINCT building FROM employees;
   ```

2. Find the list of all buildings and their capacity

   ```sql
   SELECT building_name, capacity
   FROM buildings
   ```

3. List all buildings and the distinct employee roles in each building (including empty buildings)

   ```sql
   SELECT DISTINCT building_name, role
   FROM buildings
   	LEFT JOIN employees
   		ON building_name = building;
   ```

   



### Nulls

BEst practices call for reducing the number of  nulls in a DB because they require special attention when creating queries, constraints, and when processing results.

One alternative is to have a set default value, like 0 for numerical data, empty strings for text, etc.... But if DB needs to store incomplete data, null can be appropriate, say when you are looking for an average and a zero default value would skew that.

We can test columns for NULL values using **IS NULL** and **NOT NULL** in the WHERE clause:

```sql
SELECT column, another_column, …
FROM mytable
WHERE column IS/NOT NULL
AND/OR another_condition
AND/OR …;
```

1. Find the name and role of all employees who have not been assigned to a building

   ```sql
   SELECT name,role,building
   FROM employees
   WHERE building IS NULL
   ```

2. Find the names of the buildings that hold no employees

```sql
SELECT DISTINCT building_name
FROM buildings 
  LEFT JOIN employees
    ON building_name = building
WHERE role IS NULL;
```



## Queries with Expressions

You can use expressions to write more complex logic on column values in a query. EX: 

```sql
SELECT particle_speed / 2.0 AS half_particle_speed
FROM physics_data
WHERE ABS(particle_position) * 10.0 > 500;
```

The use of expressions can save time and reduce post data collecting processing, but can also make the query more difficult to read. To increase readability, you can rename the SELECT using the AS keyword and rename the collected data as something more descriptive.

1. List all movies and their ratings **in percent**

   ```sql
   SELECT title, rating * 10 AS percent
   FROM movies
   JOIN boxoffice
       ON movies.id = boxoffice.movie_id
   ```

2. List all movies and their combined sales in **millions** of dollars 

   ```sql
   SELECT title, (domestic_sales + international_sales)/1000000 as combined_sales
   from Movies
   Join boxoffice
       ON movies.id = boxoffice.movie_id
   ```

3. List all movies that were released on even number years

   ```sql
   SELECT title, year as even_year_release
   from movies
   WHERE year % 2 = 0
   ```



### Queries with Aggregates

SQL also supports the use of aggregate expressions (or functions) that allow you to summarize information about a group of rows of data. 

```sql
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression;
```

Without specified groupings, each aggregate fx is going to run on the whole set of result rows and return a single value.

Common aggregate fx:

| Function                                  | Description                                                  |
| ----------------------------------------- | ------------------------------------------------------------ |
| **COUNT(*****)**, **COUNT(***column***)** | A common function used to counts the number of rows in the group if no column name is specified. Otherwise, count the number of rows in the group with non-NULL values in the specified column. |
| **MIN(***column***)**                     | Finds the smallest numerical value in the specified column for all rows in the group. |
| **MAX(***column***)**                     | Finds the largest numerical value in the specified column for all rows in the group. |
| **AVG(***column*)                         | Finds the average numerical value in the specified column for all rows in the group. |
| **SUM(***column***)**                     | Finds the sum of all numerical values in the specified column for the rows in the group. |

### Grouped Aggregate Fx

You can apply AggFx to individual groups of data within that group. This creates as many results are there are unique groups as defined by the GROUP BY clause:

```sql
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression
GROUP BY column;
```

1. Find the longest time that an employee has been at the studio

   ```sql
   SELECT max(years_employed)
   FROM employees
   ```

2. For each role, find the average number of years employed by employees in that role

   ```sql
   SELECT role, AVG(years_employed) as Average_years_employed
   FROM employees
   GROUP BY role;
   ```

3. Find the total number of employee years worked in each building

   ```sql
   SELECT building, SUM(years_employed) AS total_years_worked
   FROM employees
   GROUP BY building
   ```

The GROUP BY clause is executed after the WHERE clause , which filters the rows that are to be grouped. So to group the filtered rows, we must use **HAVING**.

```sql
SELECT group_by_column, AGG_FUNC(column_expression) AS aggregate_result_alias, …
FROM mytable
WHERE condition
GROUP BY column
HAVING group_condition;
```

**HAVING** clause constraints are written the same way as the **WHERE** clause constraints, and are applied to the grouped rows. <u>HAVING works on aggregates while WHERE does not!</u>

1. Find the number of Artists in the studio (without a **HAVING** clause) 

   ```sql
   SELECT SUM(role="Artist") AS Total_artists
   FROM employees;
   
   or
   
   SELECT role, COUNT(*) as Number_of_artists
   FROM employees
   WHERE role = "Artist";
   ```

2. Find the number of Employees of each role in the studio

   ```sql
   SELECT role, COUNT(*)
   FROM employees
   GROUP BY role;
   ```

3. Find the total number of years employed by all Engineers

   ```sql
   SELECT role, SUM(years_employed) AS total_years 
   FROM employees
   GROUP BY role
       HAVING role = "Engineer";
   ```

   

## Order of Execution

So far we have covered: 

```sql
SELECT DISTINCT column, AGG_FUNC(column_or_expression), …
FROM mytable
    JOIN another_table
      ON mytable.column = another_table.column
    WHERE constraint_expression
    GROUP BY column
    HAVING constraint_expression
    ORDER BY column ASC/DESC
    LIMIT count OFFSET COUNT;
```

First to run are FROM and JOIN clauses, and any subqueries within those. 

Next is the WHERE clause once we have our working set of data and its applied to the individual rows that satisfy the constraints. 

Afterwards, GROUP BY clauses are processed based on common values in the column specified by the group by clause. As a result, there will only be as many rows as unique values in that column. Essentially you should only need to use this when you have aggregate fx in your query.

Next comes HAVING, which is applied to the grouped rows. 

Now any expressions in the SELECT part of the query are finally computed.

Of the remaining rows, DISTINCT will remove any duplicates

If a row is specified by the ORDER BY clause, rows are then sorted by the specified data in either asc.desc order. Since all expressions in the SELECT have been computed, you can reference aliases now.

Finally, LIMIT/OFFSET

1. Find the number of movies each director has directed

   ```sql
   SELECT director, COUNT (*)
   FROM Movies
   GROUP BY director
   ```

2. Find the total domestic and international sales that can be attributed to each director

   ```sql
   SELECT director, SUM(domestic_sales + international_sales) AS gross_sales
   FROM Movies
   JOIN Boxoffice
       ON movies.id = boxoffice.movie_id
   GROUP BY director
   ```



## Inserting Rows

In SQL, a schema is what describes the structure of each table, and the datatypes that each column of the table can contain. This fixed structure is what allows a DB to be efficient and consistent despite storing millions or billions of rows. 

### Inserting New Data

When inserting new data, we use the **INSERT** keyword, which declares a table to write to, the columns of data that we are filling and one or more rows of data to insert. You can insert multiple rows by listing them sequentially:

```sql
INSERT INTO mytable
VALUES (value_or_expr, another_val_or_expr,...),
			(value_or_expr, another_val_or_expr,...),
			...;
```

In some cases, if you have incomplete data and the table contains columns that support default values, you can insert rows with onlt the columns of data you have by specifying them explicitly. In these cases, the number of values need to match the number of columns specified. 

```
INSERT INTO myTable
(column, another_col, ...),
VALUES (val_or_expr, another_val_or_exp,...),
				(val_or_expr, another_val_or_exp,...),
				...;
```

In addition, you can use mathematical and string expressions with the values that you are inserting. This can be useful to ensure that all data inserted is formatted a certain way.

```sql
INSERT INTO boxoffice
(movie_id, rating, sales_in_millions)
VALUES(1, 9.9, 2837442034/1000000)
```

1. Add the studio's new production, **Toy Story 4** to the list of movies (you can use any director)

   ```sql
   INSERT INTO movies 
   VALUES (4, "Toy Story 4", "El Directore", 2015, 90);
   ```

2. Toy Story 4 has been released to critical acclaim! It had a rating of **8.7**, and made **340 million domestically** and **270 million internationally**. Add the record to the `BoxOffice` table.

   ```sql
   INSERT INTO Boxoffice
   (movie_id, rating, domestic_sales, international_sales)
   VALUES(15,8.7, 340000000, 270000000)
   ```



## Updating Rows

Similar to the INSERT statement, **UPDATE** needs to be specified exactly which table, cols, and rows to update. Additionally, the data you add needs to match the datastype set in the schema.

```sql
UPDATE myTable
SET column = val_or_expr,
		other_col = val_or_expr,
		...
WHERE condition;
	
```

At some point, you will make a mistake. One helpful tip is to always write the constraint first and test it in a `SELECT` query to make sure you are updating the right rows, and only then writing the column/value pairs to update.

1. The director for A Bug's Life is incorrect, it was actually directed by **John Lasseter**

   ```sql
   UPDATE movies
   SET director = "John Lasseter"
   WHERE id = 2;
   ```

2. The year that Toy Story 2 was released is incorrect, it was actually released in **1999**

   ```sql
   UPDATE movies
   SET year = 1999
   WHERE id = 3;
   ```

3. Both the title and director for Toy Story 8 is incorrect! The title should be "Toy Story 3" and it was directed by **Lee Unkrich**

   ```sql
   UPDATE movies
   SET title = "Toy Story 3", director = "Lee Unkrich"
   WHERE id = 11;
   ```



## Deleting Rows

you can use a `DELETE` statement, which describes the table to act on, and the rows of the table to delete through the `WHERE` clause.

```sql
DELETE FROM myTable
WHERE condition;
```

If you decide to leave out the `WHERE` constraint, then *all* rows are removed, which is a quick and easy way to clear out a table completely (if intentional).

Like the `UPDATE` statement from last lesson, it's recommended that you run the constraint in a `SELECT` query first to ensure that you are removing the right rows. Without a proper backup or test database, it is downright easy to irrevocably remove data, so always read your `DELETE` statements twice and execute once.



1. This database is getting too big, lets remove all movies that were released **before** 2005.

   ```sql
   DELETE FROM movies
   WHERE year < 2005;
   ```

2. Andrew Stanton has also left the studio, so please remove all movies directed by him.

   ```sql
   DELETE FROM movies
   WHERE director = "Andrew Stanton";
   ```



## Creating Tables

Using the **CREATE TABLE** keywords, you can create a new table for when you have new entities and relationships to store in your DB. 

```sql
CREATE TABLE IF NOT EXISTS mytable (
    column DataType TableConstraint DEFAULT default_value,
    another_column DataType TableConstraint DEFAULT default_value,
    …
);
```

**IF NOT EXISTS** throws an error if a table exists with the name you have chosen. Each column has a name, the type of data allowed in that col, and an optional table constraints on values being inserted, and an optional default value. 

### Data Types

| Data type                                            | Description                                                  |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| `INTEGER`, `BOOLEAN`                                 | The integer datatypes can store whole integer values like the count of a number or an age. In some implementations, the boolean value is just represented as an integer value of just 0 or 1. |
| `FLOAT`, `DOUBLE`, `REAL`                            | The floating point datatypes can store more precise numerical data like measurements or fractional values. Different types can be used depending on the floating point precision required for that value. |
| `CHARACTER(num_chars)`, `VARCHAR(num_chars)`, `TEXT` | The text based datatypes can store strings and text in all sorts of locales. The distinction between the various types generally amount to underlaying efficiency of the database when working with these columns.Both the CHARACTER and VARCHAR (variable character) types are specified with the max number of characters that they can store (longer values may be truncated), so can be more efficient to store and query with big tables. |
| `DATE`, `DATETIME`                                   | SQL can also store date and time stamps to keep track of time series and event data. They can be tricky to work with especially when manipulating data across timezones. |
| `BLOB`                                               | Finally, SQL can store binary data in blobs right in the database. These values are often opaque to the database, so you usually have to store them with the right metadata to requery them. |

### Table Constraints

limit what values can be inserted into the column. Here are a few examples: 

| Constraint           | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| `PRIMARY KEY`        | This means that the values in this column are unique, and each value can be used to identify a single row in this table. |
| `AUTOINCREMENT`      | For integer values, this means that the value is automatically filled in and incremented with each row insertion. Not supported in all databases. |
| `UNIQUE`             | This means that the values in this column have to be unique, so you can't insert another row with the same value in this column as another row in the table. Differs from the `PRIMARY KEY` in that it doesn't have to be a key for a row in the table. |
| `NOT NULL`           | This means that the inserted value can not be `NULL`.        |
| `CHECK (expression)` | This allows you to run a more complex expression to test whether the values inserted are valid. For example, you can check that values are positive, or greater than a specific size, or start with a certain prefix, etc. |
| `FOREIGN KEY`        | This is a consistency check which ensures that each value in this column corresponds to another value in a column in another table.  For example, if there are two tables, one listing all Employees by ID, and another listing their payroll information, the `FOREIGN KEY` can ensure that every row in the payroll table corresponds to a valid employee in the master Employee list. |

1. Create a new table named Database with the following columns:

   – `Name` A string (text) describing the name of the database
   – `Version` A number (floating point) of the latest version of this database
   – `Download_count` An integer count of the number of times this database was downloaded

   This table has no constraints.

   ```sql
   CREATE TABLE IF NOT EXISTS Database(
       Name TEXT, 
       Version FLOAT,
       Download_count INTEGER
   )
   ```



## Altering Tables

If your data changes over time, SQL provides a way for you to update your corresponding tables and database schemas by using **ALTER TABLE** to add, remove, or modify columns and table constraints.

### Adding Columns

​	The syntax for adding a new column is similar to the syntax when creating new rows in the `CREATE TABLE` statement. You need to specify the data type of the column along with any potential table constraints and default values to be applied to both existing *and* new rows. In some databases like MySQL, you can even specify where to insert the new column using the `FIRST` or `AFTER` clauses, though this is not a standard feature.

```sql
ALTER TABLE mytable
ADD column Datatype optionalTableConstraint
	DEFAULT default_value;
```

### Removing Columns

Dropping columns is as easy as specifying the column to drop, however, some databases (including SQLite) don't support this feature. Instead you may have to create a new table and migrate the data over.

```sql
ALTER TABLE mytable
DROP column_to_be_deleted
```

### Renaming the Table

by using the **RENAME TO** keywords:

```sql
ALTER TABLE mytable
RENAME TO new_table_name
```



1. Add a column named **Aspect_ratio** with a **FLOAT** data type to store the aspect-ratio each movie was released in.

   ```sql
   ALTER TABLE movies
   ADD
       COLUMN Aspect_ratio 
       FLOAT
   ```

2. Add another column named **Language** with a **TEXT** data type to store the language that the movie was released in. Ensure that the default for this language is **English**.

   ```sql
   ALTER TABLE Movies
   ADD
       COLUMN Language
       TEXT
       DEFAULT 'English';
   ```



## Dropping Tables

In rare cases, you will need to remove an entire table and its metadata. **DROP TABLE** is what you will use and differs from DELETE in that it also removes the schema.

```sql
DROP TABLE IF EXISTS mytable;
```

In addition, if you have another table that is dependent on columns in table you are removing (for example, with a `FOREIGN KEY` dependency) then you will have to either update all dependent tables first to remove the dependent rows or to remove those tables entirely.





## Advanced Problems and Explanations

**Problem**:  create a simple query to display each unique clan with their total points and ranked by their total points.

```sql
SELECT RANK() OVER(ORDER BY total_points DESC) AS rank, 
p.* FROM 
(SELECT COUNT(DISTINCT(name)) AS total_people, 
 SUM(points) AS total_points, 
 COALESCE(NULLIF(clan,'') ,'[no clan specified]') AS clan 
 FROM people GROUP BY clan) AS p	
```

New keywords:

1. Rank()
   1. Returns the rank of each row within the partition of a result set. Will return duplicates for ties
   2. Syntax
      1.  RANK ( ) OVER ( [ partition_by_clause ] order_by_clause ) 
   3. Order
      1. *partition_by_clause* divides the result set produced by the FROM clause into partitions to which the function is applied. If not specified, the function treats all rows of the query result set as a single group.
2. COALESCE
   1. returns the first non-null value in a list
3. NULLIF
   1. returns null if the two supplied values are not equal
4. problem explanation:
   1. create a rank from total points that are ordered by descending value, name as rank
   2. select all the columsn from p (people) table
   3. select and count the distinct names, name as total people
   4. select points, sum, and name total points
   5. check if clan has no value, return default value if so, name as clan
   6. Group people table, p, by the clans



**Problem** : create a SELECT statement, this SELECT statement will use an IN to check whether a department has had a sale with a price over 98.00 dollars.

```sql
SELECT id, name
FROM departments
WHERE id IN (SELECT department_id FROM sales
	WHERE price > 98)
```

Explanation: 

​	The second select statement creates a list from the sales table where the price is greater than 98.

​	The first where statement looks for matching ids from the department table in the queried sales table.



**Problem**: returns the top 10 customers by total amount spent ordered from highest to lowest. Total number of payments has also been requested. The query should output the following columns:

- customer_id [int4]
- email [varchar]
- payments_count [int]
- total_amount [float]

and has the following requirements:

- only returns the 10 top customers, ordered by total amount spent from highest to lowest

```sql
SELECT c.customer_id, email, COUNT(amount) AS payments_count, CAST(SUM(amount) AS float) as total_amount
FROM customer c
JOIN payment p
  ON c.customer_id = p.customer_id
GROUP BY c.customer_id
ORDER BY total_amount DESC
LIMIT 10
```

New keywords:

 1. CAST

     1. converts a value of any type into a specified datatype.

        Syntax:

        ```
        CAST(value AS desired_datatype(length))
        ```

        1. value is what is to be changed
        2. desired type is what you want
           1. Int for integer, float for decimal etc...
        3. Length is optional, defines the length of the resulting data type (for char, varchar, nchar, nvarchar, binary and varbinary)



## LIKE & ILIKE

These two comparison operators use two special characters:
	-**percent sign(%**: a wildcard matching one or more characters

​	-**underscore(_)**: a wildcard matching just one character		

Example: Trying to find the word 'baker', using the LIKE keyword:

```sql
LIKE 'b%'
LIKE '%ak%'
LIKE '%aker'
LIKE '_aker'
LIKE 'ba_er'
```

The difference between LIKE and ILIKE is that LIKE is part of the ANSI SQL standard and <u>case sensitive</u> while ILIKE is a PostgresSQL-only implementation and is <u>case-insensitive.</u>

**By using ILIKE, we can make sure we are not inadvertently excluding results from searches by assuming whoever entered the data into a table/DB used correct capitalization methods**

