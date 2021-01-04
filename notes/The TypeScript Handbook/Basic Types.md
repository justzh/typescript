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

