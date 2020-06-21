

## First Normal Form

<iframe width="560" height="315" src="https://www.youtube.com/embed/snp6XDFcMXY" frameborder="0" allowfullscreen></iframe>



In this video we are going to talk about first normal form or 1NF. There are two rules for 1NF. Here they are


### In This Video: Rules for First Normal Form

1. Each row of data must contain atomic values
2. Each row of data must have a unique identifier called the primary key

Let's go through each one by one. 




### The two rules for Atomic Values

1. Data in a column should not be able to be broken down further into parts (for your current purposes)
2. Do not have multiple values of same type in a single row
	1. You can't have several values of the same type in a single column
	2. You can't have multiple columns with the same type of thing.

For the first rule, suppose we want to create a database storing the following information about the characters in the video game *Detroit Become Human*

	The character Kara is played by Valorie Curry, whose hometown was Orange County, CA and was born on Feb. 12, 1986.
	The character Connor is played by Bryan Dechart, whose hometown was St. Lake City, UT  and was born on March 17, 1987.
	The character Markus is played by Jesse Williams, whose hometown was Chicago, IL and was born on August 5, 1981.
	The character North is played by Minka Kelly, whose hometown was Los Angeles, CA and was born on June 24, 1980.
	The character Hank Anderson is played by Clancy Brown, whose hometown was Urbana, OH and was born on January 5, 1959.


### Data in a column should not be able to be broken down further into parts

Consider doing the following:

	CREATE TABLE characters (
		info text
		);


	INSERT INTO characters (info) VALUES 
	  ('The character Kara is played by Valorie Curry, whose hometown was Orange County, CA and was born on Feb. 12, 1986.'),
	  ('The character Connor is played by Bryan Dechart, whose hometown was St. Lake City, UT  and was born on March 17, 1987.');

or some variation of that scheme such as

	INSERT INTO characters (info) VALUES 
	  ('Kara, Valorie Curry,  Orange County, CA,  Feb. 12, 1986.'),
	  ('Connor, Bryan Dechart, St. Lake City, UT, March 17, 1987.');

In both these cases all the data is jammed into one column. Note that we can divide the information in that column into recognizable parts. There is the part that is the character name, the part that is the actor name, and so on. So this representation violates *Data in a column should not be able to be broken down further into parts* and is not in First Normal Form. 

Consider a representation  like the following:


| role | actor | hometown           | born |
| --------- | -------- | -------------- | ----- |
| Kara       | Valorie Curr  | Orange County, CA|  1986-02-12
| Connor  | Bryan Dechart   | St. Lake City, UT   | 1987-03-17
| Markus      | Jesse Williams | Chicago, IL    | 1981-08-05  
| North | Minka Kelly | Los Angeles, CA | 1980-06-24
| Hank Anderson | Clancy Brown | Urbana, OH | 1959-01-05

This looks better. For each column we should ask whether the data in that column could be broken down further, with the caveat of 'for our current purposes'. Maybe for our purposes it would be better to divide the actor column into firstname and lastname. And perhaps the hometown column could be divided into hometown and home state. This results in

| role | firstname | lastname | hometown  | homestate         | born |
| --------- | -------- | -------------- | ----- | ----- | ----- 
| Kara       | Valorie | Curr  | Orange County | CA|  1986-02-12
| Connor  | Bryan | Dechart   | St. Lake City | UT   | 1987-03-17
| Markus      | Jesse | Williams | Chicago | IL    | 1981-08-05  
| North | Minka | Kelly | Los Angeles | CA | 1980-06-24
| Hank Anderson | Clancy | Brown | Urbana | OH | 1959-01-05


That first rule *Data in a column should not be able to be broken down further into parts* is a bit of a wishy washy "it depends" rule. If we might want to search by the homestates of actors, this last representation would be better.  Don't be compulsive about applying this rule. For example, looking at a slightly different table:




| firstname | lastname | address             | city           | state |
| --------- | -------- | ------------------- | -------------- | ----- |
| Ann       | Mulkern  | 1290 Washington Ave | Virginia Beach | VA    |
| Ben       | Rhodes   | 107 Canyon Vista    | Reston         | VA    |
| Matt      | Carriker | 227 FM 207          | San Antonio    | TX    |



we might be tempted to divide that address into the number and street name 



| firstname | lastname | street_number | street_name             | city           | state |
| --------- | -------- | ------------------- | -------------- | ----- | ---- |
| Ann       | Mulkern  | 1290 | Washington Ave | Virginia Beach | VA    |
| Ben       | Rhodes   | 107 | Canyon Vista    | Reston         | VA    |
| Matt      | Carriker | 227 | FM 207          | San Antonio    | TX    |




but for most applications this doesn't matter. At some point you will have to say good enough. And dividing further would be silly. **it's subjective**

Now the hard and fast rule about atomic data.


