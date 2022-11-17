---
{"dg-publish":true,"dg-permalink":"ts","permalink":"/ts/","dgHomeLink":true,"dgPassFrontmatter":false}
---


Repo: https://github.com/mike-north/ts-fundamentals-v3
Material: https://www.typescript-training.com/course/fundamentals-v3

# Hello TS

A good way to think of TS files:

-   `.ts` files contain both type information and code that runs
-   `.js` files contain only code that runs
-   `.d.ts` Declaration file:  contain only type information

# Variables and Values
**In TypeScript, variables are “born” with their types.** Although there are ways of making them more specific in certain branches of code, there’s no (safe) way of changing `age`’s type from `number` to `string`.

```ts
const age = 6
```
Notice that the type of this variable is not `number`, it’s `6`. **TS is able to make a more specific assumption here**.

### Literal Types

The type `6` is called a **literal type**. If our `let` declaration is a variable that can hold any `number`, the `const` declaration is one that can hold only `6` — a specific number.

## Implicit `any` and type annotations
TypeScript might not have enough information around the declaration site to infer what `endTime` should be, so it gets **the most flexible type: `any`**.
Think of `any` as “the normal way JS variables work”.

If we wanted more safety in the case of `any`, we could add a **type annotation**:
```ts
let startTime = new Date()

let endTime: Date
```

## Function arguments and return values
Without type annotations, “anything goes” for the arguments passed into a function.

Let’s add some type annotations to our function’s arguments:

```ts
function add(a: number, b: number) {

return a + b

}

const result = add(3, 4)
```

If we wanted to specifically state a return type, we could do so using the `:foo` syntax in one more place.

```ts
function add(a: number, b: number): number {}
```

# Objects, Arrays and Tuples

## Objects
```ts
let car: {
	make: string
	model: string
	year: number
}

function printCar(car: {
	make: string
	model: string
	year: number
	chargeVoltage?: number // optional property
}) {
	...
}
```

### Index signatures

```ts
const phones = {
	home: { country: "+1", area: "211", number: "652-4515" },
	work: { country: "+1", area: "670", number: "752-5856" },
	fax: { country: "+1", area: "322", number: "525-4357" },
}

const phones: {
	[k: string]: {
	country: string
	area: string
	number: string
	}
} = {}

phones.fax
```

## Array Types

```ts
const fileExtensions = ["js", "ts"] -> string[]

const cars = [{
	make: "Toyota",
	model: "Corolla",
	year: 2002,
	},
] -> { make: string; model: string; year: number; }[]
```

## Tuples
A multi-element, ordered data structure, where position of each item has some special meaning or convention.

**We need to explicitly state the type of a tuple** whenever we declare one.

```ts
let myCar: [number, string, string] = [
	2002,
	"Toyota",
	"Corolla",
]
```

