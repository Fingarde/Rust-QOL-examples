# Serde
https://crates.io/crates/serde
https://serde.rs/attributes.html

## Serde language support
- JSON: https://crates.io/crates/serde_json
- YAML: https://crates.io/crates/serde_yml
- TOML: https://crates.io/crates/toml



## Example
```rust
use serde::{Serialize, Deserialize};
use serde_json;
use chrono::{DateTime, Local};

// Common struct
#[derive(Serialize, Deserialize)]
struct Event {
    date: DateTime<Local>,
    user: String,
    #[serde(flatten)]
    data: EventKind
}


#[derive(Serialize, Deserialize)]
#[serde(tag = "type", content = "event")]
#[serde(rename_all = "SCREAMING_SNAKE_CASE")]
enum EventKind {
    Apt(AptEvent),
    Power(PowerEvent),
    Button(ButtonEvent)
}


// APT EVENT DECLARATION
#[derive(Serialize, Deserialize)]
#[serde(tag = "action", content = "value")]
#[serde(rename_all = "SCREAMING_SNAKE_CASE")]
enum AptEvent {
    Update,
    Install {
        package: String
    },
    MultipleInstall {
        packages: Vec<String>
    }
}


// POWER EVENT DECLARATION
#[derive(Serialize, Deserialize)]
struct PowerEvent {
    state: PowerState
}

#[derive(Serialize, Deserialize)]
#[serde(rename_all = "SCREAMING_SNAKE_CASE")]
enum PowerState {
    On, OFF
}

// APT EVENT DECLARATION
#[derive(Serialize, Deserialize)]
#[serde(tag = "action", content = "value")]
#[serde(rename_all = "SCREAMING_SNAKE_CASE")]
enum ButtonEvent {
    Press (u8),
    Release (u8)
}

```


```rust
// Example usage
fn main() {
    {
        let data = r#"
        {
            "date": "2024-09-23T21:25:55.165925716Z",
            "user": "tibertra",
            "type": "BUTTON",
            "event": {
                "action": "PRESS",
                "value": 1
            }
        }"#;

        // Parse the string of data into serde_json::Value.
        let v = serde_json::from_str::<Event>(data);
        if v.is_err() {
            println!("{:?}", v.as_ref().err());
        }
        
        let value = v.unwrap();
        println!("{}", serde_json::to_string_pretty(&value).unwrap());
    }


    {
        let obj = Event {
            date: Local::now(),
            user: "tibertra".to_string(),
            data : EventKind::Apt(
                AptEvent::Install {
                    package: "python".to_string()
                }
                
            )
        };
        
        let str = serde_json::to_string(&obj).unwrap();
        println!("{}", serde_json::to_string_pretty(&obj).unwrap());
    }
    
    {
        let obj = Event {
            date: Local::now(),
            user: "tibertra".to_string(),
            data : EventKind::Power(
                PowerEvent {
                    state: PowerState::On
                }
                
            )
        };
        
        let str = serde_json::to_string(&obj).unwrap();
        println!("{}", serde_json::to_string_pretty(&obj).unwrap());
    }
}
```