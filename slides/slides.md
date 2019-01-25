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
  handlebars = "1.1.0"

  [dependencies.rocket_contrib]
  version = "0.4"
  features = ["handlebars_templates"]
```

---

## Hello, world!

----

```
  #![feature(proc_macro_hygiene, decl_macro)]
  #[macro_use] extern crate rocket;
 
  #[get("/")]
  fn index() {
      "Hello, world!"
  }

  fn main() {
      rocket::ignite().mount("/", routes![index]).launch;
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

## Requests

```
  #[get("/world")]
  fn handler() { .. }
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

----

```
  #[post("/")]
```

---

## Dynamic Paths

```
  #[get("/hello/<name>")]
  fn hello(name: &RawStr) -> String {
      format("Hello, {}!", name.as_string())
  }
```

---

## Directory structure

- src
- static <!-- .element: class="fragment" -->
- templates <!-- .element: class="fragment" -->
- Cargo.toml <!-- .element: class="fragment" -->

---

### static

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
      rocket::ignite().mount("/", routes![index, files]).launch
  }
```

---

## Templates

- Handlebars
- Tera <!-- .element: class="fragment" -->

----

```
  ...
  extern crate rocket_contrib;
  use std::collections::HashMap;
  use crate::handlebars::{to_json};
  use rocket_contrib::templates::{template, handlebars}

  #[get("/")]
  fn index() -> Template {
      let variable = "value";
      let mut data = HashMap::new();
      data.insert("v", &variable);
      Template::render("index", &data)
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