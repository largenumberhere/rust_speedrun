# Common macros
Rust has an extensive list of common macros. 
You can see how they work by installing `cargo expand` and using it on projects with macros. 
It will show what the source code looks like after the macros have done what they should.
### Common function-like macros:
- `println!("Hello world")` you have used before. It performs several actions behind the scenes that are necessary to print just about anything in a thread-safe manner. 
    If more than one argument is passed, it uses the same syntax as [format!](https://doc.rust-lang.org/std/fmt/) eg: `println!("{}", "hello!");`.

- `print!()` - very similar to println, you can use this to append to the console before calling `println!()`. If you are a avoiding a newline, you will have to flush the stdout manually.
```rust
    use std::io::Write; // brings flush method into scope
    fn main() {
        print!("hello"); //write some text
        print!(" world!"); //write more text
        let stdout = std::io::stdout().lock().flush(); //flush stdout so our text is visible in the console immediately
    }
```

- `include_str!("my_file.txt")` - read the contents of a file inside `src` at compile time and place it into a &str.

- `env!("CARGO_PKG_VERSION")` - read an environment variable (such as  `CARGO_PKG_VERSION`) at compile time. See [Environment variables for crates](https://doc.rust-lang.org/cargo/reference/environment-variables.html#environment-variables-cargo-sets-for-crates) for useful options


### Common attribute-like macros:
Attribute-like macros are placed above something
- `#[derive()]` - used for creating a default implementation of various traits aka interfaces. Examples: `#[derive(Debug)]`, `#[derive(Clone)]`, `#[derive(Copy)]`. Many more are added. These are for `structs` only. This is valid code : `#[derive(Clone, Debug, Copy)]`
- `#[cfg()]` - compile the following code in only certain situations. Some examples: `[cfg(target_os = "linux")]`. You can place them above just about anything 
- `#[test]` - for specifying a method to test in cargo's builtin testing framework. Placed above functions 
- `#[allow()]` - for telling the compiler to ignore your strange looking code. Generally the compiler knows best, but sometimes it's useful to tell it back-off. Placed just about anywhere.  
- `#[repr()]` - specify unusual memory layout configurations. `[repr(C)]` is useful for C ABI interoperability. Placed above structs