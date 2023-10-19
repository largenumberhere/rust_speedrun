# Basic syntax
Here is a list of basic syntax we'll speed through to construct our own hello world program from scratch.

[//]: # (You will need to read to the end of [functions]&#40;#functions&#41; before writing the first line)

[//]: # (Every statement in rust ends with a semicolon.)

### Rust program skeleton
The most basic rust program contains a single file called `main.rs`, inside of it will be the following:
```rust
fn main() {
    
}
```
It will exit with no errors or output.

### Comments & Inline docs
Basic rust comments consume the rest of a line and start with `//`. Code comments do nothing too. Here's some examples:
- ```rust
  fn main() {
    // The follwing lines are a codesmell
  }
  ```

- ```rust
  fn main() {
    let age = 69; // This is very funny 
  }
  ```

- ```rust
    fn main(){
        // TODO: This is hardcoded for now. We'll fix it later!
        let name = "peter";
    }
    ```
Inline docs are similar to comments. They are supported by the documentation tool `cargo doc`. The *must* be directly above a top-level item. They can be several lines
```rust
/// On the next line, there is a main method!
/// It does nothing
fn main() {
    
}
```

Multiline comments exist but uncommon and not supported by `cargo doc`
```rust
/*
    This program is a waste of time
*/
fn main(){
    
    
}
```

Comments will be included in all following code

### Functions
The following is a function. When called, this one takes in nothing, does nothing and outputs nothing 
```rust
fn do_nothing() {
  // nothing is done here
}
```

It could also be written as 
```rust
fn do_nothing() -> () {
    return (); // Return statement is optional if it returns no real value
}
```
`()` is called the `unit type`. It is functionally equivalent to `void` or None return type of other languages.
<br/><br/>

The following takes 2 numbers and adds them. The type of these numbers is an *u*nsigned *32*-bit integer. It one of the most common integers used
```rust
/// The following takes 2 numbers and adds them. everything between `{` and `}` is the function body. 
/// number1 is of type u32 which is the most common integer type
/// the output is specified after `->`
fn add(number1: u32, number2: u32) -> u32 {
    return number1 + number2;
}
```

It could also be written like this, broken down into more discrete steps
```rust
fn add(number: u32, number2: u32) -> u32 {
    let result = number + number2;
    return result;
}
```

It could also be written like this. 
```rust
/// If a semicolon is not added, the value is implicitly returned
fn add(number: u32, number2: u32) ->u32 {
    number + number2
}
```
Some macros look like functions. All you need to know for now is to not mix them up. `println!()` is a macro. It must always be called with an exclamation mark.

### Hello world (put it all together)
Create a new cargo project and delete everything inside of `src/main.rs`
Type out the following:
```rust
/// hello world from scratch in rust!
fn main() {
    println!("Hello world!");
}
```

Modify it to the following to print out a variable:
```rust
/// hello word again
fn main(){
    let message = "hello world!";
    println!("{}", message);
}
```
