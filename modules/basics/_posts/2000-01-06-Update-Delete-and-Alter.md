## Update, Delete, Alter


<iframe width="560" height="315" src="https://www.youtube.com/embed/ZlF0eItJ4Eo" frameborder="0" allowfullscreen></iframe>


In this video we will learn how to delete rows from a table, how to update rows, and how to alter the structure of a table. 

### files used in this video
To follow along, you will need to download and install 2 sql files:

* [tomato.sql](https://raw.githubusercontent.com/zacharski/devCourse/master/sqlFiles/tomato.sql)
* [world.sql](https://raw.githubusercontent.com/zacharski/devCourse/master/sqlFiles/world.sql)

## DELETE

#### Delete the movie Styx from the database


	DELETE FROM actors WHERE movie = 'Styx'
	
#### Suppose Naomi Scott was not in The Martian. delete that entry

	DELETE FROM actors WHERE firstname = 'Naomi' AND
	lastname = 'Scott' AND movie = 'The Martian';


Be careful and be precise.


##  UPDATE

#### The population of Albuqueue is currently listed in our database as 448607 but it needs to be updated to 558,545

	UPDATE city SET population = 558545 WHERE name = 'Albuquerque';

#### The population of Springfield, Illinois has increased by 10,000. 

UPDATE city SET population = population + 10000 where name = 'Springfield' AND district = 'Illinois';


## SLIDE ALTER -- changes structure of table

	ALTER TABLE <table>
  	DROP COLUMN <columnName>
  	ADD COLUMN <columnName> <type> <attributes>
  	ADD PRIMARY KEY (<columnNames>)
  	RENAME COLUMN <oldColumnName> TO <newColumnName>
  	ALTER COLUMN <columnName> 
  	{SET, DROP} NOT NULL
  	SET DATA TYPE <type>

For this example we are using the file:


	CREATE DATABASE demo;
	\c demo

	CREATE TABLE actors (
		firstname text,
		lastname text,
		born  text,
		age int
	);

	INSERT INTO actors (firstname, lastname, born, age) VALUES
	    ('Valorie', 'Curry', '1986-02-12', 33),
	    ('Bryan', 'Dechart', '1987-03-17', 32),
	    ('Jesse', 'Williams', '1981-08-05', 37),
	    ('Clancy', 'Brown', '1959-01-05', 60),
	    ('Minka', 'Kelly', '1980-06-24', 39),
	    ('Judi', 'Beecher', '1987-11-05', 32);





#### drop column age

	ALTER TABLE actors
	DROP COLUMN age;

#### add column hometown 

	ALTER TABLE actors
	ADD COLUMN hometown text;		


#### add column id  - primary key

	ALTER TABLE actors
	ADD COLUMN id SERIAL,
	ADD PRIMARY KEY (id);

#### rename id column to actor_id

	ALTER TABLE acators
	RENAME COLUMN id TO actor_id;
	
#### set firstname and lastname to not null

	ALTER TABLE actors
	ALTER COLUMN firstname SET NOT NULL,
	ALTER COLUMN firstname SET NOT NULL;

#### remove the not null constraint for firstname

	ALTER TABLE actors
	ALTER COLUMN firstname DROP NOT NULL,

#### change data type of firstname to varchar(20)

	ALTER TABLE actors 
	ALTER COLUMN firstname
	SET DATA TYPE varchar(20);
	
