## Normalization

### An Introduction to Second and Third Normal Form
<iframe width="560" height="315" src="https://www.youtube.com/embed/60qH6lQtd50" frameborder="0" allowfullscreen></iframe>




### Practice Designing and Querying Multiple Table Databases
<iframe width="560" height="315" src="https://www.youtube.com/embed/fYMK-DyJIuU" frameborder="0" allowfullscreen></iframe>


### First Normal Form:

1. each row contains atomic values
2. each row has a unique identifier - primary key

For example, this table

band | members
 :---: | :---: 
 Coldplay | Chris Martin, Jonny Buckland, Guy Berryman
 Chainsmokers | Andrew Taggart, Alex Pall
 Fall Out Boy | Patrick Stump, Joe Trohman, Pete Wentz
 
does not contain atomic values because the members column has multiple values of the same type (Chris Martin and Jonny Buckland are the same type of thing and there are multiple of those in a cell )

This next table is also not atomic because multiple columns represent the same sort of thing (member1, member2, member3).

band | member1 | member2 | member3
 :---: | :---: | :---: | :---: 
 Coldplay | Chris Martin | Jonny Buckland |Guy Berryman
 Chainsmokers | Andrew Taggart | Alex Pall
 Fall Out Boy | Patrick Stump |  Joe Trohman | Pete Wentz

The following table meets both criteria of First Normal Form (1NF) because all the values are atomic and each row has a unique key. The username is unique and in the table we represent the primary key with PK:

username  (PK)| firstname | lastname | password
:---: |  :---:|  :---: | :---:
daiho | Jill |Miller | 123456
mitsugo | Ann | Mulkern | password
awe | Adam |Waite | qwerty
lingual | Ann | Miller | password

 Synthetic Primary Key is a key that is not part of the original Data. So the `username` column in the table above was not a synthetic primary key but the `id` column in the following table is:

id (PK)| city | state | ST  | population
:---: | :---:  | :---: | :---: | :---:
1 | Santa Fe | New Mexico | NM | 67947
2 | Santa Fe  | Texas | TX |12222
3 | Springfield | Massaschusetts |   MA | 15360
4 | Springfield | Illinois | IL | 11706

Alternatively we can make a two column primary key. So, for example, the city and state columns combined may be unique:

 city (PK) | state (PK)| ST  | population
:---: | :---:  | :---: | :---: 
Santa Fe | New Mexico | NM | 67947
Santa Fe  | Texas | TX |12222
Springfield | Massaschusetts |   MA | 15360
Springfield | Illinois | IL | 11706

This is called a **Composite key** (a key made up of more than one column.)


##### Functional Dependency

The value of one column depends on another column.

##### Partial Functional Dependency
Some non-key column is dependent is dependent on some but not all of the columns of the composite key.

##### Transitive Functional Dependency
A non-key column is related to another non-key column

 So in the above table repeated here:
 

 city (PK) | state (PK)| ST  | population
:---: | :---:  | :---: | :---: 
Santa Fe | New Mexico | NM | 67947
Santa Fe  | Texas | TX |12222
Springfield | Massaschusetts |   MA | 15360
Springfield | Illinois | IL | 11706

We have a functional dependency between the `ST` column and the `state` column. If we change the value in the `state` column we would need to change the value in the `ST` column.  It is a **partial functional dependency** by definition (some non-key column is dependent is dependent on some but not all of the columns of the composite key). 

### Second Normal Form
1. The table needs to be in 1NF
2. The table cannot have any partial functional dependencies. 

### Third Normal Form
1. The table needs to be in 2NF
2. The table has no transitive functional dependencies

Suppose we have a university class schedule:

crn (PK) | course | title | section | credits | days 
:---: | :---: | :---: | :---: | :---: | :---:
10405 | cpsc110 | Introduction to computer science | 4 | 3 |MWF
10406 | cpsc110 | Introduction to computer science | 3 | 3 | TTh
13091 | musp301 | Intermediate Piano | 1 | 1 | TBD
13092 | musp301 | Intermediate Piano | 2 | 1 | TBD

**If a table is in 1NF and has a one column primary key it is in 2NF**. This table is in 2NF but not 3NF. The title column is dependent on the course column and this is a transitive functional dependency. 

The solution is to divide the table into two tables:

**SECTIONS TABLE**

crn (PK) | course | section  | days 
:---: | :---: | :---: | :---: 
10405 | cpsc110 | 4 |  MWF
10406 | cpsc110  | 3 | TTh
13091 | musp301 |  1 | TBD
13092 | musp301 |  2 | TBD

**COURSES**

