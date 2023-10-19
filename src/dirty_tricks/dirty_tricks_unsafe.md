# Dirty tricks - `unsafe`
The following sections contain code that is considered unsafe, that is, it leads to undefined behaviour if not used exactly as instructed.
The standard for using unsafe code in rust involves a plethora of inline documentation. 
You should only use unsafe code when there is no safe code available or as a learning exercise. 
Rust has many safe zero cost abstractions, it should be a rarity that they are not suitable for everyday use.

### Calling external functions
A use-case for unsafe code is for calling external C libraries. Since foreign languages have no safety guarantees, they must be called in unsafe blocks or functions
  ```rust
  // libc is a crate that acts as a shim for the C standard library functions, 
  // so they may be called from rust correctly
  use  libc; // 0.2.147
  fn main() {
      
      // get time in millis
      let time_now = std::time::SystemTime::now();
      let time_since = time_now.duration_since(std::time::UNIX_EPOCH).unwrap();
      let time_ms = time_since.as_millis();
      
      // cast to u32 so it matches the size of the C `int` in the function's signature
      let time_ms = time_ms as u32;
      
      let mut number = 0;
      unsafe{
          // seed random number generator with time as millis
          libc::srand(time_ms);
          
          // request a random number
          number = libc::rand();
      }
      
      println!("random number generated with this system's native C library: {}", number);
  }
  ```
### Raw memory managment
Unsafe may also be used for low-level memory management in rust. 
The following would be better achieved using box, however, some complex examples like implementing a non-standard ArrayList may require unsafe code
```rust
use std::alloc::alloc;
use std::alloc::Layout;
fn main() {
    struct B {
        a: u128,
        b: usize,
        c: isize
    }
    
    // get the required memory to store a B
    let layout =  Layout::new::<B>();
    
    unsafe{
        /// allocate the space required on the heap and receive a mutable byte pointer
        /*
        
        */
        let memory: *mut u8 = alloc_zerod(layout);
        
        // cast the mutable byte pointer to a mutable B pointer
        let memory = memory as *mut B;
        
        // turn the mutable B pointer into a mutable B reference
        /*
            
        */
        let mut memory = &* memory;
    }
    
    // 
}
```


### 