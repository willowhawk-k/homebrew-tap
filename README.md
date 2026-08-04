# willowhawk-k/homebrew-tap

Homebrew packages for [Keith Willowhawk](https://github.com/willowhawk-k)'s software.

```bash
brew tap willowhawk-k/tap
```

You only need to tap once — anything published here afterwards is a plain
`brew install` away.

## What's here

| Package | Install | What it is |
|---------|---------|------------|
| **netlights** (cask) | `brew install --cask netlights` | The NetLights app — a live, layered map of your network interfaces — plus the `netlights` command. |
| **netlights-cli** (formula) | `brew install netlights-cli` | Just the `netlights` command, for people who keep the **Mac App Store** build of the app (which Homebrew can't manage). |

Install **one or the other**, not both — they each provide a `netlights` executable
and are declared as conflicting.

### Which one do I want?

Most people want the cask. Take `netlights-cli` only if you already have NetLights
from the App Store and want the command line without switching.

|  | Cask (`netlights`) | Formula (`netlights-cli`) |
|--|--------------------|---------------------------|
| Installs the app | yes | no — uses the app you already have |
| `netlights tui` | yes | yes |
| `netlights serve` | yes | **no** — the App Store build is sandboxed without the incoming-connections entitlement, so it cannot listen on a socket |
| Updates | `brew upgrade` | App Store, for the app itself |

The cask refuses to install over a Mac App Store copy of the app, because both
live at `/Applications/NetLights.app`. It explains the trade-offs and stops rather
than deleting an app Homebrew didn't install.

## Usage

```bash
netlights            # open the app
netlights tui        # live terminal dashboard, top-style
netlights serve      # the web UI at http://127.0.0.1:8765
netlights --help
```

`serve` listens on loopback only by default. It has no authentication and publishes
your interfaces, addresses, routes and DNS servers, so exposing it to the network is
an explicit `--bind all`. See
[PRIVACY.md](https://github.com/willowhawk-k/NetLights/blob/main/PRIVACY.md).

## Maintenance

Both files are generated from the NetLights repo, which is the source of truth —
`Casks/netlights.rb` and `Formula/netlights-cli.rb` there. On each release, bump
**both** `version` + `sha256` and copy them here:

```bash
shasum -a 256 dist/NetLights-<version>.zip                                   # cask
curl -sL .../archive/refs/tags/v<version>.tar.gz | shasum -a 256             # formula
brew style Casks/netlights.rb Formula/netlights-cli.rb                       # lint
```
