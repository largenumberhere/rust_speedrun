# Dirty tricks - `unsafe`
The following sections contain code that is considered unsafe, that is, it leads to undefined behaviour if not used exactly as instructed.
The standard for using unsafe code in rust involves a plethora of inline documentation addressing the used feature's memory safety concerns. There is normally a safety section on unsafe item's documentation.  
You should only use unsafe code when there is no safe code available or as a learning exercise. 
Rust has many safe zero cost abstractions, it should be a rarity that these abstractions are not suitable for your use.

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
      
    /*
    Safety:
        - We are counting on this system's c libraries to correctly handle `srand` and `rand` calls with the correct types
        - Any errors created by srand and rand are expected to be handled by the kernel. From our perspective, they are expected to be infallible functions
        - These calls are not concurrent, async, multithreaded, etc, thus very low standards are required for these functions.   
    */
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
The following would be better achieved using box, however, some complex examples like implementing a non-standard ArrayList may require unsafe spaghetti code like this
```rust

fn main() {
    #[derive(Debug)]
    struct B {
        a: u128,
        b: usize,
        c: isize
    }
    
    // get the required memory to store a B
    let layout =  std::alloc::Layout::new::<B>();
    
    // allocating a size of 0 will cause undefined behaviour, here we're causing a crash if it's true to prevent UD
    assert_ne!(layout.size(), 0);
    
    // let's manually allocate room for the B struct
    
    let bee: &B; //declare a reference to a B
    unsafe{
        // allocate the space required on the heap and receive a mutable byte pointer
        /*
        Safety:
            - Layout has non-zero size as asserted above
            - The given memory is guaranteed to be initialized or null
        */
        let memory: *mut u8 = std::alloc::alloc_zeroed(layout);
        
        // Cause a crash in the suggested manner if no heap space is received 
        if memory == core::ptr::null_mut() {
            std::alloc::handle_alloc_error(layout);
        }
        
        // Cast the mutable byte pointer to a mutable B pointer. 
        /*
        Safety:
            - B is all integers which mean they all have a valid state of being all zeros
            - memory has the correct layout for B
        */
        let memory = memory as *mut B;
        
        // turn the mutable B pointer into a mutable B reference
        /*
        (See speaker notes at https://google.github.io/comprehensive-rust/unsafe/raw-pointers.html#speaker-notes for guidelines on dereferencing pointers in rust) 
        Safety:
            - We know memory is not null because we abort earlier if it happens to be
            - We know the pointer is at the start of an object with B's layout, therefore it is dereferencable.
            - We know memory has not yet been deallocated
            - This is the only place we are dereferencing memory, so there is no concurrent access
            - We know memory is valid, correctly aligned and writeable because alloc must return a region of memory with these properties
        */
        bee = &* memory; //set the bee reference to the memory we just allocated
    }
    
    // Let's see its contents!
    println!("bee: {:?}", bee);
    
    // We have to manually deallocate structs that do not implement the Drop trait. Else, we get memory leaks. We could have also have chosen to implement Drop for B
    unsafe {
        // Let's cast it to a mutable pointer to bytes. We need to do it in 3 steps
        let memory = bee as *const B; // first we cast bee to a pointer
        let memory = memory as *const u8; // next we cast that to a byte pointer
        let memory = memory as *mut u8; // next we cast that to a mutable byte pointer 
        
        /*
        Safety:
            - memory was allocated with rust's allocator and has not yet been dealloc'd
            - layout is the same as memory was allocated with
        */
        std::alloc::dealloc(memory, layout);
    }
    
    // 
}
```


### 