> [!NOTE] Limitations
> As of [TypeScript 4.3](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-3.html#separate-write-types-on-properties), there’s limited support for enforcing tuple length constraints.

# Structural vs. Nominal Types
## Ways of categorizing type systems

### Static vs dynamic
Sorting type systems as either [static](https://www.typescriptlang.org/docs/handbook/2/basic-types.html#static-type-checking) or dynamic has to do with whether type-checking is performed **at compile time or runtime**.
- Static: TS, Java, C#, C++
- Dynamic: JavaScript, Python, Ruby, Perl and PHP

### Nominal vs structural
**Nominal type systems are all about NAMES**.
```ts
Car myCar = new Car();
	// TYPE CHECKING
	// -------------
	// Is `myCar` type-equivalent to
	// what `checkCar` wants as an argument?
CarChecker.checkCar(myCar);
```

**Structural type systems are all about STRUCTURE or SHAPE**

```ts
function printCar(car: {
	make: string
	model: string
	year: number
}) {
	console.log(`${car.make} ${car.model} (${car.year})`)
}

printCar(new Car()) // Fine
printCar(new Truck()) // Fine
printCar(vehicle) // Fine
```

The function `printCar` doesn’t care about which constructor its argument came from, it only cares about whether the arguments passed to it meets the requirements.

# Union and Intersection Types

## Union and Intersection Types, Conceptually
Union and intersection types can conceptually be thought of as logical boolean operators (`AND`, `OR`) as they pertain to types.

## Union Types in TypeScript
Union types in TypeScript can be described using the `|` (pipe) operator.

```ts
const outcome: "heads" | "tails"
```

Using Tuples
```ts
const outcome: ["error", Error] | ["success", { name: string; email: string; }]
```

### Working with union types
When a value has a type that includes a union, we are only able to use the “common behavior” that’s guaranteed to be there.

### Narrowing with type guards
Type guards are expressions, which when used with control flow statement, allow us to have a more specific type for a particular value.

```ts
const outcome = maybeGetUserInfo()
const [first, second] = outcome

// const second: Error | { name: string; email: string; }

if (second instanceof Error) {
	// In this branch of your code, second is an Error
	second -> const second: Error
} else {
	// In this branch of your code, second is the user info
	second -> const second: { name: string; email: string; }
}

```

### Discriminated Unions
TypeScript understands that the first and second positions of our tuple are linked.
```ts
const outcome = maybeGetUserInfo()

if (outcome[0] === "error") {... } 
// const outcome: ["error", Error]
```

## Intersection Types in TypeScript
Intersection types in TypeScript can be described using the `&` (ampersand) operator.

```ts
function makeWeek(): Date & { end: Date } {...}
```

This is quite different than what we saw with union types — this is quite literally a `Date` and `{ end: Date}` mashed together, and we have access to everything immediately.

# Interfaces and Type Aliases
Type aliases allow us to:
-   define **a more meaningful name** for this type
-   declare the particulars of the type **in a single place**
-   **import and export** this type from modules, the same as if it were an exported value

```ts
export type UserContactInfo = {
	name: string
	email: string
}
```

### Inheritance in type aliases
You can create type aliases that combine existing types with new behavior by using Intersection (`&`) types.

While there’s no true `extends` keyword that can be used when defining type aliases, this pattern has a very similar effect.

```ts
type SpecialDate = Date & { getReason(): string }

const newYearsEve: SpecialDate = {
	...new Date(),
	getReason: () => "Last day of the year",
}

newYearsEve.getReason
```

## Interfaces
An [interface](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#interfaces) is a way of defining an [_object type_](https://www.typescriptlang.org/docs/handbook/2/objects.html). An “object type” can be thought of as, “an instance of a class could conceivably look like this”.

Like type aliases, interfaces can be imported/exported between modules just like values, and they serve to provide a “name” for a specific type.

```ts
interface UserInfo {
	name: string
	email: string
}
```

### Inheritance in interfaces

#### `extends`
-   Just as in in JavaScript, **a subclass `extends` from a base class**.
-   Additionally **a “sub-interface” `extends` from a base interface**, as shown in the example below
```ts
interface Animal {
	isAlive(): boolean
}

interface Mammal extends Animal {
	getFurOrHairColor(): string
}
```

#### `implements`
TypeScript adds a second heritage clause that can be used to state that **a given class should produce instances that confirm to a given interface**: [`implements`](https://www.typescriptlang.org/docs/handbook/2/classes.html#implements-clauses).

```ts
interface AnimalLike {
	eat(food): void
}

class Dog implements AnimalLike {
	bark() {
		return "woof"
	}

	eat(food) {
		consumeFood(food)
	}
}
```

### Open Interfaces
TypeScript interfaces are “open”, meaning that unlike in type aliases, you can have multiple declarations in the same scope:
```ts
interface AnimalLike {
	isAlive(): boolean
}

interface AnimalLike {
	eat(food): void
}
```

## Choosing which to use

In many situations, either a `type` alias or an `interface` would be perfectly fine, however…

1.  **If you need to define something other than an object type** (e.g., use of the `|` union type operator), you must use a type alias
2.  If you need to define a type **to use with the `implements` heritage term**, it’s best to use an interface
3.  If you need to **allow consumers of your types to _augment_ them**, you must use an interface.

# Functions
## Callable types
Both type aliases and interfaces offer the capability to describe [call signatures](https://www.typescriptlang.org/docs/handbook/2/functions.html#call-signatures):

```ts
interface TwoNumberCalculation {
	(x: number, y: number): number
} 

type TwoNumberCalc = (x: number, y: number) => number

const add: TwoNumberCalculation = (a, b) => a + b

const subtract: TwoNumberCalc = (x, y) => x - y
```

Let’s pause for a minute to note:

-   The return type for an interface is `:number`, and for the type alias it’s `=> number`
-   Because we provide types for the functions `add` and `subtract`, we don’t need to provide type annotations for each individual function’s argument list or return type

### `void`
[`void`](https://www.typescriptlang.org/docs/handbook/2/functions.html#void) is a special type, that’s specifically used to describe function return values. It has the following meaning:
> The return value of a void function is intended to be _ignored_


```ts
function invokeInFiveSeconds(callback: () => void) {
	setTimeout(callback, 5000)
}
```

### Construct signatures
[Construct signatures](https://www.typescriptlang.org/docs/handbook/2/functions.html#construct-signatures) are similar to call signatures, except they describe what should happen with the `new` keyword.

```ts
interface DateConstructor {
	new (value: number): Date
}

let MyDateConstructor: DateConstructor = Date

const d = new MyDateConstructor()
```

## Function overloads
What if we had to create a function that allowed us to register a “main event listener”?

-   If we are passed a `form` element, we should allow registration of a “submit callback”
-   If we are passed an `iframe` element, we should allow registration of a ”`postMessage` callback”

We can solve this using [_function overloads_](https://www.typescript-training.com/course/fundamentals-v3/09-functions/dict,), where we define multiple function heads that serve as entry points to a single implementation:

## `this` types
```ts
// @errors: 2684

function myClickHandler(
	this: HTMLButtonElement,
	event: Event
) {
	this.disabled = true
}

const myButton = document.getElementsByTagName("button")[0]

const boundHandler = myClickHandler.bind(myButton)

boundHandler(new Event("click")) // bound version: ok

myClickHandler.call(myButton, new Event("click")) // also ok
```

## Function type best practices

### Explicitly define return types
TypeScript is capable of inferring function return types quite effectively, but this accommodating behavior can lead to unintentional ripple effects where types change throughout your codebase.
If we return type explicitly, the error message is surfaced at the declaration site.

# Top and bottom types
## Types describe sets of allowed values

## Top types

A [top type](https://en.wikipedia.org/wiki/Top_type) (symbol: `⊤`) is a type that describes **any possible value allowed by the system**. To use our set theory mental model, we could describe this as `{x| x could be anything }`

TypeScript provides two of these types: [`any`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#any) and [`unknown`](https://www.typescriptlang.org/docs/handbook/2/functions.html#unknown).

### `any`

You can think of values with an `any` type as “playing by the usual JavaScript rules”. 
`any` is not always a “bug” or a “problem” — it just indicates _maximal flexibility_ and _the absence of type checking validation_.
### `unknown`

Like `any`, unknown can _accept_ any value:
```ts
let flexible: unknown = 4
```

However, `unknown` is different from `any` in a very important way:

> Values with an `unknown` type cannot be _used_ without first applying a type guard

```ts
	let myUnknown: unknown = 14
	myUnknown.it.is.possible.to.access.any.deep.property
	// This code runs for { myUnknown| anything }
	if (typeof myUnknown === "string") {
	// This code runs for { myUnknown| all strings }
	console.log(myUnknown, "is a string")
}
```

## Bottom type: `never`

A [bottom type](https://en.wikipedia.org/wiki/Bottom_type) (symbol: `⊥`) is a type that describes **no possible value allowed by the system**. To use our set theory mental model, we could describe this as “any value from the following set: `{ }` (intentionally empty)”

```ts
const neverValue: never = myVehicle
```

# Type guards and narrowing

## Built-in type guards
```ts
// instanceof
if (value instanceof Date) {
	value
}

// typeof
else if (typeof value === "string") {
	value
}

// Specific value check
else if (value === null) {
	value
}

// Truthy/falsy check
else if (!value) {
	value
}

// Some built-in functions
else if (Array.isArray(value)) {
	value
}

// Property presence check
else if ("dateRange" in value) {
	value
} else {
	value
}
```

## User-defined type guards
```ts
// @noImplicitAny: false
interface CarLike {
	make: string
	model: string
	year: number
}

let maybeCar: unknown

// the guard
function isCarLike(valueToTest: any) {
return (
		valueToTest &&
		typeof valueToTest === "object" &&
		"make" in valueToTest &&
		typeof valueToTest["make"] === "string" &&
		"model" in valueToTest &&
		typeof valueToTest["model"] === "string" &&
		"year" in valueToTest &&
		typeof valueToTest["year"] === "number"
	)
}

// using the guard
if (isCarLike(maybeCar)) {
	maybeCar
]}
```

# Nullish values
## `null`
`null` means: there is a value, and that value is _nothing_.
This _nothing_ is very much a defined value, and is certainly a presence — not an absence — of information.

## `undefined`
`undefined` means the value isn’t available (yet?)

## `void`
`void` should exclusively be used to describe that a function’s return value should be ignored

## Non-null assertion operator
The non-null assertion operator (`!.`) is used to cast away the possibility that a value might be `null` or `undefined`.

# Generics
[Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html) allow us to parameterize types, which unlocks great opportunity to reuse types broadly across a TypeScript project.

It would be nice to have some kind of utility that would allow us to convert a “list of things into” a “dictionary of things”.

**What we need here is some mechanism of defining a relationship between the type of the thing we’re passed, and the type of the thing we’ll return. This is what Generics are all about**

## Defining a type parameter
Type parameters can be thought of as “function arguments, but for types”.

Functions may return different values, depending on the arguments you pass them.

> Generics may change their type, depending on the type parameters you use with them.

Our function signature is going to now include a type parameter `T`:

```ts
function wrapInArray<T>(arg: T): [T] {
	return [arg]
}
```

### The TypeParam, and usage to provide an argument type

- `<T>` to the right: means that the type of this function is now parameterized in terms of a type parameter `T` (which may change on a per-usage basis)
- list: `T[]` as a first argument: means we accept a list of `T`‘s.
- **TypeScript will infer what `T` is, on a per-usage basis, depending on what kind of array we pass in**. If we use a `string[]`, `T` will be `string`, if we use a `number[]`, `T` will be `number`.
The `return` type. Based on the way we have defined this function, a `T[]` will be turned into a `{ [k: string]: T }` _for any `T` of our choosing_.


-----
[Intermediate TypeScript](https://www.typescript-training.com/course/intermediate-v1)
# Declaration Merging

A  **identifier**[1](https://www.typescript-training.com/course/intermediate-v1/02-declaration-merging/#fn-1) can provide a named reference to some information (be it a value, or a type).

```ts
(alias) interface Fruit 
(alias) const Fruit: { 
	name: string; color: string; mass:number; 
	} 
export Fruit
```

We have one identifier that’s three things in one:

-   a value (class)
-   a type
-   a namespace

> [!Classes]
> Classes are both a type and a value.

# #Type Queries