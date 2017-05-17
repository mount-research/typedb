typedb
========

[![crates.io version](https://img.shields.io/crates/v/typedb.svg)](https://crates.io/crates/typedb)

Simple persistent generic HashMap/Key-value store, using file locking to limit writing between threads.

This is in a beta state at the moment.

[Documentation](https://docs.rs/typedb)

Basic usage:
```rust
extern crate typedb;

use typedb::{ KV, Value };

fn main() {
    let mut test_store = KV::<String, Value>::new("./db.cab");

    let _ = test_store.insert("key".to_string(), Value::String("value".to_string()));
    println!("{:?}", test_store.get("key".to_string()));
    let _ = test_store.remove("key".to_string());
}
```

Usage with user defined Key and Value types:
```rust
extern crate typedb;
extern crate serde;
#[macro_use]
extern crate serde_derive;

use typedb::KV;

#[derive(Clone, Serialize, Deserialize, PartialEq, Eq, Hash)]
enum MyKey {
    String(String),
    Int(i32),
}

#[derive(Clone, Serialize, Deserialize, Debug)]
enum MyValue {
    String(String),
    Int(i32),
}

fn main() {
    let mut test_store = KV::<MyKey, MyValue>::new("./db.cab").unwrap();

    let _ = test_store.insert(MyKey::Int(1i32), MyValue::String("value".to_string()));
    println!("{:?}", test_store.get(MyKey::Int(1i32)));
    let _ = test_store.remove(MyKey::Int(1i32));
}
```
