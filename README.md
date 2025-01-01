# Goose local AI agent

A local AI agent with a Rust CLI/server, a desktop interface, configurable model providers and tool integrations.

## Architecture and stack

`crates/goose` contains agent/provider/tool behavior; `crates/goose-cli` supplies the CLI; other Rust crates implement server and integrations; `ui/desktop` contains the desktop application; `workflow_recipes/` holds reusable task recipes.

## Requirements

Rust 1.99.0 is pinned in `rust-toolchain.toml`; the workspace minimum is 1.94.1. Native dependencies include OpenSSL, DBus, Clang and protobuf tooling for the Linux Docker build. Desktop development additionally needs its Node/pnpm environment.

## Installation and local development

```sh
cargo build --release -p goose-cli --bin goose
./target/release/goose --help
```

The root Dockerfile builds the CLI in a separate builder stage. Its Rust base image follows the repository toolchain rather than the obsolete 1.82 image.

## Configuration

Configure a supported model provider through Goose, supplying credentials locally. Recipes and tool extensions can run commands; review them before execution. `CUSTOM_DISTROS.md` describes custom distribution configuration.

## Testing

`cargo test -p goose` targets the agent crate; full workspace tests require native dependencies and may need service credentials. `just test-buzz` runs the separate GitHub work-management Node tests without building Rust.

## Deployment and integrations

Use the root Dockerfile for the CLI image and `BUILDING_LINUX.md` / `BUILDING_DOCKER.md` for platform details. Persist provider/configuration state separately from images; do not embed credentials in a build.

## Source provenance and license

This repository imports source from [aaif-goose/goose](https://github.com/aaif-goose/goose). Original product functionality belongs to its upstream contributors. The local history begins with a November 2026 source snapshot; it does not retain upstream development history or prove the exact upstream revision. GitHub reports the upstream repository was created on 2024-08-23; that is repository-origin evidence, not the date of this contribution. Existing year folders are retained as local organization.

Preserve `LICENSE` and existing third-party notices when redistributing. Missing root license text was recovered from the upstream repository during this maintenance pass; existing license files were preserved.

## Validation status

Setup and test commands above were checked against this checkout’s manifests, scripts and entry points. Full dependency installation, application builds and integration tests were not performed in this maintenance pass. The Rust toolchain/native dependencies and provider-backed integration services were not provisioned. These instructions identify the supported workflow without claiming a successful product build.

Targeted maintenance verification: `node --test buzz/*.test.mjs` passed all 10 tests; `docker manifest inspect rust:1.99.0-bookworm` confirmed the builder tag exists for Linux amd64 and arm64. The full Docker/Rust build was not run.

See [source provenance](SOURCE_PROVENANCE.md) for pinned upstream license retrieval evidence and redistribution limitations.
