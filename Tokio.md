```rust
use std::time::Duration;
use reqwest::Response;
use tokio::select;
use tokio::time::sleep;

enum Error {
    Timeout,
}

async fn fetch(url: &str, timeout: Option<u64>) -> Result<Response, Error> {
    let timeout = timeout.unwrap_or(500);

    let res = select! {
        v = reqwest::get(url) => v.unwrap(),
        _ = sleep(Duration::from_millis(timeout)) => return Err(Error::Timeout),
    };

    Ok(res)
}

#[tokio::main]
async fn main() {
    let res = fetch("https://www.rust-lang.org", None).await;

    match res {
        Ok(_) => println!("Success"),
        Err(Error::Timeout) => println!("Timeout"),
    }

    let res = fetch("https://www.rust-lang.org", Some(10)).await;

    match res {
        Ok(_) => println!("Success"),
        Err(Error::Timeout) => println!("Timeout"),
    }
}
```