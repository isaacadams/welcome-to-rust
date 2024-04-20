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

### Multi-threading and Concurrency

- primers on [multi-threading] in rust
    - [primer #1](https://archive.is/BBoEq)
    - [primer #2](https://archive.is/Op3Q6)
- tutorial for [concurrency](https://www.koderhq.com/tutorial/rust/concurrency/)

## Database Development & SQLx

Database interaction is a fundamental and complex aspect of backend development. If you have done any backend work, it is likely you have seen things like the "repository pattern", ORMs, connection strings, queries, etc. 

Some of those things are great, others not so much. For instance, ORMs. *dun dun dun*. While ORM frameworks were introduced to help reduce complexity (does anyone ever create something intending to *increase* complexity?), it often ends up increasing it. Developers have gone to extreme lengths to keep themselves from ever having to leave the comfort zone of their favorite langauge. Truthfully, ORMs were made because developers don't like SQL and/or don't want to learn it. 

As it turns out, there is no subsitute for SQL. And in trying to 'reduce the complexity of SQL', the manufactured layer (the ORM) between the code and the database has only been a welcome opportunity for complexity and inefficiency to take hold and thrive.

SQLx was created to remove this manufactured layer. Using a tool like SQLx does, generally speaking, required more knowledge of relational databases and the query language (SQL) itself. However, the payoff is enormous.

- learning the [SQLx basics](https://tms-dev-blog.com/rust-sqlx-basics-with-sqlite/)

## Tools

- [just](https://github.com/casey/just)
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
