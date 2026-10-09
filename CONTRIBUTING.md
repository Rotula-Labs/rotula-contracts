# Contributing to Rotula Contracts

Thanks for your interest in improving the Rotula Soroban contracts. Open an issue
before a larger change so the work can be discussed first.

## Development setup

The contract lives in `contracts/` and is a Cargo package. From the repository
root:

```bash
rustup target add wasm32v1-none
cd contracts
cargo test
cargo build --target wasm32v1-none --release
```

## Pre-commit hooks

`.pre-commit-config.yaml` defines local hooks that run the same checks as CI.
Install [pre-commit](https://pre-commit.com) and enable them:

```bash
pip install pre-commit
pre-commit install
pre-commit run --all-files
```

The configured hooks are:

- `cargo-fmt` — `cargo fmt --manifest-path contracts/Cargo.toml --all -- --check`
- `cargo-clippy` — `cargo clippy --manifest-path contracts/Cargo.toml --all-targets -- -D warnings`

Keep both clean before opening a pull request.

## Testing

Contract changes should include tests for successful behavior and relevant
failures: authorization, invalid amounts, membership changes, cycle boundaries,
pause/recovery, and token-transfer edge cases. Run the full suite from
`contracts/`:

```bash
cargo test
```

Snapshot tests under `contracts/test_snapshots/` are produced by `cargo test`;
commit any snapshots your change legitimately updates.

## Deployment

`contracts/deploy.sh` builds, optimizes, and deploys the contract. It reads
`STELLAR_SOURCE_ACCOUNT` (default `deployer`) and `STELLAR_NETWORK` (default
`testnet`):

```bash
STELLAR_SOURCE_ACCOUNT=<identity> STELLAR_NETWORK=testnet ./contracts/deploy.sh
```

Never commit secret keys or seed phrases.

## License

By contributing you agree that your contributions are licensed under the MIT
License (see `LICENSE`).
