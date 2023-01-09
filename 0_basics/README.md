Step 0: Become familiar with Rust basics
========================================

__Estimated time__: 3 days

Read through [Rust Book], [Rust FAQ], and become familiar with basic [Rust] concepts, syntax, memory model, type and module systems.

Polish your familiarity by completing [Rust By Example].

Read through [Cargo Book] and become familiar with [Cargo] and its workspaces.

After completing these steps, you should be able to answer (and understand why) the following questions:
- What memory model [Rust] has? Is it single-threaded or multiple-threaded? Is it synchronous or asynchronous?

Rust's memory model mostly copied from C++, doesn't match any existing processor architecture, but instead is an abstract model with strict set of rules that attempt to represent the greatest common denominator of all current and future acrchitectures, while also giving the compiler enough freedom to make useful assumptions while analyzing and optimizing programs. Rust's memory model allows for concurrent atomic stores, but considers concurrent non-atomic stores to the same variable to be a data race, resulting in undefined behavior. On most processor architectures there's no difference between an atomic store and a regular non-atomic store. Strict rules in rust make easier to reason about a program for both compiler and the programmer. The Rust language has been designed to make it easy to create safe multithreaded applications. It's designed with multiple containers such as Arc to make it easy to pass data between threads. The problem with multithreading is that spawning a thread means allocating significant CPU, memory, and OS resources, or what is colloquially known as being expensive. The solution is to use a different technique called asynchronous programming, where a single thread is reused by different tasks without waiting for the first task to finish. People can easily write an async program in Rust because it's been incorporated into the language since November 7, 2019.


- What runtime [Rust] has? Does it use a GC (garbage collector)?

It doesn't have garbage collector, an interpreter, or a build-in user-level scheduler. Though, rust does have some special code that runs before main function and in response to certain special conditions in code. Rust still needs to know what to do when a panic occurs. lang_start performs the setup for the Rust runtime, including stashing the program's command-line arguments, setting the name of the main thread, handling panics in the main function. 

- What statically typing means? What is a benefit of using it? Weak typing vs strong typing? Implicit / explicit?

Statically-typed language checks the data type at compile time. The benefits are next, a programmer has to write less unit tests because they don't have to compensate for not checking the type at compile time. Statically typed language also considered less expensive because every time a function is called there's no need to check passed parameters. It is easier to optimise statically typed language. It is very hard to make mistakes such as passing a string as a number. 
Generally, a strongly typed language has stricter typing rules at compile time, which implies that errors and exceptions are more likely to happen during compilation. Most of these rules affect variable assignment, function return values, procedure arguments and function calling. Dynamically typed languages (where type checking happens at run time) can also be strongly typed. A weakly typed language has looser typing rules and may produce unpredictable or even erroneous results or may perform implicit type conversion at runtime. Implicit type means that the type is automatically declared for a variable. So instead of annotating types explicitly.

- What are generics and parametric polymorphism? Which problems do they solve?

parametric is a universal polymorphism. It is also called "true"  polymorphism because it lets you write true generic code that works for any types. Sometimes, it is also refered to as generics. In parametric polymorphism, a piece of code is writtern in such a way that it works on any type. Parametric polymorphism is achieved by using a type variable when writing the code, rather than using any specific type. It provides generic and reusable and maintainable code. 

- What are traits? How are they used? How do they compare to interfaces? What are an auto trait and a blanket impl? Uncovered type? What is a marker trait?

Uncovered type? What is a marker trait?
Trairs are Rust's take on interfaces or abstract base classes. For example the trait for writing bytes is called std::io::Write. Trait can offer several methods. 
Most often a trait represent a capability: something a type can do. A value that represents str::io::Write can write out bytes and standart type std::fs::File implements the write trait. There's a rule about trait methods, a trait itself must be in scope with use keyword. 
A reference to trait type is called a trait object, and t can be either mut or shared. 
Rust let you implement any trait on any type, as long as either the trait or the type is introduced in current crate. 
To declare trait use trait keyword, to implement it use impl.

Auto traits are merker traits that are automatically implemented for every type. When a type is declared as an auto trait it will automatically create impls for every struct, enum, uniot unless an explicit impl is provided.

Marker trait are mostly used to bound generic type variables to express constraints you can't capture otherwise. These include Size and Copy. They are called so bacause Rust language itself uses them to mark certain types as having charasteristics of interest.

Blanket implementation is a implementation of a trait on any type that satisfies the trait bounds and it is extensively used in the Rust standart library.

Uncovered tyoe is a tyoe which does not appear as an argument to another type. For example, T is uncovered, the the T in Vec<T> is covered.

- What are static and dynamic dispatches? Which should I use, and when?

