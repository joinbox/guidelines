# Continous Integration @ Joinbox

Joinbox uses travis-ci for software tests for code executed on the server. It's important that all functionality of all libraries and applications is tested.

## travis with node.js

### .travis.yml

The travis file must contain always up to date directives for the current platform. The test must be executed against the current stable and the current unstable branch of node.js. If the tests require secure configuration paramters they must be added to the .travis.yml file using the [et-travis-tools](https://npmjs.org/package/travis-tools) application.

```yaml
	language: node_js

	node_js : 
	    - "0.10"
	    - "0.11"

	env:
	    global:
	         - seucre: ""
```

In the example configuration above the code is tested against the current stable (0.10) and current unstable branch of node.js (as of 03.12.2013).

### package.json

The package json file must be configured correctly so that tests can be executed by travis-ci

	...
	, "devDependencies": {
        "mocha" : "*"
    }
    , "scripts": {
        "test" : "./node_modules/mocha/bin/mocha --reporter spec"
    }

### tests

When using travis with node the [mocha](http://visionmedia.github.io/mocha/) testing framework must be used. The framework will execute alljavascript file found in the /test direcotry of the project.

A sample test.js file could look like this

	
	var   Class 		= require('ee-class')
		, log 			= require('ee-log')
		, assert 		= require('assert')
		, fs 			= require('fs');

	var project = require('../');


	describe('The Project library', function(){

		it('Should be able to return the project path', function(){
			assert.ok(project.root);
		});

		it('Should be able to return the config.js contents', function(){
			assert.deepEqual(project.config,  {
				  test:   	true
				, passing: 	'test'
			});
		});	

		it('Should be able to return the GIT revision', function(done){
			project.git.revision(done);
		});

		it('Should be able to return the GIT remote', function(done){
			project.git.remote(done);
		});

		it('Should be able to return the GIT remote repository', function(done){
			project.git.remoteRepository(done);
		});
	});


### Executing the tests

The tests can be executed using the «npm test» command in the projects root dir.