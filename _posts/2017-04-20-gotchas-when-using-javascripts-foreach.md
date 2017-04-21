
----
 -layout: post
 -title: Gotchas When Using JavaScript's `forEach()` 
 ----

Using JavaScript's `forEach()` or Angular's `angular.forEach()` may not always be the best tool. 
When used for iteration, these functions actually create a **callback** for each of the items in the
array. What this means is that (if you're like me) the following may not behave as you expect:

```javascript
var categories = [
	{
		type: "type1"
	},
	{
		type: "type2"
	},
	{
		type: "other"
	}
];
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

In fact, JavaScript's `forEach` function will behave the same way, and it also uses callbacks. Reading
the MDN documentation[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach?v=example] for `forEach()`, it's pretty clear:

> There is no way to stop or break a forEach() loop other than by throwing an exception. If you need such behavior, the forEach() method is the wrong tool, use a plain loop instead. If you are testing the array elements for a predicate and need a Boolean return value, you can use every() or some() instead. If available, the new methods find() or findIndex() can be used for early termination upon true predicates as well.

`some()` sounds a lot closer to what we need, and the MDN describes it like this:

> The some() method tests whether some element in the array passes the test implemented by the provided function.

`some()` will return `true` immediately if one of the elements in the array matches the condition;
otherwise, it'll return `false`.

```javascript 
var categories = [
	{
		type: "type1"
	},
	{
		type: "type2"
	},
	{
		type: "other"
	}
];
categories.some(function(category) {
	return category.type === 'other';
});
```