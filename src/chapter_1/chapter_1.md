# Getting started
**Waring: this book is incomplete**

### TL;DR
This is a terse book for one to learn some rust quickly. Skip to [install and test](#install-and-test) if you want to skip the boring bits.

### Quick into
The goal of this book is too teach you just enough rust to be dangerous. It will omit explanations or details wherever possible - many things will not be justified or explained in detail. Readers are encouraged to practice their research skills whenever anything is asserted that the reader wants a detailed explanation for. 
The official rust documentation is extremely thorough and well researched, this book does not intent to compete with the objectives of it.
This book will be very terse; you may need to consult official documentation or your local rustacean (rust friend) at times.
You will also find immense value in rust's compile error messages. They are world-class, read them!

### Try try try!
Please try out the given examples by clicking the green arrow that pops up in the code block when you hover over it.
Better yet, practice typing them out manually on your own computer and running them. 
It will give you give good muscle memory for writing rust. It will also make you run into some errors from typos or whatnot which you will learn to debug. 
99% of the time, the solution to your error will be in the console output - read it!

### Official documentation 
The rust language's documentation is all linked at [rust-lang.org](https://www.rust-lang.org/learn). It is outstanding. The following are some highlights:
- [Standard library documentation](https://doc.rust-lang.org/std/index.html)
- For practicing or learning the core of the language, [rustlings](https://github.com/rust-lang/rustlings/) is highly recommended
- For a detailed overview of the entire language, "[The book](https://doc.rust-lang.org/book/)" is indesposible
- For a slightly less verbose overview of the lanuage [rust by example](https://doc.rust-lang.org/rust-by-example/) will do thed trick

### IDE
The most popular rust integrated development environment at time of writing is vscode. If it is your preference, [setup vscode now](https://code.visualstudio.com/docs/languages/rust) .Others are available but are much more experimental.
This book will avoid ide-specific instructions wherever possible and instead rely on the commandline. You will need to be familiar with your platform's command line on windows that is `powershell` and most linux distributions use `bash`. 
Mac-os uses `zsh`. You may need to adjust some commands listed if your platform is not windows.

### Install and test
1. Rust's defacto toolchain is `Cargo`. You will need to [Install it](https://doc.rust-lang.org/cargo/getting-started/installation.html) before preceding.
2. Open up a new terminal and `cd` into a folder. Run `cargo new hello_world` to setup a cargo project.
3. `cd` into the folder `hello_world` which was just created
4. `cargo run` compiles, links and assembles the current project folder and then runs the resulting binary

You should now see hello world in your console. If you do not, read more relevant documentation. If you cannot fix it yourself, consult your local rustacean (rust person) for help!