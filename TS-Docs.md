# My Typescript Docs

## Data Types

> **number**

```TypeScript
var x: number;
x = 5.6;

var y: number = 7;

let hexNum: number = 0xf0043;
let octNum: number = 0o5467;
let binNum: number = 0b110101;

```

_integers and floats are numbers_

<br/>

> **string**

```TypeScript
var name: string;
name = 'Jack';

const country: string = 'Sri Lanka';
```

<br/>

> **boolean**

```TypeScript
const x: boolean = false;

var z: boolean;
z = boolean;
```

<br/>

> **Array**

```TypeScript
var arr: srting[];
arr = ['abc', 'fdg'];

var arr2: number[] = [1, 2.3, 678];

var myarr: any[];
myarr = [1, true, 'XYZ'];

const X: Array = [false, 'names', 45, 'X'];

var arr3: (string | number)[] = ['age', 78, 65, 'xyz'];

var multiArr: number[][] = [[1, 2, 56.7], [0.6, 567, 4, 98]];

const arrObj: string[] = new Array('abc', 'name', 'fdgd');

// With Spread Operator
var ar = [...arr];   // output ['abc', 'fdg']
var ar2 = [...arr, 'XS', 'OOP', ...arrObj];   // output ['abc', 'fdg', 'XS', 'OOP', 'abc', 'name', 'fdgd']

```

<br/>

> **Tuples**

```TypeScript
var t: [number, string];
t = [2, 'xyz'];

const tp: [number, number] = [1, 5.6];
```

_Tuples are fixed length and fixed type Data Structure_

<br/>

> **enum**

```TypeScript
// ADMIN's value is 0
// Guest's value is 2
enum myRole {ADMIN, READ_ONLY, Guest};

// ADMIN's value is 2
// GUEST's valye is 3
// ROOT's value is 4
enum role {ADMIN = 2, GUEST, ROOT};

enum role2 {ADMIN = 5, GUEST = 67};

enum role3 {X = 'NO', OK = 1};
```

