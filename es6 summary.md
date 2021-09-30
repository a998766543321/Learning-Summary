# ES6 Cheat Sheet

### Object.freeze
```javascript
const magic = () => new Date();
Object.freeze(magic);
Object.isFrozen(magic);

```
### Array.concat
```javascript
var myConcat = function (arr1, arr2) {
    return arr1.concat(arr2);
};
const myConcat = (arr1, arr2) => arr1.concat(arr2);
console.log(myConcat([1, 2], [3, 4, 5]));


```
### Function default value
```javascript
const greeting = (name = "Anonymous") => "Hello " + name; 
console.log(greeting("John")); // Hello John
console.log(greeting()); // Hello Anonymous

const increment = (number, value = 1) => number + value;
console.log(increment(3,4)); // 7
console.log(increment(3)); // 4


```
### Function rest argument
```javascript
function howMany(...args) { return "You have passed " + args.length + " arguments."; } console.log(howMany(0, 1, 2)); 
console.log(howMany("string", null, [1, 2, 3], {}));


```
### Destructuring assignment
- Use destructuring assignment to swap the values of a and b so that a receives the value stored in b, and b receives the value stored in a.
```javascript
let a = 8, b = 6;
[a, b] = [b, a]; 
console.log(a, b);


```
### Destructuring assignment with spread operator
```javascript
const source = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
function removeFirstTwo(list) {
    // Variable arr is assigned to the rest of the array elements
    const [a, b, ...arr] = list;
    return arr;
}
const arr = removeFirstTwo(source);
console.log(arr); // [ 3, 4, 5, 6, 7, 8, 9, 10 ]


```
### Object Destructuring
```javascript
const stats = {
    max: 56.78,
    standard_deviation: 4.34,
    median: 34.54,
    mode: 23.87,
    min: -0.75,
    average: 35.85
};

// Express equals to: const half = (stats) => (stats.max + stats.min) / 2.0; 
const half = ({ max, min }) => (max + min) / 2.0;
console.log(half(stats));


```
### Template literal
```javascript
const result = {
    success: ["max-length", "no-amd", "prefer-arrow-functions"],
    failure: ["no-var", "var-on-top", "linebreak"],
    skipped: ["no-extra-semi", "no-dup-keys"]
};
function makeList(arr) {
    const failureItems = [];
    arr.forEach(element => {
        // ${} to map the variable 
        failureItems.push(`<li class="text-warning">${element}</li>`);
    });
    return failureItems;
}

const failuresList = makeList(result.failure);
console.log(failuresList);


```
### Object field assignment with the same name
```javascript
const createPerson = (name, age, gender) => {
    return {
        name,
        age,
        gender
    };
    // Same as 
    /*
    return {
      name: name,
      age: age,
      gender: gender
    }
    */
};

const bicycle = {
    gear: 2,
    // same as setGear: setGear(newGear) {this.gear = newGear}
    setGear(newGear) {
        this.gear = newGear;
    }
};
bicycle.setGear(3);
console.log(bicycle.gear);


```
### Class constructor
```javascript
class Vegetable{ constructor(name) {this.name = name;} };

const carrot = new Vegetable('carrot');
console.log(carrot.name); // Should display 'carrot'


```
### Class getter and setter
```javascript
class Book { constructor(author) { this._author = author; } 
  //getter 
  get writer() { return this._author + '_123'; } 
  //setter
  set writer(updatedAuthor) { this._author = updatedAuthor; } 
} 

const novel = new Book('anonymous'); 
console.log (novel.writer); // anonymous_123
novel.writer = 'newAuthor'; 
console.log(novel.writer); // newAuthor_123

```
### Class getter and setter example
```javascript
// Use the class keyword to create a Thermostat class. The constructor accepts a Fahrenheit temperature.

// In the class, create a getter to obtain the temperature in Celsius and a setter to set the temperature in Celsius.

// Remember that C = 5/9 * (F - 32) and F = C * 9.0 / 5 + 32, where F is the value of temperature in Fahrenheit, and C is the value of the same temperature in Celsius.

// Note: When you implement this, you will track the temperature inside the class in one scale, either Fahrenheit or Celsius.

// This is the power of a getter and a setter. You are creating an API for another user, who can get the correct result regardless of which one you track.

// In other words, you are abstracting implementation details from the user.

class Thermostat{
    constructor(fahrenheit) {this._fahrenheit = fahrenheit}
    get temperature() {return 5/9 * (this._fahrenheit - 32)}
    set temperature(celsius) {this._fahrenheit = celsius * 9.0 / 5 + 32}
};

const thermos = new Thermostat(76); // Setting in Fahrenheit scale
let temp = thermos.temperature; // 24.44 in Celsius
thermos.temperature = 26;
temp = thermos.temperature; // 26 in Celsius

```
### Module javascript
- index.js can import class from *canvas.js* and *square.js*
- The html only needs to specify the *main.js* with the type *module*
Project file structure
```
index.html
main.js
modules/
    canvas.js
    square.js
```
```html

<html>
  <body>
      <script type="module" src="main.js"></script>
  </body>
</html>

```
### Promise, resolve and reject
```javascript

const makeServerRequest = new Promise((resolve, reject) => {
  // responseFromServer is set to true to represent a successful response from a server
  let responseFromServer = true;
    
  if(responseFromServer) {
    resolve("We got the data");
  } else {  
    reject("Data not received");
  }
}).then(result => {console.log(result)}).catch(error => console.log(error));
// output: We got the data
```
