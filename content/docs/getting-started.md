---
title: Getting Started
menu: docs_basics
weight: 130
---

# Getting Started

This guide will take your from zero to having an `actix-web` server displaying content
in your browser.

## Prerequisites

A machine with Rust 1.39 (or newer) installed is the only prerequisite for `actix-web`.
You can find out which version you have installed by running `rustc --version`.

If you're running an older version of Rust, `rustup update` will upgrade you to the
latest version.

If you have yet to install Rust, you should follow the official
[getting started guide][rust-get-started] before going any further.

## Hello, world!

Start by creating a new Cargo project. Note that by default `cargo new` will use the
binary application template, which is what we want.

```shell
$ cargo new hello-world
$ cd hello-world
```

Next we'll add our dependencies to `Cargo.toml`. In addition to the expected `actix-web`
dependency, you'll notice the inclusion of the Actix runtime crate `actix-rt` which
gives us access to a macro we'll be using later.

```toml
[dependencies]
actix-web = "2.0"
actix-rt = "1.0"
```

With the setup out of the way, we can now get into `actix-web` itself. The remainder of
this section of the guide takes place in `src/main.rs`.

`main()` is going to be the function that starts our server. As such, we need to make 3
changes to its definition:

1. Define it as `async`
2. Set its return type to `std::io::Result<()>`
3. Mark it with the `#[actix_rt::main]` [attribute macro][attribute-macro] provided
   by `actix-rt`

```rust
#[actix_rt::main]
async fn main() -> std::io::Result<()> {
    // ...
}
```

[rust-get-started]: https://www.rust-lang.org/learn/get-started
[attribute-macro]: https://doc.rust-lang.org/reference/procedural-macros.html#attribute-macros
[docs-app]: https://docs.rs/actix-web/2.0.0/actix_web/struct.App.html

---

# Getting Started

Let’s write our first `actix-web` application!

## Hello, world!

Start by creating a new binary-based Cargo project and changing into the new directory:

```bash
cargo new hello-world
cd hello-world
```

Now, add `actix-web` as a dependency of your project by ensuring your `Cargo.toml`
contains the following:

```ini
[dependencies]
actix-web = "{{< actix-version "actix-web" >}}"
```

If you want to use the `#[actix_rt::main]` macro, you have to add `actix-rt` to your dependency.
Now your `Cargo.toml` should look like following:

```ini
[dependencies]
actix-web = "{{< actix-version "actix-web" >}}"
actix-rt = "{{< actix-version "actix-rt" >}}"
```

In order to implement a web server, we first need to create a request handler.

A request handler is an async function that accepts zero or more parameters that can be
extracted from a request (ie, `impl FromRequest`) and returns a type that can be
converted into an `HttpResponse` (ie, `impl Responder`):

{{< include-example example="getting-started" section="setup" >}}

Next, create an `App` instance and register the request handler with the application's
`route` on a _path_ and with a particular _HTTP method_. After that, the application
instance can be used with `HttpServer` to listen for incoming connections. The server
accepts a function that should return an application factory.

{{< include-example example="getting-started" section="main" >}}

That's it! Now, compile and run the program with `cargo run`.
Head over to `http://localhost:8088/` to see the results.

**Note**: You could notice `#[actix_rt::main]` attribute macro. This
macro executes marked async function in actix runtime. Any async function
could be marked and executed by this macro.

### Using Attribute Macros to Define Routes

Alternatively, you can define routes using macro attributes which
allow you to specify the routes above your functions like so:

{{< include-example example="getting-started" section="macro-attributes">}}

You can then register the route using `service()`:

```rust
App::new()
    .service(index3)
```

For consistency reasons, this documentation only uses the explicit syntax shown at the
beginning of this page. However, if you prefer this syntax you should feel free to
use it any time you declare a route as it's only syntactic sugar.

To learn more, see [actix-web-codegen].

### Auto-reloading

If you want, you can have an automatically reloading server during development
that recompiles on demand. This isn't necessary, but it makes rapid prototyping
more convenient as you can see changes instantly upon saving.
To see how this can be accomplished, have a look at the [autoreload pattern][autoload].

[actix-web-codegen]: https://docs.rs/actix-web-codegen/
[autoload]: ../autoreload/
