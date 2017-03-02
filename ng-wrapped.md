```
				    ng-app		    ng-controller
angular		=>		module		=>  controlers
						        =>  directives
						        =>  services
```


## 1. Controllers
The controller has a `$scope`.
```js
function MainController($scope) {
  $scope.name = 'Bill Gates';
}
```
This scope is accessible in the view like this
```html
<div ng-controller="MainController">
	<p>{{ name }}</p>
</div>
```
or using one-way data-binding
```html
<div ng-controller="MainController">
	<p><span ng-bind="name"></span></p>
</div>
```

Anything inside {{}} is an expression, we can do javascript stuff inside
```html
{{ someArray.length }}

<h6>You have {{ ctrl.emails.length == 0 && 'no' || ctrl.emails.length }} emails</h6>
```

A controller scope defines the data that can be accessed on the html portion.
![scope](https://camo.githubusercontent.com/1aa668071bcd1300d8d142969ca56f406f6f93de/68747470733a2f2f646f63732e616e67756c61726a732e6f72672f696d672f67756964652f636f6e63657074732d73636f70652e706e67)

### Using ControllerAs syntax
We tell angular we want to use our controller as a variable ('main' for instance).
It creates a new instance of our controller (with all of our values bound to the controller's instance, rather than $scope) and assigns the instance to a variable named main.


This enables nested scopes
```js
function MainController($scope) {
  $scope.name = 'Bill Gates';
}
```

becomes
```js
function MainController() {
  this.name = 'Bill Gates';
}
```
We're allowing our controllers to assign anything they want our view to access to the this keyword

And we change our view
```html
<div ng-controller="MainController as main">
	<p>{{ main.name }}</p>
</div>
```

## 2. Services
Services are helpers. They're JavaScript functions and are responsible for doing specific tasks only.
```js
function NotificationService() {
  this.notify = function (message) {
    alert(message);
  };
}

angular
  .module('app')
  .service('NotificationService', NotificationService);
```
While attaching the service to the module, the first parameter is the name of our service and will be used when we need to inject it elsewhere. The function doesn't need to have the same name, but for debugging purposes, we keep it the same, it will appear in the console.


Dependency injection enables to inject a service into the controller that needs it.
```js
function ContactController(NotificationService, UserService, AuthService) {
  NotificationService.notify('Hello!');
}
```

### Built in services
Angular has built in services like `$timeout`, or `$http`

We can use $http as a function, passing through a configuration object with it, as such:
```js
$http({
    method: 'GET',
    url: '/someURL'
});

$http({
    method: 'POST',
    data: {
        username: 'Bill'
    },
    url: '/someOtherURL'
});

$http.get('/someURL');

$http.post('/someOtherURL', { username: 'Bill' });

// and handle the promise data
$http({
    method: 'GET',
    url: '/someURL'
})
    .then(function (data) {
        console.log(data);
    });
```

We can inject service into our controller
```js
// our service
function UserService($http) {
    this.getLoggedInUser = function () {
        return $http.get('/rest/user');
    }

    this.updateEmail = function (emailAddress) {
        return $http.post('/rest/user/email', {email: emailAddress});
    };    
}

angular
    .module('app')
    .service('UserService', UserService);
```

And in our  header controller
```js
function HeaderController(UserService) {
    var ctrl = this;

    ctrl.user = '';

    UserService
        .getLoggedInUser()
        .then(function (res) {
            ctrl.user = res.data.username;
        });
}

angular
    .module('app')
    .controller('HeaderController', HeaderController);
```

And in our settings controller
```js
function SettingsController(UserService) {

    this.emailAddress = ''; // this is bound to an input via `ng-model`

    this.submitForm = function () {
        UserService
            .updateEmail(this.emailAddress)
            .then(function () {
                alert('Email updated!');
            });
    };
}

angular
    .module('app')
    .controller('SettingsController', SettingsController);    
```

In our service, we can also use angular's `$resource` service that gives us `.get()`, `$save()` and `$delete()` functions.
```js
function UserService($resource) {
    var User = $resource('/user/:userId');

    this.getUser = function (userId, callback) {
        User.get({userId: userId}, callback);
    };

	// User.get({userId: 3}, function (user) {
	//    console.log(user);
	// });    
}
```

And then we can use that as follows in our controller:
```js
function MyController(UserService) {
    UserService.getUser(3, function (user) {
        console.log(user);
    });
}
```


### Minification safe Dependency Injection
We need to specify the real names of our injected services, otherwise they'll be squished during minification.
```js
function ContactController(whatever, weWant) {
  // whatever === $scope
  // weWant === $timeout
}

ContactController.$inject = ['$scope', '$timeout'];

angular
  .module('app')
  .controller('ContactController', ContactController);
```

### HTTP Interceptors
Now, using $http or $resource means we're going to end up doing a lot of HTTP requests in our applications, so that we want to handle responses and errors globally. HTTP interceptors allow us to define functionality that will happen before every request is made, or just after every request has come back from the server.

HTTP interceptors are just simple services that we then push into an interceptors array in the $httpProvider service.

```js
function MyInterceptor() {
    this.request = function (config) {
        config.headers['X-Requested-From'] = 'Angular';
        return config;
    };
}

angular
    .module('app')
    .service('MyInterceptor', MyInterceptor)
    .config(function ($httpProvider) {
        $httpProvider.interceptors.push('MyInterceptor');
    });
```
This will append the X-Requested-From header to every single request going through $http!

```js
function MyInterceptor() {
    this.response = function (config) {
        config.config.responseTime = Date.now();
        return config;
    };
}

angular
    .module('app')
    .service('MyInterceptor', MyInterceptor)
    .config(function ($httpProvider) {
        $httpProvider.interceptors.push('MyInterceptor');
    });
```
We can also intercept after a request has been completed, and log what time the request came back.

```js
function MyInterceptor(NotificationService) {
    this.responseError = function (config) {
        NotificationService
            .showError(config);
    };
}

angular
    .module('app')
    .service('MyInterceptor', MyInterceptor)
    .config(function ($httpProvider) {
        $httpProvider.interceptors.push('MyInterceptor');
    });
```
We can just purely kick in our interceptor after an error too. `NotificationService` will be a custom service that we have made, alerting the user when we call `.showError` 


### Custom Services
With services, much like our controllers (when using controllerAs), we attach all of our functions to this. These will be publicly accessible by anyone who injects the service. We can also create private functions by using normal functions, as such:
```js
function SomeService() {
    function privateMethod() {

    }

    this.publicMethod = function () {
        privateMethod();
    };
}

angular
    .module('app')
    .service('SomeService', SomeService);
```

To inject our SomeService above, we just add SomeService as an argument to our controller:
```js
function SomeController(SomeService) {
    SomeService.publicMethod();
}

angular
    .module('app')
    .controller('SomeController', SomeController);
```


## 3. Directives
Directives are essentially "markers" on a HTML element that tell Angular to attach a specific behavior to the HTML element - either something small like a click event, or actually completely transforming the DOM node to display a list of data. Directives can be either an actual HTML element or an attribute on a HTML element.

There are two types of directives

- event: events on a HTML element we might need - for example, click, mouseover, keydown, etc
- behavioral: a list that repeats our DOM node for every item in a list, hiding and showing content depending on one of our variables' value

Angular comes with a ton of built-in directives

### Event directives
Our HTML
```html
<div ng-controller="CounterController as vm">
	<button ng-click="vm.incrementCounter()">Increment the counter!</button>
	{{ vm.counter }}
</div>	
```
and controller 
```js
function CounterController() {
  this.counter = 0;

  this.incrementCounter = function () {
    this.counter++;
  };
}
```


### Behavioral directives
We can manipulate the DOM

#### ng-repeat
Our controller
```js
function TodosController() {
  this.todos = [{
     title: 'Learn Angular',
     complete: false,
   },{
     title: 'Create GitHub profile',
     complete: false
   },{
     title: 'Brush teeth',
     complete: true
   }];
}
```

Our HTML
```html
<ul>
  <li ng-repeat="todo in vm.todos">
    <h4>{{ todo.title }}</h4>

    {{ todo.complete && 'Completed!' || 'Not completed :(' }}
  </li>
</ul>
```


#### ng-model
We can have an input connected to a display
```html
<input ng-model="vm.username" />

{{ vm.usernname }} 
```



#### ng-class
SHow a class depending on value
```html
<ul>
  <li ng-repeat="todo in vm.todos" ng-class="{'complete': todo.complete}">
    <h4>{{ todo.title }}</h4>

    {{ todo.complete && 'Completed!' || 'Not completed :(' }}
  </li>
</ul>
```

#### ng-show
depending on the value of a function, we can show or not a DOM node
```html
<div ng-show="vm.isInReadMode(task.id)">
    <p>
        {{ $index +1 }}: {{ task.name }} - completed: {{ task.complete }}
    </p>
    <!-- task options -->
    <button ng-click="vm.startEdit(task.id)">Edit</button>
    <button ng-click="vm.startRemove(task.id)">Delete</button>              
</div>
```


#### Filters
We can filter our datasets for what needs to be listed.
```html
<input ng-model="ctrl.search" />

<ul>
    <li ng-repeat="data in ctrl.data | filter: ctrl.search">
    </li>
</ul>
```

Another powerful filter is date that takes a UNIX timestamp
```html
<abbr>
    {{ ctrl.date | date:'medium' }} <!-- Dec 25, 2016 2:23:56 PM -->
</abbr>
```

We can also filter in our controllers, using the `$filter` service. This is the preferred method for filtering on big sets of data as it helps increase performance.
```js
function SomeController($filter) {
    this.list = [{
        name: 'Bob'
    }, {
        name: 'Tom'
    }];

    this.search = 'B';

    this.filteredList = $filter('filter')(this.list, this.search);
}
```
We would then use `filteredList` like normal in our ng-repeat:
```html
<ul>
    <li ng-repeat="item in ctrl.filteredList">
        {{ item.name }}
    </li>
</ul>
```

### Custom Directives
Our directive names are what we then use to reference them in the DOM. However, as the DOM is case-insensitive, we change the capital letters in our name to hypens. For instance, the directive name `myDirective` becomes `<my-directive></my-directive>`.

In our directive function we return an object - this describes all the functionality and configuration of our directive. 

The **function name is a capital letter but the directive name is always camel case** - this is to keep function names consistent when compared to our custom services, filters, etc.

```js
function MyDirective() {
    return {
        template: '<div>Hello world!</div>'
    };
}

angular
    .module('app')
    .directive('myDirective', MyDirective);
```

The object can have several parameters

- restrict: 'EA' (element, attribute)
- replace: true (to get rid of the markup our-directive in the HTML)
- templateUrl: 'js/module/templates/views/directive.html' (to stop using join('') and externalize the view code)
- scope
- controller
- controllerAs
- bindToController
- transclude
- require

#### Inheriting/isolating scope
Our directives can display dynamic data, by inheriting from the scope above.
```html
<div ng-controller="SomeController">
    <twitter-card></twitter-card>
</div>
```

And the custom directive
```js
function TwitterCard() {
    return {
        template: [
            '<div class="twitter">',
                '<a href="https://twitter.com/{{ twitter }}">Follow @{{ twitter }} on Twitter!</a>',
            '</div>'
        ].join(''),
        restrict: 'E'
    };
}

angular
    .module('app')
    .directive('twitterCard', TwitterCard);
```
Now if our SomeController `$scope.twitter` equals BillGates it would be printed.

To make the scope independent on the parent scope and being able to reuse our directive, we can **Isolate Scope**. It creates a new scope for our directive - it can either be a completely brand new one or copied from our parent.

- scope: false  (doesn't set a scope)
- scope: true   (sets a scope by copying the parent but if parent changes, the child doesn't - they are unlinked after being copied)
- scope: {} (passing an object to our scope with '@' to copy the value as is or '=' to pass through)

```html
<twitter-card handle="billgates"></twitter-card>
<twitter-card handle="bob"></twitter-card>
```

and our Directive
```js
function TwitterCard() {
    return {
        template: [
            '<div class="twitter">',
                '<a href="https://twitter.com/{{ handle }}">Follow @{{ handle }} on Twitter!</a>',
            '</div>'
        ].join(''),
        scope: {
            handle: '='
        },
        restrict: 'E'
    };
}

angular
    .module('app')
    .directive('twitterCard', TwitterCard);
```

We can also have the alternative writing like so, to be more explicit
```js
function TwitterCard() {
    return {
        template: [
            '<div class="twitter">',
                '<a href="https://twitter.com/{{ twitter }}">Follow @{{ twitter }} on Twitter!</a>',
            '</div>'
        ].join(''),
        scope: {
            twitter: '=handle' // that changes
        },
        restrict: 'E'
    };
}

angular
    .module('app')
    .directive('twitterCard', TwitterCard);
```

#### Custom directive controllers

##### Creating directive controllers
We can create controllers for our directives - awesome! We'd generally use these for functions to change data or retrieve data from a service (we might have a list of contacts), or manipulate the data given us (we might want to verify an email address or phone number for a contact).

Our controllers, much like the controllers we've created before, allow us to directly access $scope (the values passed through to us from attributes), as well as all the services that we have created, as well as built-in ones too, such as $timeout.

```html
<twitter-card handle="billgates"></twitter-card>
```

And our directive
```js
function TwitterCard() {
    return {
        template: [
            '<div class="twitter">',
                '<a href="https://twitter.com/{{ twitter }}">Follow @{{ twitter }} on Twitter!</a>',
            '</div>'
        ].join(''),
        scope: {
            handle: '@'
        },
        controller: function ($scope, $timeout) {
            $timeout(function () {
                $scope.handle = 'angularjs'
            }, 5000);
        },
        restrict: 'E'
    };
}

angular
    .module('app')
    .directive('twitterCard', TwitterCard);
```
This will set our Twitter handle to "angularjs" after 5 seconds.


We can also make use of controllerAs
```js
function TwitterCard() {
    return {
        template: [
            '<div class="twitter">',
                '<a href="https://twitter.com/{{ handle }}">Follow @{{ handle }} on Twitter!</a>',
                '<button ng-click="ctrl.changeHandle()">Change Handle</button>',
                				   // here we use ctrl
            '</div>'
        ].join(''),
        scope: {
            handle: '@'
        },
        controller: function ($scope) {
            // $scope.handle === 'billgates'

			// this !
            this.changeHandle = function () {
                $scope.handle = 'angularjs';
            };
        },
        controllerAs: 'ctrl',
        restrict: 'E'
    };
}

angular
    .module('app')
    .directive('twitterCard', TwitterCard);
```

We can clean this using `bindToController`. It works exactly like our scope property, but puts the items into `this` instead of `$scope`. This is awesome - as we're going to be using `controllerAs`, we should have all of our values assigned the same variable.

```js
function TwitterCard() {
    return {
        template: [
            '<div class="twitter">',
                '<a href="https://twitter.com/{{ ctrl.handle }}">Follow @{{ ctrl.handle }} on Twitter!</a>',
                '<button ng-click="ctrl.changeHandle()">Change Handle</button>',
            '</div>'
        ].join(''),
        bindToController: {
            handle: '@'
        },
        scope: {}, // we do still want a brand new scope
        controller: function () {
            // this.handle === 'billgates'

            this.changeHandle = function () {
                this.handle = 'angularjs';
            };
        },
        controllerAs: 'ctrl',
        restrict: 'E'
    };
}

angular
    .module('app')
    .directive('twitterCard', TwitterCard);
```




##### Using existing controllers
Instead of a function, we can pass through a string to the controller property as well.
This means we can use an existing controller as our directive's controller. 
It also means that we can remove the controllerAs property if we're using a string, as we can do 'SomeController as some'.


Our controller
```js
function TwitterController() {
    // this.handle === 'billgates';

    this.changeHandle = function () {
        this.handle = 'angularjs';
    };
}
```

Our directive
```js
angular
    .module('app')
    .controller('TwitterController', TwitterController);

function TwitterCard() {
    return {
        template: [
            '<div class="twitter">',
                '<a href="https://twitter.com/{{ handle }}">Follow @{{ handle }} on Twitter!</a>',
                '<button ng-click="ctrl.changeHandle()">Change Handle</button>',
            '</div>'
        ].join(''),
        scope: {
            handle: '@'
        },
        controller: 'TwitterController as ctrl',
        restrict: 'E'
    };
}

angular
    .module('app')
    .directive('twitterCard', TwitterCard);
```

##### Requiring directives
We can require the parent controller to send us the directive from a parent directive, it gives us access to the required directive's controller.

```js
function Tab() {
	return {
		restrict: 'E',
		scope: {
			label: '@'
			// from the view <tab label="Tab 1">
		},
		require: '^tabs',
		transclude: true,
		template: [
    	    '<div class="tabs__content" ng-if="tab.selected">',
      	        '<div ng-transclude></div>',
            '</div>'
            // will go inside of <tab label="Tab 1">
		].join(''),
		link: function ($scope, $element, $attrs, $ctrl) {
			$scope.tab = {
				label: $scope.label,
				selected: false
			};
			$ctrl.addTab($scope.tab);
			// $ctrl is the 4th argument, given because we use 'require'
			// it gives us access to the parent's directive controller
		}
	}
}
```

You'll notice we've added `require` with the value `^tabs` - this is telling Angular to require the parent controller from our `tabs` component. Normally, our link function has three parameters - scope (`$scope`), element (the mounted DOM element) and attrs (the attributes passed through to the directive). However, when we use `require`, we get a fourth - ctrl. This will be equal to the parents (directive) controller, allowing us to access everything to do with it.

Parent directive
```js
function Tabs() {
	return {
		restrict: 'E',
		scope: {},
		transclude: true,
		controller: function () {
			this.tabs = [];

			this.addTab = function addTab(tab) {
				this.tabs.push(tab);
			};
			
			this.selectTab = function selectTab(index) {
				for (var i = 0; i < this.tabs.length; i++) {
					this.tabs[i].selected = false;
				}
				this.tabs[index].selected = true;
			};
		},
		controllerAs: 'tabs',
		link: function ($scope, $element, $attrs, $ctrl) {
			$ctrl.selectTab($attrs.active || 0);
		},
		template: [
      	'<div>',
        	'<ul>',
          	    '<li ng-repeat="tab in tabs.tabs">',
            	    '<a href="" ng-bind="tab.label" ng-click="tabs.selectTab($index);"></a>',
                '</li>',
            '</ul>',
        	'<div ng-transclude></div>',
        '</div>'
		].join('')
	};
}

angular
	.module('app')
	.directive('tabs', Tabs);
```

##### Lifeycle: Controller, Link, Compile, 
Let's play before and after the directive is mounted in the DOM

- `compile($element, $attrs)` - ready to compile the directive
- `controller($scope)` - manipulate all data **before** the directive is actually compiled
- `pre-link($scope, $element, $attrs)` - the directive is **compiled** to DOM nodes, but isn't inserted into the DOM. Use it for DOM manipulation (moving nodes, changing HTML...)
- `post-link($scope, $element, $attrs)` - the directive has been **inserted into the DOM** (**same as the link function**). Use it to add event listeners. Same as `link`.

```js
function SomeDirective() {
	return {
		template: [
			'<div>',
				'Replace this text!',
			'</div>'
		].join(''),
		compile: function (elem, attrs) {
			return {
				pre: function (scope, element, attrs) {
					console.log(element);
					element[0].innerText = "My Text";
				},
				post: function (scope, element, attrs) {
					console.log(element);
					element[0].addEventListener('click', function(){
						alert('Yo');
					});
				}
			}
		}
	}
}

angular
	.module('app')
	.directive('someDirective', SomeDirective);
```
Note that `elem` isn't *actually* the raw DOM node. It is a jqLite (a light version of jQuery) (or jQuery if you have loaded jQuery before Angular) element. We can access the raw DOM node via `elem[0]`.

With Link we can create native events (not using `ng-click()`)
```js
function SomeDirective() {
	return {
		template: [
			'<div>',
				'<span>Click on me!</span>',
			'</div>'
		].join(''),
		link: function (scope, elem, attrs) {
			var actualElement = elem[0];

			var spanElement = actualElement.querySelector('span');

			spanElement.addEventListener('click', function () {
				alert('You clicked me!');
			});
		}
	}
}

angular
	.module('app')
	.directive('someDirective', SomeDirective);
```

Now let's put a scope value in our view and controller. We're outside the scope of Angular here, so Angular isn't able to tell that we've updated our scope. However, we can manually push Angular to update, by using `scope.$apply()`.

```js
function SomeDirective() {
	return {
		template: [
			'<div>',
				'<span>Click on me!</span>',
				'{{ status }}',
			'</div>'
		].join(''),
		controller: function ($scope) {
			$scope.status = 'Not clicked!';
		},
		link: function (scope, elem, attrs) {
			var actualElement = elem[0];

			var spanElement = actualElement.querySelector('span');

			spanElement.addEventListener('click', function () {
				scope.status = 'Clicked!';
				scope.$apply();
			});
		}
	}
}

angular
	.module('app')
	.directive('someDirective', SomeDirective);
```
This is also effectively how `ng-click` works, which is why we use them instead of doing all of our DOM events in the link function!

Now instead of using `$scope` we can use controller values, by using `require` to get the directive's controller accessible in our link function.

```js
function SomeDirective() {
	return {
		template: [
			'<div>',
				'<span>Click on me!</span>',
				'{{ some.status }}',
			'</div>'
		].join(''),
		require: 'someDirective',
		controller: function () {
			this.status = 'Not clicked!';
		},
		controllerAs: 'some',
		link: function (scope, elem, attrs, ctrl) {
			var actualElement = elem[0];

			var spanElement = actualElement.querySelector('span');

			spanElement.addEventListener('click', function () {
				// ctrl.status = undefined;
				ctrl.status = 'Clicked!';

				scope.$apply();
			});
		}
	}
}

angular
	.module('app')
	.directive('someDirective', SomeDirective);
```
We're still using `scope.$apply()` though - why? We still use it because `$apply()` is a function only available on scope, and we still need to tell Angular that we've updated our view.

#### Transclude
##### Basic
Say we want to be able to customize the content inside of our directives, like so:
```html
<our-directive>
    Content here! We want to put custom content in our directive!
</our-directive>
```

We can then use the directive `ng-transclude` to put our content wherever we want it in our template.

```js
function ourDirective() {
  return {
    transclude: true,
    template: [
      '<div class="ourDirective">',
        'The content of our directive is: <span ng-transclude></span>',
      '</div>'
    ].join('')
  };
}

angular
  .module('app', [])
  .directive('ourDirective', ourDirective);
```

It's as simple as that! When Angular runs, we will end up with this rendered in the HTML:

```html
<our-directive>
	<div class="ourDirective">
		The content of our directive is: <span>Content here! We want to put custom content in our directive!</span>
	</div>
</our-directive>
```

##### Manual
We can also do it manually, using the fifth function provided in our `link` function - `transclude`. This function returns our transcluded content as an actual DOM element. This allows us to manually append our elements into our directives instead.

```js
function ourDirective() {
  return {
    transclude: true,
    template: [
      '<div class="ourDirective">',
        'The content of our directive is: <span></span>',
      '</div>'
    ].join(''),
    link: function (scope, element, attrs, ctrl, transclude) {
        element.find('span').after(transclude());
    }
  };
}

angular
  .module('app', [])
  .directive('ourDirective', ourDirective);
```

##### Manual Multi-slot
Instead of a string or a boolean value, to do multi-slot transclusion, we pass through an object.
```html
<user-profile>
    <h4>Tim Cook</h4>
    <h6>CEO, Apple</h6>
</user-profile>
```

We can now access these elements using the familiar `ng-transclude` directive. We pass in the slot name that we want as the value.
To make a slot as optional, inside of our string we add a `?` before the element.

```js
function UserProfile() {
  return {
    transclude: {
        name: 'h4',
        position: '?h6'
    },
    template: [
      '<div class="user">',
        '<h2>User Profile</h2>',
        '<div>Name: <span ng-transclude="name"></span></div>',
        '<div>Position: <span ng-transclude="position"></span></div>',
      '</div>'
    ].join('')
  };
}

angular
  .module('app', [])
  .directive('userProfile', UserProfile);
```

#### jqLite
jqLite allows us to use `.on` and `.off` on our `element` inside the link function. `.off()` will remove/unbind all event listeners that we've added to the element, and `.on` let's us add them for different event types, such as `click`, so that we don't run into performance issues.

Let's add a click event to our element:

```js
function ourDirective() {
  return {
    transclude: true,
    template: [
      '<div>',
        'Click on me!',
      '</div>'
    ].join(''),
    link: function (scope, element) {
        element.on('click', function () {
            alert('You clicked me!');
        });
    }
  };
}

angular
  .module('app', [])
  .directive('ourDirective', ourDirective);
```

Luckily, we get an event fired when our directive is *about* to be unmounted from the DOM. We can subscribe to this event using `scope.$on`, and unsubscribe using `element.off();`

```js
function ourDirective() {
  return {
    transclude: true,
    template: [
      '<div>',
        'Click on me!',
      '</div>'
    ].join(''),
    link: function (scope, element) {
        scope.$on('$destroy', function () {
           alert('About to be destroyed!');
           element.off();
        });
    }
  };
}

angular
  .module('app', [])
  .directive('ourDirective', ourDirective);
```

## 4. Components
Directives are old and a bit verbose for what we may need 99% of the time. Components are a new, updated method that has matched the way the frontend has changed. Think of them as a simple way to write a directive, and if you need to go complicated, use a directive instead.

There are a few restrictions to using the `.component()` method:

- Every item is an element - we can't create attributes
- We no longer use the `scope` property. Everything is bound to the controller
- We no longer use `bindToController` - this is simply named `bindings` instead
- When we use `require`, we have to pass through an object instead of a string, with the property being anything that we want and the string being what we'd normally use
    - For example, instead of `{ require: '^parentController' }`, we use `{ require: { parent: '^parentController' } }`
- We cannot use a `link` or `compile` function

```js
var ContactCard = {
	bindings: {
		name: '=',
		email: '=',
		phone: '='
	},
	template: [
		'<div>',
			'<h4>Contact Card</h4>',
			'<label>Name:</label>',
			'{{ ctrl.name }}',
			'<label>Email:</label>',
			'{{ ctrl.email }}',
			'<label>Phone:</label>',
			'{{ ctrl.phone }}',
		'</div>'
	].join(''),
	controllerAs: 'ctrl',
	restrict: 'E'
};


angular
	.module('app')
	.component('contactCard', ContactCard);
```

