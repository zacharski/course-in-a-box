## Querying an SQL Database Part 2


<iframe width="560" height="315" src="https://www.youtube.com/embed/jKBjgVtzElg" frameborder="0" allowfullscreen></iframe>


In this video we continue our exploration of writing queries involving a single table.

### files used in this video
To follow along, you will need to download and install 2 sql files:

* [tomato.sql](https://raw.githubusercontent.com/zacharski/devCourse/master/sqlFiles/tomato.sql)
* [world.sql](https://raw.githubusercontent.com/zacharski/devCourse/master/sqlFiles/world.sql)

## Summary

1. `DISTINCT` - getting unique entries
2. `ORDER BY {ASC/DESC} -  arrange results in a particular order 
2. `LIMIT` - limit the number of rows returned. (for example., in combination with the above -- the top ten cities in terms of population. 
3. `AVG`, `COUNT`, ETC -- summary statistics like average, minimum, max values. (for example, the average tomato rating, or count things like how many movies Robert Downey was in.)
4. `GROUP BY` -- group rows in order to perform some summary statistic (for example, what is the average tomato rating of each actor in our database -- so we are grouping by actor.)
5. `AS` - Column aliases -- ways of renaming the columns of a result. 
6. `IN`, `BETWEEN`, `NOT`  -- syntactic sugar. these don't add to the expressive power but help make our queries simpler.


### ORDER BY

#### Generate an alphabetical list of movies in our database




	SELECT DISTINCT movie FROM actors ORDER BY movie;

#### Generate an alphabetical list of actors in our database


	SELECT DISTINCT firstname, lastname FROM actors ORDER BY lastname, firstname;


####  All cities in the US ordered by population



	SELECT  name, population FROM city WHERE countrycode = 'USA' 
	ORDER BY population DESC;




and for completeness  (ascending order)


	SELECT  name, population FROM city WHERE countrycode = 'USA' 
	ORDER BY population ASC;




### LIMIT


#### Top 10 cities in the US based on population

	SELECT  name, population FROM city WHERE countrycode = 'USA' 
	ORDER BY population DESC LIMIT 10;

	
	
#### Top 5 movies based on t rating

	SELECT DISTINCT movie, trating FROM actors
	ORDER BY trating DESC LIMIT 5;

#### ## SLIDE longest movie in our database?

	SELECT movie, runtime FROM actors WHERE runtime IS NOT NULL
	ORDER BY runtime DESC LIMIT 1;


## Summary Statistics

### COUNT

#### How many cities are there in our datebase?

	SELECT COUNT(*) FROM city';

#### How many cities are there in the US?

	SELECT COUNT(*) FROM city WHERE countrycode = 'USA';


### AVG 
#### What is the average tomato rating of movies with Scarlet Johansson?

	SELECT AVG(trating) FROM actors WHERE firstname = 'Scarlet' 
	AND lastname = 'Johansson';

#### What was the highest tomato rating for a movie that Robert Downey was in?

	SELECT MAX(trating) FROM actors WHERE firstname = 'Robert' 
	AND lastname = 'Downey';

#### What was the movie with the highest tomato rating that Robert Downey was in?

	SELECT movie, trating FROM actors WHERE firstname = 'Robert' 
	AND lastname = 'Downey' ORDER BY trating DESC LIMIT 1;



### GROUP BY

#### How many cities are there in each state?

	SELECT district, COUNT(name) FROM city WHERE countrycode = 'USA' 
	GROUP BY district ORDER BY district;
	
####  How many movies was each actor in?

	SELECT firstname, lastname, COUNT(movie) FROM actors 
	GROUP BY firstname, lastname ORDER BY COUNT(movie) DESC;
	


## SLIDE Syntactic Sugar


### IN


#### An alphabetical list of cities and their populations that are in Texas, Nevada, and Arizona.

The old way using `OR`:

	SELECT name, population FROM city  WHERE district = 'Texas' OR 
	district = 'Arizona' OR district = 'Nevada' ORDER BY name;
an alternative

Using `IN`:

	SELECT name, population FROM city WHERE district 
	IN ('Arizona', 'Texas', 'Nevada') ORDER BY name;

### BETWEEN

#### Cities (and their population) in Texas whose populations are between 100,000 and 200,000

The old way using `AND`:

	SELECT name, population FROM city WHERE district = 'Texas' AND population < 200000 
	AND population > 100000 ORDER BY name;


Using  `between`:

	SELECT name, population FROM city WHERE district = 'Texas' AND 
	population BETWEEN 100000 AND 200000 ORDER BY name;



### NOT

#### How many cities are there in the continental United States?

	SELECT COUNT(*) FROM city WHERE countrycode = 'USA' 
	AND NOT district IN ('Alaska', 'Hawaii');
	

## Aliases

#### The top 10 in terms of population cities in the US (the city and state and population)
We want the names of cities, the state name, and the population in columns named, *city, state* and *population*. 


	SELECT name AS city, district AS state,population FROM city WHERE countrycode = 'USA' order by population desc limit 10;

The `AS` is optional

## Escaping Single Quotes

To have a single quote be interpreted as a character rather than a string delimiter use two single quotes.

	SELECT movie FROM actors WHERE movie LIKE '%''%';
     movie   
	---------------
 	King's Speech
	(1 row)
