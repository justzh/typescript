# Basic Types

## Boolean

The most basic datatype is the simple true/false value, which JavaScript and TypeScript call a `boolean` value.

```typescript
let isDone: boolean = false;
```

## Number

All numbers in TypeScript are either floating point values or BigIntegers. These floating point numbers get the type `number`, while BigIntegers get the type `bigint`. In addition to hexadecimal and decimal literals, TypeScript alsosupports binary and octal literals introduced in ECMAScript2015.
	
```typescript
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let big: bigint = 100n;
```

## String

Another fundamental part of creating programs in JavaScript for webpages and servers alike is working with textual data. As in other langauges, we use the type `string` to refer to the textual datatypes. Just like JavaScript, TypeScript also uses double quotes (`"`) or single quotes (`'`) to surround string data.

```typescript
let color: string = "blue";
color = 'red';
```

You can also use *template strings*, which can span multiple lines and have embedded expressions. These strings are surrounded by the backtick/backquote (``'`'``) characters, and embedded expressions are of the form `${ expr }`.

```typescript
let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${fullName}.

I'll be ${age + 1} years old next month.`;
```

This is equivalent to declaring `sentence` like so:

```typescript
let sentence: string =
	"Hello, my name is " +
	fullName + 
	".\n\n" +
	"I'll be " + 
	(age + 1) +
	" years old next month.";
```

## Array

TypeScript, like JavaScript, allows you to work with arrays of values. Array types can be written in one of two ways. In the first, you use the type of the elements followed by `[]` to denote an array of that element type:

```typescript
let list: number[] = [1, 2, 3];
```

The second way uses a generic array type, `Array<elemType>`:

```typescript
let list: Array<number> = [1, 2, 3];
```

## Tuple

Tuple types allow you to express an array wit a fixed number of elements whose types are known, but need not be the same. For example, you may want to represent a value as a pair of a `string` and a `number`:

```typescript
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
// Type 'number' is not assignable to type 'string'.
// Type 'string' is not assignable to type 'number'.
```

When accessing an element with a known index, the correct type is retrieved:

```typescript
// OK
console.log(x[0].substring(1));

console.log(x[1].substring(1));
// Property 'substring' does not exist on type 'number'.
```

Accessing an element outside the set of known indices fails with an error:

```typescript
x[3] = "world";
// Tuple type '[string, number]' of length '2' has no element at index '3'.

console.log(x[5].toString());
// Object is possible 'undefined'.
// Tuple type '[string, number]' of length '2' has no element as index '5'.
```

## Enum

A helpful addition to the standard set of datatypes from JavaScript is the `enum`. As in languages like C#, an enum is a way of giving more friendly names to sets of numeric values.

```typescript
enum Color {
	Red,
	Green,
	Blue
}
let c: Color = Color.Green;
```

Or, even manually set all the values in the enum:

```typescript
enum Color {
	Red = 1,
	Green = 2,
	Blue = 4,
}
let c: Color = Color.Green;
```

A handy feature of enums is that you can also go from a numeric value to the name of that value to the name of that value in the enum. For example, if we had the value `2` but weren't sure what that mapped to in the `Color` enum above, we could look up the corresponding name:

```typescript
enum Color {
	Red = 1,
	Green,
	Blue,
}
let colorName: string = Color[2];

// Displays 'Green'
console.log(colorName);
```

## Unknown

We may need to describe the type of variables that we do not know when we are writing an applicaiton. These values may come from dynamic content--e.g. from the user--or we may want to intentionally accept all values in our API. In these cases, we want to provide a type that tells the compiler and future readers that this variable could be anything, so we give it the `unknown` type.

```typescript
let notSure: unknown = 4;
notSure = "maybe a string instead";

// OK, definitely a boolean
notSure = false;
```

If you have a variable with an unknown type, you can narrow it to something more specific by doing `typeof` checks, comparison checks, or more advanced type guards that will be discussed in a later chapter:

```typescript
declare const maybe: unknown;
// 'maybe' could be a string, object, boolean, undefined, or other types
const aNumber: number = maybe;
// Type 'unknown' is not assignable to type 'number'.

if (maybe == true) {
	// TypeScript knows that maybe is a boolean now
	const aBoolean: boolean = maybe;
	// So, it cannot be a string
	const aString: string = maybe;
// Type 'boolean' is not assignable to type 'string'.
}

if (typeof maybe === "string") {
	// TypeScript knows that maybe is a string
	const aString: string = maybe;
	// So, it cannot be a boolean
	const aBoolean: boolean = maybe;
// Type 'string' is not assignable to type 'boolean'.
}
```

## Any

In some situations, not all type information is available or its declaration would take an inappropriate amount of effort. These may occur for values from code that has been written without TypeScript or a 3rd party library. In these cases, we might want to opt-out of type checking. To do so, we label these value with the `any` type:

```typescript
declare function getValue(key: string): any;
// OK, return value of 'getValue' is not checked
const str: string = getValue("myString");
```

The `any` type is a powerful way to work with existing JavaScript, allowing you to gradually opt-in and opt-out of type checking during compilation.

Unlike `unknown`, variables of type `any` allow you to access arbitrary properties, even ones that don't exist. These properties include functions and TypeScript will not check their existence or type:

```typescript
let looselyTyped: any = 4;
// OK, ifItExists might exist at runtime
looselyTyped.ifItExists();
// OK, toFixed exists (but the compiler doesn't check)
looselyTyped.toFixed();

let strictlyTyped: unknown = 4;
strictlyTyped.toFixed();
// Object is of type 'unknown'.
```

The `any` will continue to propagate through your objects:

```typescript
let looselyTyped: any = {};
let d = looselyTyped.a.b.c.d;
//  ^ = let d: any
```

After all, remember that all the convenience of any comes at the cost of losing type safety. Type safety is one of the main motivations for using TypeScript and you should try to avoid using `any` when not necessary.
