# _A Look at Rust: An Empowering Language_

- _Jonathan Perry_
- _jonathan.perry@uoit.net_

## About the language

### History 
> - First appeared in mid 2010
> - Latest stable release as of writing: November 8, 2018
> - Most loved programming language of 2016, 2017, and 2018 according to Stack Overflow
### Some Interesting Features
> - Type inference
> - Uses the idea of Ownership rather than a Garbage Collector or explicit programmer allocation/dellocation for memory management
> - Takes a different approach to Strings; uses UTF-8 encoding for its Strings (defined in the std library)
> - No explicit type for Null/None. Instead, must be wrapped in an ```Option<T>```
>   ```rust
>       enum Option<T> {
>         Some(T),
>         None,
>      }
>    ```
>   - Forces the programmer to handle  any time there is an ```Option<T>``` which may have a None value
> - When an error could occur in a, wraps the return value in an ```Result<T,E>```
>   - Functions return ```Result``` when errors are expected and can be recovered from
>   - ```Result``` is most prominently used for I/O.   
> - Keyword ```match``` which is a control flow operator with pattern matching, especially useful with ```enum```
> - Automatic referencing and dereferencing
> - Does not have a ```class``` keyword

## About the syntax

*Hello, World!*
```rust
fn main() {
  println!("Hello, world!");
}
```
*Function Declaration*
```rust
fn say_hello(name: &String) { // by reference
  println!("Hello, {}!", name);
}
```
*Let form*

```rust
let x = 5;
let mut y = 5;
x = 10; // invalid, will give a compiler error
y = 10; // valid
```

*Primitive Types*
```rust
let s: &str = "sdfsdf"; // string slice
let t: bool = true; // bool
let u = (s, t); // tuple of a specific length
let v: (i8, i16, i32, i64, i128) = (-1, 2, -3, 4, 5); // signed integers
let w: (u8, u16, u32, u64, u128) = (1, 2 ,3, 4, 5); // unsigned integers
let x: (f32, f64) = (33.5, 22.5); // floating point
let y: char = 'z'; // char
let z: [u8; 3] = [0, 1, 2]; // array of specific type and size
// There are also fn pointers, pointer, reference, unit, and usize
```

*Structs & Methods*
```rust
struct Rectangle {
  width: i32,
  height: i32,
}

impl Rectangle {
  fn is_square(&self) -> bool {
    self.width == self.height
  }
  
  fn print_about() {
      println!("Rectangles have four sides. Have the dimensions width and height");
  }
}

fn main() {
    Rectangle::print_about(); // similar to a static method for a class
    let x = Rectangle {
        width: 5,
        height: 5,
    };
    println!("Is x a square? {}", x.is_square()); // Is x a square? true
}
```

*```enum``` & ```match``` Example*
```rust
enum Day {
  Sunday,
  Monday,
  Tuesday,
  Wednesday,
  Thursday,
  Friday,
  Saturday,
}

fn how_to_feel_about_today(day: Day) {
  match day {
    Day::Sunday => println!("Relax all day"),
    Day::Monday => println!("Mondays... dread it."),
    Day::Tuesday => println!("At least it's not Monday!"),
    Day::Wednesday => println!("Hump day as they say"),
    Day::Thursday => println!("Almost there..."),
    Day::Friday => println!("We made it. Finally!"),
    Day::Saturday => println!("Pure bliss"),
  }
}

fn main() {
    let x = Day::Saturday;
    how_to_feel_about_today(x) // Pure bliss
}

```
*Closures/Anonymous Functions*
```rust
fn using_closure_example() -> impl Fn(u8) -> u8 {
    let x = 5;
    move |y| x+y // anonymous function which takes ownership of x
}
fn main() {
    let x = 100;
    let y = 5;
    println!("x+y = {}", x+y); // x+y = 105
    println!("Using closure: x+y = {}", using_closure_example()(y)); // Using closure: x+y = 10
}
```

## About the tools
> - ```$ rustc <filename>.rs``` to create an executable
> - ```./<filename>``` to run the file
> - Downloading Rust includes their build system and package manager Cargo
> - ```$ cargo new hello_world``` creates a new project directory of hello_world
>   - Contains src folder with main.rs in it
>   - Contains a *Cargo.toml* file (the manifest file to specify name, version, dependencies, etc.)
>   - Initializes the project as a git repo
> - ```$ cargo build``` builds the project 
> - ```$ cargo run``` builds the project (if needed) and then runs it
> - ```$ cargo check``` checks if the code compiles, but doesn't create an executable
> - ```VS CODE```, ```SUBLIME TEXT 3```, ```ATOM```,```INTELLIJ IDEA```,```ECLIPSE```,```VIM```, and ```EMACS``` all have the functionality to help develop in Rust

## About the standard library

