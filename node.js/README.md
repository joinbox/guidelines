# Node.js Programming Guidlines for Joinbox Developers

## Module & File Structure

This is the recommended file structure for all applications. You may add additional folders if you see fit for them, but keep it clean.

	/lib/ 				-> application code
	/test/ 				-> application tests
	/docs/				-> docs
	/index.js  			-> main entry point for the applciation, references normally a class from the lib directory
	/test.js  			-> test execution
	/README.md 			-> basic documentation
	/package.json 		-> npm instructions
	/.gitignore


All file and foldernames must be written in camelCase, normally with the first letter in lowercase (except for classes).

## code structure

You should implement the aplications & modules like it's done in the demo project [ee-bookshelf-schema](https://github.com/eventEmitter/ee-bookshelf-schema). It's very important that you use the modules recommended by joinbox. Classes must be implemented using the [ee-class](https://npmjs.org/package/ee-class) module. If you plan to use other modules & libraries you must contact joinbox before you start using them.

You should format the code as described below:


	var MyClass = new Class({

		  CONSTANT: 5
		, variable: {}
		, _privateVariable: 4


		// class constructor
		, init: function(options) {

		}

		/**
		 * the myMethod() method executes ...
		 *
		 * @param <String> name of the ...
		 * @param <Function> callback for ..
		 */
		, myMethod: function(myArgument1, myArgument2) {

		}


		/**
		 * the _privateMethod() method calculates ...
		 *
		 * @param <Function> callback for ..
		 */
		, _privateMethod: function(callback) {

		}

	});


	var myClassInstance = new MyClass();


See the (Stylegiude)[/styleguide/javascript.md] for detailed instructions!



## control flow

*** NEVER USE A SYNCHRONOUS API! NEVER! ***

You may either use events or callbacks for asynchronous processing. If you use callbacks argument 0 passed to the callback must be either null or an Error object. If you use events you must handle errors via the «error» event.

You may never throw exceptions unless its a problem which gets handled at development time. Errors are always passed to callbacks or emitted via the error event.

You must use the «ee-error» npm module for creating Error objects.

	// import the library once per project
	require( "ee-error" );

	// you must use the setName property to set an unique error code on the error object.
	// the code must be camelcase and start with an uppercase letter. 
	var err = new Error( "some error description" ).setName( "SomeErrorException" );


## recommended libraries

List of modules provided & approved by joinbox. Some of the libraries created by joinbox ( e*-* ) will have bugs or not all the required functionality. You may contribute code to the libraries ( all libraries listed below are opensource and MIT licenced ) or file bugreports so that we can fix it asap for you. if you provide code for these modules and you expect us to pay for it you have ask us prior to contributing.


### Basic components

- [ee-class](https://npmjs.org/package/ee-class) 									-> js class implementation
- [ee-event-emitter](https://npmjs.org/package/ee-event-emitter) 					-> event emitter implementation
- [ee-types](https://npmjs.org/package/ee-types) 									-> type checking, dont use typeof
- [ee-log](https://npmjs.org/package/ee-log) 										-> simple & colorful console log replacement
- [ee-async](https://npmjs.org/package/ee-async) 									-> simple control flow helpers
- [ee-project](https://npmjs.org/package/ee-project)  								-> determines the projects root path, loads a config.js if present, exports them
- [ee-arguments](https://npmjs.org/package/ee-arguments) 							-> assign values passed to a function to a variable by their type and optional by their index
- [ee-error](https://npmjs.org/package/ee-error) 									-> Extends the native error object with a setName & setDescription method


### Webservices & Network

- [ee-webservice](https://npmjs.org/package/ee-webservice) 							-> A framework for Webservices
	- [em-multilang](https://npmjs.org/package/em-multilang)						-> multilang middleware for ee-webservice
	- [em-session-identity-cookie](https://npmjs.org/package/em-session-identity-cookie) 	-> cookie handler middleware for em-session-manager
	- [em-rest-service](https://npmjs.org/package/em-rest-service) 					-> rest service middleware for ee-webservice
	- [em-webfiles](https://npmjs.org/package/em-webfiles) 							-> web files middleware for ee-webservice
	- [em-rewrite](https://npmjs.org/package/em-rewrite)  							-> url rewriting middleware for ee-webservice

- [ee-formdata-reader](https://npmjs.org/package/ee-formdata-reader) 				-> reads formdata from a request
- [request](https://npmjs.org/package/request) 										-> make http requests
- [ee-net](https://npmjs.org/package/ee-net) 										-> simple tcp client / server for easy json objects / data transmission
- [ee-request-pool](https://npmjs.org/package/ee-request-pool) 						-> rate limited http requests


### Database Acces & API clients

- [ee-bookshelf-schema](https://npmjs.org/package/ee-bookshelf-schema)  			-> simple schema definitoion for bookshelf.js
- [ee-dynamodb](https://npmjs.org/package/ee-dynamodb) 								-> simple & fast dynamodb access
- [ee-aws-s3-bucket](https://npmjs.org/package/ee-aws-s3-bucket) 					-> Easy AWS S3 Bucket implementation. Upload, download and delete (recursive) files from your buckets.


### Tools

- [ee-taskscheduler](https://npmjs.org/package/ee-taskscheduler)					-> simple task scheduler class
- [ee-random-data-provider](https://npmjs.org/package/ee-random-data-provider)		-> random data provider, returns cached data
- [ee-argv](https://npmjs.org/package/ee-argv) 										-> dead simple commandline parser
- [ee-machine-id](https://npmjs.org/package/ee-machine-id) 							-> unique machine identifier reader ( using network interfaces mac addresses & cpus )
- [node-ee-mime-magic](https://npmjs.org/package/node-ee-mime-magic) 				-> In memory mime sniffer without depencies ( using the files magic numbers )
- [ee-travis](https://npmjs.org/package/ee-travis) 									-> Encode private env varibles for travis using a travis public key
- [ee-simple-repl](https://npmjs.org/package/ee-simple-repl) 						-> Collect user input from a stream / stdin
- [travis-tools](https://npmjs.org/package/travis-tools) 							-> Easy secure data encryption for travis - without the gems!


### Data Structures

- [ee-lru-cache](https://npmjs.org/package/ee-lru-cache) 							-> LRU Memory Cache ( last recent used )
- [ee-ttl-queue](https://npmjs.org/package/ee-ttl-queue) 							-> ee-ttl-queue
- [ee-resource-pool](https://npmjs.org/package/ee-resource-pool) 					-> resource pool with queueing and rate limiting
- [ee-rate-limiter](https://npmjs.org/package/ee-rate-limiter) 						-> simple rate limiter implementation ( token bucket )
- [ee-retry](https://npmjs.org/package/ee-retry) 									-> Repeat some action in an exponential increased capped interval


### Streams 

- [ee-stream-collector](https://npmjs.org/package/ee-stream-collector) 				-> collects all data from a readable stream
- [ee-stream-decoder](https://npmjs.org/package/ee-stream-decoder)					-> ee-stream-decoder
- [ee-stream-url-decoder](https://npmjs.org/package/ee-stream-url-decoder)  		-> stream decodes url encoded data
- [ee-stream-url-decoder](https://npmjs.org/package/ee-stream-url-decoder)	 		-> stream decodes url encoded data
- [ee-mime-decoder](https://npmjs.org/package/ee-mime-decoder)  					-> streaming mime message decoder, supports streaming of attachments & file uploads


### Data Processing

- [ee-xml-to-json](https://npmjs.org/package/ee-xml-to-json) 						-> convert xml documents to json objects, with support for simple transformation rules

### Other

- [ee-aws-v4-signature](https://npmjs.org/package/ee-aws-v4-signature) 				-> sign requests to amazon
- [ee-config](https://npmjs.org/package/ee-config) 									-> get & set config values using a mysql db
- [ee-webserver](https://npmjs.org/package/ee-webserver) 							-> simple webserver which can plug into many iridium modules
- [ee-aws-v4-request](https://npmjs.org/package/ee-aws-v4-request) 					-> class for making requests to some of the aws services



## documentation

- All code must be commented inline so that a skilled third party is able to read the code easily. keep it clean and simple.
- Each application / module / library must have at least one README.md file which describes the api or usage for the current module. 
- Advanced data structures & algorithms must be documented in separate markdown files in the docs folder
- Documentation must be written in github flavoured markdown
- The documentation must be written in english

## testing

Modules / applications must have extensive tests. See the [Testing Guidelines](/CI/Readme.md)

## Contributing

We will provide access to a private git repository where you must commit your work to.

Before you start to code, the functionality and / or the API must be documented in the repositories README.md file. Joinbox must approve your architecture / api / functionality before you may start to implement it. We expect our freelancers to think about what we ask them to do and to improve our concepts.

We expect you to commit all changes you made to the codebase & docs every day you worked on the project.
