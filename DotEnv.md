# Dotenvy & Envy

dotenvy: https://crates.io/crates/dotenvy
envy: https://crates.io/crates/envy

# Example
```rust
use dotenvy::dotenv_override;
use envy::from_env;
use serde::{Deserialize, Serialize};


#[derive(Deserialize, Debug)]
struct Config {
    api_url: String,
    sso_url: String,
    sso_client_id: String,
    sso_client_secret: String,
    username: String,
    password: String
}

fn main() -> Result<()> {
    dotenv_override().ok();
    let config: Config = from_env().unwrap();
    println!("{:?}", config);
}
```

# EnvLogger

env_logger: https://crates.io/crates/env_logger


.env
```env
RUST_LOG=info,my_target=trace
```

```rust
use dotenvy::dotenv_override;
use log::{trace, debug, info, warn, error};
use env_logger::{init_from_env, Env};


fn main() -> Result<()> {
    dotenv_override().ok();
    init_from_env(Env::default().default_filter_or("info"));
    
    trace!("This is a trace message");
    debug!("This is a debug message");
    info!("This is an info message");
    warn!("This is a warning message");
    error!("This is an error message");
    
    trace!(target: "my_target", "This is a trace message");
    
    Ok(())
}
```