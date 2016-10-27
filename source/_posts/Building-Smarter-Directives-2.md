---
title: 'Building Smarter Directives #2'
tags: |-

  - angularjs
  - javascript
  - directives
permalink: building-smarter-directives-2
id: 14
updated: '2014-07-13 17:31:58'
date: 2014-06-25 22:29:53
---

My [last post](https://emosher.org/2014/05/19/building-smarter-directives/) talked about conditionally loading templates using `ng-include`. 

I spent a good amount of time using this, but found it to be problematic because it was causing my HTML to become bloated. Instead of adding to the div soup, I set out to find a better way.

This leads to nested divs:
```
<div ng-include="contentUrl"></div>
```

Renders something like this:
```
<div ng-include="contentUrl">
    <div> This is the template </div>
</div>
```

As it turned out, there are options! First, I found out that in 1.1.4, Angular added the ability to use a function as the `templateUrl`. In your directive definition object, you can have the following:

```javascript
angular.module('myApp', [])
    .directive('example', function() {
    	return {
        	templateUrl: function(tElement, tAttrs) {
                // Template can be chosen here
                // Try something like this
                return tAttrs.example.toLowerCase() + '.html';
			}
		};
	});
```

Then use it like this: 

```
<!-- Includes 'one.html' -->
<div example="one"></div>
<!-- Includes 'two.html' -->
<div example="two"></div>
```

The other option that I found was to do what is essentially a manual `ng-include`. The problem I've found with this is that it makes the directive a bit harder to test. By going around Angular's built in ways of including the template, you are circumventing the  `$templateCache`.

To do this, you use the `$http` and `$compile` service to retrieve and compile the template yourself:

```javascript
angular.module('myApp')
    .directive('example', function($http, $compile) {
    	return {
        	link: function(scope, element, attrs) {
        		$http.get(attrs.example + '.html').then(function(response) {
                	element.html($compile(response.data)(scope));
                });
            }
        };
    });
```

This version of the directive would be used the same as above, allowing you to include `one.html` or `two.html`.

Whatever you decide to use, the good news is that you have options to suit your situation.