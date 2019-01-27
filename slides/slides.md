## Rust + Rocket

#### [Mario Garcia](https://mariog.xyz) · [@mariogmd](https://twitter.com/mariogmd)

---

## Rocket

----

### Simple, Fast, Type Safe Web Framework for Rust

https://rocket.rs/

---

## Configuration

---

### Rust Nightly

```
  $ rustup install nightly
```

----

### New project with Cargo

```
  $ cargo new hello_rocket
```

----

### Set toolchain

```
  $ cd hello_rocket
  $ rustup override set nightly
```

---

## Directory structure

- src
- static <!-- .element: class="fragment" -->
- templates <!-- .element: class="fragment" -->
- Cargo.toml <!-- .element: class="fragment" -->

---

## Templates

- Handlebars
- Tera <!-- .element: class="fragment" -->

---

## Hello, world!

----

### Cargo.toml

```
  [package]
  name = "hello_rocket"
  version = "0.1.0"
  authors = ["mattdark"]
  edition = '2018'

  [dependencies]
  rocket = "0.4"
```

----

### src/main.rs

```
  #![feature(proc_macro_hygiene, decl_macro)]
  #[macro_use] extern crate rocket;
 
  #[get("/")]
  fn index() {
      "Hello, world!"
  }

  fn main() {
      rocket::ignite().mount("/", routes![index]).launch();
  }
```

----

### Running

```
  $ cargo run
```

---

## Routing

```
  #[get("/world")] {            // <- route attribute
  fn world() -> &'static str {  // <- request handler
      "Hello, world!"
  } 
```

---

## Mounting

```
  fn main() {
      rocket::ignite.mount("/hello", routes![world]).launch();
  }
```

---

## Methods

- get
- put <!-- .element: class="fragment" -->
- post <!-- .element: class="fragment" -->
- delete <!-- .element: class="fragment" -->
- head <!-- .element: class="fragment" -->
- patch <!-- .element: class="fragment" -->
- options <!-- .element: class="fragment" -->

---

## Dynamic Paths

```
  ...
  use rocket::http::RawStr;

  #[get("/hello/<name>")]
  fn hello(name: &RawStr) -> String {
      format("Hello, {}!", name.as_str())
  }
```

---

## Static files

```
  ...
  use std::path::{Path, PathBuf};
  use rocket::response::NamedFile;
  ...
  #[get("/<file..>")]
  fn files(file: PathBuf) -> Option<NamedFile> {
       NamedFile::open(Path::new("static/").join(file)).ok()
  }

  fn main() {
      rocket::ignite().mount("/", routes![index, files]).launch();
  }
```

---

## Using templates

----

### Cargo.toml

```
  ...
  handlebars = "1.1.0"

  [dependencies.rocket_contrib]
  version = "0.4"
  features = ["handlebars_templates"]
```

----

### src/main.rs

```
  ...
  extern crate rocket_contrib;
  use rocket_contrib::templates::{template, handlebars};

  #[get("/")]
  fn index() -> Template {
      Template::render("index")
  }
  ...
  fn main() {
      rocket::ignite().mount("/", routes![index, files])
      .attach(Template::fairing())
      .launch();
  }
```

---

## Learn More

_[rocket.rs](https//rocket.rs/)_

___

[mariog.xyz](https://mariog.xyz/)
