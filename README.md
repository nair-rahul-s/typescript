# Typescript

## What is typescript?

1. Typescript is superset of javascript, with static typing enabled for better coding. PS: Javascript is dynamically typed.
2. Dynamically typed languages perform type inference at runtime, hence lot prone to runtime errors, if used incorrectly.
3. Statically typed languages provision the upfront declaration of types, and throws compilation errors if types are not adhered to, thereby giving us early insight into some problems that can be prevented at runtime.
4. Typescript transpiles (translates and compiles) code at runtime, thereby pointing us to relevant errors during compilation.

**Examples:**

```javascript
function add(a, b) {
  return a + b;
}

const result = add(2, 5);
const result1 = add("2", 5);

console.log(result); // Will give 7
console.log(result1); // will give 25 due to string concatenation
```

```typescript
function add(a: number, b: number): number {
  // accepts number params and returns a value of type of number
  return a + b;
}

const result = add(2, 5);
const result1 = add("2", 5); // gives compilation error here saying `Argument of type 'string' is not assignable to parameter of type 'number'.`

console.log(result);
console.log(result1);
```

## How to install typescript

1. Install node and npm
2. Run `npm install typescript` to install locally or `npm install -g typescript` to install globally.
3. Run `npx tsc <filename>.ts` to transpile typescript code

## Basic types and primitives

1. `string`,`number`,`boolean` are the primary primitive types in TypeScript
2. `null` and `undefined` are also some useful primitives.
3. Typescript is also quite forgiving if you dont specify a static type to it. By default it assigns `any` type to the variable. But this is the problem in js that we are trying to overcome using typescript, so should be used minimally.
4. Arrays and Objects are more complex types in Typescript.
5. Use object type definition or model classes to define type of objects

**Examples**

```typescript
let age: number = 12;
let personName: string = "Rahul";
let isMinor: boolean = true;
let hobbies: string[] = ["coding", "sleeping"];

// custom object type declaration
let contactDetails: {
  phoneNumber: number;
  address: string;
};
// custom object definition
contactDetails = {
  phoneNumber: 99999999999,
  address: "A Street, B Block, C Wing, D Floor, X House",
};

// custom object type array declaration
let multipleContactDetails: {
  phoneNumber: number;
  address: string;
}[];

// custom object array definition
multipleContactDetails = [
  {
    phoneNumber: 99999999999,
    address: "A Street, B Block, C Wing, D Floor, X House",
  },
  {
    phoneNumber: 99999999990,
    address: "A Street, B Block, C Wing, D Floor, Y House",
  },
];
```

6. TyeScript also does a compile time type inference, when we do not explicitly provide a type. This feature allows us to prevent wrong assignment of variables at runtimes.

**Example**:

```typescript
let value = 10;
value = "abc"; // Will throw a compile time error saying `Type 'string' is not assignable to type 'number'`
```

## Union Types in TypeScript

1. Sometimies it is realistic to have a single variable to be able to hold multiple types.
2. In such cases we utilize union types. Union types are denoted using `|` operator during variable declaration.

**Example:**

```typescript
let phoneNumber: number | number[];
phoneNumber = 99999999999;
phoneNumber = [99999999999, 88888888888];
```

## Type Aliases

1. In scenarios where we are repeatedly using custom objects of same type, its weary to define same object type again and again.
2. In such scenarios we can use type aliases.
3. Type aliases can be defined using `type` keyword.

**Example:**

```typescript
// custom object type alias declaration
type ContactDetails = {
  phoneNumber: number;
  address: string;
};

// custom object definition
let contactDetails: ContactDetails = {
  phoneNumber: 99999999999,
  address: "A Street, B Block, C Wing, D Floor, X House",
};

// custom object array definition
let multipleContactDetails: ContactDetails[] = [
  {
    phoneNumber: 99999999999,
    address: "A Street, B Block, C Wing, D Floor, X House",
  },
  {
    phoneNumber: 99999999990,
    address: "A Street, B Block, C Wing, D Floor, Y House",
  },
];
```

## Generics in typescript

1. Generics is a powerful feature that help us to write complex generic functions/classes that are type safe at compile time and runtime, yet can be utilized to execute for any types of values.
2. For example, we need to write a generic function that takes an input array of specific type, and a subsequent element of same type, and adds it to the beginning if the list.

**Problems if we use `any` type in typescript for above scenario**

```typescript
ffunction insertAtBeginning(existingValues: any, newValue: any): any[] {
  const newValues = [newValue, ...existingValues];
  return newValues;
}

const demoArray = [1, 2, 3];
const newArray = insertAtBeginning(demoArray, 4);
const newArray2 = insertAtBeginning(demoArray, "4"); // This will run fine and cause a problem later, because we might be expecting only integer values in this array
```

**Generics type in typescript for above scenario**

```typescript
function insertAtBeginning<T>(existingValues: T[], newValue: T): T[] {
  const newValues = [newValue, ...existingValues];
  return newValues;
}

const demoArray = [1, 2, 3];
const demoArray2 = ["a", "b", "c"];
const newArray = insertAtBeginning(demoArray, 4); //
const newArray2 = insertAtBeginning(demoArray, "4"); // This will give compilation error `Argument of type 'string' is not assignable to parameter of type 'number'.`
const newArray3 = insertAtBeginning(demoArray2, "d"); // This works!. Same function reused.
```

## Classes in typescript

1. Blueprints of objects can be defined in typescript using `class` keyword.
2. default access modifier in typescript is `public`

**Class Example 1**

```typescript
class Person {
  firstName: string;
  lastName: string;
  age: number;
  private _courses: string[];

  // constructor defintion
  constructor(
    firstName: string,
    lastName: string,
    age: number,
    courses: string[]
  ) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
    this._courses = courses;
  }

  enrol(course: string) {
    this._courses.push(course);
  }

  listcourses() {
    return this._courses.slice(); // returns copy of the _courses object, and not the original object
  }
}
```

**Shorthand version of Example 1**

```typescript
class Person {
  // shorthand constructor
  // declare the properties with access modifiers and compiler understands the class properties
  constructor(
    public firstName: string,
    public lastName: string,
    public age: number,
    private _courses: string[]
  ) {}

  enrol(course: string) {
    this._courses.push(course);
  }

  listcourses() {
    return this._courses.slice(); // returns copy of the _courses object, and not the original object
  }
}
```

## Interfaces in TypeScript

1. Interfaces are an alternative to `type` keyword in TypeScript with an additional feature, that, classes can implement interfaces.
2. Interfaces simple declare a structure that we need to adhere to for a particular object type. Classes implementing that interface can now give its specific implementations.
3. Interfaces are great abstraction of structure and help in code maintainence.

**Example**

```typescript
interface Humanable {
  firstName: string;
  lastName: string;
  age: number;
  greet: () => void;
}

// TS forces this implementation. Else gives compile time error saying  `incorrect implementation of interface Humanable`
class Human implements Humanable {
  firstName: string;
  lastName: string;
  age: number;

  constructor(firstName: string, lastName: string, age: number) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }

  greet() {
    console.log("Hello");
  }
}
```
