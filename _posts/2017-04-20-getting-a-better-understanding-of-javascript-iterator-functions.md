layout: post
title: Getting a Better Understanding of JavaScript Iterator Functions

Lately, I've worked a lot with AngularJS and JavaScript in general. While developing AngularJS applications, you'll inevitably need to work with JSON responses from an API that returns a list of objects. Now, depending on the implementation, the list could return a simple array or a key-value-pair, and you'll need to iterate over the list. If you're like me, you may feel led to use JavaScript's `forEach()` function or maybe Angular's `angular.forEach()` function. Both are similar in what they do; although, how you use them is a little different. Say you have an array of categories like this:

```javascript
var categories = [
  {
    name: "Type 1",
    type: "type1"
  },
  {
    name: "Type 2",
    type: "type2"
  },
  {
    name: "Other",
    type: "other"
  }
];
```
Using `forEach()`, you can iterate over the array using something like 
```javascript
categories.forEach(function(category) { 
    // do something with the category 
});
```
Using `angular.forEach()`, it would look more like
```javascript
angular.forEach(categories, function(category) {
	// do something with the category
});
```

When used for iteration, these functions actually create a **callback** for each of the items in the
array. What this means is that (if you're like me) the following may not behave as you expect

```javascript
// This will always return false
function includesOther() {
	angular.forEach(categories, function(category) {
		if(category.type === "other") {
			return true;
		}
	});

	return false;
}
```
Looking at the [MDN documentation for `forEach()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach?v=example), it's pretty clear

> There is no way to stop or break a forEach() loop other than by throwing an exception. If you need such behavior, the forEach() method is the wrong tool, use a plain loop instead. If you are testing the array elements for a predicate and need a Boolean return value, you can use every() or some() instead. If available, the new methods find() or findIndex() can be used for early termination upon true predicates as well.

So, if you need to implement this kind of logic, you're better off using the tried and true `for` loop. But what about the `some()` function mentioned in the documentation? It turns out `some()` is a lot closer to what we need, and the MDN describes it like this:

> The some() method tests whether some element in the array passes the test implemented by the provided function.

`some()` will return `true` immediately if one of the elements in the array matches the condition;
otherwise, it'll return `false`. Pretty handy.

```javascript 
categories.some(function(category) {
	return category.type === 'other';
});
```
