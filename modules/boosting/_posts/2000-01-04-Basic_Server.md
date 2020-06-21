## A Basic Server in Node.js

<iframe width="560" height="315" src="https://www.youtube.com/embed/iXi5G1P6Gns" frameborder="0" allowfullscreen></iframe>

In this video we are going to ignore PostgreSQL for a moment and just learn how to get a simple nodejs server working. 

After that, we will add in postgres.


### HTTP Requests
Initially, we will be focusing on two types of requests:

#### Get Requests
This request is used when the request is idempotent meaning that the request results in no changes to the server or database. An example of this would be a search request--we just query the database for the search results and nothing changes in the database or server.

#### Put Requests
This request is used when changes are made to the database. For example, when the request results in adding something to the database.  

### CRUD
The acronym CRUD refers to all of the major functions that are implemented in persistant storage including relational database applications. 

* CREATE
* READ
* UPDATE
* DELETE 

Each letter in the acronym can map to a standard Structured Query Language (SQL) statement, and the related http requests

CRUD | SQL | HTTP Request
:---: | :---: | :---: 
CREATE |INSERT  POST
READ | SELECT | GET
 UPDATE | UPDATE | PUT (or POST)
DELETE  | DELETE | DELETE




### The code


#### Step 1
Create a new directory

	mkdir campgrounds
	cd campgrounds

and 

	npm init

then 

	npm install --save pg express 


This step installs express

**express** is a minimal node web application framework. It is designed to build web applications that use the requests we just discussed. POST, GET, DELETE. we will see it in action in a few moments.



#### Step 2 Create index.js  -- Hello world
using your favorite editor


	const express = require("express");

	const app = express();

	app.set("port", 8080);

	app.get("/hello", (req, res) => {
		res.json("Hello World!");
	});

	app.listen(app.get("port"), () => {
		console.log(`Find the server at: http://localhost:${app.get("port")}/`);
		 // eslint-disable-line no-console
	});


To run the code type

	node index.js

Point your browser to `http://localhost:8080`

#### Step 3 - a get request

Just a quick demo printing out the query parameters: 

	app.get("/vacancy", (req, res) => {
		console.log(req.query);
	});
	
then the real code

	app.get("/vacancy", (req, res) => {
		console.log(req.query.q);
		const campground = req.query.q;
		if (campground == "Saddle") {
			res.json({ campground: campground, sites: 5 });
		} else {
			res.json({ campground: campground, sites: 0 });
		}
	});

Try it out with `http://localhost:8080/vacancy?q=Saddle`


#### Step 4 - Post request

	const bodyParser = require("body-parser");
	app.use(bodyParser.json({ type: "application/json" }));
	app.use(bodyParser.urlencoded({ extended: true }));

	app.post("/reserve", (req, res) => {
		console.log(req.body);
		const first = req.body.firstname;
		const last = req.body.lastname;
		const campground = req.body.campground;
		const sites = req.body.sites;
		res.json({ reservation_number: 1, campground: campground });
	});