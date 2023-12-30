# Welcome To Rust 🦀

a guideline to getting started with rust

## Guides

1. [The Rust Book](https://doc.rust-lang.org/book/title-page.html)
2. [Setup Dev Environment](https://dev.to/cthutu/rust-1-creating-your-development-environment-55bi)
3. [Rust the Right Way](https://github.com/flowchartsman/rust-the-right-way)
4. [How to Not Learn Rust](https://dystroy.org/blog/how-not-to-learn-rust/)
5. [Practice on Exercism](https://exercism.org/tracks/rust)

## [Concepts](./Concepts.md)

1. [Memory](./Concepts.md#memory)
2. [Ownership](./Concepts.md#ownership)
3. [Moving](./Concepts.md#moving)
4. [MARSAW](./Concepts.md#marsaw)
5. [Lifetimes](./Concepts.md#lifetimes)
6. [Dynamic vs Static Dispatch](./Concepts.md#dynamic-dispatch-vs-static-dispatch)
7. [Smart Pointers](./Concepts.md#smart-pointers)
8. [Errors](./Concepts.md#errors)
9. [Performance](./Concepts.md#performance)
10. [Strings](./Concepts.md#strings)

## [Crates](./Crates.md)

1. [My Common Crates](./Crates.md#my-common-crates)
2. [Logging](./Crates.md#logging)
3. [Serialization](./Crates.md#serialization)

## Async

Rust does not have a standard async runtime. The most popular is tokio.

- a primer on rust [multi-threading](https://priver.dev/blog/rust/multi-threading/)
- tutorial for [concurrency](https://www.koderhq.com/tutorial/rust/concurrency/)

## Database Development & SQLx

Interacting with databases is a fundamental aspect of backend development, but it can also be complex. Frameworks like ORMs were introduced to help reduce the complexity for developers unfamiliar with databases, especially relational databases. However, these ORMs often introduce and can even encourage inefficiency while also creating new opportunities for complexity.

SQLx was created to remove the manufactured layer between the database and the application crated by ORMs. Using a tool like SQLx does, generally speaking, required more knowledge of relational databases and the query language (SQL) itself. However, the payoff is enormous.

- learning the [SQLx basics](https://tms-dev-blog.com/rust-sqlx-basics-with-sqlite/)

## Tools

- just
- cargo-expand
- cargo-watch
- watchexec: https://github.com/watchexec/watchexec#readme

## CLI Development

- [helpful starting guide](https://rust-cli.github.io/book/index.html)
- clap
- https://crates.io/crates/indicatif

## [Macros](./Macros.md)

1. [Procedural Macros](./Macros.md#procedural-macros)
2. [Macro Helpers](./Macros.md#macro-helpers)

## Writing Rust Documentation

https://doc.rust-lang.org/rustdoc/how-to-write-documentation.html

## Writing Rusty APIs

https://deterministic.space/elegant-apis-in-rust.html#dont-stringly-type-your-api
