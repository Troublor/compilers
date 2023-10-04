# foundry-compilers

Utilities for working with native `solc` and compiling projects.

To also compile contracts during `cargo build` (so that ethers `abigen!` can pull in updated abi automatically) you can configure a `foundry_compilers::Project` in your `build.rs` file

First add `ethers-solc` to your cargo build-dependencies.

Once you compiled the project, you can configure cargo change detection with `rerun_if_sources_changed`, so that cargo will execute the `build.rs` file if a contract in the sources directory has changed

```toml
[build-dependencies]
foundry-compilers = { git = "https://github.com/foundry-rs/compilers" }
```

```rust
use foundry_compilers::{Project, ProjectPathsConfig};

fn main() {
    // configure the project with all its paths, solc, cache etc.
    let project = Project::builder()
        .paths(ProjectPathsConfig::hardhat(env!("CARGO_MANIFEST_DIR")).unwrap())
        .build()
        .unwrap();
    let output = project.compile().unwrap();

    // Tell Cargo that if a source file changes, to rerun this build script.
    project.rerun_if_sources_changed();
}
```
