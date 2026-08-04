# Cleotic CLI

Command line tools for Cleotic.

The 0.3 release adds built-in updates, making it easier to keep Cleotic
current from your terminal. You can still use the CLI interactively, in
scripts, or through an AI agent to manage projects, brands, monitors,
and prompts.

## Install

Install the latest release:

```sh
curl -fsSL https://raw.githubusercontent.com/generaldataworks/cleotic-cli/main/install.sh | sh
```

Install a specific release:

```sh
curl -fsSL https://raw.githubusercontent.com/generaldataworks/cleotic-cli/main/install.sh | CLEOTIC_VERSION=v0.3.0 sh
```

Install to a different directory (default is `$HOME/.local/bin`):

```sh
curl -fsSL https://raw.githubusercontent.com/generaldataworks/cleotic-cli/main/install.sh | CLEOTIC_INSTALL_DIR="$HOME/bin" sh
```

## Keep Cleotic up to date

Run the update command whenever you want Cleotic to check for a newer
release:

```sh
cleotic update
```

If an update is available, Cleotic shows the new version and asks whether
you want to install it. You can also choose a mode that suits how you are
using the CLI:

```sh
# Check without installing anything
cleotic update --check

# Install an available update without a confirmation prompt
cleotic update --yes

# Get a machine-readable check result for a script or agent
cleotic update --check --json

# Install non-interactively and return a machine-readable result
cleotic update --yes --json --no-input
```

During normal interactive use, Cleotic also checks occasionally and prints
a short notice when a new version is available. These background checks do
not run with `--json`, `--no-input`, piped output, or in CI. Set
`CLEOTIC_NO_UPDATE_CHECK=1` if you prefer to turn them off entirely.

## Log in

```sh
cleotic auth login
```

Login opens your browser (or prints a URL and one-time code to enter on
another device) and signs you in as your Cleotic user. Tokens are stored
in your OS keychain and refreshed automatically. If you belong to
multiple organizations you'll be asked to pick one; `--org <id>`
preselects it. `cleotic auth status` shows the active session and
`cleotic auth logout` removes the local tokens.

If your organization still needs to accept Cleotic's Terms and
Conditions, the CLI opens the web app, waits for you to accept, and
continues on its own.

For CI and automation, use an API key created on the Cleotic API-keys
settings page (read-only or read-write) and provide it through the
environment:

```sh
CLEOTIC_API_KEY=... cleotic projects list --json
```

## Common Workflows

```sh
# First-time setup: create a project, brand, and monitor in one go
cleotic setup

# Read
cleotic projects list
cleotic projects show <project-id>
cleotic projects summary <project-id>
cleotic brands list --project <project-id>
cleotic monitors list --project <project-id>
cleotic prompts list --monitor <monitor-id>

# Write
cleotic projects create --name "Acme"
cleotic projects use <project-id>
cleotic brands create --name "Acme" --domain acme.com --primary
cleotic monitors create --name "Acme AI visibility"
cleotic prompts create --monitor <monitor-id> --text "best crm for smb"
cleotic prompts run --monitor <monitor-id> --prompt <prompt-id>

# Delete (asks for confirmation; --yes skips it)
cleotic prompts delete <prompt-id> --monitor <monitor-id>
cleotic monitors delete <monitor-id> --yes
cleotic brands delete <brand-id> --project <project-id>
cleotic projects delete <project-id>
```

## Scripting

The CLI holds a firm machine contract across every command:

- Every command supports `--json`: exactly one JSON document on stdout,
  with progress and prompts kept on stderr, so piping into `jq` just
  works.
- Exit codes are stable: `0` success, `1` the operation failed, `2` you
  got the usage wrong.
- Deletes are safe by default: they ask for confirmation in a terminal,
  and with `--no-input` (or no terminal) a delete without `--yes` fails
  with exit code `2` instead of prompting.

## Configuration

Settings are resolved flag → environment → config file, first match
wins:

| Source | Key | Purpose |
|---|---|---|
| flag | `--api-url` | Cleotic API URL for this invocation |
| env | `CLEOTIC_API_URL` | Cleotic API URL |
| env | `CLEOTIC_API_KEY` | API-key auth for CI/automation; overrides any browser login |
| config | `api_url` | Persistent API URL (`cleotic config set api-url <url>`) |
| config | `default_project_id` | Default project (`cleotic projects use <id>`) |

The config file lives at `~/.config/cleotic/config.yaml` (honoring
`XDG_CONFIG_HOME`). Browser-login tokens are never written there — they
stay in the OS keychain.

## Release Assets

Release binaries are published as macOS and Linux tarballs for `arm64`
and `x86_64`.

The release asset filenames include the CLI version:

```text
cleotic_0.3.0_mac-os_arm64.tar.gz
cleotic_0.3.0_mac-os_x86_64.tar.gz
cleotic_0.3.0_linux_arm64.tar.gz
cleotic_0.3.0_linux_x86_64.tar.gz
```

Each archive has a matching `.sha256` checksum file.

## Support

For help, contact Cleotic support or your Cleotic account team.
