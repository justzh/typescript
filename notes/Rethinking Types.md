

# Rethinking Types

## Types as Sets

It's easy to think of a one-to-one correspondence between runtime types and their compile-time declarations.

In TypeScript, every type is just a set.

TypeScript provides a number of mechanisms to work with types in a set-theoretic way, and you'll find them more intuitive if you think of types as sets!

## Erased Structural Types

In TypeScript, objects are *not* of a single exact type.

Consider the following code:

	interface Pointlike {
	  x: number;
	  y: number;
	}
	interface Named {
	  name: string;
	}

	function logPoint(point: Pointlike) {
	  console.log("x = " + point.x + ", y = " + point.y);
	}

	function logName(x: Named) {
	  console.log("Hello, " + x.name);
	}

	const obj = {
	  x: 0,
	  y: 0,
	  name: "Origin",
	};

	logPoint(obj);
	logName(obj);

TypeScript's type system is *structural*, not nominal: We can use `obj` as a `Pointlike` because it has properties that are both numbers. The relationships between types are determined by the properties they contain.

TypeScript's type system is also not *reified*: there's nothing at runtime that will tell us that `obj` is `Pointlike`. In fact, the `PointLike` type is not present *in any form* at runtime.

Going back to the idea of *types as sets*, we can think of *obj* as being a member of both the `PointLike` set of values and the `Named` set of values.

## Consequences of Structural Typing

**Empty Types**

There exists the *empty type*:

	class Empty {}

	function fn(arg: Empty) {
		// do something?
	}

	// No error, but this isn't an 'Empty' ?
	fn({ k: 10 });

