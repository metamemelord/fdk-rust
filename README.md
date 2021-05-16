# FDK: Fn Function Development Kit

<a href="https://crates.io/crates/fdk"><img src="https://img.shields.io/crates/v/fdk.svg" alt="fdk’s current version badge" title="fdk’s current version badge" /></a>

The API provided hides the implementation details of the Fn platform
contract and allows a user to focus on the code and easily implement
function-as-a-service programs.

# Usage

The Fn platform offers a
[command line tool](https://github.com/fnproject/fn/blob/master/README.md#quickstart)
to initialize, build and deploy function projects. Follow the `fn` tool
quickstart to learn the basics of the Fn platform. You will need to use docker as
runtime.

The initializer will actually use cargo and generate a cargo binary project
for the function. It is then possible to specify a dependency as usual.

```toml
[dependencies]
fdk = "1.0.0"
```

# Examples

This is a simple function which greets the name provided as input.

```rust
use fdk::{Function, FunctionError, RuntimeContext};
use tokio; // Tokio for handling future.

#[tokio::main]
async fn main() -> Result<(), FunctionError> {
    if let Err(e) = Function::run(|_: &mut RuntimeContext, i: String| {
        Ok(format!(
            "Hello {}!",
            if i.is_empty() {
                "world"
            } else {
                i.trim_end_matches("\n")
            }
        ))
    })
    .await
    {
        eprintln!("{}", e);
    }
    Ok(())
}
```
