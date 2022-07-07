## Different ways to create Objects in JavaScript

After Primitive types in JavaScript, Objects are another type of Variables in JavaScript. And JavaScript is being Object-Oriented with the help of Prototype Inheritance. Hence Objects become the crucial construct of JavaScript.

You can save some time in your application development workflow by knowing some handy ways to create Objects in Javascript. /Congratulations on A happy productive day/.

All the ways or strategies to create Objects in JS have their specific uses. Of course, you can use them anywhere you want. But keep in mind, it might not serve the purpose of readability or less complexity.

And use these methods with precaution, because:

> Always code as if the guy who ends up maintaining your code will be a violent psychopath who knows where you live.

%[https://twitter.com/SoftwareWisdom/status/1216934034212704257]

---

## Table of Contents:
(Note: The links in Table of Content will take you to the original article)


- [Using Object Notation](https://time2hack.com/different-ways-to-create-objects-in-javascript/#using-object-notation)
- [Object.assign](https://time2hack.com/different-ways-to-create-objects-in-javascript/#object-assign)
- [Using Object Spread Operator](https://time2hack.com/different-ways-to-create-objects-in-javascript/#using-object-spread-operator)
- [Object notation with JS Variables](https://time2hack.com/different-ways-to-create-objects-in-javascript/#object-notation-with-js-variables)
- [Having the value of variable as the key](https://time2hack.com/different-ways-to-create-objects-in-javascript/#having-the-value-of-variable-as-the-key)
    - [Access key of Object as Array and assign new value](https://time2hack.com/different-ways-to-create-objects-in-javascript/#access-key-of-object-as-array-and-assign-new-value)
    - [Array index access in Object Notation and Object.assign](https://time2hack.com/different-ways-to-create-objects-in-javascript/#array-index-access-in-object-notation-and-object-assign)
- [Using Object.create](https://time2hack.com/different-ways-to-create-objects-in-javascript/#using-object-create)
- [Using a Constructor Function i.e. with `new` keyword](https://time2hack.com/different-ways-to-create-objects-in-javascript/#using-a-constructor-function-i-e-with-new-keyword)


---

## Using Object Notation
The simplest way to create an object in JavaScript is by using the Object Notation.

Enclose the key and value pair in between the curly braces i.e. `{ }`

```js
const person = {
  name: 'Full Name',
  email: 'full.name@domain.com',
};

const employee = {
  id: 123456,
	person: person,
}
``` 

---

## Object.assign
Another way to create objects is by using Object.assign. It will also allow you to create immutable copies of any object.

```js
const person = Object.assign({
  name: 'Full Name',
  email: 'full.name@domain.com',
});

const newPersonDetails = Object.assign({}, person, {
  email: 'new.email@domain.com'
});
```

You can also change the object values with the `Object.assign`. Like in the following example, we will change the email of the `person` object with `Object.assign`
 
```js
const person = Object.assign({
  name: 'Full Name',
  email: 'full.name@domain.com',
});

Object.assign(person, {
  email: 'new.email@domain.com'
});
```

---

## Using Object Spread Operator
You can use the object spread operator to spread the values of any object into another object.

And hence if the target object is using the object notation, it will create a new one. Let’s see in the following example:

```js
const person = {
  name: 'Full Name',
  email: 'full.name@domain.com',
};

const personWithAddress = {
  ...person,
  address: 'Somewhere on the Earth'
};
```

---

## Object notation with JS Variables
With ES6+, you don’t need to write the key and then JS variable if the name of both is the same.

For example, if you wanna add the key named `website` to the person object and you already have a variable named `website`. You don’t need to write the same thing twice.

For example, if you wanna add the key named `website` to the `person` object. You can have a variable named `website` and then you don’t need to write them twice in the object as `website: website,`


```js
const person = {
  name: 'Full Name',
  email: 'full.name@domain.com',
};

const website = 'https://time2hack.com';

const personWithWebsite = {
  ...person,
  website
};
```

---

## Having the value of the variable as the key

Sometime you wanna create a key on the existing object, but you don’t know the name of the key; it is dynamic. In those cases, there are two ways to create an object with dynamic keys:

### Access key of Object like an Array and assign a new value

As you know, you can access the value in an object in the same way you access the Array value using indexes. You can use the same way to create those keys in the object.

```js
const person = {
  name: 'Full Name',
  email: 'full.name@domain.com',
};

console.log( person.name ) // Full Name
console.log( person['name'] ) // Full Name

const fullNameKey = 'name';

console.log( person[fullNameKey] ) // Full Name

const newKey = 'phone';
const phoneNum = '00123456789';

person[newKey] = phoneNum;

console.log(person);
// 👆→ { name: ..., email: ..., phone: '00123456789' }
```

### Array index access in Object Notation and Object.assign

```js
const person = {
  name: 'Full Name',
  email: 'full.name@domain.com',
};
const newKey = 'phone';
const phoneNum = '00123456789';

Object.assign(person, {
  [newKey]: phoneNum,
});

console.log(person);
// 👆→ { name: ..., email: ..., phone: '00123456789' }
```

---

## Using Object.create

This is a very interesting way to create new objects. In this way, you can create a new object by taking another object as reference or prototype.

This means that the new object will keep the sample object as a reference in its prototype. The prototype values are accessible in the same way you can access other values.

One more thing to notice is that you can override any value present in the prototype. But the new object will have its own value without changing the prototype.


```js
const person = {
  name: 'Full Name',
  email: 'full.name@domain.com',
};

const pankaj = Object.create(person);

console.log(pankaj); // 👈 → {}
console.log(pankaj.name); // 👈 → 'Full Name'

person.name = 'Fullest Name';

console.log(pankaj.name); // 👈 → 'Fullest Name'

console.log(pankaj.__proto__);
// 👆→ { name: 'Fullest Name', email: 'full.name@domain.com', phone: '00123456789' }

pankaj.name = 'Pankaj';
console.log(pankaj); // 👈 → { name: 'Pankaj' }
console.log(pankaj.name); // 👈 → 'Pankaj'
console.log(pankaj.__proto__.name); //👈 → 'Fullest Name'
```

And what if you wanna add some new properties to the object while creating the new object; this example shows us that:

```js
const person = {
  name: 'Full Name',
  email: 'full.name@domain.com',
};

const pankaj = Object.create(person, {
  website: { value: 'https://pankaj.pro/' }
});
```

And the final object will look like the following:

![](https://dev-to-uploads.s3.amazonaws.com/i/i4vte2bwrss8955xp9j0.png)

---

## Using a Constructor Function i.e. with `new` keyword

Now you would more likely define a Class and then create an object from that class with the new keyword.

For a long time, JavaScript didn’t have classes but still, it was Object-Oriented (OO). It achieved the OO by prototypal inheritance.

And a constructor function was a primary way to construct custom objects.

```js
const Person = function(name, email) {
	this.name = name;
	this.email = email;
  return this;
};

const person = new Person('Full Name', 'full.name@domain.com');
```

Later in ES6, JavaScript got the support of Class related keywords. And it reduced the complexity and learning curve for modern JavaScript.

Now you would more likely define a Class and then create an object from that class with the `new` keyword

```js
class Person {
  constructor(name, email) {
	  this.name = name;
	  this.email = email;
  }
}

const person = new Person('Full Name', 'full.name@domain.com');
```

---

# Conclusion
As you can see among these basic ways of creating the Objects in JavaScript; every approach has it use case.

So _“which way do you usually use to create objects?”_

Let me know through comments 💬 or on Twitter at  [@patel_pankaj_](https://twitter.com/patel_pankaj_)  and  [@time2hack](https://twitter.com/time2hack) 

If you find this article helpful, please share it with others 🗣

Subscribe to [Time to Hack](https://time2hack.com) to receive new posts right to your inbox.

---

Originally published at [https://time2hack.com](https://time2hack.com/different-ways-to-create-objects-in-javascript/) on January 31, 2020.