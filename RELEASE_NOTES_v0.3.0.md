# Release Notes: v0.3.0

**Release Date:** 2026-05-03

Module renamed to `omni-keyring` and restructured as a monorepo following the omni-<provider> pattern.

## Highlights

- Renamed to `omni-keyring` following the omni-<provider> pattern (like omni-aws, omni-onepassword)
- Restructured as monorepo with `omnivault/` subdirectory

## Breaking Changes

- **Module Path Changed**: `github.com/plexusone/omnivault-keyring` → `github.com/plexusone/omni-keyring`
- **Import Path Changed**: Provider now at `github.com/plexusone/omni-keyring/omnivault`

## Upgrade Guide

### 1. Update Import Paths

Replace all imports in your Go files:

```go
// Before
import keyring "github.com/plexusone/omnivault-keyring"

// After
import keyring "github.com/plexusone/omni-keyring/omnivault"
```

### 2. Update go.mod

```bash
# Remove old module
go mod edit -droprequire github.com/plexusone/omnivault-keyring

# Add new module
go get github.com/plexusone/omni-keyring/omnivault@v0.3.0
```

### 3. Tidy Dependencies

```bash
go mod tidy
```

## Changes

### Structure

The module now follows the omni-<provider> monorepo pattern:

```
omni-keyring/
├── README.md           # Root README with quick start
├── go.mod              # Single module at root
├── omnivault/
│   ├── README.md       # Detailed provider documentation
│   ├── keyring.go      # Provider implementation
│   └── examples/       # Usage examples
└── CHANGELOG.md
```

### Documentation

- New root README.md following omni-aws pattern
- Updated omnivault/README.md with new import paths and related projects

## Installation

```bash
go get github.com/plexusone/omni-keyring/omnivault@v0.3.0
```

## Quick Start

```go
package main

import (
    "context"
    "fmt"
    "log"

    keyring "github.com/plexusone/omni-keyring/omnivault"
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

- [Documentation](https://pkg.go.dev/github.com/plexusone/omni-keyring/omnivault)
- [Source Code](https://github.com/plexusone/omni-keyring)
- [Changelog](https://github.com/plexusone/omni-keyring/blob/main/CHANGELOG.md)
- [OmniVault](https://github.com/plexusone/omnivault)
- [omni-aws](https://github.com/plexusone/omni-aws) - AWS providers
- [omni-onepassword](https://github.com/plexusone/omni-onepassword) - 1Password provider
