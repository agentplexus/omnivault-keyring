# Release Notes: v0.1.0

**Release Date:** 2025-12-27

Initial release of OmniVault Keyring provider - cross-platform OS credential store integration.

## Highlights

- Cross-platform OS credential store provider for OmniVault
- Native integration with macOS Keychain, Windows Credential Manager, and Linux Secret Service

## Features

- Cross-platform keyring provider implementing `vault.Vault` interface
- Support for macOS Keychain, Windows Credential Manager, and Linux Secret Service (GNOME Keyring/KWallet)
- Multi-field secrets support with JSON format option
- Internal index for List() functionality (OS keyrings don't support enumeration)
- Configurable service name for secret namespacing
- URI resolution support via `keyring://` scheme
- OnIndexError callback for handling non-fatal index operation errors

## Supported Platforms

| Platform | Backend | Storage Location |
|----------|---------|------------------|
| macOS | Keychain | `~/Library/Keychains/login.keychain-db` |
| Windows | Credential Manager | Windows Credential Locker |
| Linux | Secret Service API | GNOME Keyring or KWallet |

## Installation

```bash
go get github.com/agentplexus/omnivault-keyring@v0.1.0
```

## Quick Start

```go
package main

import (
    "context"
    "fmt"
    "log"

    keyring "github.com/agentplexus/omnivault-keyring"
    "github.com/agentplexus/omnivault/vault"
)

func main() {
    ctx := context.Background()

    // Create keyring provider
    kr := keyring.New(keyring.Config{
        ServiceName: "myapp",
    })

    // Store a secret
    err := kr.Set(ctx, "api-key", &vault.Secret{Value: "secret123"})
    if err != nil {
        log.Fatal(err)
    }

    // Retrieve the secret
    secret, err := kr.Get(ctx, "api-key")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("API Key:", secret.Value)
}
```

## Multi-Field Secrets

```go
// Enable JSON format for multi-field support
kr := keyring.New(keyring.Config{
    ServiceName: "myapp",
    JSONFormat:  true,
})

// Store database credentials
err := kr.Set(ctx, "database/prod", &vault.Secret{
    Value: "super-secret-password",
    Fields: map[string]string{
        "username": "db_admin",
        "host":     "db.example.com",
        "port":     "5432",
    },
})

// Retrieve and access fields
secret, _ := kr.Get(ctx, "database/prod")
fmt.Printf("Host: %s\n", secret.Fields["host"])
fmt.Printf("User: %s\n", secret.Fields["username"])
```

## With OmniVault Resolver

```go
resolver := omnivault.NewResolver()
resolver.Register("keyring", keyring.New(keyring.Config{
    ServiceName: "myapp",
}))

// Resolve secrets using URI syntax
dbPassword, _ := resolver.Resolve(ctx, "keyring://database/password")
```

## Requirements

- Go 1.21 or later
- Platform-specific requirements:
  - **Linux**: gnome-keyring or KWallet with libsecret

## Contributors

- [@plexusone](https://github.com/plexusone)

## Links

- [Documentation](https://pkg.go.dev/github.com/agentplexus/omnivault-keyring)
- [Source Code](https://github.com/agentplexus/omnivault-keyring)
- [OmniVault](https://github.com/agentplexus/omnivault)
- [zalando/go-keyring](https://github.com/zalando/go-keyring) - Underlying keyring library
