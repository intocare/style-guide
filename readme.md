# into.care TypeScript Style Guide

## Table of Contents

  1. [Types](#types)
  1. [References](#references)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Arrow Functions](#arrow-functions)
  1. [Classes & Constructors](#classes--constructors)
  1. [Modules](#modules)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks)
  1. [Control Statements](#control-statements)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Standard Library](#standard-library)


## Types

<a name="types--primitives"></a><a name="1.1"></a>
### [1.1](#types--primitives) Primitives

When you access a primitive type you work directly on its value.

- `string`
- `number`
- `boolean`
- `null`
- `undefined`
- `symbol`

```ts
const foo = 1;
let bar = foo;

bar = 9;

console.log(foo, bar); // => 1, 9
```

> Symbols cannot be faithfully polyfilled, so they should not be used when targeting browsers/environments that don't support them natively.

<a name="types--primitives"></a><a name="1.1"></a>
### [1.2](#types--complex) Complex

When you access a complex type you work on a reference to its value.

- `object`
- `array`
- `function`

```ts
const foo = [1, 2];
const bar = foo;

bar[0] = 9;

console.log(foo[0], bar[0]); // => 9, 9
```

**[⬆ back to top](#table-of-contents)**


## References

<a name="references--prefer-const"></a><a name="2.1"></a>
### [2.1](#references--prefer-const) Prefer const

Use `const` for all of your references; avoid using `var`.

> Why? This ensures that you can't reassign your references, which can lead to bugs and difficult to comprehend code.

```ts
// Bad
var a = 1;
var b = 2;

// Good
const a = 1;
const b = 2;
```

<a name="references--prefer-let"></a><a name="2.2"></a>
### [2.2](#references--prefer-let) Prefer let

If you must reassign references, use `let` instead of `var`.

> Why? `let` is block-scoped rather than function-scoped like `var`.

```ts
// Bad
var count = 1;
if (true) {
	count += 1;
}

// Good
let count = 1;
if (true) {
	count += 1;
}
```

<a name="references--block-scope"></a><a name="2.3"></a>
### [2.3](#references--block-scope) Block scope

Note that both `let` and `const` are block-scoped.

```ts
// Both const and let only exist in the blocks they are defined in.
{
	let a = 1;
	const b = 1;
}

console.log(a); // ReferenceError
console.log(b); // ReferenceError
```

**[⬆ back to top](#table-of-contents)**


## Objects

<a name="objects--no-new"></a><a name="3.1"></a>
### [3.1](#objects--no-new) No new

Use the literal syntax for object creation.

```ts
// Bad
const item = new Object();

// Good
const item = {};
```

<a name="objects--computer-properties"></a><a name="3.2"></a>
### [3.2](#objects--computer-properties) Computed properties

Use computed property names when creating objects with dynamic property names.

> Why? They allow you to define all the properties of an object in one place.

```ts
const getKey = k => `a key named ${k}`;

// Bad
const obj = {
	id: 5,
	name: 'San Francisco',
};
obj[getKey('enabled')] = true;

// Good
const obj = {
	id: 5,
	name: 'San Francisco',
	[getKey('enabled')]: true,
};
```

<a name="objects--object-shorthand"></a><a name="3.3"></a>
### [3.3](#objects--object-shorthand) Method shorthand

```ts
// Bad
const atom = {
	value: 1,
	addValue: function (value) {
		return atom.value + value;
	}
};

// Good
const atom = {
	value: 1,
	addValue(value) {
		return atom.value + value;
	}
};
```

<a name="objects--property-value-shorthand"></a><a name="3.4"></a>
### [3.4](#objects--property-value-shorthand) Property value shorthand

> Why? It is shorter and descriptive.

```ts
const lukeSkywalker = 'Luke Skywalker';

// Bad
const obj = {
	lukeSkywalker: lukeSkywalker
};

// Good
const obj = {
	lukeSkywalker
};
```

<a name="objects--quoted-props"></a><a name="3.5"></a>
### [3.5](#objects--quoted-props) Quoted properties

Only quote properties that are invalid identifiers.

> Why? In general we consider it subjectively easier to read. It improves syntax highlighting, and is also more easily optimized by many JavaScript engines.

```ts
// Bad
const bad = {
	'foo': 3,
	'bar': 4,
	'data-blah': 5
};

// Good
const good = {
	foo: 3,
	bar: 4,
	'data-blah': 5
};
```

<a name="objects--rest-spread"></a><a name="3.6"></a>
### [3.6](#objects--rest-spread) Object spread operator

Prefer the object spread operator over [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) to shallow-copy objects. Use the object rest operator to get a new object with certain properties omitted.

```ts
// Very bad
const original = {a: 1, b: 2};
const copy = Object.assign(original, { c: 3 }); // This mutates `original` ಠ_ಠ
delete copy.a; // So does this

// Bad
const original = {a: 1, b: 2};
const copy = Object.assign({}, original, {c: 3}); // copy => {a: 1, b: 2, c: 3}

// Good
const original = {a: 1, b: 2};
const copy = {...original, c: 3}; // copy => {a: 1, b: 2, c: 3}

const {a, ...excludeA} = copy; // excludeA => { b: 2, c: 3 }
```

**[⬆ back to top](#table-of-contents)**


## Arrays

<a name="arrays--no-new"></a><a name="4.1"></a>
### [4.1](#arrays--no-new) No new

Use the literal syntax for array creation.

```ts
// Bad
const items = new Array<string>();

// Good
const items: string[] = [];
```

<a name="arrays--push"></a><a name="4.2"></a>
### [4.2](#arrays-push) Array#push

Use [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) instead of direct assignment to add items to an array.

```ts
const someStack: string[] = [];

// Bad
someStack[someStack.length] = 'abracadabra';

// Good
someStack.push('abracadabra');
```

<a name="arrays--spreads"></a><a name="4.3"></a>
### [4.3](#arrays--spreads) Spread operator

Use array spreads `...` to copy arrays.

```ts
// Bad
const itemsCopy: string[] = [];

for (let i = 0; i < items.length; i += 1) {
	itemsCopy[i] = items[i];
}

// Good
const itemsCopy = [...items];
```

<a name="arrays--from-iterable"></a><a name="4.4"></a>
### [4.4](#arrays--from-iterable) Convert from iterable

Use the spread operator `...` instead of [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) to convert an iterable object to an array.

```ts
const uniqueNumbers = new Set<number>([1, 2, 3]);

// Good
const numbers = Array.from(uniqueNumbers);

// Best
const numbers = [...uniqueNumbers];
```

<a name="arrays--bracket-newline"></a><a name="4.5"></a>
### [4.5](#arrays--bracket-newline) Multiple lines

Use line breaks after open and before close array brackets if an array has multiple items.

```ts
// Bad
const arr = [
	[0, 1], [2, 3], [4, 5]
];

const objectInArray = [{
	id: 1
}, {
	id: 2
}];

const numberInArray = [
	1, 2
];

// Good
const arr = [
	[0, 1],
	[2, 3],
	[4, 5]
];

const objectInArray = [
	{
		id: 1
	},
	{
		id: 2
	}
];

const numberInArray = [
	1,
	2
];
```

<a name="arrays--prefer-for-of"></a><a name="4.6"></a>
### [4.6](#arrays--prefer-for-of) Prefer for-of

Use a `for-of` loop instead of a standard `for` loop when the index is not needed.

> Why? A `for-of` loop is easier to implement and read when the index is not needed.

```ts
const items = ['🦄', '🌈'];

// Bad
for (let i = 0; i < items.length; i++) {
	console.log(items[i]);
}

// Good
for (const item of items) {
	console.log(item);
}
```

<a name="arrays--prefer-for"></a><a name="4.7"></a>
### [4.7](#arrays--prefer-for) Prefer for

Use a `for` loop instead of [`Array.forEach`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) when the index is needed.

> Why? First of all, a `for` loop is easier to reason about than a callback style `forEach` loop. The main reason is that `forEach` most of the time breaks the pure function principle which means that it will most likely trigger side effects.

```ts
const items = ['🦄', '🌈'];

// Bad
items.forEach((item, i) => {
	console.log(`${i}. ${item}`);
});

// Good
for (let i = 0; i < items.length; i++) {
	console.log(`${i}. ${item}`);
}
```

**[⬆ back to top](#table-of-contents)**


## Destructuring

<a name="destructuring--object"></a><a name="5.1"></a>
### [5.1](#destructuring--object) Object

Use object destructuring when accessing and using multiple properties of an object.

> Why? Destructuring saves you from creating temporary references for those properties.

```ts
// Bad
const getFullName = (user: User) => {
	const firstName = user.firstName;
	const lastName = user.lastName;

	return `${firstName} ${lastName}`;
};

// Good
const getFullName = (user: User) => {
	const {firstName, lastName} = user;

	return `${firstName} ${lastName}`;
};

// Best
const getFullName => ({firstName, lastName}: User) => {
	return `${firstName} ${lastName}`;
};
```

<a name="destructuring--array"></a><a name="5.2"></a>
### [5.2](#destructuring--array) Array

Use array destructuring.

```ts
const items = [1, 2, 3, 4];

// Bad
const first = items[0];
const second = items[1];

// Good
const [first, second] = items;
```

<a name="destructuring--object-over-array"></a><a name="5.3"></a>
### [5.3](#destructuring--object-over-array) Object over Array

Use object destructuring for multiple return values, not array destructuring.

> Why? You can add new properties over time or change the order of things without breaking call sites.

```ts
// Bad
const processInput = (input: ProcessInput) => {
	// Then a miracle occurs
	return [left, right, top, bottom];
}

// The caller needs to think about the order of return data
const [left,, top] = processInput(input);

// Good
const processInput = (input: ProcessInput) => {
	// Then a miracle occurs
	return {
		left,
		right,
		top,
		bottom
	};
}

// The caller selects only the data they need
const {left, top} = processInput(input);
```

**[⬆ back to top](#table-of-contents)**


## Strings

<a name="strings--quotes"></a><a name="6.1"></a>
### [6.1](#strings--quotes) Single quotes

Use single quotes `''` for strings.

```ts
// Bad
const name = "Capt. Janeway";

// Bad - template literals should contain interpolation or newlines
const name = `Capt. Janeway`;

// Good
const name = 'Capt. Janeway';
```

<a name="strings--line-length"></a><a name="6.2"></a>
### [6.2](#strings--line-length) No line split

Strings that cause the line to go over 100 characters should not be written across multiple lines using string concatenation.

> Why? Broken strings are painful to work with and make code less searchable.

```ts
// Bad
const errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// Bad
const errorMessage = 'This is a super long error that was thrown because ' +
	'of Batman. When you stop to think about how Batman had anything to do ' +
	'with this, you would get nowhere fast.';

// Bad
const errorMessage = [
	'This is a super long error that was thrown because',
	'of Batman. When you stop to think about how Batman had anything to do',
	'with this, you would get nowhere fast.'
].join(' ');

// Good
const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
```

<a name="strings--template-literals"></a><a name="6.3"></a>
### [6.3](#strings--template-literals) Use template literals

When programmatically building up strings, use template strings instead of concatenation.

> Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

```ts
// Bad
const sayHi = (name: string) => {
	return 'How are you, ' + name + '?';
};

// Bad
const sayHi = (name: string) => {
	return ['How are you, ', name, '?'].join();
};

// Bad
const sayHi = (name: string) => {
	return `How are you, ${ name }?`;
};

// Good
const sayHi = (name: string) => {
	return `How are you, ${name}?`;
};
```

<a name="strings--eval"></a><a name="6.4"></a>
### [6.4](#strings--eval) No `eval`

Never use `eval()` on a string, it opens too many vulnerabilities.

<a name="strings--escaping"></a>
### [6.5](#strings--escaping) No useless escape

Do not unnecessarily escape characters in strings.

> Why? Backslashes harm readability, thus they should only be present when necessary.

```ts
// Bad
const foo = '\'this\' \i\s \"quoted\"';

// Good
const foo = '\'this\' is "quoted"';
const foo = `my name is '${name}'`;
```

**[⬆ back to top](#table-of-contents)**


## Functions

<a name="functions--declarations"></a><a name="7.1"></a>
### [7.1](#functions--declarations) Use arrow-function expressions

Use arrow-function expressions instead of function declarations or function expressions.

> Why? Function declarations are hoisted, which means that it's easy - too easy - to reference the function before it is defined in the file. This harms readability and maintainability. If you find that a function's definition is large or complex enough that it is interfering with understanding the rest of the file, then perhaps it's time to extract it to its own module!

```ts
// Bad
function foo() {
	// ...
}

// Bad
const foo = function() {
	// ...
};

// Good
const foo = () => {
	// ...
};
```

<a name="functions--iife"></a><a name="7.2"></a>
### [7.2](#functions--iife) Wrap IIFE

Wrap immediately invoked function expressions (IIFE) in parentheses.

> Why? An immediately invoked function expression is a single unit - wrapping both it, and its invocation parens, in parens, cleanly expresses this. In a world with modules everywhere, you almost never need an IIFE, except when using top-level-await.

```ts
// Immediately-invoked function expression (IIFE)
(() => {
	console.log('Welcome to the Internet. Please follow me.');
}());

(async () => {
	console.log('Welcome to the Internet. Please follow me.');

	await Promise.resolve('🦄');
}());
```

<a name="functions--arguments-shadow"></a><a name="7.3"></a>
### [7.3](#functions--arguments-shadow) No `arguments` parameter

Never name a parameter `arguments`. This will take precedence over the `arguments` object that is given to every function scope. Note that arrow-functions don't have a function scope and thus don't have `arguments` implicitely.

```ts
// Bad
function foo(name, options, arguments) {
	// ...
}

// Bad
const foo = (name, options, arguments) => {
	// ...
};

// Good
function foo(name, options, args) {
	// ...
}

// Best
const foo = (name, options, args) => {
	// ...
};
```

<a name="functions--rest"></a><a name="7.4"></a>
### [7.4](#functions--rest) Use rest syntax

Never use `arguments`, opt to use rest syntax `...` instead.

> Why? `...` is explicit about which arguments you want pulled. Plus, rest arguments are a real Array, and not merely Array-like like `arguments`.

```ts
// Bad
function concatenateAll() {
	const args = Array.prototype.slice.call(arguments);

	return args.join('');
}

// Good
function concatenateAll(...args: string[]) {
	return args.join('');
}

// Best
const concatenateAll = (...args: string[]) => {
	return args.join('');
};
```

<a name="functions--default-parameters"></a><a name="7.5"></a>
### [7.5](#functions--default-parameters) Default parameters

Use default parameter syntax rather than mutating function arguments.

```ts
// Really bad
const handleThings = (opts: Options) => {
	// No! We shouldn't mutate function arguments.
	// Double bad: if opts is falsy it'll be set to an object which may
	// be what you want but it can introduce subtle bugs.
	opts = opts || {};
	// ...
};

// Still bad
const handleThings = (opts: Options) => {
	if (!opts) {
		opts = {};
	}
	// ...
};

// Good
const handleThings = (opts: Options = {}) => {
	// ...
};
```

<a name="functions--defaults-last"></a><a name="7.6"></a>
### [7.6](#functions--defaults-last) Default parameters position

Always put default parameters last.

```ts
// Bad
const handleThings = (opts: Options = {}, name?: string) => {
	// ...
};

// Good
const handleThings = (name?: string, opts: Options = {}) => {
	// ...
};
```

<a name="functions--signature-spacing"></a><a name="7.7"></a>
### [7.7](#functions--signature-spacing) Signature spacing

> Why? Consistency is good, and you shouldn't have to add or remove a space when adding or removing a name.

```javascript
// Bad
const f = function(){};
const g = function (){};
const h = function () {};

// Good
const x = function() {};
const z = () => {};
```

<a name="functions--mutate-params"></a><a name="7.8"></a>
### [7.8](#functions--mutate-params) Never mutate parameters

> Why? Manipulating objects passed in as parameters can cause unwanted variable side effects in the original caller.

```ts
// Bad
const f1 = (obj: Options) => {
	obj.key = 1;
};

// Good
const f2 = (obj: Options) => {
	const options = {
		key: 1,
		...obj
	};
};
```

<a name="functions--reassign-params"></a><a name="7.9"></a>
### [7.9](#functions--reassign-params) Never reassign parameters

> Why? Reassigning parameters can lead to unexpected behavior, especially when accessing the `arguments` object. It can also cause optimization issues, especially in V8.

```ts
// Bad
const f1 = (a: number) => {
	a = 1;
	// ...
};

const f2 = (a?: number) => {
	if (typeof a === 'undefined') {
		a = 1;
	}
	// ...
};

// Good
const f3 = (a?: number) => {
	const b = typeof a === 'undefined' ? 1 : a;
	// ...
};

// Best
const f4 = (a: number = 1) => {
	// ...
};
```

<a name="functions--spread-vs-apply"></a><a name="7.10"></a>
### [7.10](#functions--spread-vs-apply) Prefer spread over apply

Prefer the use of the spread operator `...` to call variadic functions.

> Why? It's cleaner, you don't need to supply a context, and you can not easily compose `new` with `apply`.

```ts
// Bad
const x = [1, 2, 3, 4, 5];
console.log.apply(console, x);

// Good
const x = [1, 2, 3, 4, 5];
console.log(...x);

// Bad
new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

// Good
new Date(...[2016, 8, 5]);
```

<a name="functions--signature-single-line"></a><a name="7.11"></a>
### [7.11](#functions--signature-single-line) Prefer single line signature

Write function signature on a single line.

```javascript
// bad
const foo = (bar: string,
			 baz: string,
			 quux: Options) => {
	// ...
}

const foo = (
	bar: string,
	baz: string,
	quux: Options
) => {
	// ...
};

// Good
const foo = (bar: string, baz: string, quux: Options) => {
	// ...
};
```

**[⬆ back to top](#table-of-contents)**


## Arrow Functions

<a name="arrows--implicit-return"></a><a name="8.1"></a>
### [8.1](#arrows--implicit-return) Implicit return

If the function body consists of a single statement returning an [expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) without side effects, omit the braces and use the implicit return. Otherwise, keep the braces and use a `return` statement.

> Why? Syntactic sugar. It reads well when multiple functions are chained together.

```ts
// Bad
[1, 2, 3].map(number => {
	return `A string containing the ${number + 1}.`;
});

// Good
[1, 2, 3].map(number => `A string containing the ${number + 1}.`);

// Good
[1, 2, 3].map((number, index) => ({
	[index]: number
}));

// No implicit return with side effects
function foo(callback: () => boolean) {
	const val = callback();

	if (val === true) {
		// Do something if callback returns true
	}
}

let bool = false;

// Bad
foo(() => bool = true);

// Good
foo(() => {
	bool = true;
});
```

<a name="arrows--confusing"></a><a name="8.2"></a>
### [8.2](#arrows--confusing) Avoid confusing syntax

Avoid confusing arrow function syntax (`=>`) with comparison operators (`<=`, `>=`).

```javascript
// Bad
const itemHeight = (item: Item) => item.height <= 256 ? item.largeSize : item.smallSize;

// Bad
const itemHeight = (item: Item) => item.height >= 256 ? item.largeSize : item.smallSize;

// Good
const itemHeight = (item: Item) => (item.height <= 256 ? item.largeSize : item.smallSize);

// Good
const itemHeight = (item: Item) => {
	const {height, largeSize, smallSize} = item;

	return height <= 256 ? largeSize : smallSize;
};
```

<a name="arrows--implicit-arrow-linebreak"></a><a name="8.3"></a>
### [8.3](#whitespace--implicit-arrow-linebreak) Implicit return line break

Enforce the location of arrow function bodies with implicit returns.

```ts
// Bad
foo =>
	bar;

foo =>
	(bar);

// Good
foo => bar;
foo => (bar);
foo => (
	bar
);
```

**[⬆ back to top](#table-of-contents)**


## Classes & Constructors

<a name="constructors--no-useless"></a><a name="9.5"></a>
### [9.1](#constructors--no-useless) No useless constructor

Classes have a default constructor if one is not specified. An empty constructor function or one that just delegates to a parent class is unnecessary.

```ts
// Bad
class Jedi {
	constructor() {}

	getName() {
		return this.name;
	}
}

// Bad
class Rey extends Jedi {
	constructor(...args) {
		super(...args);
	}

	getName() {
		return this.name;
	}
}

// Good
class Rey extends Jedi {
	constructor(...args) {
		super(...args);

		this.name = 'Rey';
	}

	getName() {
		return this.name;
	}
}
```

**[⬆ back to top](#table-of-contents)**


## Modules

<a name="modules--no-export-from-import"></a><a name="10.1"></a>
### [10.1](#modules--no-export-from-import) No export from import

Do do not export directly from an import.

> Why? Although the one-liner is concise, having one clear way to import and one clear way to export makes things consistent.

```javascript
// Bad
export {rainbow as default} from './unicorn';

// Good
import {rainbow} from './unicorn';

export default rainbow;
```

<a name="modules--no-duplicate-imports"></a><a name="10.2"></a>
### [10.2](#modules--no-duplicate-imports) No duplicate import

Only import from a path in one place.

> Why? Having multiple lines that import from the same path can make code harder to maintain.

```ts
// Bad
import foo from 'foo';
// Some other imports
import {named1, named2} from 'foo';

// Good
import foo, {named1, named2} from 'foo';
```

<a name="modules--no-mutable-exports"></a><a name="10.3"></a>
### [10.3](#modules--no-mutable-exports) No mutable export

Do not export mutable bindings.

> Why? Mutation should be avoided in general, but in particular when exporting mutable bindings. While this technique may be needed for some special cases, in general, only constant references should be exported.

```ts
// Bad
let foo = 3;

export {foo};

// Good
const foo = 3;

export {foo};
```

<a name="modules--prefer-default-export"></a><a name="10.3"></a>
### [10.4](#modules--prefer-default-export) Prefer default export

In modules with a single export, prefer default export over named export.

> Why? To encourage more files that only ever export one thing, which is better for readability and maintainability.

```ts
// Bad
export function foo() {}

// Good
export default function foo() {}
```

<a name="modules--imports-first"></a><a name="10.5"></a>
### [10.5](#modules--imports-first) Imports first

Put all `import`s above non-import statements.

> Why? Since `import`s are hoisted, keeping them all at the top prevents surprising behavior.

```ts
// Bad
import foo from 'foo';
foo.init();

import bar from 'bar';

// Good
import foo from 'foo';
import bar from 'bar';

foo.init();
```

<a name="modules--import-order"></a><a name="10.6"></a>
### [10.6](#modules--imports-first) Import order

Following order should be respected.
1. Standard library
2. External packages
3. Internal modules


```ts
// Bad
import foo from './foo';
import * as path from 'path';
import ow from 'ow';

// Good
import * as path from 'path';
import ow from 'ow';
import foo from './foo';
```

**[⬆ back to top](#table-of-contents)**


## Properties

<a name="properties--dot"></a><a name="11.1"></a>
### [11.1](#properties--dot) Dot notation

Use dot notation when accessing properties.

```ts
const luke = {
	jedi: true,
	age: 28
};

// Bad
const isJedi = luke['jedi'];

// Good
const isJedi = luke.jedi;
```

<a name="properties--bracket"></a><a name="11.2"></a>
### [11.2](#properties--bracket) Bracket notation for dynamic properties

Use bracket notation `[]` when accessing properties with a variable.

```ts
const luke = {
	jedi: true,
	age: 28
};

const getProp = (prop: string) => luke[prop];

const isJedi = getProp('jedi');
```

<a name="properties--exponentiation-operator"></a><a name="11.3"></a>
### [11.3](#es2016-properties--exponentiation-operator)

Use exponentiation operator `**` when calculating exponentiations.

```ts
// Bad
const binary = Math.pow(2, 10);

// Good
const binary = 2 ** 10;
```

**[⬆ back to top](#table-of-contents)**


## Variables

<a name="variables--const"></a><a name="12.1"></a>
### [12.1](#variables--const) Use `const`/`let`

Always use `const` or `let` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

```ts
// Bad
superPower = new SuperPower();

// Good
const superPower = new SuperPower();
```

<a name="variables--one-const"></a><a name="12.2"></a>
### [12.2](#variables--one-const) One variable assignment

Use one `const` or `let` declaration per variable or assignment.

> Why? It's easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs. You can also step through each declaration with the debugger, instead of jumping through all of them at once.

```javascript
// Bad
const items = getItems(),
	goSportsTeam = true,
	dragonball = 'z';

// Bad
// (compare to above, and try to spot the mistake)
const items = getItems(),
	goSportsTeam = true;
	dragonball = 'z';

// Good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';
```

<a name="variables--const-let-group"></a><a name="12.3"></a>
### [12.3](#variables--const-let-group) Const first

Group all your `const`s and then group all your `let`s.

> Why? This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

```ts
// Bad
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;
let len;

// Good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
let length;
```

<a name="variables--define-where-used"></a><a name="12.4"></a>
### [12.4](#variables--define-where-used) Define where used

Assign variables where you need them, but place them in a reasonable place.

> Why? `let` and `const` are block scoped and not function scoped.

```ts
// bad - unnecessary function call
const checkName = (hasName: string) => {
	const name = getName();

	if (hasName === 'test') {
		return false;
	}

	if (name === 'test') {
		this.setName('');

		return false;
	}

	return name;
};

const checkName = (hasName: string) => {
	if (hasName === 'test') {
		return false;
	}

	const name = getName();

	if (name === 'test') {
		this.setName('');

		return false;
	}

	return name;
};
```

<a name="variables--no-chain-assignment"></a><a name="12.5"></a>
### [12.5](#variables--no-chain-assignment) Don't chain assignments

Don't chain variable assignments.

> Why? Chaining variable assignments creates implicit global variables.

```javascript
// Bad
(() => {
	// JavaScript interprets this as
	// let a = ( b = ( c = 1 ) );
	// The let keyword only applies to variable a; variables b and c become global variables.
	let a = b = c = 1;
}());

console.log(a); // throws ReferenceError
console.log(b); // 1
console.log(c); // 1

// Good
(() => {
	let a = 1;
	let b = a;
	let c = a;
}());

console.log(a); // throws ReferenceError
console.log(b); // throws ReferenceError
console.log(c); // throws ReferenceError

// The same applies for `const`
```

<a name="variables--unary-increment-decrement"></a><a name="13.6"></a>
### [12.6](#variables--unary-increment-decrement) No unary increments

Avoid using unary increments and decrements (`++`, `--`) except within `for`-loops.

> Why? Per the eslint documentation, unary increment and decrement statements are subject to automatic semicolon insertion and can cause silent errors with incrementing or decrementing values within an application. It is also more expressive to mutate your values with statements like `num += 1` instead of `num++` or `num ++`. Disallowing unary increment and decrement statements also prevents you from pre-incrementing/pre-decrementing values unintentionally which can also cause unexpected behavior in your programs.

```ts
// Bad
const array = [1, 2, 3];
let num = 1;
num++;
--num;

let sum = 0;
let truthyCount = 0;

for (let i = 0; i < array.length; i++) {
	let value = array[i];
	sum += value;

	if (value) {
		truthyCount++;
	}
}

// Good
const array = [1, 2, 3];
let num = 1;
num += 1;
num -= 1;

const sum = array.reduce((a, b) => a + b, 0);
const truthyCount = array.filter(Boolean).length;
```

<a name="variables--no-unused-vars"></a><a name="12.7"></a>
### [12.7](#variables--no-unused-vars) No unused vars

Disallow unused variables.

> Why? Variables that are declared and not used anywhere in the code are most likely an error due to incomplete refactoring. Such variables take up space in the code and can lead to confusion by readers.

```ts
// Bad
var some_unused_var = 42;

// Write-only variables are not considered as used.
let y = 10;
y = 5;

// A read for a modification of itself is not considered as used.
let z = 0;
z = z + 1;

// Unused function arguments.
const getX = (x: number, y: number) => x;

// Good
const getXPlusY = (x: number, y: number) => x * y;

let x = 1;
let y = a + 2;

alert(getXPlusY(x, y));

// 'type' is ignored even if unused because it has a rest property sibling.
// This is a form of extracting an object that omits the specified keys.
var {type, ...coords} = data;
// 'coords' is now the 'data' object without its 'type' property.
```

**[⬆ back to top](#table-of-contents)**


## Comparison Operators & Equality

<a name="comparison--eqeqeq"></a><a name="13.1"></a>
### [13.1](#comparison--eqeqeq) Prefer triple-equal

Use `===` and `!==` over `==` and `!=`.

<a name="comparison--if"></a><a name="13.2"></a>
### [13.2](#comparison--if) If evaluation

Conditional statements such as the `if` statement evaluate their expression using coercion with the `ToBoolean` abstract method and always follow these simple rules:
- **Objects** evaluate to **true**
- **Undefined** evaluates to **false**
- **Null** evaluates to **false**
- **Booleans** evaluate to **the value of the boolean**
- **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
- **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

```ts
if ([0] && []) {
	// true
	// An array (even an empty one) is an object, objects will evaluate to true
}
```

<a name="comparison--shortcuts"></a><a name="13.3"></a>
### [13.3](#comparison--shortcuts) Comparison shortcuts

Use shortcuts for booleans, but explicit comparisons for strings and numbers.

```ts
// Bad
if (isValid === true) {
	// ...
}

// Good
if (isValid) {
	// ...
}

// Bad
if (name) {
	// ...
}

// Good
if (name !== '') {
	// ...
}

// Bad
if (collection.length) {
	// ...
}

// Good
if (collection.length > 0) {
	// ...
}
```

<a name="comparison--switch-blocks"></a><a name="13.4"></a>
### [13.4](#comparison--switch-blocks) Switch blocks

Use braces to create blocks in `case` and `default` clauses that contain lexical declarations (e.g. `let`, `const`, `function`, and `class`).

> Why? Lexical declarations are visible in the entire `switch` block but only get initialized when assigned, which only happens when its `case` is reached. This causes problems when multiple `case` clauses attempt to define the same thing.

```ts
// Bad
switch (foo) {
	case 1:
		let x = 1;
		break;
	case 2:
		const y = 2;
		break;
	case 3:
		function f() {
			// ...
		}
		break;
	default:
		class C {}
}

// Good
switch (foo) {
	case 1: {
		let x = 1;
		break;
	}
	case 2: {
		const y = 2;
		break;
	}
	case 3: {
		function f() {
			// ...
		}
		break;
	}
	case 4: {
		bar();
		break;
	}
	default: {
		class C {}
	}
}
```

<a name="comparison--nested-ternaries"></a><a name="13.5"></a>
### [13.5](#comparison--nested-ternaries) Nested ternaries

Ternaries should not be nested and generally be single line expressions.

```javascript
// Bad
const foo = maybe1 > maybe2
	? 'bar'
	: value1 > value2 ? 'baz' : null;

// Split into 2 separated ternary expressions
const maybeNull = value1 > value2 ? 'baz' : null;

// Good
const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
```


<a name="comparison--unneeded-ternary"></a><a name="13.6"></a>
### [13.6](#comparison--unneeded-ternary) No unneeded ternary

Avoid unneeded ternary statements.

```ts
// Bad
const foo = a ? a : b;
const bar = c ? true : false;
const baz = c ? false : true;
const qux = a ? a : '🦄';

// Good
const foo = a || b;
const bar = !!c;
const baz = !c;
const qux = a || '🦄';
```

<a name="comparison--no-mixed-operators"></a><a name="13.7"></a>
### [13.7](#comparison--no-mixed-operators) No mixed operators

When mixing operators, enclose them in parentheses.

> Why? This improves readability and clarifies the developer's intention.

```ts
// Bad
const foo = a && b < 0 || c > 0 || d + 1 === 0;

// Bad
const bar = a ** b - 5 % d;

// Bad
// one may be confused into thinking (a || b) && c
if (a || b && c) {
	return d;
}

// Bad
const bar = a + b / c * d;

// Good
const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

// Good
const bar = (a ** b) - (5 % d);

// Good
if (a || (b && c)) {
	return d;
}

// Good
const bar = a + ((b / c) * d);
```

**[⬆ back to top](#table-of-contents)**


## Blocks

<a name="blocks--braces"></a><a name="14.1"></a>
### [14.1](#blocks--braces) Use braces

Use braces with all blocks.

```ts
// Bad
if (test)
	return false;

// Bad
if (test) return false;

// Good
if (test) {
	return false;
}

// Bad
const foo = () => { return false; }

// Good
const bar = () => {
	return false;
}

// Good
cost bar = () => false;
```

<a name="blocks--cuddled-elses"></a><a name="14.2"></a>
### [14.2](#blocks--cuddled-elses) Cuddled elses

If you're using blocks with `if` and `else`, put `else` on the same line as your `if` block's closing brace.

```ts
// Bad
if (test) {
	thing1();
	thing2();
}
else {
	thing3();
}

// Good
if (test) {
	thing1();
	thing2();
} else {
	thing3();
}
```

<a name="blocks--no-else-return"></a><a name="14.3"></a>
### [14.3](#blocks--no-else-return) No unnecessary return

If an `if` block always executes a `return` statement, the subsequent `else` block is unnecessary. A `return` in an `else if` block following an `if` block that contains a `return` can be separated into multiple `if` blocks.

```ts
// Bad
const foo = () => {
	if (x) {
		return x;
	} else {
		return y;
	}
};

// Bad
const cats = () => {
	if (x) {
		return x;
	} else if (y) {
		return y;
	}
};

// Bad
const dogs = () => {
	if (x) {
		return x;
	} else {
		if (y) {
			return y;
		}
	}
};

// Good
const foo = () => {
	if (x) {
		return x;
	}

	return y;
};

// Good
const cats = () => {
	if (x) {
		return x;
	}

	if (y) {
		return y;
	}
};

// Good
const dogs => () => {
	if (x) {
		if (z) {
			return y;
		}
	} else {
		return z;
	}
};
```

<a name="blocks--multi-line"></a><a name="14.4"></a>
### [14.4](#blocks--multi-line) No single line

Write blocks always on multiple lines.

```ts
// Bad
if (test) return false;

// Bad
while(x > y) doOperation();

// Good
if (test) {
	return false;
}

// Good
while(x > y) {
	doOperation();
}
```

**[⬆ back to top](#table-of-contents)**


## Control Statements

<a name="control-statement--value-selection"></a><a name="15.1"></a>
### [15.1](#control-statements--value-selection) Value selection

Don't use selection or ternary operators in place of control statements.

```ts
// Bad
!isRunning && startRunning();

// Bad
!isRunning ? startRunning() : printWarning();

// Good
if (!isRunning) {
	startRunning();
}

// Good
if (!isRunning) {
	startRunning();
} else {
	printWarning();
}
```

**[⬆ back to top](#table-of-contents)**


## Comments

<a name="comments--singleline"></a><a name="16.1"></a>
### [16.1](#comments--singleline) Single line comments

Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it's on the first line of a block.

Start every comment with caps.

```ts
// Bad
const active = true;  // is current tab

// Good
// Is current tab
const active = true;

// Bad
const getType = () => {
	console.log('fetching type...');
	// Set the default type to 'no type'
	return this.type || 'no type';
};

// Bad
const getType = () => {
	console.log('fetching type...');

	// set the default type to 'no type'
	return this.type || 'no type';
};

// Good
const getType = () => {
	console.log('fetching type...');

	// Set the default type to 'no type'
	const type = this.type || 'no type';

	return type;
};

// Good
const getType = () => {
	// Set the default type to 'no type'
	return type = this.type || 'no type';
};
```

<a name="comments--multiline"></a><a name="16.2"></a>
### [16.2](#comments--multiline) Multiline comments

Use `/** ... */` for multi-line comments.

```ts
// Bad
// Create a new element based on the passed in tag name.
//
// @param tag - Name of the tag.
const make => (tag: string) => {

	// ...

	return element;
};

// Good
/**
 * Create a new element based on the passed in tag name.
 *
 * @param tag - Name of the tag.
 */
const make => (tag: string) => {

	// ...

	return element;
};
```

<a name="comments--spaces"></a></a><a name="16.3"></a>
### [16.3](#comments--spaces)

Start all comments with a space to make it easier to read.

```ts
// Bad
//Is current tab
const active = true;

// Good
// Is current tab
const active = true;

// Bad
/**
 *Create a new element based on the passed in tag name.
 *
 *@param tag - Name of the tag.
 */
const make => (tag: string) => {

	// ...

	return element;
};

// Good
/**
 * Create a new element based on the passed in tag name.
 *
 * @param tag - Name of the tag.
 */
const make => (tag: string) => {

	// ...

	return element;
};
```

<a name="comments--actionitems"></a><a name="16.4"></a>
### [16.4](#comments--actionitems) Action items

Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME: -- need to figure this out` or `TODO: -- need to implement`.

<a name="comments--fixme"></a><a name="16.5"></a>
### [16.5](#comments--fixme) Fixme

Use `// FIXME:` to annotate problems.

```ts
class Calculator extends Abacus {
	constructor() {
		super();

		// FIXME: shouldn't use a global here
		total = 0;
	}
}
```

<a name="comments--todo"></a><a name="16.6"></a>
### [16.6](#comments--todo) Todo

Use `// TODO:` to annotate solutions to problems.

```ts
class Calculator extends Abacus {
	constructor() {
		super();

		// TODO: total should be configurable by an options param
		this.total = 0;
	}
}
```

**[⬆ back to top](#table-of-contents)**


## Whitespace

<a name="whitespace--spaces"></a><a name="17.1"></a>
### [17.1](#whitespace--tabs) Tabs

Use a tab character for indentation.

```ts
// Bad
const foo = () => {
∙∙∙∙let name;
};

// Good
const baz = () => {
---⇥let name;
};
```

<a name="whitespace--before-blocks"></a><a name="17.2"></a>
### [17.2](#whitespace--before-blocks) Space before blocks

Place 1 space before the leading brace.

```ts
// Bad
const test = () =>{
	console.log('test');
};

// Good
const test = () => {
	console.log('test');
};

// Bad
dog.set('attr',{
	age: '1 year',
	breed: 'Bernese Mountain Dog',
});

// Good
dog.set('attr', {
	age: '1 year',
	breed: 'Bernese Mountain Dog',
});
```

<a name="whitespace--around-keywords"></a><a name="17.3"></a>
### [17.3](#whitespace--around-keywords) Space around keywords

Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space between the argument list and the function name in function calls and declarations. eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html)

```ts
// Bad
if(isJedi) {
	fight ();
}

// Good
if (isJedi) {
	fight();
}

// Bad
function fight () {
	console.log ('Swooosh!');
}

// Good
function fight() {
	console.log('Swooosh!');
}
```

<a name="whitespace--infix-ops"></a><a name="17.4"></a>
### [17.4](#whitespace--infix-ops) Infix operations

Add spaces between operands.

```ts
// Bad
const x=y+5;

// Bad
const x = y+5;

// Good
const x = y + 5;
```

<a name="whitespace--newline-at-end"></a><a name="17.5"></a>
### [17.5](#whitespace--newline-at-end) End with newline

End files with a single newline character.

```ts
// bad
import {rainbow} from './unicorn';
// ...
export default rainbow;
```

```ts
// Bad
import {rainbow} from './unicorn';
// ...
export default rainbow;↵
↵
```

```ts
// Good
import {rainbow} from './unicorn';
// ...
export default rainbow;↵
```

<a name="whitespace--chains"></a><a name="17.6"></a>
### [17.6](#whitespace--chains) Chaining

Use indentation when making long method chains. Use a leading dot, which emphasizes that the line is a method call, not a new statement.

```ts
// Gad
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// Gad
$('#items').
	find('.selected').
	highlight().
	end().
	find('.open').
	updateCount();

// Good
$('#items')
	.find('.selected')
	.highlight()
	.end()
	.find('.open')
	.updateCount();

// Bad
const leds = stage.selectAll('.led').data(data);

// Good
const leds = stage
	.selectAll('.led')
	.data(data);

// Bad
const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
	.attr('width', (radius + margin) * 2).append('svg:g')
	.attr('transform', `translate(${radius + margin},${radius + margin})`)
	.call(tron.led);

// Good
const leds = stage
	.selectAll('.led')
	.data(data)
	.enter()
	.append('svg:svg')
	.classed('led', true)
	.attr('width', (radius + margin) * 2)
	.append('svg:g')
	.attr('transform', `translate(${radius + margin},${radius + margin})`)
	.call(tron.led);

// good
const leds = stage.selectAll('.led').data(data);
```

<a name="whitespace--after-blocks"></a><a name="17.7"></a>
### [17.7](#whitespace--after-blocks) Empty line after block

Leave a blank line after blocks and before the next statement.

```ts
// Bad
if (foo) {
	return bar;
}
return baz;

// Good
if (foo) {
	return bar;
}

return baz;

// Bad
const obj = {
	foo() {
		// ...
	},
	bar() {
		// ...
	}
};
return obj;

// Good
const obj = {
	foo() {
		// ...
	},
	bar() {
		// ...
	}
};

return obj;
```

<a name="whitespace--padded-blocks"></a><a name="17.8"></a>
### [17.8](#whitespace--padded-blocks) Don't pad blocks

Do not pad your blocks with blank lines.

```javascript
// Bad
const bar = () => {

	console.log(foo);

};

// Bad
if (baz) {

	console.log(qux);
} else {
	console.log(foo);

}

// Bad
class Foo {

	constructor(bar: string) {
		//
	}
}

// Good
const bar = () => {
	console.log(foo);
};

// Good
if (baz) {
	console.log(qux);
} else {
	console.log(foo);
}
```

<a name="whitespace--no-multiple-blanks"></a><a name="17.9"></a>
[17.9](#whitespace--no-multiple-blanks) No multiple empty lines

Do not use multiple blank lines to pad your code.

```javascript
// Bad
class Person {

	private age: number;


	constructor(private fullName: string, private email: string, birthday: Date) {
		this.setAge(birthday);
	}


	setAge(birthday: Date) {
		const today = new Date();


		const age = this.getAge(today, birthday);


		this.age = age;
	}


	private getAge(today: Date, birthday: Date) {
		// Calculate age
	}
}

// Good
class Person {
	private age: number;

	constructor(private fullName: string, private email: string, birthday: Date) {
		this.setAge(birthday);
	}

	setAge(birthday: Date) {
		const today = new Date();

		this.age = getAge(today, birthday);
	}

	private getAge(today: Date, birthday: Date) {
		// Calculate age
	}
}
```

<a name="whitespace--in-parens"></a><a name="17.10"></a>
### [17.10](#whitespace--in-parens) No space in parentheses

Do not add spaces inside parentheses.

```ts
// Bad
const bar = ( foo: string ) => {
	return foo;
};

// Good
const bar = (foo: string) => {
	return foo;
};

// Bad
if ( foo ) {
	console.log(foo);
}

// Good
if (foo) {
	console.log(foo);
}
```

<a name="whitespace--in-brackets"></a><a name="17.11"></a>
### [17.11](#whitespace--in-brackets) No space in brackets

Do not add spaces inside brackets.

```ts
// Bad
const foo = [ 1, 2, 3 ];
console.log(foo[ 0 ]);

// Good
const foo = [1, 2, 3];
console.log(foo[0]);
```

<a name="whitespace--in-braces"></a><a name="17.12"></a>
### [17.12](#whitespace--in-braces) No space in braces

Add spaces inside curly braces.

```javascript
// Bad
const foo = { clark: 'kent' };

// Good
const foo = {clark: 'kent'};
```

<a name="whitespace--comma-spacing"></a><a name="17.13"></a>
### [17.13](#whitespace--comma-spacing) Comma spacing

Avoid spaces before commas and require a space after commas.

```ts
// Bad
const data = {unicorn: '🦄',rainbow:'🌈'};
const data = ['🦄' , '🌈'];

// Good
const data = {unicorn: '🦄', rainbow:'🌈'};
const data = ['🦄', '🌈'];
```

<a name="whitespace--func-call-spacing"></a><a name="17.14"></a>
### [17.14](#whitespace--func-call-spacing) Function call spacing

Avoid spaces between functions and their invocations.

```ts
// Bad
func ();

func
();

// Good
func();
```

<a name="whitespace--key-spacing"></a><a name="17.15"></a>
### [17.15](#whitespace--key-spacing) Key-value spacing

Enforce spacing between keys and values in object literal properties.

```ts
// Bad
const obj = {foo : 42};
const obj2 = {foo:42};

// Good
const obj = {foo: 42};
```

<a name="whitespace--no-trailing-spaces"></a><a name="17.16"></a>
### [17.16](#whitespace--no-trailing-spaces) No trailing whitespace

Avoid trailing spaces at the end of lines.


## Commas

<a name="commas--leading-trailing"></a><a name="18.1"></a>
### [18.1](#commas--leading-trailing) No leading commas

Do not use leading commas.

```ts
// Bad
const story = [
	once
	, upon
	, aTime
];

// Good
const story = [
	once,
	upon,
	aTime
];

// Bad
const hero = {
	firstName: 'Ada'
	, lastName: 'Lovelace'
	, birthYear: 1815
	, superPower: 'computers'
};

// Good
const hero = {
	firstName: 'Ada',
	lastName: 'Lovelace',
	birthYear: 1815,
	superPower: 'computers'
};
```

<a name="commas--dangling"></a><a name="18.2"></a>
### [18.2](#commas--dangling) No dangling comma

Do not use dangling commas.

```ts
// Bad
const hero = {
	firstName: 'Dana',
	lastName: 'Scully',
};

const heroes = [
	'Batman',
	'Superman',
];

// Good
const hero = {
	firstName: 'Dana',
	lastName: 'Scully'
};

const heroes = [
	'Batman',
	'Superman'
];

// Bad
function createHero(
	firstName: string,
	lastName: string,
	inventorOf: unknown,
) {
	// ...
}

// Good
function createHero(
	firstName: string,
	lastName: string,
	inventorOf: unknown
) {
	// ...
}
```

**[⬆ back to top](#table-of-contents)**


## Semicolons

<a name="semicolons--required"></a><a name="19.1"></a>
### [19.1](#semicolons--required) Use semicolons

> Why? When JavaScript encounters a line break without a semicolon, it uses a set of rules called [Automatic Semicolon Insertion](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion), also known as ASI, to determine whether or not it should regard that line break as the end of a statement, and (as the name implies) place a semicolon into your code before the line break if it thinks so. ASI contains a few eccentric behaviors, though, and your code will break if JavaScript misinterprets your line break. These rules will become more complicated as new features become a part of JavaScript. Explicitly terminating your statements and configuring your linter to catch missing semicolons will help prevent you from encountering issues.

```ts
// Bad - raises exception
const reaction = 'No! That's impossible!'
(async function meanwhileOnTheFalcon() {
	// handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
	// ...
}())

// Bad - returns `undefined` instead of the value on the next line - always happens when `return` is on a line by itself because of ASI!
const foo = () => {
	return
	'search your feelings, you know it to be foo'
};

// Good
const luke = {};
const leia = {};
[luke, leia].forEach((jedi) => {
	jedi.father = 'vader';
});

// Good
const reaction = 'No! That's impossible!';
(async function meanwhileOnTheFalcon() {
	// handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
	// ...
}());

// Good
const foo = () => {
	return 'search your feelings, you know it to be foo';
};
```

[Read more](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ back to top](#table-of-contents)**


## Type Casting & Coercion

<a name="coercion--explicit"></a><a name="20.1"></a>
### [20.1](#coercion--explicit) Explicit type coercion

Perform type coercion at the beginning of the statement.

<a name="coercion--strings"></a><a name="20.2"></a>
### [20.2](#coercion--strings) No new wrappers

> Why? The `new` keyword makes sure it returns an object, not the casted value.

```ts
// => this.reviewScore = 9;

// Bad
const totalScore = new String(this.reviewScore); // typeof totalScore is "object" not "string"

// Bad
const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

// Bad
const totalScore = this.reviewScore.toString(); // isn't guaranteed to return a string

// Good
const totalScore = String(this.reviewScore);
```

<a name="coercion--numbers"></a><a name="20.3"></a>
### [20.3](#coercion--numbers) Number coercion

Use `Number` for type casting and `parseInt` always with a radix for parsing strings.

```ts
const inputValue = '4';

// Bad
const val = new Number(inputValue);

// Bad
const val = +inputValue;

// Bad
const val = inputValue >> 0;

// Bad
const val = parseInt(inputValue);

// Good
const val = Number(inputValue);

// Good
const val = parseInt(inputValue, 10);
```

<a name="coercion--booleans"></a><a name="20.4"></a>
### [20.4](#coercion--booleans) Boolean coercion

Use `Boolean` for type casting.

```ts
const age = 0;

// Bad
const hasAge = new Boolean(age);

// Bad
const hasAge = !!age;

// Good
const hasAge = Boolean(age);
```

**[⬆ back to top](#table-of-contents)**


## Naming Conventions

<a name="naming--descriptive"></a><a name="21.1"></a>
### [21.1](#naming--descriptive) Be descriptive

Avoid single letter names. Be descriptive with your naming.

```ts
// Bad
function q = () => {
	// ...
};

// Good
const query = () => {
	// ...
};
```

<a name="naming--camelCase"></a><a name="21.2"></a>
### [21.2](#naming--camelCase) Camelcase

Use camelCase when naming objects, functions, and instances.

```ts
// Bad
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}

// Good
const thisIsMyObject = {};
function thisIsMyFunction() {}
```

<a name="naming--PascalCase"></a><a name="21.3"></a>
### [21.3](#naming--PascalCase) PascalCase classes

Use PascalCase only when naming constructors or classes. eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

```ts
class user {
	private name: string;

	constructor(options: {name: string}) {
		this.name = options.name;
	}
}

const good = new user({
	name: 'nope',
});

// good
class User {
	private name: string;

	constructor(options: {name: string}) {
		this.name = options.name;
	}
}

const good = new User({
	name: 'yup',
});
```


<a name="naming--leading-underscore"></a><a name="21.4"></a>
### [21.4](#naming--leading-underscore) No trailing/leading underscores

Do not use trailing or leading underscores.

> Why? JavaScript does not have the concept of privacy in terms of properties or methods. TypeScript does with it's `private` keyword, but it's still compiled away. Although a leading underscore is a common convention to mean `private`, in fact, these properties are fully public, and as such, are part of your public API contract. This convention might lead developers to wrongly think that a change won't count as breaking, or that tests aren't needed. tl;dr: if you want something to be “private”, it must not be observably present.

```ts
// Bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';
this._firstName = 'Panda';

// Good
this.firstName = 'Panda';
```

<a name="naming--self-this"></a><a name="21.5"></a>
### [21.5](#naming--self-this) Naming `this`

Don't save references to `this`. Use arrow functions or [Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

```ts
// Bad
function foo() {
	const self = this;

	return function () {
		console.log(self);
	};
}

// Bad
function foo() {
	const that = this;

	return function () {
		console.log(that);
	};
}

// Good
function foo() {
	return () => {
		console.log(this);
	};
}
```

<a name="naming--filename-matches-export"></a><a name="21.6"></a>
### [21.6](#naming--filename-matches-export) Filenames

A base filename should match the name of its default export and should be in kebab-case. Never import an `index` file directly.

```ts
// File 1 contents
class CheckBox {
	// ...
}

export default CheckBox;

// File 2 contents
export default function fortyTwo() {
	return 42;
}

// File 3 contents
export default function insideDirectory() {}

// In some other file
// Bad
import CheckBox from './CheckBox'; // PascalCase filename
import fortyTwo from './forty_two'; // Snake-case filename

// Bad
import index from './inside_directory/index'; // Requiring the index file explicitly
import insideDirectory from './inside-directory/index'; // Requiring the index file explicitly

// Good
import CheckBox from './check-box';
import fortyTwo from './forty-two';
import insideDirectory from './inside-directory';
```

<a name="naming--Acronyms-and-Initialisms"></a><a name="21.7"></a>
### [21.7](#naming--Acronyms-and-Initialisms) Acryonyms

Acronyms and initialisms should always be all uppercased, or all lowercased.

> Why? Names are for readability, not to appease a computer algorithm.

```ts
// Bad
import SmsContainer from './containers/sms-container';

// Bad
const HttpRequests = [
	// ...
];

// Good
import SMSContainer from './containers/sms-container';

// Good
const httpRequests = [
	// ...
];
```

<a name="naming--uppercase"></a>a name="21.8"></a>
### [21.8](#naming--uppercase) Uppercase naming

You may optionally uppercase a constant only if it (1) is exported, (2) is a `const` (it can not be reassigned), and (3) the programmer can trust it (and its nested properties) to never change.

> Why? This is an additional tool to assist in situations where the programmer would be unsure if a variable might ever change. UPPERCASE_VARIABLES are letting the programmer know that they can trust the variable (and its properties) not to change.

- What about all `const` variables? - This is unnecessary, so uppercasing should not be used for constants within a file. It should be used for exported constants however.
- What about exported objects? - Uppercase at the top level of export (e.g. `EXPORTED_OBJECT.key`) and maintain that all nested properties do not change.

```ts
// Bad
const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

// Bad
export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

// Bad
export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables';

// ---

// Allowed but does not supply semantic value
export const apiKey = 'SOMEKEY';

// Better in most cases
export const API_KEY = 'SOMEKEY';

// ---

// Bad - unnecessarily uppercases key while adding no semantic value
export const MAPPING = {
	KEY: 'value'
};

// Good
export const MAPPING = {
	key: 'value'
};
```

**[⬆ back to top](#table-of-contents)**


## Standard Library

The [Standard Library](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects) contains utilities that are functionally broken but remain for legacy reasons.

<a name="standard-library--isnan"></a><a name="22.1"></a>
### [22.1](#standard-library--isnan) Use `Number.isNaN`

Use `Number.isNaN` instead of global `isNaN`.

> Why? The global `isNaN` coerces non-numbers to numbers, returning true for anything that coerces to NaN.

```ts
// Bad
isNaN('1.2'); // false
isNaN('1.2.3'); // true

// Good
Number.isNaN('1.2.3'); // false
Number.isNaN(Number('1.2.3')); // true
```

<a name="standard-library--isfinite"></a><a name="22.2"></a>
### [22.2](#standard-library--isfinite) Use `Number.isFinite`

Use `Number.isFinite` instead of global `isFinite`.

> Why? The global `isFinite` coerces non-numbers to numbers, returning true for anything that coerces to a finite number.

```ts
// Bad
isFinite('2e3'); // true

// Good
Number.isFinite('2e3'); // false
Number.isFinite(parseInt('2e3', 10)); // true
```

**[⬆ back to top](#table-of-contents)**


## License

MIT
