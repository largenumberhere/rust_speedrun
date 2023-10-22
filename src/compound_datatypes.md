### Structs
Rust has no classes, only structs. Everything that isn't a primitive is a struct!
```rust
// Define a struct
#[derive(Debug)]// Add this above the struct definition to make it easily printable with `{:?}`
struct MyStruct {
    age: u32,
    name: String
}

fn main() {
    // Make an instance of the struct 
    let my_struct = MyStruct {
        age: 20,
        name: "peter".to_string()
    };
    
    // print the struct
    println!("{:?}", my_struct);
    
}
```

You can attach functions to a type
```rust
#[derive(Debug)]
struct Person {
    name: String,
    age: u32
}

impl Person {
    fn what_am_i() {
        println!("I am a person!");
    }
}

fn main() {
    Person::what_am_i();
}
```

You can attach methods to a struct instance with no runtime penalty.
These always take a `&self`, `self` or similar, and you use these prefixes to refer to the structs instance.
We are going to use `self` for now to avoid the topic [references](../references/references.md)
```rust
// Make our struct printable with `Debug` and easy to make a copy of with `Clone`
#[derive(Debug, Clone)]
struct Person {
    name: String,
    age: u32
}

impl Person {
    fn identiy(self) {
        println!("My name is {}", self.name);
        println!("I am {} years old", self.age);
    }
}

fn main() {
    let person = Person{
        age: 20,
        name: "Jason".to_string()
    };
    
    // make a copy of person
    let person2 = person.clone();
    
    // the easiest way to call it
    person.identiy();

    // sometimes you will need to call it like this
    Person::identiy(person2);

}
```

### Enums
Rust has fantastic enums. They are implemented as tagged unions.
```rust
#[derive(Debug)] //make it easily printable
enum Choice {
    Walk,
    Sit
}

fn main() {
    let a = Choice::Walk;
    println!("{:?}", a);
}
```

You can attach data to any enum variants
```rust
#[derive(Debug)]
enum Choice {
    Sit,
    Walk(u32)
}

fn main() {
    let choice = Choice::Walk(10);
    println!("{:?}", choice);
}
```

They are especially convenient in match statements
```rust
enum Choice {
    Sit,
    Walk(u32)
}

fn main() {
    let choice = Choice::Walk(32);
    match choice {
        Choice::Sit => {
            println!("You just sat around");
        }
        Choice::Walk(metres) => {
            println!("You walked for {} metres!", metres);
        }
        
    }
}
```

A common enum is `Option<T>`. The T just means it can hold anything. 
It is defined like so in the `core` and standard library
```
pub enum Option<T> {
    None,
    Some(T),
}
```

```rust
fn main() {
    let number:u32 = u32::MAX; //set a u32 number to the largest it can be. 
    // If it goes any higher, it will roll-around back to zero, unless if we state otherwise 
    
    // the method u32::checked_add returns an Option<u32>. 
    let number_result: Option<u32> = number.checked_add(4);
    
    // Let's see what happened
    match number_result {
        
        // Some() is returned on success
        Some(number) => {
            println!("The addition succeeded and returned {}!", number);        
        }
        
        // None is returned on failure
        None => {
            println!("The addition failed");
        }
    }
    
}
```
Option<T> is very common across all rust libraries
See the [errors](errors.md) chapter for more details on Option<T> and error handling
### Tuple
```rust
fn main() {
    // Rust tuples have the type (type_name1, type_name2, type_name3...)
    let tuple: (u32, i32);
    
    // Rust tuples are populated like so
    tuple = (1,2);
    
    // Rust tuples implement Debug, which means you can print them with 0 effort
    println!("{:?}", tuple);
}
```

### Common standard library structures
- std::vec::Vec
    An array-list aka dynamic array.
    ```rust
    fn main() {
        let mut my_vec = std::vec::Vec::new();
        // you can insert items to a `mut`able vec with push
        my_vec.push(1);
        my_vec.push(2);       
        my_vec.push(3);
        
        // as with most of the standard library, vec implements debug, so you may print it with the format specifier `"{:?}"` 
        println!("{:?}", my_vec);
        
        // you can remove items with pop
        let last =  my_vec.pop();
        println!("last item in vec was: {:?}", last);
        
        // vec can be easily used as an iterator
        for item in  my_vec.iter() {
            println!("my_vec contains '{}'", item);
        }
        
        // vec has a len method
        let item_count = my_vec.len();
        println!("my_vec has {} items", item_count);
    }
  ```
    Because vec implements [Iterator](https://doc.rust-lang.org/std/iter/trait.Iterator.html) it has access to every single one of its interface \ 'trait' methods. Please take a look! 
    See [rust vec documentation](https://doc.rust-lang.org/std/vec/struct.Vec.html) for a peek at it all!

- std::String

- std::Args

- std::io::Error

