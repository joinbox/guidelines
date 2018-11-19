# Joinbox Angular Guidelines
This document describes general guidelines for creating Angular code at Joinbox. The explanations are mainly important for legacy Angular code (_i.e._ 1.6.x versions as used for the SBC project).

## Angular 1.6.x
The following sections describe on how to handle, refactor and create Angular code in the future. It will not explain core concepts. Instead, the following sections explain how and why to move forward with the legacy codebase.

> **Note:** Upgrading the Angular version on a such extensive codebase as the SBC project resides between being impossible and absolutely impossible. So what we are looking for is a way forward that allows us to leverage the old code to be able to benefit from the concepts that make newer Angular versions _better_.  

### Rules of Thumb

Let's start with an overview (please see the other sections if you're not aware of the concepts):

  1. If your project allows it, use `ES6` (precompile your code if necessary).
  1. Name your files consistently!
  1. Bundle logic (especially shared logic) in services!
  1. Write components (not directives)!
  1. Handle errors in your components!
  1. Use bindings wisely especially in presence of deep nesting and loops!
  1. Use `ng-if` as guard for view rendering and to control its timing!

### Components
In general, Angular supports  a variety of [concepts](https://docs.angularjs.org/guide/concepts). The most important one for our legacy code are [components](https://docs.angularjs.org/guide/component). They are the most basic common denominator of Angular 1.x.x and >2.0.0.

Components provide a way cleaner interface than directives did in earlier versions of Angular!

> **Rule of thumb:** Don't write directives! Refactor your directives towards a component. As soon as one has done it once, it will be straight forward (..._most of the time_).   

> **To Clarify:** ~~Where do we manipulate the DOM in components?~~ DOM manipulations happen during the `$postLink` lifecycle hook.  

1. Forget about scope inheritance! It is evil. All components have an isolated scope! All the data that is passed into the component is clearly defined in the `bindings` section of the components definition.
2. Create services to share logic and load data if necessary.

### Structure
Reworking a codebase towards components brings up an old discussion about how to structure Angular code. Some prefer to group the code together based on their type of content (_e.g._ having a `service` folder, a `directive` folder ...). 

Even though this might be subjective, the most:

  - `app.js`
  - `/user`
    - `userProfile.component.js`
    - `userProfile.template.html`
    - `userProfileForm.component.js`
    - `userProfileForm.template.html`
    - `userService.service.js`
    - `userStatusFilter.filter.js`
  - `/common`

How and where your application is served from depends on the project and the rest of the tech stack.

> **Discuss:** We are currently only working using one single module per application. We should probably think about that! It prevents us from sharing code between the website and the back office application.  

### Controllers  and Timing

When using components one can _safely_ write the controllers as ES6 classes (depending on the projects requirements in terms of browser support [Can I use... Support tables for HTML5, CSS3, etc](https://caniuse.com/#feat=es6-class)).

> **Note:** Safely means that I did not encounter cases where it did not work (in contrast to plain directives). Further, we do cross-compile the source code via Babel for IE11 support.  

Components (_i.e._ their controllers) have a well defined [interface for lifecycle hooks](https://code.angularjs.org/1.6.10/docs/guide/component#component-based-application-architecture).  To get  the timing right one can follow a simple template:

```Javascript
class MyComponentController {
	/**
     * Dependencies defined in $inject are passed here.
     */
    constructor($timeout, someService) {
        // assign the dependencies to your controller
        this.$timeout = $timeout;
        this.someService = someService;
    }
    /**
     * $onInit is called if the bindings are resolved.
     * Even though Angular will not `await` your $onInit method you can use async here to simplify your code and control the timing using a guard.
	 */
    async $onInit() {
       const requiredData = await this.someService.loadData();
       // Prepare data.
       // Use myImportantEntity in your view as a guard for the rendering (using ng-if).
       // Since your loading code could take some time and the digest cycles in your view might be stabilized you often have to make sure a new digest cycle is triggered.
       this.$timeout(() => {
			  this.myImportantEntity = entity;
       });
    }
}

/**
 * Define your dependencies in the $inject property as an array of strings.
 * This makes the injection safe if you minify your sourcecode (string content cannot be minified, variable names can and will be renamed).
 */
MyComponentController.$inject = [
	  '$timeout',
    'someService',
];
```

```HTML
  <!-- use the entity as guard to prevent the view from unneccesary rendering -->
    <div class="my-component-container" ng-if="$ctrl.myImportantEntity">
  </div>
```

  1. Go the extra mile and properly pass in the dependencies via an array of strings to avoid problems due to preprocessing. Also keep the naming between the controller's constructor and the `$inject` property consistent (do not rename the injections).
  1. Assign data consumed by the view at the end of your `$onInit` method to the controller. It makes the source of the data more predictable and avoids view rendering in an incomplete state (_i.e._ when not all the necessary data is present yet).
  1. At the end of your `$onInit` method you might have to trigger a digest cycle since the bindings within your view might have stabilised and no further updates are performed.

> **Note:** Handle your errors!

### Error Handling and Logging

In general it is advisable to handle errors, in your components! There is no point of handling errors
in your services (except if you need to normalize the error). Instead, handle them where the code is
consumed and display appropriate error messages to the user within the markup (by using the elements
given by the design of the application _e.g._ the `alerts` in Bootstrap).

Further, you should not log using the console outside your development process (better: use the
[`debugger` statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger))!
A logger should be a centralized service that can be at least appropriately configured for different
environments (_e.g_ the `$log` service). We have to avoid cases such as having tons of debug logs
in a productive environment because they can have negative impacts on performance.

### Bindings and Performance

Besides the classical scope definitions, Angular 1.6.x accepts an extended  (but not really well documented) set of [scope bindings](https://code.angularjs.org/1.6.10/docs/api/ng/service/$compile#-scope-). All component scopes are isolated! In general there is no reason to create two way bindings for data that is not updated.

Bindings in components are defined analogous to the previous scopes in directives:

```Javascript 
{
    /**
     * All bindings can be optional by adding a question mark after the operator:
     *   oneWayBinding: '<?'
     * All bindings can be aliased by adding a name after the operator:
     *   oneWayBinding: '<aOneWayBoundProperty'
     */
    bindings: {
		oneWayBinding: '<',
		twoWayBinding: '=',
        expressionBinding: '&',
        aString: '@',
    }
}
```

  1. **One-Way-Binding:** Use them for values you don't want to change bottom-up. Even though you can propagate changes (they are passed by reference!) don't do it.
  1. **Two-Way-Binding:** Use them for values that need to be synchronised between components. Use them with caution since they will heavily affect  performance of the application!
  1. **Expression-Binding:** Will wrap the expression in a callback and you can call it in your component's controller. This is especially useful for passing callbacks. Be aware that you have to call the function with an object that passes its properties as parameters to the function (tracked by the name of the parameter):
```HTML
  <my-component expression-binding="$ctrl.selectValue(result)"></my-component>
```
```Javascript 
class MyComponentController {
   onValueSelected(selected) {
      // property of the object is called result as specified in the expression
      this.expressionBinding({ result: selected });
   }
}
```
  1. **String-Binding:** interpolated string value

#### One-Time-Binding

A special - but important - case are [one-time bindings](https://docs.angularjs.org/guide/expression#one-time-binding), a an expression syntax used in your views. It will not trigger re-rendering values as soon as the expression has stabilised. **Use it for values that will not change anymore once they are present. This will increase the performance of your Angular app, especially if used in loops.**

Let's add an example here. For SBC we have some sort of language panels for the translation of entities. Each panel renders a form and can be accessed using a tab. Therefore we iterate over the languages we load from the api. These languages will not change in real time, so we use a one time binding (note the double-colon in front of the expression):

```HTML 
<ul>
    <li ng-repeat="language in ::$ctrl.languages">
       <translation-form language="language">
       </translation-form>
    </li>
</ul>
```

#### Ng-if vs. Ng-show

Do not hide content using `ng-show` as long as you really need the content to be processed! Be aware that `ng-show` will hide an element in the DOM but it will be processed by Angular and and impact your performance.

### Testing

> **Discussion:** We do not have a proper testing strategy.  

## General
Discussion of general problems and approaches.

### Global Variables
Now and then one is forced to exchange data between the front- and the backend (_i.e._ Typo3 and Angular). If you have to do so, please stick to the following rules:

1. **Use a namespace:**  Do not pollute the global scope with variables! Define all the configuration and data you have to provide to your Angular application in a Map _e.g._ `window.sbcGlobalConfig = new Map(Object.entries(/*...*/);`.
1. **Document it:** Please document all the variables and their purpose.
1. **Define Constants:** Do not consume global variables in different components of your Angular application!! This will obfuscate the source as well as the consumers of the data and is heavily prone to unpredictable side-effects (_e.g._ if the variable has to be renamed).  Define a [Constant](https://docs.angularjs.org/api/ng/type/angular.Module#constant) during the bootstrapping of your application which can be injected into your components:
```Javascript
angular
    .module('yourmodule')
    .constant('API_URL', applicationConfig.apiUrl);
```