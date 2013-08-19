# [MathJax](http://www.mathjax.org/)-directive for AngularJS


## Usage


### HTML

```
<div ng-app="app">
  <div mathjax="{expression}"></div>
<div>
```

Where `{expression}` is the expression that will be `$watched` for running
MathJax (in most cases simply a `$scope` variable).


### JS

```
var app = angular.module('app', ['mathjax']);
```


### MathJax config

Include MathJax

```
<script type="text/javascript"
  src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML&delayStartupUntil=configured">
</script>
```

and configure it

```
MathJax.Hub.Config({
  skipStartupTypeset: true
});
MathJax.Hub.Configured();
```


## Comments

Why require `{expression}` to be set explicitly and not listen to changes on
the DOM-element? That's a bad idea because the html might change at any time
for whatever reasone — other directives modifying the DOM, like adding tooltips
etc.

Also, there's no good way to listen to DOM mutations. There exists events such
as `DOMNodeInserted`, but that event would get triggered very many times when
angular re-renders the html. Furthermore,
[MDN tells you to avoid DOM mutation event listeners](https://developer.mozilla.org/en-US/docs/Extensions/Performance_best_practices_in_extensions#Avoid_DOM_mutation_event_listeners).

It might be tempting to use `ngBind` and run MathJax when the `ngBind`
variable changes. However, you might not want to use `ngBind` because you're
using some other (possibly custom) directive, maybe `ngBindHtmlUnsafe`. Thus I
choose to set the `{expression}` explicitly.
