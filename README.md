<div align="center">
  <img alt="JavaScript Tips & Tidbits" src="https://i.imgur.com/K7MVMOn.png" />
</div>
<p>&nbsp;</p>
<p align="center">
  A continuously-evolving compendium of javascript tips based on common areas of confusion or misunderstanding.
</p>

---

## Sections

- [General Concepts](#general-concepts)
- [Comparison](#comparison)
- [Callbacks, Promises, Async Await](#callbacks-promises-async-await)
- [Interview Questions](#interview-questions)
- [Miscellaneous](#miscellaneous)

## General Concepts

### Closures

Closure is an important javascript pattern to give private access to a variable. In this example, createGreeter returns an anonymous function that has access to the supplied greeting, "Hello." For all future uses, sayHello will have access to this greeting! 

```javascript
function createGreeter(greeting) {
  return function(name) {
    console.log(greeting + ', ' + name);
  }
}

const sayHello = createGreeter('Hello');
sayHello('Joe');
// Hello, Joe
```

### Destructuring

Don't be thrown off by javascript parameter destructuring! It's a common way to cleanly pass an object to a function. In this example, destructuring is used to cleanly pass the `person` object to the `introduce` function!

```javascript
const person = {
  name: 'Eddie',
  age: 24
}

function introduce({ name, age }) {
  console.log(`I'm ${name} and I'm ${age} years old!`);
}

console.log(introduce(person));
// "I'm Eddie and I'm 24 years old!"
```

### Spread Syntax

A javascript concept that can throw people off but is relatively simple is the spread operator! In the following case, `Math.max` can't be applied to the `arr` array because it doesn't take an array as an argument, it takes the individual elements as arguments. The spread operator `...` is used to pull the individual elements out the array.

```javascript
const arr = [4, 6, -1, 3, 10, 4];
const max = Math.max(...arr);
console.log(max);
// 10
```

### Rest Syntax

Let's talk about javascript rest syntax. You can use it to put any number of arguments passed to a function into an array!

```javascript
function myFunc(...args) {
  console.log(args[0] + args[1]);
}

myFunc(1, 2, 3, 4);
// 3
```

### Array Methods

There is some confusion around the javascript array methods `map`, `filter`, `reduce`. 

- map: return array where each element is transformed as specified by the function
- filter: return array of elements where the function returns true
- reduce: accumulate values as specified in function

```javascript
const arr = [1, 2, 3, 4, 5, 6];
const mapped = arr.map(el => el + 2);
const filtered = arr.filter(el => el === 2 || el === 4);
const reduced = arr.reduce((total, current) => total + current);

console.log(mapped);
// [3, 4, 5, 6, 7, 8]
console.log(filtered);
// [2, 4]
console.log(reduced);
// 21
```

### Generators

Don’t fear the \*. The generator function specifies what `value` is yielded next time `next()` is called. Can either have a finite number of yields, after which `next()` returns an undefined `value`, or an infinite number of values using a loop.

```javascript
function* greeter() {
  yield 'Hi';
  yield 'How are you?';
  yield 'Bye';
}

const greet = greeter();

console.log(greet.next().value);
// 'Hi'
console.log(greet.next().value);
// 'How are you?'
console.log(greet.next().value);
// 'Bye'
console.log(greet.next().value);
// undefined
```

```javascript
function* idCreator() {
  let i = 0;
  while (true)
    yield i++;
}

const ids = idCreator();

console.log(ids.next().value);
// 0
console.log(ids.next().value);
// 1
console.log(ids.next().value);
// 2
// etc...
```

## Comparison

### Identity Operator (`===`) vs. Equality Operator (`==`)

Be sure to know the difference between the identify operator (`===`) and equality operator (`==`) in javascript! The `==` operator will do type conversion prior to comparing values whereas the `===` operator will not do any type conversion before comparing.

```javascript
console.log(0 == '0');
// True
console.log(0 === '0');
// False
```

### Comparing Objects

A mistake I see javascript newcomers make is directly comparing objects. Variables are pointing to references to the objects in memory, not the objects themselves! One method to actually compare them is converting the objects to JSON strings. This has a drawback though: object property order is not guaranteed! A safer way to compare objects is to pull in a library that specializes in deep object comparison (e.g., [lodash's isEqual](https://lodash.com/docs#isEqual)).

```javascript
const joe1 = { name: 'Joe' };
const joe2 = { name: 'Joe' };

console.log(joe1 === joe2);
// False
```

## Callbacks, Promises, Async Await

### Callbacks

Far too many people are intimidated by #javascript callback functions! They are simple, take this example. The `console.log` function is being passed as a callback to `myFunc`. It gets executed when `setTimeout` completes. That’s all there is to it!

```javascript
function myFunc(text, callback) {
  setTimeout(function() {
    callback(text);
  }, 2000);
}

myFunc('Hello world!', console.log);
// 'Hello world!'
```

### Promises

Once you understand javascript callbacks you’ll soon find yourself in nested “callback hell.” This is where Promises help! Wrap your async logic in a Promise and `resolve` on success or `reject` on fail. Use “then” to handle success and `catch` to handle failure.

```javascript
const myPromise = new Promise(function(res, rej) {
  setTimeout(function(){
    if (Math.random() < 0.9) {
      return res('Hooray!');
    }
    return rej('Oh no!');
  }, 1000);
});

myPromise
  .then(function(data) {
    console.log('Success: ' + data);
   })
   .catch(function(err) {
    console.log('Error: ' + err);
   });
   
// If Math.random() returns less than 0.9 the following is logged:
// "Success: Hooray!"
// If Math.random() returns 0.9 or greater the following is logged:
// "Error: On no!"
```

### Async Await

Once you get the hang of javascript promises, you might like `async await`, which is just “syntactic sugar” on top of promises. In the following example we create an `async` function and within that we `await` the `greeter` promise. 

```javascript
const greeter = new Promise((res, rej) => {
  setTimeout(() => res('Hello world!'), 2000);
})

async function myFunc() {
  const greeting = await greeter;
  console.log(greeting);
}

myFunc();
// 'Hello world!'
```

## Interview Questions

### Traversing a Linked List

Here's a javascript solution to a classic software development interview question: traversing a linked list. You can use a while loop to recursively iterate through the linked list until there are no more values!

```javascript
const linkedList = {
  val: 5,
  next: {
    val: 3,
    next: {
      val: 10,
      next: null
    }
  }
}

let arr = [];
let head = linkedList;

while(head !== null) {
  arr.push(head.val);
  head = head.next;
}

console.log(arr);
// [5, 3, 10]
```

## Miscellaneous

### Increment and Decrement

Ever wonder what the difference between `i++` and `++i` was? Did you know both were options? `i++` returns `i` and then increments it whereas `++i` increments `i` and then returns it.

```javascript
let i = 0;
console.log(i++);
// 0
```

```javascript
let i = 0;
console.log(++i);
// 1
```

# Contributing

Contributions welcome! All I ask is that you open an issue and we discuss your proposed changes before you create a pull request.