### Do not have multiple values of same type in a single row

1. you can't have several values of the same type in a single column
2. you can't have multiple columns with the same type of thing.

An example will clarify this. 



Here is some data we want to represent in a database table



	Ann Mulkern works out of our Detroit MI office and knows Python and Javascript
	Ben Rhodes works out of our Detroit MI office and knows Golang and Java
	Clara Wieck works out of our Reston VA office and knows Python and R.
	Samantha Fish works out of our Reston VA office and knows Python, Javascript, and Golang.



One way to represent this is 

| firstname | lastname       | city           | state | languages         |
| --------- | -------------- | -------------- | ----- | -----             |
| Ann        | Mulkern       | Boston         | MA    | Python Javascript |
| Ben        | Rhodes        | Boston         | MA    | Golang Java       |
| Clara      | Wieck         | Reston         | VA    | Python R          |
| Samantha   | Fish          | Reston         | VA    | Python, Javascript, Golang |

However, this violates **condition 1** *A row cannot have several values of the same type in one column*  
since Python and Javascript are the same type (programming languages) and there are two values of the same type in the languages column. 

#### Option 2

Here is a slightly different way to represent the information:

| firstname | lastname       | city           | state | language1   |  language2
| --------- | -------------- | -------------- | ----- | ----------  |   -----     |
| Ann        | Mulkern       | Boston         | MA    | Python | Javascript |
| Ben        | Rhodes        | Boston         | MA    | Golang | Java       |
| Clara      | Wieck         | Reston         | VA    | Python | R          |
| Samantha   | Fish          | Reston         | VA    | Python | Javascript |



This violates **condition 2**  *multiple columns cannot have the same type of data* since columns `language1` and `language2` contain the same type of information, namely programming languages the people know.

Later on we will learn how to design databases that have multiple tables and handle this beautifully. For now we can make this table meet the atomic rule by doing:


| firstname | lastname       | city           | state | language   |  
| --------- | -------------- | -------------- | ----- | ----------  |  
| Ann        | Mulkern       | Boston         | MA    | Python | 
| Ann        | Mulkern       | Boston         | MA    | Javascaript 
| Ben        | Rhodes        | Boston         | MA    | Golang |    
| Ben        | Rhodes        | Boston         | MA    | Java | 
| Clara      | Wieck         | Reston         | VA    | Python | 
| Clara      | Wieck         | Reston         | VA    | R |    
| Samantha   | Fish          | Reston         | VA    | Python |
| Samantha   | Fish          | Reston         | VA    | Javascript | 
| Samantha   | Fish          | Reston         | VA    | Golang | 


This table may look a little wonky but it meets the atomic criteria. 


### first normal form reminder

1. Each row of data must contain atomic values
2. Each row of data must have a unique identifier called the primary key

In this database we don't have a column that contains a unique identifier -- meaning that makes each row unique. Later we will learn other options for primary keys but for now we will make an id column.


id | firstname | lastname       | city           | state | language   |  
| ---| --------- | -------------- | -------------- | ----- | ----------  |  
1 | Ann        | Mulkern       | Boston         | MA    | Python | 
2 | Ann        | Mulkern       | Boston         | MA    | Javascaript 
3 | Ben        | Rhodes        | Boston         | MA    | Golang |    
4 | Ben        | Rhodes        | Boston         | MA    | Java | 
5 | Clara      | Wieck         | Reston         | VA    | Python | 
6 | Clara      | Wieck         | Reston         | VA    | R |    
7 | Samantha   | Fish          | Reston         | VA    | Python |
8 | Samantha   | Fish          | Reston         | VA    | Javascript | 
9 | Samantha   | Fish          | Reston         | VA    | Golang | 



**Now we have a table in 1NF or first normal form.**  



### declaring a primary key

there are two ways to declare a primary key in SQL



The first is on the line itself
	



	CREATE DATABASE library;
	\c library

	CREATE TABLE books_new
	(
	  id SERIAL PRIMARY KEY,
	  author TEXT NOT NULL,
	  title  TEXT NOT NULL,
	  acquired DATE
	);




the second is at the end




	CREATE TABLE users
	(
	  id SERIAL,
	  name text,
	  PRIMARY KEY(id)
	);



You can see the results by using \d to describe the tables.



### summary

so far we

1. learned how to install postgresql on our machine
2. learned how to initially create a password for the default user postgres
3. learned how to create a database
4. learned how to show all databases and to delete dbs.
5. learned how to create and delete tables.
6. learned the basic data types (text, integer, numeric, date)
7. learned how to insert data into a table. 
8. learned about NULL
9. learned about SERIAL
10. learned about first normal form or 1NF.
11. learned how to declare a column to be the primary key -- must be unique
12. learned how to see the structure of a database table \d
13. learned how to see the entries in a table using SELECT.

