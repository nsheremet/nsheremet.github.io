+++
author = "Nazarii Sheremet"
categories = ["code"]
date = "2017-04-18T13:11:29+03:00"
description = "Two weeks ago I started to learn [Rust](https://www.rust-lang.org/),
for better learning, I decided to rewrite my library `mruby-smallhttp` on Rust."
draft = false
tags = ["rust", "open-source", "cargo", "crates", "rust-lang"]
title = "My first Rust Experience"

+++

### Hi guys.

Two weeks ago I started to learn [Rust](https://www.rust-lang.org/),
for better learning, I decided to rewrite my library `mruby-smallhttp` on Rust.
Let's get to know Cargo. It's something like `bundler` in Ruby world.


```shell
$ cargo new library # => generate a template for project
$ tree # =>
library
├── Cargo.toml
└── src
    └── lib.rs
```


`Cargo.toml` it's a manifest file. If you want to know more about how to get started in Rust, here is the [link](http://doc.crates.io/index.html).
`lib.rs` it's a main file for a library. If you create a binary project the main file is `main.rs`.


If you want to use my lib in your projects, here is examples:

##### Install
```toml
# Cargo.toml
[dependencies]
knock = "0.1"
```

##### Simple GET request
```rust
extern crate knock;

use knock::HTTP;

fn main() {
	let http = HTTP::new("https://google.com").unwrap();
	let response = http.get().send();
}
```

##### Simple POST request
```rust
extern crate knock;

use knock::HTTP;
use std::collections::HashMap;

fn main() {
    let http = HTTP::new("https://google.com").unwrap();
    let mut body = HashMap::new();
    let mut headers = HashMap::new();

    body.insert("file", Data::File("/path/to/file.file"));
    body.insert("field", Data::String("value"));

    headers.insert("Content-Type", "multipart/form-data");

    let response = http.post().body(body).header(headers).send();
}
```

After writing `knock`, I wanted to write a command line tool to send an HTTP request.
Before that, I used POSTMAN, but I don't like electron because it's Chrome.
I don't like chrome at all, but when it needs to be used to work with POSTMAN it's too much.
So I started using my `knock` lib for writing command-line tool on Rust.

I wrote it quickly, I had to add some code to `knock` lib.
Now I have a command-line tool with name `getsb`. It's not over yet, and now version v0.1.0. I want to add more features. So I will very glad for any help.

##### How to Install

```shell
$ git clone https://github.com/nsheremet/getsb-cli.git
$ cd getsb-cli
$ cargo build --release
```

And then move binary file in bin dir:

###### Linux

```shell
$ sudo mv target/release/getsb /usr/local/bin
```

###### OSX


```shell
$ sudo mv target/release/getsb /usr/local/bin/getsb
```

###### Windows

- Create a folder for getsb
- search for `env`
- open "edit your enviroment variables"
- edit `PATH`
- append folder path to the end of the string ie: `<path_stuff_here>;C:/getsb/;`

##### How to use

```shell
$ getsb POST https://example.com/api/data -b "key=value" -h "Content-Type: application/x-www-form-urlencoded"
```

For more info check [README.md](https://github.com/nsheremet/getsb-cli/blob/master/README.md) file.


After all this, I really liked Rust. This is an awesome language with safe working with memory and other awesome things.

If you want to start to learn here is few links:

- [Rust-Lang](https://www.rust-lang.org/en-US/index.html)
- [The Rust Programming Language Book](https://doc.rust-lang.org/book/)
- [Awesome Rust](https://github.com/kud1ing/awesome-rust)

And also link on my projects:

- [knock](https://github.com/nsheremet/knock)
- [getsb-cli](https://github.com/nsheremet/getsb-cli)

So, if you want to learn Rust, here is a very nice [book](https://doc.rust-lang.org/book/).

#### Thanks.
