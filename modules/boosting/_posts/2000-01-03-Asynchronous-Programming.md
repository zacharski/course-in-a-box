## Asynchronous Javascript Programming

<iframe width="560" height="315" src="https://www.youtube.com/embed/AvQhQ6RHEyQ" frameborder="0" allowfullscreen></iframe>

### Javascript event loop

Javascript whether it runs in the browser or on the server executes an event loop.
Javascript runs as a single process and handles those events.

### asynchronous programming

### Callback Functions
> Keep going -- don't wait for me.


#### Callback example using setTimeout

	/* CALLBACK - 
    setTimeout(function, milliseconds); */
	function getData(ms, query) {
		setTimeout(function() {
			console.log(query);
		}, ms);
	}
	function query() {
		getData(3000, "Unicorn Store");
		console.log("A1");
		getData(1000, "Captain Marvel");
		console.log("A2");
	}
	query();

#### Callback example using processFile

	// callback example
	var fs = require("fs");
	function processFile(filename) {
		fs.readFile(filename, { encoding: "utf8" }, (error, data) => {
			if (error) {
				console.log(error);
				return;
			}
			//now we have the data
			console.log("ProcessingComplete");
		});
	}
	console.log("A1");
	processFile("/Users/raz/data/glove.6B/glove.6B.50d.txt");
	console.log("A2");


Some points to remember:
* Writing asynchronous code with callbacks requires a fair amount of code. As we will see in a second there are easier, less verbose ways of writing asynch. functions.
* Aysynchronous code is infectious. Recursive. 

### Basic Synchronous Example

	/* demo of synchronous calls 
	   using a fake db query */

	function sleep(ms) {
		const start = Date.now();
		while (Date.now() - start < ms) {}
	}

	function dbQuery(q, ms) {
		sleep(ms);
		if (q == "SELECT unicorn") {
			return "Unicorn Store";
		} else {
			return "Captain Marvel";
		}
	}

	function queryDB(query, ms) {
		let result = dbQuery(query, ms);
		console.log(result);
		return;
	}

	function doIt() {
		queryDB("SELECT unicorn", 3000);
		console.log("B");
		queryDB("SELECT captain marvel", 1000);
		console.log("C");
	}

	doIt();


### Promises Example


	function dbQuery(q, ms) {
		return new Promise(resolve => {
			setTimeout(() => {
				if (q == "SELECT unicorn") {
					resolve("Unicorn Store");
				} else {
					resolve("Captain Marvel");
				}
			}, ms);
		});
	}

	function queryDB(query, ms) {
		dbQuery(query, ms)
			.then(result => {
				console.log(result);
			})
			.catch(error => console.log(error));
	}

	function doIt() {
		queryDB("SELECT unicorn", 3000);
		console.log("B");
		queryDB("SELECT captain marvel", 1000);
		console.log("C");
	}

	doIt();


### Async and Await

	function dbQuery(q, ms) {
		return new Promise(resolve => {
			setTimeout(() => {
				if (q == "SELECT unicorn") {
					resolve("Unicorn Store");
				} else {
					resolve("Captain Marvel");
				}
			}, ms);
		});
	}

	async function queryDB(query, ms) {
		let result = await dbQuery(query, ms);
		console.log(result);
		return;
	}

	function doIt() {
		queryDB("SELECT unicorn", 3000);
		console.log("B");
		queryDB("SELECT captain marvel", 1000);
		console.log("C");
	}

	doIt();


