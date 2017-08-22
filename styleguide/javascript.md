# Joinbox Style Guide
Date of latest change: August 22, 2017

## Basics

Use Airbnb's [style  guide](https://github.com/airbnb/javascript)

## Joinbox-Specific Changes

- 4.6 If an array has multiple lines, you may set the brackets for the containing structures on the same line as the surrounding brackets:

    ````javascript
    // good
    const objectInArray = [{
        id: 1,
    }, {
        id: 2,
    }];
    ````

- 6.2 You may also use template strings for long strings: 

    ````javascript
    //good
    const errorMessage = `This is a super long error that was thrown because 
        of Batman. When you stop to think about how Batman had anything to do
        with this, you would get nowhere fast.`;
    ````
    
- 7.1 You may also use function declarations – but know what you are doing (hoisting)
    ````javascript
    // good
    function foo() {
    }
    ````
    
    You may use function expressions but not named function expressions with two separate names. 
    The only reason for using named function expression is that they are easier debuggable. 
    node, by default, creates named function expressions out of function expressions. babel 
    does this too.
    ````javascript
    // good
    const sayHi = function() {
    }
    ````
    
    ````javascript
    // barely okay
    const sayHi = function sayHi() {
    }
    ````
    
    ````javascript
    // bad
    const sayHi = function hi() {
    }
    ````
    
- 16.2 You may use line breaks before `else` statements:
    ````javascript
    // also good
    if(test) {
        thing1();
    }
    else {
        thing2();
    }
    ````

- 18.4 Don't use FIXME comments – fix the problem instead. TODO may be used when it helps on a feature that will be implemented later. 
    ````javascript
    // bad
    const value = '15';
    // TODO: Add radix
    const total = parseInt(value);

    // bad
    const value = '15';
    // FIXME: parseInt sometimes returns wrong value
    const total = parseInt(value);

    // okay
    const value = '15';
    // TODO: More detailed error messages when value may be set by the users
    const total = parseInt(value, 10);
    ````

- 19.1 Use soft tabs set to 4 spaces.

    > Why? Because it's easier to differentiate indentations with 4 (instead of 2) spaces.

    ````javascript
    // good
    function foo() {
    ∙∙∙∙let name;
    }
    ````

- 19.8 If it improves readability and code structuring, you may use extensive blank lines in and outside of (long) blocks. No empty lines before closing curly braces.

    ````javascript
    // good
    function foo() {
        
        // Get the data
        fetch('/api')
            .then((data) => data.json());
            .then((json) => this.data = json);
        
        // Work with data
        this.data
            .filter((item) => item.visible === true)
            .forEach((item) => {
                this.collection.addVisible(item);
            });
    }
    ````

- 20.1 Leading commas: Yep.

    > Why? Because it's easy to read.

    > Why? Because you can remove a line and instantly recognize problems with commas.

    > Why? Because trailing commas cause [problems](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas) in legacy browsers.

    ````javascript
    // good
    const story = [
        once
        , upon
        , aTime
    ];

    // bad
    const story = [
        once,
        upon,
        aTime,
    ];
    ````

- 23.4 You shall use leading underscores to mark functions or variables as private. They should not be accessed from the outside.

    > Why? Because it allows for clearer interfaces and internal changes that don't break the component's functionality. 

    > Why? Because private fields [are coming](https://tc39.github.io/proposal-private-fields/) but not yet here.

    ````javascript
    // good
    class PrivatesAndPublics {
    
        constructor() {
            // public property
            this.addresses = [];
            // private property
            this._countries = new Map();
        }
        
        // Public method
        addAddress(address) {
            if (!this._validateAddress()) throw(`Address ${ JSON.stringify(address) } not valid.`);
            // Update list of countries
            if (!this._countries.has(address.country.short) {
                this._countries.set(address.country.short, address.country);
            }
            this.addresses.push(address);
        }
        
        // Private method
        _validateAddress(address) {
            return (
              address.zip % 1 === 0 && 
              address.hasOwnProperty('country') &&
              address.country.short)
        }    
    
    }
    ````

- 23.6 Use camelCase for all file names

    > Why? Because most libraries do so.

    ````javascript
    // good (class)
    import ClassName from './className';
    ````

- 23.8 Use camelCase for exported objects

    > Why? Because [objects are camelCased](https://github.com/airbnb/javascript#naming--camelCase)

    ````javascript
    // good (class)
    import ClassName from './className';
    ````

- 28.2 You may use decorators (even though it's a Stage 2 draft)

    > Why? Because they're the future, too.

    ````javascript
    // good
    class Memoized {
        @memoize
        makeComplexComputation() {
        }
    }
    ````
    
    Async/await are ES2017 – use and understand them.



## Basic Considerations

#### Libraries

- Don't use third party libraries where there are native JavaScript solutions. Prefer polyfills over libraries. 

    > Why? Because libraries tend to contain more bugs than pure JavaScript.
    > Why? Because libraries tend to require updates that need testing. 
    > Why? Because libraries tend to lose support with time.

    ````javascript
    // bad
    if (_.isUndefined(foo)) {}
    
    // good
    if (foo === undefined) {}
    
    // bad
    myXHRLibrary.makeXHR('api')
        .then((status, body, headers) => console.log(status));
    
    // good – use polyfill for old browsers
    fetch('api')
        .then((data) => console.log(data.status));
    
    // bad
    const element = $('.elementClass');
    
    // good
    const element = document.getElementsByClassName('.elementClass');
    
    // bad
    function doTwoThings() {    
        return async.parallel([
          foo()
          , bar()
        ], (err, result) => result)
    }

    // good
    function doTwoThings() {    
        return Promise.all([
          foo()
          , bar()
        ]
        .then((data, err) => data);
    }
    ````
    
    // bad
    const leftPad = require('leftpad');
    
    class Something {
        leftPad(input, length, char = ' ') {
            return leftPad(input, length, char);
        }
    }

    // good
    class Something {
        leftPad(input, length, char = ' ') {
            if (input.length >= length) return input;
            else return char.repeat(length - input.length)+input;
        }
    }
    ````

 - Only use libraries with licenses that match the project, that are well supported, well tested and have a large user base.
 - Install libraries through package managers. Prefer npm over bower. 


### Don't use babel for node

 - Never compile or transpile sources for node


### Path Forward

- If you find the time, the project budget allows for it and the project's life cycle is still young, do small refactorings of files you touch. 
- As lead developer, find and define a way forward together with the development team (e.g. introduce ES2016 and make sure it's backwards compatible). Use the new syntax for all new components and existing components you touch. 

### Don't Overengineer 

- Prefer simple and well readeable code over fancy solutions. If a complex solution is needed, document it well.

