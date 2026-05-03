# Release Notes: v0.2.0

**Release Date:** 2026-03-01

Module migrated to new GitHub organization with updated dependencies.

## Highlights

- Module path migrated from agentplexus to plexusone organization

## Breaking Changes

- **Module Path Changed**: Import path changed from `github.com/agentplexus/omnivault-keyring` to `github.com/plexusone/omnivault-keyring`

## Upgrade Guide

### 1. Update Import Paths

Replace all imports in your Go files:

```go
// Before
import keyring "github.com/agentplexus/omnivault-keyring"

// After
import keyring "github.com/plexusone/omnivault-keyring"
```

### 2. Update go.mod

```bash
# Remove old module
go mod edit -droprequire github.com/agentplexus/omnivault-keyring

# Add new module
go get github.com/plexusone/omnivault-keyring@v0.2.0
```

### 3. Tidy Dependencies

```bash
go mod tidy
```

## Changes

### Dependencies

| Module | Change |
|--------|--------|
| `github.com/plexusone/omnivault` | Updated to v0.3.0 |

### Infrastructure

- Migrate to standard plexusone reusable workflows
- Update documentation URLs to plexusone organization

### Documentation

- Add emoji icons to README Features section

## Installation

```bash
go get github.com/plexusone/omnivault-keyring@v0.2.0
```

## Quick Start

```go
package main

import (
    "context"
    "fmt"
    "log"

    keyring "github.com/plexusone/omnivault-keyring"
    "github.com/plexusone/omnivault/vault"
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

## Supported Platforms

| Platform | Backend |
|----------|---------|
| macOS | Keychain |
| Windows | Credential Manager |
| Linux | Secret Service (GNOME Keyring, KWallet) |

## Contributors

- [@plexusone](https://github.com/plexusone)

## Links

- [Documentation](https://pkg.go.dev/github.com/plexusone/omnivault-keyring)
- [Source Code](https://github.com/plexusone/omnivault-keyring)
- [Changelog](https://github.com/plexusone/omnivault-keyring/blob/main/CHANGELOG.md)
- [OmniVault](https://github.com/plexusone/omnivault)
