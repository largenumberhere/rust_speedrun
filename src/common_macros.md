# Common macros
Rust has an extensive list of common macros. 
You can see how they work by installing `cargo expand` and using it on projects with macros. 
It will show what the source code looks like after the macros have done what they should.
### Common function-like macros:
- `println!()` you have used before. It performs several actions behind the scenes that are necessary to print just about anything in a thread-safe manner
- `print!()` - very similar to println, you can use this to append to the console before calling `println!()`. If you are a avoiding a newline, you will have to flush the stdout manually.
- `include_str!()` - read the source code of a file at compile time and place it into a &str.
- `env!()` - read an environment variable provided at compile time. See [Environment variables for crates](https://doc.rust-lang.org/cargo/reference/environment-variables.html#environment-variables-cargo-sets-for-crates)

### Common attribute-like macros:
Attribute-like macros are placed above something
- `[derive()]` - used for creating a default implementation of various traits aka interfaces. Common ones are `Debug`, `Clone`, `Copy`. Many more are added. These are for `structs` only
- `[cfg()]` - compile the following code in only certain situations. Some examples: `[cfg(target_os = "linux")]`, 
- `[test]` - for specifying a method to test in cargo's builtin testing framework
- `[allow()]` - for telling the compiler to ignore your strange looking code. Generally the compiler knows best, but sometimes it's useful to tell it back-off. 
- `[repr()]` - specify unusual memory layout configurations. `[repr(C)]` is useful for code interoperability.