# Variables and references

### Create a variable
You create variables with `let` `=` and `;`
```rust
fn main(){
    // Create a variable and give it the value 3
    let a = 3;
    println!("{}", a);
}
```

All rust variables are strongly typed. Sometimes you may need to specify the type rust should use for a variable
```rust
fn main(){
    // Create a u64 aka big integer, call it my_var, and assign 3 to it.  
    let my_var: u64 = 3;
    println!("{}", my_var);
}
```

Variables must be initialized. The following **won't** compile
```rust, compile_fail
fn my_function() {
    let a:u64;
    
    // Print out the value of a
    println!("{}", a);
}
```

All rust variables are strongly typed.
```rust
fn main() {
    // numbers default to i32
    let a = 3;
    
    print_type_name(a);
}

/// Don't mind all the fancy in stuff here we'll cover it later, it just prints out a variable's type name
fn print_type_name<T>(value: T) {
    let name = std::any::type_name::<T>();
    println!("name of type: {}", name);
}
```


Some common variables types are bellow:

| Name    | Description                                             | Common use case                                                              |
|---------|---------------------------------------------------------|------------------------------------------------------------------------------|
| usize   | fast default system-dependant size for unsigned integer | Counting with numbers that are unlikely to be large                          | 
| isize   | fast default system-dependant size of unsigned integer  | Counting with negative and positive numbers that are unlikely to be large    |
| i32     | 32-bit signed integer                                   | Counting that may require a fixed size negative or positive value            |
| u32     | 32-bit unsigned integer                                 | Counting that may require a fixed size positive value                        |
| i64     | 64-bit signed integer                                   | Counting that may require a large negative or positive value                 |
| u64     | 64-bit unsigned integer                                 | Counting that may require a large positive value                             |
| f32     | A floating-point number with 32-bits of precision       | Approximate counting decimal numbers                                         |
| f64     | A floating-point number with 64-buts of precision       | Approximate counting decimal number but more accurate                        |
| &str    | pointer to a string of utf-8 characters                 | Using a static string like `"hello"` or passing around a pointer to a String |
| String  | Heap-allocated version of &str.                         | String manipulation, Lazy string passing-around                              |
| u8      | Unsigned byte                                           | Any low level bit manipulation                                               |

Keep in mind:
- Only **signed** variants of primitives can be less than 0. 
- Greater bits means bigger numbers can be stored. 
- Be aware that a u64 and i64 have different max sizes
- All ingers have a `MAX` and `MIN` size availible in [online docs](https://doc.rust-lang.org/std/primitive.i64.html#associatedconstant.MIN) and in your code
- There are many more types
- All the primitives can be used as a type suffix in code (eg: `let a = 32usize `)
- The default integer type is `i32`

### Mutability
By default, all rust variables are immutable, that means the compiler prevents you from changing them unless if you use [advanced dirty tricks](/dirty_tricks/change_non_mutable_variable.md)
```rust, compile_fail
fn main () {
    let a = 3;
    a = 3;
    println!("{}", a);
}
```
This prevents unexpected variable changes in your codebase. Everything that should change state is self-documenting and anything that shouldn't is compiler-enforced.

If you need to change a local variable, just add `mut` after `let` when you define it
```rust
fn main() {
    let mut a = 3;
    println!("{}", a);
    a = 4;
    println!("{}", a);
}
```

You can change variables passed into a method, but doing so does not act as you may expect. The following compiles but is **discouraged** and gives warnings
```rust
fn main() {
    let mut a = 2;
    println!("{}", a);
    change_my_number(a);
    println!("{}", a);
}

fn change_my_number(mut a: u32) {
    a = 3;
}
```
[run this example in rust playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=b9412c2085dca3613c2f66041c332c78) to see the warnings this fishy code is creating.
Instead, you will need to use rust's mutable [references](/references/references.md#mutable) (`&mut`)

### Shadowing
Rust has variable shadowing.
```rust
fn main(){
    let a = 3; // I am useless :(
    let b = 2;
    let a = 2; // Shadows the assignment on line 2
    
    let result = a + b;
    println!("{}", result);
}
```

It is extremely useful if you want to change something's type and value without having to worry about mutability of the original.
```rust
fn main() {
    let a = 4; // Why do you hate me :c
    
    let mut a = "four";
    a = "one greater than 3";
    println!("{}", a);
}
```
Combining it with more complex features like control flow, match statements and scope rules, it becomes indispensable

### Casting
Rust has strict 'safe casting' using the `as` keyword. 
It is only used for converting between primitives of similar type(eg: u32 <-> u64). 
Casting in rust is borderline useless outside of raw pointer manipulation.
```rust
fn main() {
    let letter = 'B';
    let letter_ascii = letter as u32;
    println!("{}", letter);
    println!("{}", letter_ascii);
}
```

```rust
fn main(){
    let number: u32 = 3;
    
    // f32 is a `float`. Primitives can be used as a suffix
    let number2 = 3f32;
    
    // f64 is a double
    let number3 = 4f64;
    
    // We cannot add 2 different types, so we need to make them the same before we try to add them
    let number2 = number2 as f64;
    
    let result = number3 + number2;
    println!("{}", result);
    
}
```

