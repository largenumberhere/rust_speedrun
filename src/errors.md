# Errors

Error handling in rust has 2 categories
- Recoverable errors

    For when an error should be handled by the library user. A common example is file io.
- Unrecoverable errors
  
    For when there's nothing left to do but `panic!`. An example is an out of memory error which a rust program has no reasonable course of action to resolve

### Recoverable errors
Recoverable errors are part of a function's return type. 
```rust
fn main() {
  // Make an dynamic array of numbers
  let numbers: Vec<i32> = vec![1,2,3];
  
  // Vec.get, tries to return a thing at the index given. The & is not important in this example
  let number1: Option<&i32> = numbers.get(1);
  match number1 {
        Some(v) => {
            println!("The number at position 1 is {}", v);
        }
        None => {
            println!("There is no number at position 1 :(");
        }
  }
  
  let number1: Option<&i32> = numbers.get(10);
  match number1 {
    Some(v) => {
      println!("The number at position 10 is {}", v);
    }
    None => {
      println!("There is no number at position 10 :(");
    }
  }
}
```

You can turn a Option or Result into an unrecoverable error
```rust
fn main() {
    // Make an dynamic array of numbers
    let numbers: Vec<i32> = vec![1, 2, 3];
    
    // This happens to be okay
    let value = numbers.get(1).unwrap();
    println!("value is {}", value);
  
    // This causes a runtime crash
    numbers.get(5).unwrap();
}
```

Let's make the error message clearer
```rust
fn main() {
    // Make an dynamic array of numbers
    let numbers: Vec<i32> = vec![1, 2, 3];

    // This happens to be okay
    let value = numbers.get(1).expect("index out of bounds");
    println!("value is {}", value);

    // This causes a runtime crash
    numbers.get(5).expect("index out of bounds");
}
```

We have created very similar behaviour to `[]`
```rust
fn main(){
    let numbers = vec![1,2,3];
    
    let value = numbers[1];
    println!("value is {}", value);
    
    numbers[5];
}
```


### Unrecoverable errors