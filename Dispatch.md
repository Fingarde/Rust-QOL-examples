# enum_dispatch

https://crates.io/crates/enum_dispatch

https://gitlab.com/antonok/enum_dispatch


## Example 
dyn dispatch
```rust
use enum_dispatch::enum_dispatch;

#[enum_dispatch]
trait Shaped {
    fn area(&self) -> f64;
}

struct Square {
    side: f64,
}

impl Shaped for Square {
    fn area(&self) -> f64 {
        (self.side * self.side)
    }
}

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
    // Dynamic dispatch
    let square: Box<dyn Shaped> = Box::new(Square { side: 32. });
    println!("Area of square: {}", square.area());
}
```

enum dispatch
```rust

use enum_dispatch::enum_dispatch;
use serde::{Deserialize, Serialize};

#[enum_dispatch]
trait Shaped {
    fn area(&self) -> f64;
}

#[enum_dispatch(Shaped)]
enum Shape {
    Square,
    Rectangle,
}


struct Square {
    side: f64,
}

impl Shaped for Square {
    fn area(&self) -> f64 {
        (self.side * self.side)
    }
}

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
    let rectangle: Shape = Rectangle { width: 32., height: 64. }.into();
    println!("Area of rectangle: {}", rectangle.area());
}
```