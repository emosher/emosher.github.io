---
title: Building Smarter Directives
tags: 
  - angularjs
  - javascript
permalink: building-smarter-directives
id: 13
updated: '2014-06-25 22:12:43'
date: 2014-05-15 22:30:18
---

I've been working a lot with AngularJS directives recently.

If you are not familiar with AngularJS or directives, head over to the [AngularJS tutorial](https://docs.angularjs.org/tutorial) and [developer guide](https://docs.angularjs.org/guide) to get up to speed.

### Template vs. TemplateUrl

If you are like me, you like to keep HTML out of your JavaScript. When I'm writing my directives, I almost always keep templates in seperate HTML files rather than using inline templates (see below for an example).
```
	  // This inline template makes me a sad panda :(
    angular.module('myApp', [])
    	.directive('example', function() {
    		return {
        		template: '<div ng-include="contentUrl"></div>'
        		};
    		};
		});
```

Or ...
```
	  // Moving that HTML into a seperate file makes me happy! :)
    angular.module('myApp', [])
    	.directive('example', function() {
    		return {
        		templateUrl: 'example.html'
        		};
    		};
		});
```

### 'Smart' Directives

One of the big challenges I have been facing is how to make my directives *smarter.* One way I have found recently is creating the ability to include one of any number of templates via the `ngInclude` directive above. This way one directive can use any number of templates which makes the directive feel like a self-contained component.

Thanks to the built-in ngInclude directive, this is fairly easy. Throw some HTML into `one.html` or `two.html` and you will have the start to a smarter directive. Suppose you have the following in your directive's `templateUrl: mydirective.html`.
```
	<div ng-include="contentUrl"></div>
```    

This allows you to set the directive's scope variable `contentUrl` to be included within this directive. For demonstration, the directive will be called `emSmarty` and you would structure it like so:
```
	app.directive('emSmarty', function() {
    	return {
        	templateUrl: 'emsmarty.html',
        	scope: true,
        	link: function(scope, element, attrs) {
            	scope.contentUrl = attrs.emSmarty + '.html';
        	}
    	};
	});
```

This allows you to use the directive within your HTML: `em-smarty="one"`. That will include `one.html` in the directive!

### Feel the Power of the Directive Side

Where ngInclude comes in very handy is where you have several templates that you might want to load depending on some condition. Think of it like this...
```
	switch (condition) {
    	case "one":
        	// Use template 'one'
            break;
        case "two":
        	// Use template 'two'
            break;
        case "three":
        	// Use template 'three'
            break;
        // This could go on...
    }
```

Here's a Plunker I created to show a simple example of conditional loading a templateUrl.

http://plnkr.co/edit/F7WRhY?p=preview

### Additional Discussion

You can find some additional resources on creating smarter directives at the following links:

* https://coderwall.com/p/mgtrkg
* http://onehungrymind.com/angularjs-dynamic-templates/
