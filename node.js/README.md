# node.js coding guidelines

## module & file structure

this is the recommended file structure for all applications. you may add additional folders if you see fit for them, but please keep it clean.

	/lib/ 				-> appllication code
	/test/ 				-> application tests
	/index.js  			-> main entry point for the applciation, references normally a class from the lib directory
	/test.js  			-> test execution
	/README.md 			-> basic documentation
	/package.json 		-> npm instructions
	/.gitignore


all file and foldernames are written in camelcase, normally with the first letter in lowercase ( except for classes ).

## code structure

	


## recommended libraries

You should use the follwoing modules provided by joinbox:

### basic libraries

- ee-class 							-> js class implementation
- ee-event-emitter 					-> event emitter implementation
- ee-types 							-> type checking, dont us typeof
- ee-log 							-> simple & colorful console log replacement
- ee-waiter 						-> control flow: parallel asynchronous job execution
- ee-project  						-> determines the projects root path, loads a config.js if present, exports them
- ee-arguments 						-> assign values passed to a function to a variable by their type and optional by their index
- ee-error 							-> Extends the native error object with a setName & setDescription method


### webservices

- ee-webservice 					-> A framework for Webservices
	- em-api-restrictions 			-> api restrictions middleware for ee-webservice ( apikey, rate limiting )
	- em-multilang					-> multilang middleware for ee-webservice
	- em-session-identity-cookie 	-> cookie handler middleware for em-session-manager
	- em-rest 						-> rest service middleware for ee-webservice
	- em-webfiles 					-> web files middleware for ee-webservice

- ee-formdata-reader 				-> reads formdata from a request


### db access

- ee-mysql-orm 						-> mysql connector
- ee-dynamodb 						-> simple & fast dynamodb access


### tools

- ee-taskscheduler					-> simple task scheduler class
- ee-random-data-provider			-> random data provider, returns cached data
- ee-argv 							-> dead simple commandline parser
- ee-machine-id 					-> unique machine identifier reader ( using network interfaces mac addresses & cpus )
- node-ee-mime-magic 				-> In memory mime sniffer without depencies ( using the files magic numbers )


### data structures

- ee-lru-cache 						-> LRU Memory Cache ( last recent used )
- ee-ttl-queue 						-> ee-ttl-queue
- ee-resource-pool 					-> resource pool with queueing and rate limiting
- ee-rate-limiter 					-> simple rate limiter implementation ( token bucket )


### streams 

- ee-stream-collector 				-> collects all data from a readable stream
- ee-stream-decoder					-> ee-stream-decoder
- ee-stream-url-decoder  			-> stream decodes url encoded data
- ee-stream-url-decoder	 			-> stream decodes url encoded data
- ee-mime-decoder  					-> streaming mime message decoder, supports streaming of attachments & file uploads


### other

- ee-aws-v4-signature 				-> sign requests to amazon
- ee-config 						-> get & set config values using a mysql db
- ee-webserver 						-> simple webserver which can plug into many iridium modules
- ee-aws-v4-request 				-> class for making requests to some of the aws services


it is very important that you implement classes using the provided class implementation. you may extend from native node.js classes.
