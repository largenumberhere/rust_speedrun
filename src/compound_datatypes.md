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

You can 