Static dispatch means that for any given copy of the method the address we are "dispatching to" is known statically. Dynamic dispatch enables code to call a trait method on a generic type without knowing what that type is. Dynamic dispatch cuts compile times, since it's no longer necessary to compile multiple copies of types and methods, and it can improve the efficiency of CPU instruction cache, it also prevents the compiler from optimizing for the specific types that are used. It is better to use static dispatch in your libraries and dynamic dispatch in your binaries. In a library it's better to allow users to decide what king of dispatch is best for them since their need are unknown. Dynamic is better choice for binaries because it allows to write cleaner code that leaves out generic parameters and will compile more quickly. 

- What is a crate and what is a module in Rust? How do they differ? How are the used? What is workspace?

Each crate is a complete, cohesive unit: all the source code for a single library or executable plus any associated tests, examples, tools, configuration. Whereas create are about code sharing between projects, modules are about code organization within a project. They act as Rust's namespaces, containers for the functions, types, constants, and so on that make up your Rust program or library. A module is a collection of items, names features.  The pub keyword makes an item public so it can be accessed from outside the module. 
A workspace is a collection of packages with the same Cargo.lock and output directory. 

- What is cloning? What is copying? How do they compare? What is for trait drop? What is special about the trait?

Clone trait is for types that can make copies of themselves. Clone method should construct an independent copy of self and return it. Cloning a value usually entails allocating copies of anything it owns so clone can be expensive in terms of time and memory. Any type that implements Copy trait is Copy. Types that own any other resources, like heap buffers or operatins system handlers cannot implement Copy. Any type that implements Drop trait cannot be Copy. Type that needs special cleanup code and must also require special copying code can't be Copy. Copying is expection to move semantics, it copies value instead of moving. Drop is used to free the resources that the implementator instance owns. Drop lets you customize what happens when a value is about to go out of scope. Rust doesn't allow to call the Drop trait manually because it will run automatically. 

- What is immutability? What is the benefit of using it?

It means that developer don't have or can't mutate values in variables, instead create new ones. Benefits are less worries about future changes of variables. By default variables in Rust are immutable. 

- What are move semantics? What are borrowing rules? What is the benefit of using them?

Rust memory model introduces ownership and moves. Ownership means that the owning objetc gets to decide when to free the owned object and when the owner is destroyed it destroys its possessions along with it. In Rust concept of ownership is build into th elanguage and enforced by compile-time checks. Every value has a single owner that determines its lifetime. When the owner is freed - it owned value is dropped too. These rules are ment to make it easy for developers to find given value's lifetime simply by inspecting the code and gives developers control over its lifetime. When variable owns its value and control leaves the block in which the variable is declared the variable is dropped, so its value is dropped along with it. Every value in a Rust program is a member of some tree, rooted in some variable. And at ht ultimate root of each tree os a variable, when that variable goes out of scope, the entive tree goes with it. 
You can move values from one owner to another. This allows you to build, rearrange, and tear down the tree.
Very simple types like integers, float point numbers, and characters are exclused from the ownership rules. These are called copy types. The standart library provides the reference-counted pointer types Rc and Arc, which allow values to have multiple owners, under some restrictions. 
You can borrow a reference to a value, references are non-owning pointers, with limited lifetimes. In Rust, for most types, operations like assigning a balue to a variable, passing it to a functins, or returning it from a function don't copy the value, they move it. The source relinquished ownership of the value to the destination and becomes ininitialized and after it the destination will controll the value's lifetime. If you want to copy walue use clone() to ask a copy.  
It is possible to create more than on reference. Rust restricts mutable references and only allows one mutable reference at a time. Rust also disallows using mutable references along with immutable references because data inconsistency may occur. 

- What is RAII? How is it implemented in [Rust]? What is the benefit of using it?

RAII (Resource Acquisition Is Inititalization) is an essential programming idiom mostly associated with c++ but it is also present in Rust: once an object exits scope, its destructor is called, and its owned resources are released. Developers don't have to perform it by hand and it is safe against resource leakage problems. 

- What are lifetimes? Which problems do they solve? Which benefits do they give?

Lifetime is a kind of generic. Lifetimes ensure that the references are valid as long as developers need them to be. Every reference in Rust has a lifetime. Developers have to specify lifetime parameters for functions or structs that use references. The main aim of lifetimes is to prevent dangling references, which cause a program to reference data other that the data it's intended to rederence for example when the variable doesn't live long enough. Rust compiler has borrow checker that compares scopes to determine whether all borrows are valid. 

- What is an iterator? What is a collection? How do they differ? How are they used?

Iterator is a value that produces a sequence of values, typically for a loop to operate on. In Rust an iterator is any value that implements std::iter::Iterator trait. Collections are data structures, usually contain multiple values. Each kind of collection has different capabilities and costs. Most frequent to see are vector, string, hash map, map and it is possible to iterate over them using for loop to get elements. 

- What are macros? Which problems do they solve? What is the difference between declarative and procedural macro?

Macros are a way of writing code that writes other code. It is useful to reduce the amount of code developer have to write and maintain. Macros contrary to functions can take a variable number of parameters. Macros are expanded before the compiler interprets the meaning of code so it can implement a trait on a given type. Declarative macros allows to write something similar to a Rust match expression. Match expressions are control structures that take an expression, compare the resulting value  of the expression to patterns and then run the code associated with the matching pattern. Macros also compare a value to patterns that have code associated with them. Procedural macros accept some Rust code as an input, operate on that code and produce some Rust code as an output rather than matching agains patterns and replacing the code with other code as declarative macros do. 

