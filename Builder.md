


## Typed builder = compile-time

https://crates.io/crates/typed-builder
https://idanarye.github.io/rust-typed-builder/typed_builder/derive.TypedBuilder.html


```rust
use typed_builder::TypedBuilder;
use enum_dispatch::enum_dispatch;
use serde::{Deserialize, Serialize};

#[enum_dispatch]
trait Shaped {
    fn area(&self) -> f64;
}

#[enum_dispatch(Shaped)]
enum Shape {
    Rectangle,
}


#[derive(TypedBuilder)]
#[builder(build_method(into = Shape))]
struct Rectangle {
    width: f64,
    height: f64
}

impl Shaped for Rectangle {
    fn area(&self) -> f64 {
        (self.width * self.height)
    }
}

fn main() {
    // Enum dispatch
    let rectangle: Shape = Rectangle::builder()
        .width(10.0)
        .height(20.0)
        .build();
    println!("Rectangle area: {}", rectangle.area());
}
```


 
## derive_builder = runtime

https://crates.io/crates/derive_builder
https://docs.rs/crate/derive_builder/0.20.2

```rust
use derive_builder::Builder;
use typed_builder::TypedBuilder;
use enum_dispatch::enum_dispatch;
use serde::{Deserialize, Serialize};

#[enum_dispatch]
trait Shaped {
    fn area(&self) -> f64;
}

#[enum_dispatch(Shaped)]
enum Shape {
    Rectangle,
}


#[derive(Builder)]
struct Rectangle {
    width: f64,
    height: f64
}

impl RectangleBuilder {
    fn build_shape(&self) -> Result<Shape, RectangleBuilderError> {
       self.build().map(|r| r.into())
    }
}

impl Shaped for Rectangle {
    fn area(&self) -> f64 {
        (self.width * self.height)
    }
}

fn main() {
    // Enum dispatch
    let rectangle: Shape = RectangleBuilder::default()
        .width(10.0)
        .height(20.0)
        .build_shape().unwrap();
    println!("Rectangle area: {}", rectangle.area());
}
```