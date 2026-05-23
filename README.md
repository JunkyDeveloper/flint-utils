# Flint Utils

Utilities for working with Flint test projects.

This repository is a Rust workspace. It currently contains one binary:

- `flint-index`: verifies the Flint test index and rebuilds it when the indexed test files no longer match the current test directory.

## Requirements

- Rust toolchain with edition 2024 support
- Access to the `flint-core` Git dependency

## Usage

Run the index utility from the workspace root:

```sh
cargo run -p flint-index
```

`flint-index` does not take command-line arguments. It loads environment variables from the current shell and from a local `.env` file if one exists.

## Configuration

The following environment variables are read through `flint-core`:

| Variable | Default | Description |
| --- | --- | --- |
| `TEST_PATH` | `./test` | Test file or directory to scan. Directories are scanned recursively. |
| `INDEX_NAME` | `.cache/index.json` | Path to the generated index file. |
| `DEFAULT_TAG` | `default` | Tag assigned to test files that do not define tags. |

Example `.env`:

```env
TEST_PATH=./test
INDEX_NAME=.cache/index.json
DEFAULT_TAG=default
```

## What `flint-index` Does

1. Loads `.env` if present.
2. Reads `TEST_PATH`, falling back to `./test`.
3. Collects JSON test files recursively.
4. Loads the existing index from `INDEX_NAME` when valid.
5. Rebuilds the index when the current test file set does not match the stored hash.

The generated index groups test files by their `tags` field. Tests without tags are grouped under `DEFAULT_TAG`.

## Development

Build the workspace:

```sh
cargo build
```

Run tests:

```sh
cargo test
```

Format the code:

```sh
cargo fmt
```

## License

MIT. See [LICENSE](LICENSE).
