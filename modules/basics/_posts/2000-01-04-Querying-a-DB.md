## Querying a Database: The SELECT statement


<iframe width="560" height="315" src="https://www.youtube.com/embed/ScIHpVrSwwY" frameborder="0" allowfullscreen></iframe>


In this video we learn about writing queries involving a single table.

### files used in this video
To follow along, you will need to download and install 2 sql files:

* [tomato.sql](https://raw.githubusercontent.com/zacharski/devCourse/master/sqlFiles/tomato.sql)
* [world.sql](https://raw.githubusercontent.com/zacharski/devCourse/master/sqlFiles/world.sql)

### Basic Query Patterns

##### Simple WHERE clause

	SELECT <columns> FROM <table> WHERE <condition>;
	SELECT * FROM books WHERE author = 'Ann Mulkern';
    SELECT author, title FROM books WHERE publisher = 'Wisdom';
    SELECT movie FROM movies WHERE tomatorating > 90;

##### and / or
	SELECT <columns> FROM <table> WHERE <condition>;
	<condition> = <columnName> = <value>
	<condition> = <condition> AND <condition>
	<condition> = <condition> OR <condition>
	SELECT movie FROM tomato WHERE firstname = 'Anne' AND lastname = 'Hathaway';

##### DISTINCT

	SELECT DISTINCT <columns> FROM <table> WHERE <condition>;
	SELECT DISTINCT firstname, lastname from tomato;


##### LIKE / ILIKE  and the wildcard %

	<condition> = <columnName> LIKE <value>
	<condition> = <columnName> ILIKE <value>    *case insensitive* 
	SELECT movie FROM tomato WHERE movie LIKE 'S%';
	SELECT movie FROM tomato WHERE movie ILIKE '%avenger%';

**Patterns**

Pattern | Description
--- | ---
`A%` | matches any string that start with an 'A' 
`%ed` | matches any string that ends with 'ed'
`A%ed` | matches any string that starts with 'A' and ends with 'ed'
`%z%` | matc hes any string that contains a 'z' (can start with, end with, or contain a z)


##### IS NULL / IS NOT NULL

	<condition> = <columnName> IS NULL
	<condition> = <columnName> IS NOT NULL
    SELECT movie FROM tomato where release-date IS NULL;

##### ORDER BY 

	SELECT <columns> FROM <table> WHERE <condition> ORDER BY <columnNames>;
	SELECT movie FROM tomato ORDER BY movie;
	SELECT firstname, lastname FROM tomato WHERE movie = 'Avengers' ORDER BY lastname, firstname;

