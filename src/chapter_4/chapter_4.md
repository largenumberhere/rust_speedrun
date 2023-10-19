# Control flow (If, else,...)

Rust has several ways of controlling the flow of your program.
It has if statements like other languages.
```rust
fn main() {
    let a = 2;
    
    
    if a == 3 {
        println!("a is exactly 3");
    }
    // is checked only if `if`s or `else if`s before it are false    
    else if a == 4 {
        println!("a is exactly 4");
    }
    // is run only if all other `else if`s and the `if` at the start are false     
    else {
        println!("a is something else");
    }
}
```

### Loops
Rust has a few different loop types
1. Infinite `loop`:
    ```rust, noplayground, no_run
    fn main() {
        // this loop lasts for ever unless if you `break` out of it. 
        // This snippet will timeout if you try to run it in rustplayground
        // You can try running it on your computer though
        loop {
            println!("I will never end...");    
        }
    }
    ```
2. `while` loop:
   ```rust
   fn main() {
      let mut a = 0;
      while a < 10 {
         println!("Here we go again...");
         a+=1;
      }
   
   }
   ```
3. `for` loop:
   ```rust
   fn main() {
     for i in 0..10 {
        println!("here we go again... but easier!");
     }
   }
   ```
   Rust for loops can operate on anything that implements the Iterator interface.
   ```rust
   fn main() {
     // An array of numbers
     let numbers = [1,1,1,4];
     
     // numbers.into_iter() is implicitly called. 
     for number in numbers {
        println!("{}", number);
     }
   }
   ```
   
   Some variables are not trivially copyable and if you want to use them after a for loop is run over them, you need to use a different iterator. `iter()`, gives a reference to each item
   ```rust
   fn main() {
        // Dynamic array of words
        let words = vec!["hi"];     
   
        for word in words.iter() {
            println!("{}", word);
        }
        
        println!("{:?}", words);
        let new_words = words;     
   
   }
   ```
Rust also allows you to `break` out of a loop or `continue` to the next iteration.

### Switch / Match
Rust has a very powerful match statement. It forces you to always handle every possible case or set a default case to prevent bugs.
It is similar to a switch statement but with much stricter rules.
```rust
fn main() {
    let a = "hello";
    match a {
       "hell" => { 
          println!("a is hell"); 
       },
       
       "hello" => {
          println!("a is a friendly greeting");
       },
       
       // The match statement exits after the first match, all others are ignored. There is also no fallthrough
       "hello" => {
          println!("a is a friendly greeting but for the 2nd time");
       }
       
       "hellop" => {
          println!("a is gibbersh");
       },
       
       // `_` in this case means anything else 
       _ => {
          println!("a was something else");
       }
    };
   
}
```

The `{` and  `}` are optional in match expressions. Without them, you must use commas and only a single expression. You must omit the semicolon at the end of the expression.
Let's redo the last example with the things we have learnt
```rust
fn main(){
   let a = "hello";
   match a {
      "hell" => println!("a is hell"),
      "hello" => println!("a is a friendly greeting"),
      "hellop" => println!("a is gibbersh"),
      // the comma is optional on the last match statement
      _ => println!("a was something else")
   };
}
```

Another great thing you can do with match expressions is return their value. This can be done by any `expression` in rust, including all the other loops that have been mentioned.
```rust
fn main() {
   let a = "hello";

   let a_description = match a {
      "hell" => "a is hell",
      "hello" => "a is a friendly greeting",
      "hellop" => "a is gibbersh",
      _ => "a was something else"
   };

   println!("{}", a_description);
}
```
Match expressions support destructuring which proves extremely useful when combined with other features of the language