- How code is tested in [Rust]? Where should you put tests and why?

There's a simple unit testing framework is build into Rust. In Rust test is a function that's annotated with the test attribute. Tests commonly use the assert! and assert_eq! macros from Rust standart library. For unit tests the convention is to create a module named tests in each file to contain the test function and to annotate the module with cfg(test). The #[cfg(test)] annotation on the tests module tells Rust to compile and run the test code only when you run cargo test, not when you run carge build. For integrations tests tests directory should be created at the top level of project directory, next to src. Cargo knows to look for integration tests in thes directory. Cargo will compile each of the files as an individual crate. 

- What is special about slice? What is layout of Rust standard data types? Difference between fat and thin pointers?

A slice is a pointer to a block of memory. Slices can be used to access portions of data stored in contiguous memory blocks. It can be used with data structures like arrays, vectors and strings. Slices use index numbers to access portions of data. Slices let you reference a contiguous sequence of elements in a collection rather than the whole collection.
Layout is how compiler decides on the in-memory representation of a type.
A fat pointer is just like a normal pointer, but it includes an extra word-sized field that gives the additional information about that pointer that the compiler needs to generate reasonable code for working with the pointer. When reference is taken to dynamically sized types the compiler automatically constructs a fat pointer for developer. For a slice the extra information is a length of the slice. Fat pointer is Sized.

- Why [Rust] has `&str` and `String` types? How do they differ? When should you use them? Why str slice coexist with slice? 

&str is a reference to str: non-owning pointer to UTF-8 text, immutable. String type is UTF-8 string, dynamically sized and stored in heap. Use String if you need owned string data like passing string to other thread or building them at runtime, and use &str if you need a view of string.

- Is [Rust] OOP language? Is it possible to use SOLID/GRASP? Does it have an inheritance? Is Rust functional language?

Rust is object oriented, structs and enums have data, impl blocks provide methods on structs and enums. Structs and enums provide same functionality as objects. Encapsulation can also be reached using pub keyword. There're no inheritance in Rust but there's composition possible by nesting structs and delegating function calls respectively. SOLID and Grasp should be applicable to any programming language. It is possible to apply functional programming principles in Rust. Proper application of functional principles may simplify the often complex design requirements, and make programming a much more productive and rewarding experience. Functional programming can greatly reduce the amount and complexity of code required to accomplish tasks.

After you're done notify your lead in an appropriate PR (pull request), and he will exam what you have learned.

_Additional_ articles, which may help to understand the above topic better:
- [Chris Morgan: Rust ownership, the hard way][1]
- [Ludwig Stecher: Rusts Module System Explained][2]
- [Tristan Hume: Models of Generics and Metaprogramming: Go, Rust, Swift, D and More][3]
- [Jeff Anderson: Generics Demystified Part 1][4]
- [Jeff Anderson: Generics Demystified Part 2][5]
- [Brandon Smith: Three Kinds of Polymorphism in Rust][6]
- [Jeremy Steward: C++ & Rust: Generics and Specialization][7]
- [cooscoos: &stress about &Strings][8]
- [Jimmy Hartzell: RAII: Compile-Time Memory Management in C++ and Rust][9]
- [Trait Drop][10]
- [Common Lifetime Misconception][11]
- [Visualizing Memory Layout][12]

[Cargo]: https://github.com/rust-lang/cargo
[Cargo Book]: https://doc.rust-lang.org/cargo
[Rust]: https://www.rust-lang.org
[Rust Book]: https://doc.rust-lang.org/book
[Rust By Example]: https://doc.rust-lang.org/rust-by-example
[Rust FAQ]: https://prev.rust-lang.org/faq.html

[1]: https://chrismorgan.info/blog/rust-ownership-the-hard-way
[2]: https://aloso.github.io/2021/03/28/module-system.html
[3]: https://thume.ca/2019/07/14/a-tour-of-metaprogramming-models-for-generics
[4]: https://web.archive.org/web/20220525213911/http://jeffa.io/rust_guide_generics_demystified_part_1
[5]: https://web.archive.org/web/20220328114028/https://jeffa.io/rust_guide_generics_demystified_part_2
[6]: https://www.brandons.me/blog/polymorphism-in-rust
[7]: https://www.tangramvision.com/blog/c-rust-generics-and-specialization#substitution-ordering--failures
[8]: https://cooscoos.github.io/blog/stress-about-strings
[9]: https://www.thecodedmessage.com/posts/raii
[10]: https://vojtechkral.github.io/blag/rust-drop-order/
[11]: https://github.com/pretzelhammer/rust-blog/blob/master/posts/common-rust-lifetime-misconceptions.md
[12]: https://www.youtube.com/watch?v=rDoqT-a6UFg
