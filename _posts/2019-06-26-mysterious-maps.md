---
layout: post
title:  "Mysterious Maps in Javascript"
categories: [javascript]
---
I always wondered why data structures like Map (Dictionary) and Set weren’t baked into Javascript from the early days much like Arrays. Coming from a Java or a Objective-C world, I wished I wouldn’t need to implement these data structures on my own every time I wanted to use one. It isn't bad for Maps since up until this point, I would just use the Object literal syntax `{}` to create new key-value pairs whenever needed. However with ES6, we got an extensive list of feature updates including new collection types like `Map, Set, WeakMap and WeakSet`. So the question is why would one use a Map collection type when we already have the built-in Object literals?

---
**RECAP**

*A Map is a data structure where you can assign a value to a key. Value lookup for that key is generally in constant O(1) time.*

{% highlight javascript %}
const employee = {};
employee.name = 'Gaurav Rane';
employee.salary = 40000;
console.log(employee.name); // 'Gaurav Rane'
console.log(employee.salary); // 40000
{% endhighlight %}
---

Researching further into this, I found some key differences between Maps and Objects (used as Maps) in Javascript which are stated below:

<ins>**Prototype Inheritance**</ins>

Literal objects in Javascript inherits properties from its built-in prototype.
This can lead to false positives if we check for the existence of a key that resembles a prototypal property.

To resolve this situation, we can check the built-in `hasOwnProperty` method of the Object which can indicate if the property is a direct property.

{% highlight javascript %}
employee.hasOwnProperty(‘name‘); // returns true
{% endhighlight %}

The only edge case here is that if we tried overriding the `hasOwnProperty` property in an Object but even in that case we can use the `hasOwnProperty` of a brand new Object (not tampered) like this

{% highlight javascript %}
({}).hasOwnProperty.call(object, ‘hasOwnproperty’); // returns true
{% endhighlight %}

**NOTE:** In order to avoid inheriting the default Object prototype, we can use bare objects like this
{% highlight javascript %}
let employee = Object.create(null);
// Object Literals behind the scenes use Object.create(Object.prototype);
{% endhighlight %}
*Performance wise this is is apparently much slower hence we rarely see this syntax being used in the read world.*

---

<ins>**Implicit Key Conversion**</ins>

Objects would convert the keys to strings so you cannot use other types as keys. However with Maps, there is no such restriction. You can even use objects as keys in Maps.
{% highlight javascript %}
var foo = {};
foo[2312] = ‘bar‘;
console.log(Object.keys(foo)[0]) // prints 'string'

var map = new Map();
map.set(foo, ‘new value‘);
console.log(map.keys().next().value) // prints {2312: ‘bar‘}
{% endhighlight %}
---

<ins>**Iteration**</ins>

Map has a built-in iterable while objects do not. You can use a `for...of` loop directly with a Map. You can even use a built-in `forEach` for the Map.  The `for...in` loop can be used to iterate over the enumerable properties of the Object but that would include the prototype properties too.

**NOTE:** Map preserves the order of the keys (in which it was added) while iteration.
{% highlight javascript %}
// iterate over keys
for (let employee of employee.keys()) {
  console.log(employee); // Sarah, Gaurav, John
}

// iterate over values
for (let salary of employeeSalaries.values()) {
  console.log(salary); // 50000, 40000, 45000
}

// iterate over [key, value] entries
for (let entry of employee)
  console.log(entry); // Sarah, 50000 (so on)
}

// Or use the built-in method
employee.forEach( (value, key, map) => {
  console.log(`${key} -> ${value}`); // Sarah -> 50000 (so on)
});
{% endhighlight %}
---

<ins>**Size calculation**</ins>

Getting size of the map is straightforward as compared to that of the Object
{% highlight javascript %}
map.size
vs
Object.keys(obj).length;
{% endhighlight %}
---

<ins>**Converting Object to Map**</ins>

While creating a Map, we can pass in an array (or any iterable) of key-value pairs. Objects have a built-in method to generate the array of key-value pairs as required by the above format. Hence converting from an Object to a Map is fairly trivial.
{% highlight javascript %}
let employee = {
    name: ‘Gaurav‘,
    age: 29,
    salary: 40000
};
let employeeMap = new Map(Object.entries(employee));
{% endhighlight %}

---

<ins>**Interesting Map features**</ins>

 `map.set(key,value)` returns the modified map itself, hence we can chain subsequent calls to keep on setting more key-value pairs if you would like
{% highlight javascript %}
map.set(‘foo‘, ‘bar‘).set(‘xyz‘, ‘abc‘).set(‘pqr‘, ‘lmn‘);
{% endhighlight %}

Also `NaN` can be used a valid key in Maps

---

### Conclusion

Generally use Maps where a lot of addition and deletion of key-value pairs is involved. (I won’t go in detail about this but basically Javascript engines have to recompile cached object classes when frequent additions and deletions are made. Check this [Stack Overflow][object-deletion-expensive] answer for further reading)

Maps seem to also perform better for very large key-value pairs, situations where keys are unknown until runtime and where the data types of the keys and values are of the same. Object literals tend to perform better for write once read many situations with string keys.

---

### References:
[https://medium.com/front-end-weekly/es6-map-vs-object-what-and-when-b80621932373][ref-1]

[https://javascript.info/map-set-weakmap-weakset][ref-2]

[http://ryanmorr.com/true-hash-maps-in-javascript][ref-3]

### Image credits:
Image by [Pexels][image-user] from [Pixabay][image-src]

[object-deletion-expensive]: https://stackoverflow.com/a/44008788/4927076
[ref-1]: https://medium.com/front-end-weekly/es6-map-vs-object-what-and-when-b80621932373
[ref-2]: https://javascript.info/map-set-weakmap-weakset
[ref-3]: http://ryanmorr.com/true-hash-maps-in-javascript
[image-user]: https://pixabay.com/users/Pexels-2286921/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1850653
[image-src]:   https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1850653