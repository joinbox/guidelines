# node.js coding guidelines

## module & file structure

this is the recommended file structure for all applications. you may add additional folders if you see fit for them, but please keep it clean.

	/lib/ 				// appllication code
	/test/ 				// application tests
	/index.js  			// main entry point for the applciation, references normally a class from the lib directory
	/test.js  			// test execution
	/README.md 			// basic documentation
	/package.json 		// npm instructions
	/.gitignore


all file and foldernames are written in camelcase, normally with the first letter in lowercase ( except for classes ).

## code structure

	


## recommended libraries

You should use the follwoing modules provided by joinbox:

- ee-class 				// class implemnetion
- ee-event-emitter 		// event emitter implementation
- ee-types 				// type checking, dont us typeof
- ee-mysql-orm 			// mysql connector
- ee-aws-v4-signature 	// sign requests to amazon
- ee-log 				// simple & colorful console log replacement
- ee-formdata-reader 	// reads formdata from a request
- ee-config 			// get & set config values using a mysql db


it is very important that you implement classes using the provided class implementation. you may extend from native node.js classes.
