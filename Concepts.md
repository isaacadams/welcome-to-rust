# Concepts

## Memory

stack: [last in, first out]

- fast memory creation and fetching = speed!
- memory size that is known at compile time

heap:

- memory can grow in size (vector, hashmap, string, etc.)
- runtime performance cost (speed)
- memory can live beyond the scope that created it
- memory is automatically recaptured when the last owner goes out of scope

rust defaults everything to the stack

functions and their arguments are allocated to the stack

## Ownership

- no garbage collection
- eliminates memory issues during runtime (null pointers, dangling pointers, data races)
- parallel processing is a breeze to implement

The concept of ownership is a central feature that helps to ensure memory safety. Every value has an "owner" and there can only be one owner at a time. When the owner goes out of scope, the value is automatically dropped (i.e., freed from memory). This eliminates the need for manual memory management and helps to prevent common programming errors such as null or dangling pointer references and data races.

The rules of ownership in Rust can be summarized as follows:

- Each value in Rust has a single owner
- When the owner goes out of scope, the value is automatically dropped
- Ownership can be transferred between variables through the use of move semantics
- It is not possible to have multiple owners of the same value

The concept of ownership in Rust helps to prevent common programming errors, significantly increases developer confidence in the code's safety, and makes it easier for developers to write efficient code. It is a key part of the Rust language and is one of the features that makes it unique compared to other programming languages.

## Moving

- when assigning a heap variable to another variable, the memory is moved to the new variable
- the new variable is now the "owner" of that memory
- the old variable no longer points to anything
- this will not cause issues because Rust will not compile if the old variable is referenced after a move
- other languages allow different variables to point to the same memory which can lead to race conditions

## MARSAW

> MARSAW: Multiple Active Readers or Single Active Writer

- all about guarantee that memory issues won't occur
- if there are active readers, the memory cannot be changed // written to
- the memory can be changed before or after the active readers

## Lifetimes

- only has to do with references
- only relevant when returning a reference
- each input is automatically given its own unique lifetime
- any lifetime not explicitly defined is given its own unique lifetime
- ensuring memory does not get dropped before a reference can use it
- avoid dangling references which is a reference to a variable which has been dropped
- assigning input and output params the same lifetime makes output lifetime choose smallest lifetime
- my own words: if a function returns a reference which has the possibility to come from more than one input reference, then those input reference lifetimes need to be shared with the output
- rust wants to guarantee memory safety; when confronted with a function that returns a reference, it must know what lifetime that output reference is based on so that it can find pointer issues if they exist

## Dynamic Dispatch vs Static Dispatch

- tradeoff: static can lead to larger assembly binaries and dynamic may lead to runtime performance cost
- static dispatch: `fn xx<C: SomeTrait>(input: C)`
  - any concrete type which implements SomeTrait can be received as the input
  - the method is copied into the assembled for every unique concrete type which is passed as an argument
- dynamic dispatch: `fn xx(input: dyn SomeTrait)`
  - resolves the passed in implementation during runtime

## Smart Pointers

Smart pointers are a type of data structure that are used to manage the ownership and lifetime of other data. They are called "smart" because they provide additional functionality beyond what is provided by the basic reference types in Rust. Smart pointers are always allocated onto the heap.

Some of the common use cases for smart pointers include:

1. Automatically deallocating memory: Smart pointers can be used to automatically deallocate memory when it is no longer needed. This can be useful when you want to avoid memory leaks or when you want to make sure that memory is freed in a deterministic way.
2. Sharing data between multiple parts of your code: Smart pointers can be used to share data between multiple parts of your code in a safe and efficient way. This can be useful when you want to pass data to a function or when you want to return data from a function.
3. Providing additional functionality: Smart pointers can provide additional functionality beyond what is provided by the basic reference types in Rust. For example, some smart pointers can provide thread-safe access to data, or they can automatically handle the locking and unlocking of mutexes.

Overall, smart pointers are a useful data structure in Rust that can be used to manage the ownership and lifetime of other data. They provide additional functionality beyond what is provided by the basic reference types and can be useful in a variety of situations.

