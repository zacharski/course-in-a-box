## Introduction to Javascript

<iframe width="560" height="315" src="https://www.youtube.com/embed/1gZK1tMy1sI" frameborder="0" allowfullscreen></iframe>



This session is intended as an introduction to Javascript for programmers. If you don't have experience with a programming language this intro might be challenging. If you are a beginner there are some excellent Javascript resources online. 

Two good ones:

* [**JavaScript for impatient programmers**](https://exploringjs.com/impatient-js/index.html) by Dr. Axel Rauschmayer
* [**Eloquent Javascript**](https://eloquentjavascript.net/) by Marijn Haverbeke.


### Basic Information

Javscript was originally developed to run in the browser to make HTML slightly interactive and to provide a bit of glue for HTML components.

It was developed by Brendan Eich at Netscape. At that time Netscape had about 2/3 of the browser market and they wanted a programming language that could run in the browser.  Brendan developed the prototype in 10 days and within 9 months Javascript was part of the Netscape Browser.  He was originally hired to port Scheme to the Netscape Browser but there were some internal conflicts at the company and they decided to go with a more traditional syntax. Brendan refers to Javascript as 'Scheme-ish'




### Let's go ahead and install node

	sudo apt-get install nodejs
	
	node -v

### What is node?

* Javascript was originally designed to be run in a browser -- client side. 
* node is a Javascript runtime environment that allows javascript to be run outside of the browser
* in addition it provides a number of tools for creating server side scripts. -- javascript can serve up web pages or deliver content.

### Basic Syntax



#### Comments

	// one line comment
	
	/* Multi line 
	comment */
	
and to print something we have

	console.log('Javascript');

save file and demo

####   Variables

There is `let` and `const`

	let x = 5;
	x = x + 2;
	console.log(x);
	

**Variables declared with `let` are mutable** -- we can change them as shown above
	
vs.

	const  x = 5;
	x = x + 2;
	console.log(x);


which results in an error. Remember:

**variables declared with `const` are immutable**


With `let` you can declare a variable without the initial assignment

	let x;
	x = 5;
	

You cannot do this with `const`

With `const` you cannot change it to point to a different object but  the values within the object can change


	const ann = {'name': 'Ann'}
	ann['age'] = 25;
	


** Scope rules are similar to other programming languages**

####  The let/const rule:

Use `let` only when you cannot use `const`

#### Values and Types
The basic datatypes in Javascript are
* Numbers (there are no integers in Javascript)
* Strings
* Arrays
* Objects
* Undefined
* Null

### Control Flow

####  if statement

The pattern is:
	
	if (cond){
	
	}
	else if () {
	}
	else {
	}

Here is an example:

	let x = 1;
	if (x%2 == 0){
		console.log('Even')
	}
	else {(}
		console.log('Odd')
	}

####  For loops

For loops are identical to those in other languages:

	for (let i=0; i<3; i++) {
 		 console.log(i);
	}


#### FizzBUzz

Hard to believe but people say 99.5% of software developer job applicants cannot program this.

> Write a program that prints the numbers from 1 to 100. But for multiples of three print “Fizz” instead of the number and for the multiples of five print “Buzz”. For numbers which are multiples of both three and five print “FizzBuzz

This example uses a for loop and if statements.

	// Fizz Buzz
	for (let i = 1; i <= 100; i++){
		if (i % 15 == 0){
			console.log('FizzBuzz');
		}
		else if (i % 3 == 0){
			console.log('Fizz');
		}
		else if (i % 5 == 0){
			console.log('Buzz');
		}
		else{
			console.log(i);
		}


	}

#### For Of loop

Javscript also has a `for of` loop similar to the `for in` loop in Python.

	let x = ['Ann', 'Ben', 'Clara', 'Dan'];
	for (const friend of x){
		      console.log(friend);
	}
	

#### while

The while loop is no different than any other programming language.

	let i = 1;
	while (i < 10) {
	  console.log(i++);
	} 


#### do while

The do-while loop is also the same as other programming languages. 

	let i = 1;
	do {	  
	  console.log(i++);
	} while (i < 10);


#### Assert
Assertions are common in programming languages. They state facts about conditions that must be true. If they are not true an exception is thrown.

Node.js supports assertions by its `assert` module.

	const assert = require('assert').strict;
	let x = 8;
	assert.equal(x, 8);
	
or   

	const assert = require('assert').strict;
	let x = 15;
	assert((x > 1) && (x < 10));





#### exceptions

##### try catch

	var fs = require('fs');
 
    try {
	var contents = fs.readFileSync('friend.js', 'utf8');
	console.log(contents);
	}
    catch(e){

	console.log("ERROR! ", e);
   }

##### throw

	var fs = require('fs');
	let prompt = 'W';

	try {
		if ((prompt != 'L') && (prompt != 'R'))
	    throw new Error("Invalid Direction");
	    	}
	catch(e){

		console.log("ERROR! ", e);
	}
	
##  Functions

#### ordinary functions

	const assert = require('assert').strict;

	function sum(a, b, c){
		let result = a + b + c;
		return result;
	}

	assert(sum(1, 2, 3), 6);
	console.log(sum(1, 2, 3));
	
parts of the function

* sum is the name of the function
* sum(a, b, c) are called the head of the function declaration
* a, b, c are called the parameters
* the body of the function is everything within the braces
* the return statement returns a value

#### Three roles of a function

##### can be invoked as a function call

as shown in example above

##### stored in a property of an object invoked via method call

	const obj =  {methodSum: sum};
	assert.equal(obj.methodSum(1, 2, 3), 6);
	
##### constructor function via new

	const inst = new add();
	assert.equal( inst instance of add, true);


### Arrow functions

	const arrow = (a, b, c) => {
	   let result =  a + b + c;
	   return result;

	}

	assert(arrow(1, 2, 3), 6);
	console.log(arrow(1, 2, 3));


The role of the arrow function is to be a regular functions. 


### Method functions

	const object = {sumMethod(a, b, c) {
   	 let result =  a + b + c;
   	 return result;

 	 }



