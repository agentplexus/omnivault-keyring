# Omni-Keyring

[![Go CI][go-ci-svg]][go-ci-url]
[![Go Lint][go-lint-svg]][go-lint-url]
[![Go SAST][go-sast-svg]][go-sast-url]
[![Go Report Card][goreport-svg]][goreport-url]
[![Docs][docs-godoc-svg]][docs-godoc-url]
[![License][license-svg]][license-url]

 [go-ci-svg]: https://github.com/plexusone/omni-keyring/actions/workflows/go-ci.yaml/badge.svg?branch=main
 [go-ci-url]: https://github.com/plexusone/omni-keyring/actions/workflows/go-ci.yaml
 [go-lint-svg]: https://github.com/plexusone/omni-keyring/actions/workflows/go-lint.yaml/badge.svg?branch=main
 [go-lint-url]: https://github.com/plexusone/omni-keyring/actions/workflows/go-lint.yaml
 [go-sast-svg]: https://github.com/plexusone/omni-keyring/actions/workflows/go-sast-codeql.yaml/badge.svg?branch=main
 [go-sast-url]: https://github.com/plexusone/omni-keyring/actions/workflows/go-sast-codeql.yaml
 [goreport-svg]: https://goreportcard.com/badge/github.com/plexusone/omni-keyring
 [goreport-url]: https://goreportcard.com/report/github.com/plexusone/omni-keyring
 [docs-godoc-svg]: https://pkg.go.dev/badge/github.com/plexusone/omni-keyring
 [docs-godoc-url]: https://pkg.go.dev/github.com/plexusone/omni-keyring
 [license-svg]: https://img.shields.io/badge/license-MIT-blue.svg
 [license-url]: https://github.com/plexusone/omni-keyring/blob/main/LICENSE

OS keyring provider packages for [PlexusOne](https://github.com/plexusone) libraries.

## Modules

This repository contains Go modules for OS credential store integrations:

| Module | Description | Install |
|--------|-------------|---------|
| [`omnivault`](omnivault/) | OS keyring provider for [omnivault](https://github.com/plexusone/omnivault) | `go get github.com/plexusone/omni-keyring/omnivault` |

## Supported Platforms

- **macOS**: Keychain
- **Windows**: Credential Manager
- **Linux**: Secret Service (GNOME Keyring, KWallet)

## Quick Start

### OmniVault - OS Keyring Provider

```go
import (
    keyring "github.com/plexusone/omni-keyring/omnivault"
)

// Create keyring provider
kr := keyring.New(keyring.Config{
    ServiceName: "myapp",
})

// Store a secret
err := kr.Set(ctx, "api-key", &vault.Secret{Value: "secret123"})

// Retrieve a secret
secret, err := kr.Get(ctx, "api-key")
fmt.Println("API Key:", secret.Value)
```

See [omnivault/README.md](omnivault/README.md) for full documentation including multi-field secrets and OmniVault integration.

## License

MIT
