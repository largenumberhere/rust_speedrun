### Change a non-mutable variable
Warning: this example is considered "memory unsafe", it is also not thread safe and may not even work correctly on your computer.
The following details a gross perversion of the language used to mutate a immutable variable
```rust
    /// An example of a bad approach
    fn main() {
        // get a variable
        let a: u32 = 10;
        
        // get a pointer to it
        let a_ptr = &a as *const u32;
        
        // cast it to a mutable pointer
        let a_ptr = a_ptr as *mut u32;
        
        // an unsafe block acknowledges we are doing things that may go terribly wrong if we do not take care to manually audit out code for violating the safety section of its documentation
        unsafe {
            // The rust convention for unsafe code is to preface all unsafe methods with a paragraph going over the documented function's safety section
            /* 
                - We hope a_ptr is valid for writes, if it is not, we accept the undefined behaviour may occur - this example is not intended for safe production use. 
                   If particular optimizations are made that assume immutability, this block of code will lead to UD
                - We know a_ptr points to properly aligned data, because a stack's variables must always be correctly aligned for the os to run it
                - We know our pointer is not null because it is a stack variable that is still in scope
            */    
            core::ptr::write(a_ptr, 2);
        }
    
    
        println!("{}", a);
    
    }
```
### Builtins
There are much more intelligently constructed abstractions in rust's standard library like 
[UnsafeCell](https://doc.rust-lang.org/stable/std/cell/struct.UnsafeCell.html#memory-layout) 
and [RefCell](https://doc.rust-lang.org/stable/std/cell/struct.RefCell.html).
They may be useful in the rare case that mutability of a non-mutable variable is
genuinely required.
```rust
fn main() {
    let cell = std::cell::RefCell::new(10);
    *cell.borrow_mut() = 2;
    println!("{}", cell.borrow());
}
```