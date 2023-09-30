# Rust Crates

Like any other modern language, rust has a package manager and its name is `cargo`.

Unlike other languages, rust does not believe in maintaining a massive standard library. This design choice is, of course, a tradeoff.

benefit: by not being forced to ship a would-be large standard library, production ready applications can have their binaries significantly optimized by removing any unused crates and/or feature flags.

cons: it is cumbersome to add the same crates to the dependency list when beginning new projects.

I think [blessed.rs](https://blessed.rs/crates) says it best:

> The standard library in Rust is much smaller than in Python or Go, for example. Those languages come with "batteries included" support for things like HTTP(S), JSON, timezones, random numbers, and async IO. Rust, on the other hand, gets things like that from the crates.io ecosystem and the Cargo package manager. But with almost 100 thousand crates to choose from, a common complaint from new Rust developers is that they don't know where to start, which crates they ought to use, and which crates they ought to trust. This list attempts to answer those questions.
>
> See also [lib.rs](https://lib.rs) which takes a more automated approach (ordering crates by number of downloads) and also has an excellent search function.
>
> source: https://blessed.rs/crates

There is a fairly large set of popular crates that are commonly used in rust projects. This document is not just a list of common crates. It is an introductory _how to_ guide to the most common and basic crates you will be reaching for in any rust project.

# My Common Crates

```bash
# my common crates
cargo add thiserror dotenvy clap reqwest lazy_static
cargo add log env_logger
cargo add serde_json serde --features derive
cargo add sqlx --features macros,uuid,chrono,postgres,runtime-tokio-rustls

# tokio for async
cargo add tokio --features full
# tokio-lite
cargo add tokio --features macros,parking_lot

# data formats
cargo add chrono --features serde
cargo add uuid --features v4,serde
cargo add base64
```

Other useful crates I have not yet tried: `yew`, `rayon`, `tracing`

# Logging

A guide for implementing logging in your rust project.

## Hello World

write hello world

```rust
fn main() {
    println!("{}", "hello world");
}
```

that was easy...

For hobby projects, the `print!` and `println!` macros will suffice.

## `log`: trace, debug, error, warn, and info

For production projects or if you desire more control, the next step is to learn about logging crates.

The most common logging pattern exposes various levels of logging: trace, debug, error, warn, and info.

This _abstract API_ is provided by the [`log`](https://docs.rs/log/latest/log/) crate.

```rust
use log::{info, warn};

fn main() {
    log::trace!("{}", "hello world");
    log::debug!("{}", "hello world");
    log::error!("{}", "hello world");
    log::warn!("{}", "hello world");
    log::info!("{}", "hello world");
}
```

Keep in mind, the `log` crate is an API abstraction. It exists so that a crate that implements logging to files uses the same API (i.e. `log`'s abstract API) as an entirely different crate, written by a different author, that logs to stdout or stderr.

This is beneficial because you can add `log` to your project and immediately begin writing your application, using `log::info!`, `log::warn`, etc. without worrying about where those logs are going to end up (file, stdout, etc.) or worrying about having to refactor those method calls, ever.

### Log Level Filtering

A higher degree of observability is often required in local development than in production. Production may only be interested in `log::error!` while higher levels of logging are required during local debugging.

This is where the concept of log filtering comes in and here is the basic logic.

```rust
pub enum LevelFilter {
    /// A level lower than all log levels.
    /// no logs will show
    Off,
    /// Corresponds to the `Error` log level.
    /// error only
    Error,
    /// Corresponds to the `Warn` log level.
    /// warn and error
    Warn,
    /// Corresponds to the `Info` log level.
    /// info, warn, and error
    Info,
    /// Corresponds to the `Debug` log level.
    /// debug, info, warn, and error
    Debug,
    /// Corresponds to the `Trace` log level.
    /// everything
    Trace,
}
```

## Logger crates

When ready to configure to what destination the logs should be written, you can choose a crate that implements `log`.

A list of some `log` implementations can be found here: https://docs.rs/log/latest/log/#available-logging-implementations

## `env_logger`: configure filtering using env variable `RUST_LOG`

A popular logger implementation choice is [`env_logger`](https://docs.rs/env_logger/latest/env_logger/).

This crate makes it possible to configure log filtering using the `RUST_LOG` environment variable.

> NOTE: By default, `env_logger` writes logs to `stderr` but can be configured to write to `stdout` instead.

As mentioned in the previous section, maybe you want production to only write error level logs. In that case, you'd do the following.

```bash
cargo add log env_logger
```

```rust
fn main() {
    env_logger::init();

    log::info!("hello world");
    log::error!("hello world");
    log::trace!("hello world");
}
```

```
# production .env or machine environment variables
RUST_LOG=error
```

Try it out for yourself. First, run the following:

```bash
cargo run
```

By default, only the error logs should be written to the terminal.

Now, lets increase the filtering to info.

```bash
RUST_LOG=info cargo run
```

Both the info and error logs should have been written.

I recommend diving into the [documentation](https://docs.rs/env_logger/latest/env_logger/) and/or watching the following tutorial:

https://www.youtube.com/watch?v=7EYDaSNSkbM

## `simplelog`: further customize logging for terminal and files

```bash
cargo add log simplelog
# for generating timestamp in example, not required in general
cargo add chrono
```

```rust
use chrono::{DateTime, Utc};
use simplelog::{
    ColorChoice, CombinedLogger, ConfigBuilder, TermLogger, TerminalMode, WriteLogger,
};

fn main() {
    let datetime: DateTime<Utc> = chrono::offset::Utc::now();
    std::fs::create_dir("logs");

    CombinedLogger::init(vec![
        // configuring terminal logging
        TermLogger::new(
            // terminal will only show info and up
            log::LevelFilter::Info,
            ConfigBuilder::default().build(),
            TerminalMode::Mixed,
            ColorChoice::Auto,
        ),
        // configuring file logging
        WriteLogger::new(
            // file will only show Debug and up
            log::LevelFilter::Debug,
            ConfigBuilder::default()
                // ignore crate logs
                //.add_filter_ignore_str("<crate_name>")
                .build(),
            std::fs::File::create(format!("logs/{}.log", datetime.format("%Y-%m-%dT%H-%M-%S")))
                .unwrap(),
        ),
    ])
    .unwrap();

    // only appears in terminal
    log::info!("hello world");
    // appears in terminal and file
    log::error!("hello world");
    // does not appear in file or terminal
    log::trace!("hello world");
}
```

# Serialization

This section answers the question: How do I ser/de(ser)-ialize (serde) things like `json`, `toml`, `xml`, `csv`, etc. into my rust structs?

| crate                                               | description     |
| --------------------------------------------------- | --------------- |
| [`serde`](https://crates.io/crates/serde)           | abstraction api |
| [`serde_json`](https://crates.io/crates/serde_json) | json            |
| [`toml`](https://crates.io/crates/toml)             | toml            |
| [`quick-xml`](https://crates.io/crates/quick-xml)   | xml             |
| [`csv`](https://crates.io/crates/csv)               | csv             |
| [`serde_yaml`](https://crates.io/crates/serde_yaml) | yaml            |

## Intro (for JS devs): where the HELL is `JSON.parse()`?

It is generally true that most people are familiar with javascript based languages and, as a result, are spoiled, especially when it comes to parsing JSON.

Yeah, parsing (`deserializing`) and stringifying (`serializing`) JSON is super easy in JS/TS. _Just_ use `JSON.parse(...)` or `JSON.stringify(...)`. It is that simple in javascript land. This is not the case in other languages because unlike javascript, other languages care about things like (real) types and performance.

My programming career began in the .NET world, which, its been a few years, but last I checked, `Newtonsoft.Json` was the most popular nuget package for handling json serialization and deserialization. Coming from that background, it was not surprising to me that rust did not have something as simple as `JSON.parse/stringify`. In fact, when I began learning javascript, I was shocked to discover that JSON parsing was so... simple.

All of that to say, if you are coming from a javascript background and that is the only language you have ever used, avoid being one of those crybaby developers who compare everything to the only language they have ever known.

## `serde`: ser/de(ser)-ialization in rust

Similar to `log`, `serde` is merely an abstraction api that empowers downstream implementations (`serde_json`, `toml`, etc.) to use a common trait or interface system for a high degree of interoperability.

Regardless of what data you need to serde (json, xml, etc.), if you need to serde into a struct, then you will run the following in your terminal

```bash
cargo add serde --features derive
```

Then, you will add the following to your struct:

```rust
use serde::{Deserialize, Serialize};

#[derive(Serialize, Deserialize)]
struct MyStruct {
    message: String
}
```

This gives your struct the ability to serde into/from any format (json, xml, etc.) that implements `serde`.

## JSON example

The following will enable json serialization and deserialization.

```bash
cargo add serde_json
```

```rust
// basic example
use serde::{Deserialize, Serialize};

// adding Debug allows us to print out the struct using `{:?}` or to pretty-print `{:#?}`
#[derive(Debug, Serialize, Deserialize)]
struct MyStruct {
    message: String,
}

fn main() {
    // deserialize
    let data_1: MyStruct = serde_json::from_str("{ \"message\": \"hello world\" }").unwrap();
    println!("{:#?}", data_1);

    // serialize
    let data_2 = MyStruct {
        message: "hello world".to_string(),
    };
    let json: String = serde_json::to_string(&data_2).unwrap();
    println!("{}", json);
}
```

```rust
// tips and tricks
use serde::{Deserialize, Serialize};

// adding Debug allows us to print out the struct using `{:?}` or to pretty-print `{:#?}`
#[derive(Debug, Serialize, Deserialize)]
struct MyStruct {
    message: String,
}

fn main() {
    // use r#""# to avoid having to use \"
    let data_1: MyStruct = serde_json::from_str(r#"
    {
        "message": "hello world"
    }
    "#).unwrap();
    println!("{:#?}", data_1);

    // or use json token to construct your json and easily drop-in variables
    let message: &str = "hello world";
    let token: serde_json::Value = serde_json::json!({
        "message": message
    });

    // deserialize token
    let json: MyStruct = serde_json::from_value(token).unwrap();
    println!("{:#?}", json);

    // serialize token
    let token: serde_json::Value = serde_json::json!({
        "message": message
    });
    println!("{}", token.to_string());

    // pick a property from json token
    println!("{}", token["message"]);
}
```

## unions and tags: handling variations in type on a field

```rust
use serde::{Deserialize, Serialize};

#[derive(Serialize, Deserialize, Debug)]
#[serde(untagged)]
pub enum Status {
    Name(String),
    Number(u8),
}

impl ToString for Status {
    fn to_string(&self) -> String {
        match self {
            Status::Name(n) => n.clone(),
            Status::Number(n) => n.to_string(),
        }
    }
}

#[derive(Debug, Serialize, Deserialize)]
#[serde(rename_all = "camelCase")]
pub struct Event {
    pub id: uuid::Uuid,
    pub status: Status,
    pub message: String,
    pub received_at: chrono::NaiveDate,
}
```
