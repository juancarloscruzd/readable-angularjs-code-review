I'm not the author of this amazing code review checklist.
[Source](https://angularcodereview.com/angularjs/)

#Code Style

## Is all code intention-revealing?

##### What?
All code should be easy to understand for other developers
##### Why?
To make sure the code can be updated, refactored and maintained by others if needed

## Is all code linted/hinted?
##### What?
All code should be syntactically correct
##### Why?
To avoid unpredictable errors

[jshint example](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md#js-hint)

## Is `controller as` syntax used?
##### What?
- Use controllerAs
- Use bindToController

##### Why?
Avoid pitfalls from scope soup
Target outer scopes easily from within inner scopes

## Is DOM manipulation handled only by directives?
##### What?
- DOM manipulation should only happen in directives
- Use the element available in the directive compile and link function to manipulate the DOM
- No DOM manipulation from controllers or services

##### Why?
Angular is declarative with directives attached to elements. The directive then provides access via the compile or link element to this DOM element

## Are names prefixed with `$ ` avoided?
##### What?
Names of variables, properties and methods should not start with $
##### Why?
The $ prefix is reserved for AngularJS internals

## Is direct use of globals avoided?
##### What?
- Create services to wrap globals
- Use dependency injection to inject global wrappers

##### Why?
Prevents unpredictable application behavior when global is not available
Easier to test: allows you to mock global

## Are AngularJS specific directives put after standard attributes in templates?
##### What?
Put AngularJS specific directives after standard attributes
##### Why?
Easier to read and maintain your code

## Are built-in AngularJS dependencies injected before custom ones?
##### What?
- Inject built-in AngularJS dependencies before customer ones
- Example: function($rootScope, $timeout, MyDependency)

##### Why?
Easier to read and maintain your code

## Are modules named correctly?
##### What?
* lowerCamelCase
* Example: myApp

##### Why?
Naming convention

## Are controllers named correctly?
##### What?
* UpperCamelCase + Ctrl or UpperCamelCase + Controller
* Example: MyCtrl or MyController

##### Why?
Naming convention

## Are directives named correctly?
##### What?
* lowerCamelCase
* Example: myDirective
* Use scope instead of $scope in link function
* Use custom prefix to prevent name collisions with third-party libraries
* Don’t use ng prefix as it is reserved for AngularJS

##### Why?
Naming convention

## Are filters named correctly?
##### What?
* lowerCamelCase
* Example: myFilter

##### Why?
Naming convention

## Are services named correctly?
##### What?
* lowerCamelCase
* Example: myService
* UpperCamelCase if service is a Constructor
* Example: MyContrstructorClassService

##### Why?
Naming convention

## Are factories named correctly?
##### What?
* lowerCamelCase
* Example: myFactory

##### Why?
Naming convention

## Are built-in AngularJS directives used whenever possible?
##### What?
* Use ng-src instead of src, ng-hrefinstead of href, ng-styleinstead of style, etc. when dynamic values are passed

##### Why?
* They make sure markup remains valid, even when expression value is not available yet
* They are well-written and tested

## Are built-in AngularJS services used whenever possible?
##### What?
* Use $ services provided by AngularJS whenever possible
* Examples: $timeout, $interval, $http, …

##### Why?
* These services are designed to work well with the $digest cycle
* They are well-written and tested

## Is ng-cloak used to prevent content from flashing
##### What?
Use ng-cloak to initially hide elements that are only visible under certain conditions
##### Why?
Prevents content flashing when page is loaded

## Is console.log() avoided?
##### What?
* use $log service
* use $log.debug() to write debug messages to the console

##### Why?
allows you to turn off debug logging in production

## Is code minification safe?
##### What?
Use array notation or $inject notation or use ng-annotate
##### Why?
Code will break when minified if not written in a way that supports minification


# Architecure

## Is there a component-driven structure?
##### What?
Group scripts, markup and styles by feature
##### Why?
* Components can easily be removed
* Locating related files is easy

## Do directives communicate correctly?
##### What?
* Never rely on scope inheritance e.g. $scope.$parent
* Use directive controllers and require to let directives communicate

##### Why?
The scope hierarchy may change, breaking your code

## Is business logic defined in services?
##### What?
Business logic should be defined in services, not in controllers
##### Why?
* Business logic is centralized and reusable
* Services are really easy to test

## UI-Router: has a default route been configured?
##### What?
Use $urlRouterProvider.otherwise() to specify the default route in case no route definition matches the request
##### Why?
To gracefully handle requests that do not match to a route definition

## ngRoute: has a default route been configured?
##### What?
Use $routerProvider.otherwise() to specify the default route in case no route definition matches the request
##### Why?
to gracefully handle requests that do not match to a route definition

## ngRoute: are route errors handled properly?
##### What?
use a $routeChangeError event listener to handle routing errors
##### Why?
To gracefully handle routing errors

## UI-Router: are state change errors handled properly?
##### What?
use a $stateChangeError event listener to handle routing errors
##### Why?
to gracefully handle errors that occur during state transition (e.g. when a resolve fails)

## Are exceptions handled properly?
##### What?
decorate the $exceptionHandler service to handle uncaught exceptions
##### Why?
To gracefully handle uncaught exceptions

## Are resolves used where needed?
##### What?
Use the resolve property of a route or state to load content that is immediately required in the view
##### Why?
Make sure content is loaded before view is shown
Prevent content flashing

# Security

## Is Strict Contextual Escaping enabled?
##### What?
* Make sure SCE is enabled
* Use $sce service to explicitly tell AngularJS which content can be marked as safe for a context
* As of version 1.2, AngularJS ships with SCE enabled by default.


##### Why?
* To deal with content that can not be trusted
* With SCE disabled, your application allows a user to render arbitrary data creating security vulnerabilities

## UI-Router: are private parts of application properly protected?
##### What?
* Use border states to protect private areas in your application
* Use $stateChangeStart event handler to control access

##### Why?
* To restrict access to private areas in your application
* To make sure the states cannot be accessed by guessing the url's


# Performance

## Are scripts at bottom of the document?
##### What?
`<script>` elements should be as low as possible in document
##### Why?
Improves initial rendering speed

## Are styles in HEAD?
##### What?
Stylesheets should be in the document <head>
##### Why?
Improves initial rendering speed

## Is expensive logic avoided in filters?
##### What?
Filter logic should be lightweight
##### Why?
Filters are called often during $digest loop so if they are slow, they will slow down your application

## Is filtering logic handled in controller if needed?
##### What?
Filtering can be done in controller if the result is not affected by every $digest cycle
##### Why?
To control manually when the filter should run instead of with every $digest cycle

## Are expensive operations cached?
##### What?
Cache operations that are expensive (e.g. CPU or bandwidth intensive)
##### Why?
Optimizes speed by avoiding repetition of expensive operation

## Are complex expressions avoided in templates?
##### What?
Complex logic should be handled in controller or service
##### Why?
* View expressions are evaluated many times during $digest cycles
* Putting the logic in a controller or services allows you to cache the result

## Is one-time binding used where possible?
##### What?
Use `{{ :: expression }}` for content that does not change after initialization
##### Why?
AngularJS will stop watching the expression as soon as it evaluates to a non-null value

## Is logic in watchers kept as simple as possible?
##### What?
* Keep computations in $watch as simple as possible
* Example: watch data.property instead of deep watching data

##### Why?
Watchers are evaluated many times during $digest cycles so keeping them fast results in better performance

## Is $watchCollection used instead of deep watch where possible?
##### What?
Use $watchCollection instead of a deep watch for collections
##### Why?
$watchCollection performs a shallow equality check of the result of the watched expression and its previous value, resulting in better performance

## Is `$applyAsync` used when possible?
##### What?
* Use $scope.$applyAsync()
* Configure $httpProvider to use $applyAsync: $httpProvider.useApplyAsync(true);

##### Why?
To reduce number of $digest cycles when expressions are evaluated around the same time

## Do timers skip `$digest` cycle when possible?
##### What?
Pass false as third parameter to $timeout when no watched variables are impacted by its callback
##### Why?
To skip unnecessary $digest cycles

## Are resources cleaned up when scope is destroyed?
##### What?
use $scope.$on(‘$destroy’, fn) to clean up resources such as timers and intervals
##### Why?
* resources that are not cleaned up can chew up memory and CPU
* every time the scope is recreated (e.g. when page is visited again and again) the problem grows bigger

## Are templates bundled with main JavaScript file?
##### What?
Bundle your templates with your scripts using tools like grunt-html2js or gulp-html2js
##### Why?
Reduce number of network requests, especially when a large number of templates needs to be downloaded

## Is ngModelOptions used where possible?
##### What?
Use ngModelOptions to manually control when $digest cycle should run
##### Why?
To prevent unnecessary $digest cycles


