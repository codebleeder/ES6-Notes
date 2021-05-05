# ES6 notes

## var vs let vs const

* var is accessible outside the block it's defined in, which can create problems. It is accessible throughout the function. i.e., function-scoped
* let is not accessible outside the block it's defined in. i.e., block-scoped. Use this over var. 
* const are also block-scoped. But, Read-only. Cannot be re-assigned. A const object can have it's properties modified, but you can't re-declare it. 

```
const person = {
    id: 1, 
    name: 'sharad'
};
console.log(person);
person.name = 'silver'; // will work 
console.log(person);
```

will give error: 
```
person = {id: 2, name: 'jameson'}; 
console.log(person);
```

## Objects
```
    const person = {
        name: 'Mosh',
        walk(){},
        talk(){}
    };

    person.talk();
    // old dot notation:
    person.name = 'Jimmy';
   
    // use bracket notation when property name is dynamic:
    const targetMember = 'name';
    peron[targetMember] = 'John';
```

## 'this' keyword

* 'this' returns a reference to the current object.

```
const person = {
        name: 'Mosh',
        walk(){
            console.log(this);
        },
        talk(){}
    };

    person.walk();

    const walkStandalone = person.walk;
    walkStandalone(); // will return global object 

    // binding: 
    const walkStandalone2 = person.walk.bind(person);
    walkStandalone2(); // will return person object 
```
* in javascript, functions are objects

## Arrow functions 

old:
```
const square = function(num){
    return num * num;
}
```

new:
```
const square = (num) => num * num;

console.log(square(5));
```
example - get active jobs: 
```
jobs = [
    {id: 1, isActive: true},
    {id: 2, isActive: true},
    {id: 3, isActive: false},
];

let activeJobs = jobs.filter((job)=>job.isActive);
```

## Arrow functions and 'this'
* arrow functions don't re-bind 'this' 

The setTimeout function is not bound to person object and will return global object:
```
const person = {
    talk: ()=>{
        setTimeout(function(){console.log(this)}, 1000);
    },
};

person.talk();
```
old way to fix this was to declare self var:
```
const person = {
    talk(){
        var self = this;
        setTimeout(function(){console.log(self)}, 1000);
    },
};
person.talk();
```
using arrow function retains 'this':
```
const person = {
    talk(){      
        setTimeout(()=>{console.log(this)}, 1000);
    },
};
person.talk();
```

## Template literals
old:
```
let color = 'red';
let s = '<li>' + color + '</li>';
console.log(s);
```

new:
```
let color = 'red';
let s = `<li>${color}</li>`;
console.log(s);
```

## Array.map()
* example: used to render lists in react
```
const colors = ['red', 'green', 'orange'];
const items = colors.map((color) => `<li>${color}</li>`);
console.log(items);
```
## Object destructuring
old:
```
const address = {
    street: '',
    city: '',
    country: '',
};
const street = address.street;
const city = address.city;
const country = address.country;
```
new:
```
const address = {
    street: '',
    city: '',
    country: '',
};
const {street, city, country} = address;
```
if you want to use different name: 
```
const {street: st} = address;
```
## Spread Operator (...)
```
const first = [1, 2, 3];
const second = [4, 5, 6];
```
old:
```
const combined = first.concat(second);
```
new: 
```
const combined = [...first, ...second];
```
advantage of this is that you can add elements in the middle:
```
const combined = [...first, 'a', 'b', ...second];
```
clone array:
```
const firstClone = [...first];
```
combine objects and optionally add other properties:
```
const first = {name: 'Mosh'};
const second = {job: 'Instructor'};
const combined = {..first, ..second, location: 'Australia'};
```
clone object:
```
const firstClone = {...first};
```
## Classes
```
class Person {
    constructor(name){
        this.name = name;
    }
    walk(){
        console.log('walk');
    }
}

const person = new Person('Sharad');
person.walk();
```
## Inheritance
Teacher can inherit from Person: 
```
class Teacher extends Person {
    construction(name, degree){
        super(name);
        this.degree = degree;
    }
    teach(){
        console.log('teach');
    }
}
const teacher = new Teacher('James', 'MS');
teacher.walk();
teacher.teach();
```
## Modules
We can Split code into multiple files:
```
// person.js
export class Person {
    constructor(name){
        this.name = name;
    }
    walk(){
        console.log('walk');
    }
}
```
teacher.js:
```
import {Person} from './person';

export class Teacher extends Person {
    construction(name, degree){
        super(name);
        this.degree = degree;
    }
    teach(){
        console.log('teach');
    }
}
```
main.js:
```
import {Teacher} from './teacher';
const teacher = new Teacher('Sharad', 'MS');
teacher.teach();
teacher.walk();
```
## Default and Named exports
Above examples showed named exports. 
To make an object default:
```
import {Person} from './person';

export default class Teacher extends Person {
    construction(name, degree){
        super(name);
        this.degree = degree;
    }
    teach(){
        console.log('teach');
    }
}
```
No need of braces when importing default object:
```
import Teacher from './teacher';

const teacher = new Teacher('James', 'MS');
```
What if a file has both default and named exports:
```
// teacher.js
import {Person} from './person';

// named export:
export promote(){
    console.log('promote');
}

// default export:
export default class Teacher extends Person {
    construction(name, degree){
        super(name);
        this.degree = degree;
    }
    teach(){
        console.log('teach');
    }
}
```
import both default and named export like this:
```
import Teacher, {promote} from './teacher';
```