course (PK) | title | section | days 
:---: | :---: | :---: | :---: 
cpsc110 | Introduction to computer science | 4 | MWF
 cpsc110 | Introduction to computer science | 3 | TTh
 musp301 | Intermediate Piano | 1 |  TBD
 musp301 | Intermediate Piano | 2 |  TBD

In the sections table, the course column is a foreign key.

	CREATE TABLE courses 
		(course TEXT PRIMARY KEY,
		 title TEXT,
		 credits, NUMERIC,
		 si CHAR(1));
		 
	CREATE TABLE sections 
		(crn NUMERIC PRIMARY KEY,
		 course TEXT REFERENCES courses (course),
		 section NUMERIC,
		 days TEXT);
		 
Suppose we want to find the course (cpsc110) and title (Introduction to Computer Science) that meet on TTh:

	SELECT courses.course, courses.title FROM courses 
	JOIN sections ON courses.course = sections.course 
	WHERE sections.days = 'TTh';
	
### VIDEO 2

This table is in 1NF but not 2NF.


band | member | member real name | band formed | birthplace
:---: | :---:  | :---:  | :---:  | :---:  
Imagine Dragons | Dan Reynolds | Daniel Reynolds | 2008 | Las Vegas, NV
Imagine Dragons | Wing Sermon | Daniel Wayne Sermon | 2008 | American Fork, UT
Imagine Dragons |. Daniel Platzman | Daniel Platzman | 2008 | Atlanta, GA
Imagine Dragons | Ben McKee | Benjamin Arthur McKee | 2008 | Forestville, CA
Egyptian | Dan Reynolds | Daniel Reynolds | 2010 | Las Vegas, NV
Egyptian | Aja Volkman | Aja Volkman-Reynolds | 2010 | Eugene, OR

A quick discussion of different types of relations

* **1:1**. (ex, a course like cpsc110 to a course title *Intro to cs* ) Can be represented in 1 table.
* **1: Many**. (ex, a publisher to a book) This requires at least 2 tables.
* **Many : Many** (ex, bands to band members -- bands have multiple band members, and a member can be in multiple bands) This requires a junction table. 3 tables

Consider

##### bands

band-id (PK) | name | genre |yr_formed
:---: | :---: | :---: | :---: 
1 | Imagine Dragons | alternative rock | 2008
2 | Egyptian | alternative rock | 2010
3 | Baby Metal | heavy metal | 2010
4 | Sakura Gakuin | J-Pop | 2010
5 | Earth, Wind & Fire | funk | 1969

##### musicians

musician_id (PK) | member | member_real_name | birthplace
:---: | :---: | :---: | :---: 
1 | Dan Reynolds | Daniel Reynolds | Las Vegas, NV
2 | Wing Sermon | Daniel Wayne Sermon | American Fork, UT
3 | Daniel Platzman | Daniel Platzman | Altlanta, FA
4 | Ben McKee | Benjamin Arthur McKee | Forestville, CA
5 | Aja Volkman | Aja Volkman-Reynolds | Eugene, OR

We connect this table with a junction table:

##### band_members

band_id (PK)| musician_id (PK)
:----: | :----: 
1 | 1
1 | 2
1 | 3
1 | 4
2 | 1
2 | 6

