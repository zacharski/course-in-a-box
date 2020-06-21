## Postgres Getting Started

<iframe width="560" height="315" src="https://www.youtube.com/embed/ubL_WBjatLk" frameborder="0" allowfullscreen></iframe>

### In this video

* Installing Postgres
* Creating databases and basic database commands
* Creating tables and how to show the structure of tables
* Inserting data into a table
* Using SELECT to see the contents of a table


### installing Postgres



installing on Linux 



	sudo apt-get update

	sudo apt-get install -y  postgresql 


Once you do this the postgresql server automatically starts and everytime you boot your machine it will automatically restart. This type of program is called a **service**



A service is simply a program that auto-starts and runs in the background.  For example, a database server like postgresql or Mysql. or a web server like apache or nginx. 

We can see what services we have running by typing the command




	service --status-all


ok back to postgres

The first thing we are going to do is add a password to the root user whose username is `postgres`


	sudo -u postgres psql
	\password postgres

Enter a password and then quit Postgres and log backin with the new password:

	\q
	psql -U postgres -h localhost





This looks great!



### Creating databases and tables

####  Creating a database named library

	CREATE DATABASE library;

Other commands include



	\l -- list databases
	\u -- use a database
	DROP DATABASE library;   -- deletes a database



#### creating a table and inserting data

The syntax for creating a table is


	CREATE TABLE table_name
	(
		column_name_1  DATATYPE,
		column_name_2  DATATYPE
	);




All keywords and identifiers are case insensitive so

	REATE TABLE books
	Create TaBLE BoOkS


are equivalent. Digits are allowed in table names in Postgres but not in the SQL standard so to make your database as portable as possible do not use digits like `CREATE TABLE table_1` 

#### built in datatypes

Let's look at some basic datatypes



| type        | comment                                                      |
| ----------- | ------------------------------------------------------------ |
| INTEGER     |                                                              |
| NUMERIC      | contains decimal                                             |
| TEXT         | obvious  --somebody's name -- a movie title  -- have as many characters as you need |
| CHAR(2)      | exactly 2 characters  there is not advantage to using this                 |
| VARCHAR(30)  | at most 30 characters. Again, no advantage                                                 |
| DATE        | 1990-09-11                                                   |

With that in mind let's create our books table



	CREATE TABLE books
	(
		id INTEGER,
		author TEXT,
		title TEXT,
		acquired DATE
	);




Now to see what tables we have in our database and the structure of each table.



	\d
	\d books




#### deleting a table


	DROP TABLE books;




### Reading in a file

There are two ways to reading in a file.

First create a file called library.sql in your favorite editor (mine is Sublime) with the contents:


CREATE DATABASE library;
\c library

CREATE TABLE books
(
  id INTEGER,
  author TEXT,
  title  TEXT,
  acquired DATE
);




##### method 1:

When you are outside of the psql program you can read in the file from the command line with `-f`:

	psql -U postgres -h localhost -f library.sql


##### method 2:

When you are inside of psql you can use:

	\i library.sql


### Inserting data

Great. now we have a database table but it is empty -- we don't have any data in it. how do we add information to our table? With the insert command:


	INSERT INTO books (id, author, title, acquired) VALUES 
	(1, 'Ann Smith', 'Murder in the Javascript Lab', '2019-03-12');
			





#### How do we see what we inserted?  Using the simplified SELECT command


	select * from books;

Which will show everything we inserted into the table.

##### we can jumble up the order of the columns in an insert


	INSERT INTO books (id, title, acquired, author) VALUES 
	(2, 'Fibonacci Murders', '2019-03-12', 'Ron Zacharski');


and we can leave information out. say we don't know when something was acquired.


	INSERT INTO books (title, author) VALUES ('Recursive W', 'Ann Mulkern');


##### finally you can leave the column list off


	INSERT INTO books VALUES (10, 'Gusty Healy', 'Death by SQL Injection', '2019-05-06');


**this is considered bad practice -- don't do it**

### inserting multiple rows


	INSERT INTO books (id, author, title, acquired) VALUES
	  (12, 'Ann Mulkern', 'Gusty Cooper and Raspberry Pi', '2018-09-12'),
	  (13, 'Ann Mulkern', 'Gusty Cooper and his Electric Bike', '2018-09-12'),
	  (14, 'Ann Mulkern', 'Gusty Cooper and his Repelotron', '2018-09-12');


## you try

come up with something creative.



## Part 2 - Enhancements

 

### Handling I Don't Know. `NULL`



Suppose we want to add the data:


id | author | title | acquired
:---: | :---: | :---: | :---: 
12 | Ann Mulkern |Gusty Cooper and Raspberry Pi | 2018-09-12
13 | Ann Mulkern | Gusty Cooper and his Electric Bike | 2018-09-12
14 | Ann Mulkern | Gusty Cooper and his Repelotron | ???





but we don't know that last date. How do we add it in. we could always add it by itself by doing:


	INSERT INTO books (id, author, title) VALUES (15, 'Dr. D', 'Romance in the OR');


but we could also add a NULL to our insert. 

	INSERT INTO books (id, author, title, acquired) VALUES 
	  (12, 'Ann Mulkern', 'Gusty Cooper and Raspberry Pi', '2018-09-12'), 
	  (13, 'Ann Mulkern', 'Gusty Cooper and his Electric Bike', '2018-09-12'), 
	  (14, 'Ann Mulkern', 'Gusty Cooper and his Repelotron', NULL);

### not null

When we are designing our database table we might think that it doesn't make sense to have one column null. For example, it doesn't make sense for a book not to have a name. We can specify this buy doing:




	CREATE TABLE books_revised (
      id INTEGER NOT NULL,
      author TEXT NOT NULL,
      title TEXT NOT NULL,
      acquired DATE);




Try to add something that has a null in one of those columns and see what happens!



#### SERIAL

We might be getting tired of keeping track of that id number: was the last number we inserted id 21 or 25? Once we start writing our Javascript code, we could have our program keep track but that would be the wrong way to go. Postgres itself can auto increment this number. We do this by using the keyword SERIAL. 



	CREATE TABLE books_new
	 (
	  id SERIAL,
	  author TEXT NOT NULL,
	  title  TEXT NOT NULL,
	  acquired DATE
	);



#### default values

	CREATE TABLE patrons
	(
		id SERIAL,
		name text,
		town text DEFAULT 'Mesilla'
		)