Here is a video tutorial

**Box<T>**

Regardless of what T is, Box<T> will tell the compiler that the memory will be allocated onto the heap. This frees the compiler from being required to know the size of T.

**Rc<T> Reference Counter**

Persists data across multiple borrows until last borrow is dropped. Internally, it keeps track of all the borrows to the memory. Only when it drops the last reference will the memory be dropped in the heap.

**Arc<T> Atomic Reference Counter**

This is the thread safe version of Rc<T>. This has more overhead than Rc<T> and, therefore, should only be used when needed.

## Errors

[anyhow](https://crates.io/crates/anyhow) for binaries [thiserror](https://crates.io/crates/thiserror) for libraries (they are related crates)

rusts bare Error trait doesn't have many features and requires significant boilerplate to use properly

if the crate has a main() function (binary) use anyhow if crate is just a library use thiserror

they are compatible so that when a binary crate is using anyhow, it is compatible with libraries which use thiserror the difference is in how they treat errors

- thiserror treats errors in a way which can be re-exported and re-packaged in a binary or another library
- anyhow assumes end-user will see errors, and they don't need to be re-exported, so there are more tools for creating errors on-the-fly and such

however, these on-the-fly errors aren't the easiest to deal with if the crate is being used as a library, but they provide lots of context for the end-user to grok what went wrong during the execution of the program

## Performance

There are several tools that you can use to measure the performance of your Rust programs. Some of the most commonly used tools include:

- The `perf` tool: a Linux-based tool that can be used to collect and analyze performance data from your Rust programs. It can provide detailed information about the performance of your code and can help you identify potential performance bottlenecks.
- The `perf-tool` crate: provides a high-level interface to the Linux `perf` tool. It allows you to easily collect and analyze performance data from your Rust programs without having to use the `perf` tool directly.
- The `criterion` crate: provides a framework for running performance benchmarks on your Rust code. It can automatically run your benchmarks, collect performance data, and provide detailed reports on the performance of your code.
- The `perf_events` crate: provides a low-level interface to the Linux `perf` tool. It allows you to directly collect and analyze performance data from your Rust programs.

These are some of the tools that you can use to measure the performance of your Rust programs. You can use these tools to collect performance data and make informed decisions about how to optimize your code for performance.

## Strings

- [the various string types explained](https://dev.to/cthutu/rust-8-strings-53o)
- [text processing](https://www.linuxjournal.com/content/text-processing-rust)

## Questions

**Why would I choose an owned variable rather than a referenced variable?**

There are a few reasons why you might want to take ownership of a variable in the Rust programming language:

1. When you take ownership of a variable, you are responsible for managing its memory. This means that you can control when the variable is allocated and freed, and you can ensure that it is used in a safe and efficient way. This can be useful when you want to optimize the performance of your code or when you need to manage resources in a specific way.
2. When you take ownership of a variable, you can move it to another part of your code. This can be useful when you want to pass a variable to a function or when you want to return a variable from a function. Moving a variable allows you to transfer its ownership to another part of your code, so that the variable can be used in a different context.
3. When you take ownership of a variable, you can modify it in place. This can be useful when you want to update a variable's value or when you want to modify the data that the variable points to. Taking ownership of a variable allows you to mutate it directly, without having to create a new variable or copy its data.

Overall, there are a few reasons why you might want to take ownership of a variable in Rust. It is worth considering the trade-offs and deciding when it is appropriate to take ownership of a variable in your code.

**Why would I define different lifetimes than the same one?**

**Why would I choose to make a string constant rather than instantiate it during runtime?**

A constant string is stored into your rust programs binary. This means that wherever it referenced during the runtime of your code, it will always be pointing to the same location of memory throughout the lifetime of the program. In other words, it will never generate new memory for that string, so it will never be stored in multiple memory locations at once.

Therefore, if you have a string that is heavily used in your program, it will likely increase the performance of your code to turn that into a string constant.