> - Quite comprehensive in what it provides
>   - Provides all common data structures/collections found in other popular programming languages
>     - These also have their typical methods associated with them
>   - Rust's hashmap implementation (using ```SipHash``` algorithm) favours reducing attacks (DoS attacks) which produce collisions. Can result in slower performance in favour of security
>     - Programmers can specify if a different hashing algorithm if they want to imrpove speed at the cost of being more vulnerable
> - Contains ```std::prelude``` which is automatically imported to every rust program and includes:
>   - ```std::marker::{Copy, Send, Sized, Sync}```
>   - ```std::ops::{Drop, Fn, FnMut, FnOnce}```
>   - ```std::mem::drop```
>   - ```std::boxed::Box```
>   - ```std::borrow::ToOwned```
>   - ```std::clone::Clone```
>   - ```std::cmp::{PartialEq, PartialOrd, Eq, Ord }```
>   - ```std::convert::{AsRef, AsMut, Into, From}```
>   - ```std::default::Default```
>   - ```std::iter::{Iterator, Extend, IntoIterator, DoubleEndedIterator, ExactSizeIterator}```
>   - ```std::option::Option::{self, Some, None}```
>   - ```std::result::Result::{self, Ok, Err}```
>   - ```std::slice::SliceConcatExt```
>   - ```std::string::{String, ToString}```
>   - ```std::vec::Vec```
>     ```rust
>     let v: Vec<i32> = Vec::new();
>     let v = vec![1, 2, 3, 4, 5]; // std macro to build to initalize a vector and append things to it
>     let does_not_exist = &v[100]; // causes the program to panic and crash
>     let does_not_exist = v.get(100); // does not cause panic, 
>     // but when the programmer goes to use the value, they will be forced to handle the None option, 
>     // which it will be in this case
>
>     // a way for a vector to hold more than one kind of type
>     enum SpreadsheetCell {
>         Int(i32),
>         Float(f64),
>         Text(String),
>      }
>     
>     let row = vec![
>         SpreadsheetCell::Int(3),
>         SpreadsheetCell::Text(String::from("blue")),
>         SpreadsheetCell::Float(10.12),
>     ];
>     ```
>     
> - Multiple different macros inlcuding such as:
>   - ```assert```, ```assert_eq```, and ```assert_ne``` which are useful for writing unit tests
>   - ```println``` for printing to the standard output, with a newline of course
>   - ```include_str``` reads a UTF-8 enconded file and turns it into a &'static str


## About open source library

> _Describe at least one contribution by the open source
community written in the language._
> - Crates (Rust's librarys) can be easily found at https://crates.io
>   - To get a random numbers, there is the Rand crate which can be used (https://crates.io/crates/rand)
>   - A multitude of other crates which can be easily used, thanks to Rust's project manager Cargo
> - Servo is one of the biggest open-source projects built in Rust (itself being open-source)
>   - Sponsored by Mozilla
>   - A high performance browser for both application and embedded use
>   - Goals include better:
>     - Parallelism
>     - Security
>     - Modularity
>     - Performance

# Analysis of the language

> _Organize your report according to the project description
document_.
> - Rust supports both functional and procedural programming, althought it is definitley not a fully functional language
>   - By default, variables are immutable
>   - Most things in Rust are expressions, not statements
>   - Scopes are easily created with  ```{``` to start a scope and ```}``` to close it
>     ```rust
>     fn main() {
>       let x = "Hello";
>       println!("{}", x); // Hello
>       
>       {
>           let x = "Goodbye";
>           println!("{}", x); // Goodbye
>       }
>       
>       println!("{}", x); // Hello
>     }
>     ```
> - The standard library offers many pre-built macros
> - Users can also define their own macros
>    - A basic implementation of the vec! macro (without preallocating the correct amount of memory):
>     ```rust
>     #[macro_export]
>     macro_rules! vec {
>         ( $( $x:expr ),* ) => {
>             {
>                 let mut temp_vec = Vec::new();
>                 $(
>                     temp_vec.push($x);
>                 )*
>                 temp_vec
>             }
>         };
>     }
>     ```
> - Rust uses lexical scoping, which can be seen in many of the examples thus far
>   - Scoping is quite closely linked with determining when Ownership transfers, when a value is borrowed, or when the lifetime of a variable ends
>   - Rust enforces RAII (Resource Acquisition Is Initialization)
>   ```rust
>   // raii.rs
>   fn create_box() {
>       // Allocate an integer on the heap
>       let _box1 = Box::new(3i32);
>       // `_box1` is destroyed here, and memory gets freed
>   }
>
>   fn main() {
>       // Allocate an integer on the heap
>       let _box2 = Box::new(5i32);
>
>       // A nested scope:
>       {
>           // Allocate an integer on the heap
>           let _box3 = Box::new(4i32);
>
>           // `_box3` is destroyed here, and memory gets freed
>       }
>
>       // Creating lots of boxes just for fun
>       // There's no need to manually free memory!
>       for _ in 0u32..1_000 {
>           create_box();
>       }
>
>       // `_box2` is destroyed here, and memory gets freed
>   }
>   ```
> - Rust is a statically typed language which supports type inferrence 
> - Rust's compiler is quite strict - with good reason
> - Pros:
>   - Performance is quite comparable to C and C++
>   - Compiler catches/handles the problematic things that C/C++ allow such as dangly pointers, memory leaks
>     - Most bugs would likey arise from an error in business logic or the implementation of that business logic
>   - Security/safety issues are also caught/handled by compiler by default
>   - Great concurrency thanks to ownership and type checking
>     - Compiler will give an error showing where you will run into a problem when using concurrency
>   - Can use the ```unsafe``` keyword to write 'unsafe' code if needed (useful if writing an OS for example)
>   - Strong support for generics
> - Cons:
>   - Complex -- Steep learning curve
>   - Not widely adopted (_yet_)
>   - Not object-oriented (but can implement functionaility)