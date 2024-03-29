# Types in TS

## Boolean

The most basic datatype is the simple true/false value, which JavaScript and TypeScript call a `boolean` value.

```ts
let isDone: boolean = false;   
```

## Number

As in JavaScript, all numbers in TypeScript are either floating point values or BigIntegers. These floating point numbers get the type `number`, while BigIntegers get the type `bigint`. In addition to hexadecimal and decimal literals, TypeScript also supports binary and octal literals introduced in ECMAScript 2015.

```ts
let decimal: number = 6;  
let hex: number = 0xf00d;  
let binary: number = 0b1010;  
let octal: number = 0o744;  
let big: bigint = 100n;   
```

## String

Another fundamental part of creating programs in JavaScript for webpages and servers alike is working with textual data. As in other languages, we use the type `string` to refer to these textual datatypes. Just like JavaScript, TypeScript also uses double quotes (`"`) or single quotes (`'`) to surround string data.

```ts
let color: string = "blue";  color = 'red'; 
```  

You can also use _template strings_, which can span multiple lines and have embedded expressions. These strings are surrounded by the backtick/backquote (`` ` ``) character, and embedded expressions are of the form `${ expr }`.

```ts
let fullName: string = `Bob Bobbington`;  let age: number = 37;  let sentence: string = `Hello, my name is ${fullName}.  I'll be ${age + 1} years old next month.`;   
```

## Array

TypeScript, like JavaScript, allows you to work with arrays of values. Array types can be written in one of two ways. In the first, you use the type of the elements followed by `[]` to denote an array of that element type:

```ts
let list: number[] = [1, 2, 3];
```
The second way uses a generic array type, `Array<elemType>`:

```ts
let list: Array<number> = [1, 2, 3];   
```

## Tuple

Tuple types allow you to express an array with a fixed number of elements whose types are known, but need not be the same. For example, you may want to represent a value as a pair of a `string` and a `number`:

```ts
// Declare a tuple type  
let x: [string, number];  
// Initialize it  
x = ["hello", 10]; // OK  
// Initialize it incorrectly  
x = [10, "hello"]; 
// Error  Type 'number' is not assignable to type 'string'.   
// Error  Type 'string' is not assignable to type 'number'.
```

When accessing an element with a known index, the correct type is retrieved:

```ts
// OK  
console.log(x[0].substring(1));  
console.log(x[1].substring(1));  
//Property 'substring' does not exist on type 'number'.
```

Accessing an element outside the set of known indices fails with an error:

```ts
x[3] = "world";  
// Error Tuple type '[string, number]' of length '2' has no element at index '3'.

 console.log(x[5].toString());
 // Error Object is possibly 'undefined'.Tuple type '[string, number]' of length '2' has no element at index '5'.
```
## Enum

A helpful addition to the standard set of datatypes from JavaScript is the `enum`. As in languages like C#, an enum is a way of giving more friendly names to sets of numeric values.

```ts   
enum Color {    
	Red,    
	reen,    
	Blue,  
}  
let c: Color = Color.Green;
```

By default, enums begin numbering their members starting at `0`. You can change this by manually setting the value of one of its members. For example, we can start the previous example at `1` instead of `0`:

```ts   
enum Color {    
	Red = 1,    
	Green,    
	Blue,  
}  
let c: Color = Color.Green;   
```

## Unknown

We may need to describe the type of variables that we do not know when we are writing an application. These values may come from dynamic content – e.g. from the user – or we may want to intentionally accept all values in our API. In these cases, we want to provide a type that tells the compiler and future readers that this variable could be anything, so we give it the `unknown` type.

```ts
let notSure: unknown = 4;  
notSure = "maybe a string instead";  
// OK, definitely a boolean  notSure = false;   
```

## Any

In some situations, not all type information is available or its declaration would take an inappropriate amount of effort. These may occur for values from code that has been written without TypeScript or a 3rd party library. In these cases, we might want to opt-out of type checking. To do so, we label these values with the `any` type:

```ts
declare function getValue(key: string): any;  
// OK, return value of 'getValue' is not checked  
const str: string = getValue("myString");   
```

The `any` type is a powerful way to work with existing JavaScript, allowing you to gradually opt-in and opt-out of type checking during compilation.

Unlike `unknown`, variables of type `any` allow you to access arbitrary properties, even ones that don’t exist.

## Void

`void` is a little like the opposite of `any`: the absence of having any type at all. You may commonly see this as the return type of functions that do not return a value:

```ts   
function warnUser(): void {    
	console.log("This is my warning message");  
}
```

Declaring variables of type `void` is not useful because you can only assign `null` (only if [`strictNullChecks`](https://www.typescriptlang.org/tsconfig#strictNullChecks) is not specified, see next section) or `undefined` to them:

```ts
let unusable: void = undefined;  
// OK if `--strictNullChecks` is not given  unusable = null;   
```

## Null and Undefined

In TypeScript, both `undefined` and `null` actually have their types named `undefined` and `null` respectively. Much like `void`, they’re not extremely useful on their own:

```ts
// Not much else we can assign to these variables!  
let u: undefined = undefined;  
let n: null = null;   
```

By default `null` and `undefined` are subtypes of all other types. That means you can assign `null` and `undefined` to something like `number`.

## Never

The `never` type represents the type of values that never occur. For instance, `never` is the return type for a function expression or an arrow function expression that always throws an exception or one that never returns. Variables also acquire the type `never` when narrowed by any type guards that can never be true.

The `never` type is a subtype of, and assignable to, every type; however, _no_ type is a subtype of, or assignable to, `never` (except `never` itself). Even `any` isn’t assignable to `never`.

Some examples of functions returning `never`:

```ts
// Function returning never must not have a reachable end point  
function error(message: string): never {    throw new Error(message);  }  
// Inferred return type is never  
function fail() {    
	return error("Something failed");  
}  
// Function returning never must not have a reachable end point  
function infiniteLoop(): never {    
	while (true) {}  
}   
```

## Object

`object` is a type that represents the non-primitive type, i.e. anything that is not `number`, `string`, `boolean`, `bigint`, `symbol`, `null`, or `undefined`.

With `object` type, APIs like `Object.create` can be better represented. For example:

```ts
declare function create(o: object | null): void;  
// OK  
create({ prop: 0 });  
create(null);  
create(undefined); // with `--strictNullChecks` flag enabled, undefined is not a subtype of null  Argument of type 'undefined' is not assignable to parameter of type 'object | null'.
```

```ts
 create(42);
 //Argument of type 'number' is not assignable to parameter of type 'object'.
```

```ts
create("string");
//Argument of type 'string' is not assignable to parameter of type 'object'.
```

```ts
create(false);
//Argument of type 'boolean' is not assignable to parameter of type 'object'.
```

## Type assertions

Sometimes you’ll end up in a situation where you’ll know more about a value than TypeScript does. Usually, this will happen when you know the type of some entity could be more specific than its current type.

_Type assertions_ are a way to tell the compiler “trust me, I know what I’m doing.” A type assertion is like a type cast in other languages, but it performs no special checking or restructuring of data. It has no runtime impact and is used purely by the compiler. TypeScript assumes that you, the programmer, have performed any special checks that you need.

Type assertions have two forms.

One is the `as`-syntax:

```ts
let someValue: unknown = "this is a string";  let strLength: number = (someValue as string).length;   
```

The other version is the “angle-bracket” syntax:

```ts
let someValue: unknown = "this is a string";  let strLength: number = (<string>someValue).length;   
```

The two samples are equivalent. Using one over the other is mostly a choice of preference; however, when using TypeScript with JSX, only `as`-style assertions are allowed.