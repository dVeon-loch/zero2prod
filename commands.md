### Build speedup
Using cargo-watch:

install:

`cargo install --locked cargo-watch` <- Workaround for current issue with cargo-watch deps

`cargo watch -x check -x test -x run`

### Testing:

Run tests: `cargo test`

Calculate code coverage not taking into account test code:

`cargo tarpaulin --ignore-tests`

### Linting

Default linter maintained by Rust is clippy. CI environments often don't have it. Install using:

`rustup component add clippy`

Run on project using:

`cargo clippy`

In our CI pipeline we would like to fail the linter check if clippy emits any warnings.
We can achieve it with:
`cargo clippy -- -D warnings`

Static analysis is not infallible: from time to time clippy might suggest changes that you do not believe to
be either correct or desirable.
You can mute a warning using the `#[allow(clippy::lint_name)]` attribute on the affected code block or
9Yes, clippy is named after the (in)famous paperclip-shaped Microsoft Word assistant.
disable the noisy lint altogether for the whole project with a configuration line in `clippy.toml` or a projectlevel
`#![allow(clippy::lint_name)]` directive.
Details on the available lints and how to tune them for your specific purposes can be found in clippy’s
README.


### Formatting

Use `rustfmt`. If not installed:

`rustup component add rustfmt`

To run:

`cargo fmt`

Add to CI Pipeline:

`cargo fmt -- --check`

It will fail when a commit contains unformatted code, printing the difference to the console.

### Security Vulnerabilities
cargo makes it very easy to leverage existing crates in the ecosystem to solve the problem at hand.
On the flip side, each of those crates might hide an exploitable vulnerability that could compromise the
security posture of your software.
The Rust Secure Code working group maintains an Advisory Database - an up-to-date collection of reported
vulnerabilities for crates published on crates.io.

It can be annoying to get a fail in CI for a formatting issue. Most IDEs support a “format on save” feature to make the process
smoother. Alternatively, you can use a git pre-push hook.

They also provide `cargo-audit`, a convenient cargo sub-command to check if vulnerabilities have been
reported for any of the crates in the dependency tree of your project.

You can install it with:

`cargo install cargo-audit`

Once installed, run:

`cargo audit`