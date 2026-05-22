# Cleotic CLI

Command line tools for Cleotic.

The 0.1 release is read-only. It lets you authenticate with a scoped CLI API key, inspect projects, view project summaries, and list brands, monitors, and prompts from your terminal.

## Install

Install the latest release:

```sh
curl -fsSL https://raw.githubusercontent.com/generaldataworks/cleotic-cli/main/install.sh | sh
```

Install a specific release:

```sh
curl -fsSL https://raw.githubusercontent.com/generaldataworks/cleotic-cli/main/install.sh | CLEOTIC_VERSION=v0.1.0 sh
```

Install to a user-writable directory:

```sh
curl -fsSL https://raw.githubusercontent.com/generaldataworks/cleotic-cli/main/install.sh | CLEOTIC_INSTALL_DIR="$HOME/.local/bin" sh
```

## Authenticate

```sh
cleotic auth login
```

Browser login creates a read-only CLI API key for your Cleotic organization and stores it in your OS keychain.

For automation, provide a CLI API key through the environment:

```sh
CLEOTIC_API_KEY=... cleotic projects list --json
```

## Common Commands

```sh
cleotic projects list
cleotic projects show <project-id>
cleotic projects summary <project-id>
cleotic brands list --project <project-id>
cleotic monitors list --project <project-id>
cleotic monitors show <monitor-id>
cleotic prompts list --monitor <monitor-id>
```

Every command in the 0.1 release is read-only and supports `--json`.

## Release Assets

Release binaries are published as macOS and Linux tarballs for `arm64` and `x86_64`.

The release asset filenames include the CLI version:

```text
cleotic_0.1.0_mac-os_arm64.tar.gz
cleotic_0.1.0_mac-os_x86_64.tar.gz
cleotic_0.1.0_linux_arm64.tar.gz
cleotic_0.1.0_linux_x86_64.tar.gz
```

Each archive has a matching `.sha256` checksum file.

## Support

For help, contact Cleotic support or your Cleotic account team.