More About [Enums](https://www.typescriptlang.org/docs/handbook/enums.html)

<br/>

> **object**

```TypeScript
var myObj : {
    name: string;
    age: number;
    is_married: boolean;
    hobbies: string[];
};

myObj = {
    name:'Jack',
    age:34,
    is_married:false,
    hobbies:['xyz', 'fgd', 'rt'],
};
```

<br/>

> **Union**

```TypeScript
var x: string | number;
x = 'df';

var y: string | number;
y = 6;
```

<br/>

> **any**

```TypeScript
var x:any;
x = 5;
x = false;
x = 'xzaz';
```

_any type can be any data type_

<br/>

> **unknown**

```TypeScript
var a:unknown;
a = 5;
a = 'fjkg';

var k:string;

// This will throw an error. Since unknown type is not similar to string
// unknown type is not like any type. IT IS UNKNOWN DATA TYPE
k = a;
```

<br/>

> **undefined**

```TypeScript
var k:undefined;

// This function's return type is undefined
function(n, m) {
    return;
};
```

<br/>

> **never**

```TypeScript
// this function's return type is never
// This function throw an exception break the code
function test(a:string): never {
    throw { message:a };
};
```

<br/>

## Functions

```TypeScript
function test(A:number, x:string, ar:string[]):boolean | number {
    // A is a number 
    // x is a string
    // ar is a string array
    // return type is boolean or number
    return false;
}
```

```TypeScript
function myfunc(n1: number | string, n2: 'ADMIN' | 4, n3: number | 'XYZ') {
    // n1 can be a number or string
    // n2 can be a only 'ADMIN' or 4
    // n3 can be only number or 'XYZ'
    // no return value
}
```

```TypeScript
function tst(p:[string, number]):void {
    // p is a tuple 
    // no return value
}
```

<br/>

## Type Alias

```TypeScript
syntax
    type TYPE_NAME = data_type(s);

type MyType = number;
type myData = string | boolean;
type testType = 'Name' | 'Age';
```

_These type aliases can be used in anywhere. Ex: As function parameter types_

<br/>

## Function Types

```TypeScript
// myFunc's data type is function
const myFunc : Funtion;

var func : Function = function(n1: number, n2:number):number {
    return n1 + n2;
};

// this will describe function's behavior
// func2 is a function take no params and no return value
// this is not a function. this only descibe about function
var func2: () => void;

// fun variable's value can be a function that takes number and string as params and return number
var fun: (n: number, m:string) => number;


function square(num: number):number {
    return num * num;
};

// fun2 variable's value is square function
const fun2: (n: number) => number = square;


// fun variable is a function, takes 2 numbers and return sum of them (number)
const fun: (n1:number, n2:number) => number = function(a: number, b:number):number {
    return a + b;
};


var tst: (n:number, m:number, c?:number, d = 5) => number = function(a, b) {
    // c is an optional parameter
    // d ahs a default value
    return a + B;
};

// Arrow Function
let arrfun: (a:string, b: string) => boolean = (x, y) => {
    return x === y;
};

function(a: number, b:string[], cb: (n:number) => void) {
    // Function with callback
    cb(a);
};
```

<br/>
<br/>

**Compile TypeScript To JavaScript**

+ `tsc file.ts`
<br/>
<br/>

**TypeScript Live Compile**

+ `tsc file.ts -w`
<br/>
<br/>

**Initialize TypeScript project**

+ `tsc --init`
<br/>
<br/>

**Initialize TypeScript project folder**

+ `tsc --init`
<br/>
<br/>

**Compile All TypeScript to JS in project folder**

+ `tsc`
+ `tsc -w   \\ With Watch mode (Live Compile)`  

<br/>
<br/>

**Exclude file from compile (in project folder)**

Add this to tsconfig.json (outside of the "compilerOptions" object)
wildcard naming supported

```json
"exclude":[
    "files or folders to exclude"
]
```

<br/>
<br/>

**Include file for compile (in project folder)**

Opposite of the exclude.<br/>
Add this to tsconfig.json (outside of the "compilerOptions" object)
wildcard naming supported.<br/>
Only these file will be compiled

```json
"include":[
    "files or folders to compile"
]
```

<br/>
<br/>

## DOM And Type Casting

```TypeScript
// name's type is HTMLFormEelement
// but TypeScript doesn't know about this element's existence
const name = document.querySelector('form');

// now typescript know this element really exists
const name = document.querySelector('form')!;

// myLink's type is Eelement
const myLink = document.querySelector('.my-class');

// myTag's type is HTMLFormElement
const myTag = document.querySelector('.my-class') as HTMLFormElement;
```

<br/>
<br/>

## Classes

```TypeScript
// Create a class
class Person {
    firstname:string;
    lastname:string;
    age:number;

    constructor(f:string, l:string, a:number) {
        this.firstname = f;
        this.lastname = l;
        this.age = a;
    }

    getFullName() {
        return `${this.firstname} ${this.lastname}`;
    }
}

// Create an instance
const p1 = Person('Bill', 'Gates', 67);
const p2 = Person('Mark', 'Twain', 56);

// persons array can only take Person objects
const persons: Person[] = [];

person.push(p1, p2);
```

<br/>
<br/>

## Access Modifiers

```TypeScript
class Person {
    private state:boolean;      // cant access outside in class 
    public firstname:string;    // default behaviour
    readonly lastname:string;   // only can read. (both inside and outside of the class)
    age:number;                 // this also public

    constructor(f:string, l:string, a:number, s:boolean) {
        this.state = s;
        this.firstname = f;
        this.lastname = l;
        this.age = a;
    }
}
```

we can also do this inside the constructor

```TypeScript
class Person {
    
    constructor(
        private state:boolean,       // cant access outside in class 
        public firstname:string,     // default behaviour
        readonly lastname:string,    // only can read. (both inside and outside of the class)
        age:number                   // this also public
        ) {
        this.state = state;
        this.firstname = firstname;
        this.lastname = lastname;
        this.age = age;
    }
}
```

<br/>
<br/>

## Interfaces

```TypeScript
interface Student {
    readonly index:string;
    name:string;
    age?:number;   // optional
    fullTime:boolean;
    speak (a: number, b:string):void;
    getData (n: string, b:string):boolean;
}

const st1 = {
    index : '56',
    name:'Mark',
    fullTime:true,
    speak(a:number, b:string):void {
        console.log(a , b);
    },
    getData(x:string, y:string):boolean {
        return x === y;
    }
}
```

<br/>
<br/>

## Implement a class from interface

```TypeScript
interface Student {
    getData (n: string, b:string):boolean;
}

// This class should follow Student interface's structure
class myClass implements Student {
    constructor(
        private state:boolean,      
        public firstname:string,     
        readonly lastname:string,    
        age:number
    ) {}

    getData(x:string, y:string):boolean {
        return x === y;
    }
}

let X: Student;
let Y: Student;

X = myClass(true, 'XYZ', 'ZZZ', 56);
Y = myClass(false, 'ttZ', 'ZZcvbZ', 54);

let students: Student[] = [];
```

<br/>
<br/>

## Extending Interfaces

```TypeScript
interface Shape {
  color: string;
}

interface Square extends Shape {
  sideLength: number;
}

let square = {} as Square;
square.color = "blue";
square.sideLength = 10;

// Multiple extending
interface Shape {
  color: string;
}

interface PenStroke {
  penWidth: number;
}

interface Square extends Shape, PenStroke {
  sideLength: number;
}

let square = {} as Square;
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```

<br/>
<br/>

## Generics

```TypeScript
function fun<T>(param:T):T {
    return param;
}

var res1 = fun('Generic');
var res2 = fun<string>('Generic');
var res3 = fun<number>(454);

function fun2<X, Y, Z>(a:X, b:Y, c:Z):Y {
    return b;
}

let result3 = fun2<string, number, boolean>('hey', 3, false);
```

[Video Tutorial](https://www.youtube.com/watch?v=nViEqpgwxHE) About Generics

[Utility Types](https://www.youtube.com/watch?v=Fgcu_iB2X04) Tutorial with time stamps

---

## TypeScript Errors

> **Property '…' has no initializer and is not definitely assigned in the constructor**

+ Add ( ! ) sign after name : `someField!:string;`
+ Add ( ? ) sign after name : `someField?:string;`
+ Set type to `undefined` like this : `someFiled: string | undefined`
+ Add *`"strictPropertyInitialization": true`* to *`"angularCompilerOptions"`* in your tsconfig.json

[Stackoverflow Answer](https://stackoverflow.com/questions/49699067/property-has-no-initializer-and-is-not-definitely-assigned-in-the-construc)

