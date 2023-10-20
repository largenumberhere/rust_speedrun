# Errors

Error handling in rust has 2 categories
- Recoverable errors

    For when an error should be handled by the library user. A common example is file io.
- Unrecoverable errors
  
    For when there's nothing left to do but `panic!`. An example is an out of memory error which a rust program has no reasonable course of action to resolve

### Recoverable errors
Recoverable errors are part of a function's return type.
Rust has 2 wrapper types for recoverable errors, `Option<T>`
- ### `Option<T>`
  This is how it's defined:
  ```rust
    pub enum Option<T> {
        None,
        Some(T),
    }
    ```
  Let's use an `Option<&i32>`
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
  
  We have created very similar behaviour to how `[]` works on Vec
  ```rust
  fn main(){
      let numbers = vec![1,2,3];
      
      let value = numbers[1];
      println!("value is {}", value);
      
      numbers[5];
  }
  ```
  
  Let's return an `Option<i32>`
  ```rust
  fn checked_doubling(number: i32) -> Option<i32> {
    return i32::checked_mul(number, 2);
  }
  
  fn main() {
    let num = 1000;
    let new_num = checked_doubling(num);
    match new_num {
        Some(v) => {
            println!("doubled number to {}", v);
        }
        Nome => {
            println!("failed to double numner {}", num);
        }
    }
  }
  ```
  
  Let's construct an `Option<i32>` ourselves
  ```rust
  /// generally, you wouldn't do this. Instead use a u32 if negative numbers should not be possible
  /// This function takes in a number and tries to convert it to cents, if it is negative, it returns None, because we consider than an invalid value
  fn to_cents(dollars: i32) -> Option<i32> {
    if dollars < 0 {
        return Option::None;
    }
  
    else {
        let value = dollars * 100;
        return Option::Some(value);
    }
  }  
  
  fn main(){
    println!("{:?}", to_cents(100));  
    println!("{:?}", to_cents(-2));
  }
  ```


- ### `Result<T,E>`
  It is defined as
  ```rust
  enum Result<T, E> {
     Ok(T),
     Err(E),
  }
  ```
  It is like option, except an error has information.
  You can also use `.unwrap()` and `.expect("")` to convert it to an unrecoverable error
  
  Let's handle a `Result`. In this case, we'll just print the error
  ```rust
  fn main() {
    let text: Result<String, std::io::Error> = std::fs::read_to_string("file.txt");
    match text {
        Ok(v) => {
            println!("read '{}' from file.txt", v);
        }
        Err(e) => {
            println!("Error reading from file.txt:\n {:?}",e);
        }
    }
  
  }
  ```
  
  We could also write it like this
    ```rust
  fn main() {
    // Rust can infer a single type if we use _
    let text: Result<String, _> = std::fs::read_to_string("file.txt");
    match text {
        Ok(v) => {
            println!("read '{}' from file.txt", v);
        }
        Err(e) => {
            println!("Error reading from file.txt:\n {:?}",e);
        }
    }
  
  }
  ```
  
  Or like this
  ```rust
  fn main() {
  // Rust can infer the entire return type
  let text = std::fs::read_to_string("file.txt");
  match text {
    Ok(v) => {
        println!("read '{}' from file.txt", v);
    }
    Err(e) => {
        println!("Error reading from file.txt:\n {:?}",e);
    }
  }

  }
  ```
  
  Or like this:
  ```rust
  fn main() {
  // Rust can infer the entire return type
  let text: Result<_,_> = std::fs::read_to_string("file.txt");
  match text {
    Ok(v) => {
        println!("read '{}' from file.txt", v);
    }
    Err(e) => {
        println!("Error reading from file.txt:\n {:?}",e);
    }
  }

  }
  ```
  Etc.. Rust's type inference can be very useful in working with error types

  Let's make an error
  ```rust
  #[derive(Debug)] // make it printable with no effort
  struct Number2MustNotBeZero;
  
  fn divide(a: i32, b: i32) -> Result<i32, Number2MustNotBeZero> {
    if b == 0 {
      return Err(Number2MustNotBeZero)
    }
  
    return Ok(a / b);
  }
  
  
  fn main() {
      let a = 10;
      let b = 0;
  
      let result = divide(a,b);
      match result {
          Ok(v) => {
              println!("result was {}", v)
          }
  
          Err(e) => {
              println!("division failed becase '{:?}'", e);
          }
      }
  }
  ```
  
  You can use an enum for errors
  ```rust
  #[derive(Debug)]
  enum StringDivisionError {
    TooShort,
    NotANumber
  }
  
  fn divide_string(number1: &str, number2: i32) -> Result<i32, StringDivisionError> {
         
  
  
  }

  ```



### Conversion between `Result<T,E>` and `Option<T>` 

### Error Propagation


### Unrecoverable errors