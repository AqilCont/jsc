# JSC

This project aims to make a programming language that's a hybrid of JS(or TS), C(and some other stuff I like), Rust and Python. I had the idea since JS was C like anyways, and I like a lot of the things JS does even though I disagree with other ideas. Also, since JS compilers kind of exist, it didn't sound too hard to make a simpler one.
Some goals are simplicity, one pass nature, ease of typing, and decent modern features.

The current name is temporary; these are a few other names I thought of: Luca, Mince, Letscript, Trip and K

### Goals of JSC

- Reduce LOC to write while also being a native language with native performance
	- Citing Rust, C++, and C. The LOC for many things in these languages is incredible.
- Compiles to assembly through every stage
	- Targetting Windows first, Mac next, and then Linux.
- Looks clean, and the syntax is simple and easy to get into
- Not too hard to get into for JS or Python people, maybe can be used for transition from learning C with a JS bg or vice versa.

- Lanugage Design:
	- Parsable in a single pass, no need to backtrack or anything
	- NONVERBOSE as much as possible, but never making anything confusing or ambiguous
	- Easy to type, easy to read
	- I plan to incorporate support for JSON and JS Objects as dicts

- Language Syntax:
	- Only postfix, binary, and tertiary ops (F for unary, no +val or \&val)
		- It's to fix things like `a ++++++ b` (`(a ++) + (++ b)`, is also hard to parse), still leaves `a +++ b` (`(a ++) + b`) possible but oh well
	- `{}` can be exchanged for `: expr`
		- If the block takes a return statement, like inside of a function (for things like `if`s) or in a function definition(`fn fname(): 3`), it is used as the return.
	- `;`s are optional when making a single statement in a line.

## Stage 1 of JSC

- Main syntax points:

```rust
let a = 1u16 // ; optional
let b = 1000u8; // Compile time error: 1000 > u8 max
let c = 2.0e10u8; // yes
let d = 'c'u16 // u16, but converted from char so treated the same lol
let e = 'abcd' // Combines the utf8 values together into a number and puts it in to an int type for now. Complains if it's too big
let f = "hi" // All strings allocated on the stack

fn features() number {
	let a = 10
	let b = 2
	// In a function environment, omitting ; means returning the value except when you declare variables
	if a < 9: panic();
	for a:
		b *= 2;
	
	if a + b < 100 {
		// you can also have scopes or something lol
		print("too big")
		panic()
	}
	if comparison(a, b): b
	else a
}
// Generic function works for all numbers. Equivalent to fn comparison (a number, b number) 
fn comparison(a, b): a > b
fn typeandreturn() number: 4;
return features(); // return opcode for program

/* Non features in Stage 1:
	Strings, heap alloc, arrays, structs, objects, traits and ptrs.
	Oh yeah, you won't be yelled at for leaving out comment closer at the end of the file
```

#### Outside of syntax:
- Straight up assembly generation for all of that (although not the fastest assembly generation :P)
- VERY powerful generics and compiler conversions etc

## Stage 2 of JSC

- Introduces Errors
- Introduces JS **Objects** (dicts)
	- Corresponding type: `object`
	- and their derivative, **Arrays**
		- Corresponding type: `array`
- Introduces **Traits**
	- Defines a type of interface for types and 
- Introduces Structs, which are actually similar to classes and objects in JS, but can only set defined members and accessing undefined members results in an error
	- Can use strings to access members too.
		- Has to be wrapped in error handling <code>obj["x"]?</code>

```rust
// Heap allocated array
let hi = []
hi.reduce((a, b) => a + b) // which has traits implemented internally, also anon funcs!!

let yes = str[].new() // Allocates an array of strings
let yes2 = array<str>.new()

let v = "string" // Heap allocated string, dk how heap allocation is going to work but we will have it :D
// Strings will implicitly store length and will function like arrays, similar to JS. Will hopefully be UTF-8 but we'll see ;-;

// OBJECTS POG
let obj = { bruh: "ez" };


// defines a struct
struct hello {
	hi str; bruh str?; yes u32, no;// In multi var declarations(with ,), the type of the first member's type is assumed for succeeding vars
	new(): hello { hi: "bruh" }
}

trait greeting { print(self) }
impl greeting for hello {
	print(self): print("hello " + )
}

hello.new().print() // 

fn func(hi str): print(hi + " world")
func("hello ")

/* Added features in Stage 2:
	Strings and arrays, which are similar to structs but structs aren't here yet so lol
```

## Stage 3

```rust
// hello.jsc
fn hello({ lol }): print(lol);
hello { lol: 30 }

export hello;

// main.jsc
import hello from "hello.jsc" // Works like node, executes all of the code in hello then proceeds to do it here

```


## Language References

- https://dart.dev/samples
- https://github.com/vlang/v/blob/master/examples/mini_calculator.v
- Tutorial
	- https://sonictk.github.io/asm_tutorial/
- Windows Calling Conventions:
	- https://learn.microsoft.com/en-us/cpp/build/x64-calling-convention?view=msvc-170
- COOL OPTIMIZATIONS POG: https://www.youtube.com/watch?v=VMpSYJ_7aYM
- Malloc in NASM on windows:
	- https://github.com/miguelperes/custom-malloc
	- https://steemit.com/programming/@orangeflash81/dynamic-memory-allocation-on-windows-in-nasm
- Academic references:
	- https://mcs.utm.utoronto.ca/~peters43/369/