We can represent this as follows:

   DROP DATABASE bands;

    CREATE DATABASE bands;
    \c bands;

    CREATE TABLE band
    (
      band_id integer PRIMARY KEY,
      name text,
      genre text,
      formed integer
    );

    INSERT INTO band (band_id, name, genre, formed) VALUES (1, 'Baby Metal', 'heavy metal', 2010), 
                                             (2, 'Imagine Dragons', 'alternative rock', 2008), 
                                             (3, 'Sakura Gakuin', 'J-pop', 2010), 
                                             (4, 'Egyptian', 'alternative rock', 2010),
                                             (5, 'Earth Wind & Fire', 'funk', 1969);

    CREATE TABLE musician
    (
      musician_id integer PRIMARY KEY,
      stagename text,
      realname text,
      origin text,
      birthdate DATE
    );

    INSERT INTO musician (musician_id, stagename, realname, origin, birthdate) VALUES 
        (1, 'Su-Metal', 'Suzuka Nakamoto', 'Hiroshima Prefecture, Japan', '1997-12-20'), 
        (2, 'MoaMetal', 'Moa Kikuchi', '  Kanagawa Prefecture, Japan', '1999-7-4'),
        (3, 'YuiMetal', 'Yui Mizuno', 'Aichi Prefecture, Japan', '1999-06-20'), 
        (4, 'Aiko Yamaide', 'Aiko Yamaide', 'Kagoshima Prefecture, Japan', '2002-12-01'),
        (5, 'Mariri Sugimoto', 'Mariri Sugimoto', 'Hiroshima Prefecture, Japan', '2000-08-04'), 
        (6, 'Hana Taguchi', 'Hana Taguchi', 'Nagano Prefecture, Japan', '1999-7-4'),
        (7, 'Dan Reynolds', 'Daniel Reynolds', 'Las Vegas, Nevada, United States', '1987-07-14'), 
        (8, 'Wing', 'Daniel Wayne Sermon', '  American Fork, Utah, United States', NULL),
        (9, 'Aja Volkman-Reynolds', 'Aja Volkman-Reynolds', 'Eugene, OR', '1987-01-01'),
        (10, 'Daniel Platzman', 'Daniel Platzman', '  Atlanta, Georgia, United States', '1986-09-28'),
        (11, 'Ben McKee', 'Benjamin Arthur McKee', 'Forestville, California, United States', '1985-04-07');

    create table bandmember (
      band_id integer REFERENCES band(band_id),
      member_id integer REFERENCES musician(musician_id),
      PRIMARY KEY (band_id, member_id));
     
    INSERT INTO bandmember VALUES (1, 1), (1, 2), (1, 3), 
         (2, 7), (2, 8), (2, 10), (2, 11),
         (3, 1), (3, 2), (3, 3), (3, 4), (3, 5), (3, 6),
         (4, 7), (4, 9);

    create table instrument (
      id integer PRIMARY KEY,
      name text,
      family text
      
      );

    insert into instrument (id, name, family) VALUES (1, 'vocals', 'voice'), 
       (2, 'screams', 'voice'), (3, 'dance', 'dance'),
       (4, 'koto', 'string'), (5, 'electric guitar', 'string'), (6, 'drums', 'percussion'), (7, 'bass', 'string'), (8, 'keyboards', 'keyboards');

    create table plays (
      member_id integer REFERENCES musician(musician_id),
      instrument_id integer REFERENCES instrument(id),
      PRIMARY KEY (member_id, instrument_id));
     
    insert into plays VALUES (1, 1), (1, 3), (2, 2), (2, 3), (3, 2), (3,3), 
       (7, 1), (7, 8), (8, 5), (9, 1), (9,8), (10, 6), (10,1), (11, 7); 
       

##### Queries

###### finding all the band members of Imagine Dragons

	SELECT musician.stagename FROM musician 
	JOIN bandmember ON 
	musician.musician_id = bandmember.member_id 
	JOIN band ON 
	bandmember.band_id = band.band_id 
	WHERE band.name = 'Imagine Dragons';
    
    stagename    
	-----------------
 	Dan Reynolds
 	Wing
 	Daniel Platzman
 	Ben McKee
	(4 rows)

##### What instruments does BenMcKee

	SELECT instrument.name FROM instrument 
	JOIN plays ON 
	instrument.id = plays.instrument_id 
	JOIN musician  ON 
	musician.musician_id = plays.member_id 
	WHERE musician.stagename = 'Ben McKee';




### Queries that require multiple copies of a table

Suppose as part of the bands database we have a table specifying who is friends with whom. 

    CREATE TABLE friends (
    person INTEGER REFERENCES musician(musician_id),
    friends_with INTEGER REFERENCES musician(musician_id));
    
     INSERT INTO friends (person, friends_with) VALUES
      (7, 8),
      (7, 9),
      (9, 11);   

For example, the first insert represents that Dan Reynolds is friends with Wing. Let's consider how we would write a query to find out who is Dan Reynolds friends with. At first we might think to write:

    SELECT musician.stagename FROM musician 
    JOIN friends ON musician.musician_id = friends.person 
    WHERE musician.stagename = 'Dan Reynolds';

You will see that this doesn't work as all it returns is `Dan Reynolds`

For this query we actually need two copies of the musicians table-- one to find the musician_id of Dan Reynolds. Once we find out his id is 7, look at the friends table and discover he is friends with id 8, we need another table to lookup the stagename for id 8. We implement it as follows:

    SELECT m1.stagename FROM musician as m1 
    JOIN friends ON m1.musician_id = friends.friends_with 
    JOIN musician as m2 ON friends.person = m2.musician_id 
    WHERE m2.stagename = 'Dan Reynolds';

           stagename       
     ----------------------
      Wing
      Aja Volkman-Reynolds
     (2 rows)


Suppose we want to know who is in bands with Dan Reynolds. For that the query is


    SELECT m1.stagename FROM musician as m1 
      JOIN bandmember AS b1 ON m1.musician_id = b1.member_id 
      JOIN bandmember as b2 ON b1.band_id = b2.band_id 
      JOIN musician AS m2 ON m2.musician_id = b2.member_id 
      WHERE m1.stagename <> 'Dan Reynolds' 
      AND m2.stagename = 'Dan Reynolds';
    


> Question: why do we have `m1.stagename <> 'Dan Reynolds'` ?

