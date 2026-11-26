# dsh-gostc

A [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) plugin that
brings the **gostc** intranet-penetration client into the Harness web client.

gostc ([SianHH/gostc-open](https://github.com/SianHH/gostc-open), Apache-2.0) is an
frp-based intranet-penetration management platform. This plugin adds a global
**Gostc** panel to the Harness sidebar and lets the plugin provision and run the
gostc command-line client for you.

## Features

- **Management panel** for gostc client connections, private tunnels and P2P
  links: create / edit / delete / start / stop, plus runtime logs.
- **Automatic client provisioning**: detects the host **OS and CPU architecture**
  (Windows / Linux / macOS / FreeBSD, Windows WoW64 aware) and downloads the
  matching command-line installer from your own gostc platform's public download
  API, validating the executable magic (PE / ELF / Mach-O) before an atomic write.
- **Process lifecycle**: reuses an already-healthy backend, otherwise launches a
  plugin-managed child process with bounded auto-restart; a **Backend** tab offers
  start / stop / restart, client download / update, and Windows-service
  install / uninstall (UAC).
- **Same-origin proxy**: the Host half forwards gostc webui API calls server-side,
  so no CORS support or rebuild of gostc is required.

The actual tunneling is performed by the gostc process; this plugin manages it
through the gostc webui HTTP API and controls the process lifecycle. The gostc
binary is downloaded at runtime from your platform — it is **not** bundled.

## Install

From the Harness plugin market, or:

```sh
dsh plugin add dsh-gostc
```

## Configuration

Edit the plugin row's `config` (Plugins config page or `cordis.patch.yml`):

| Key | Default | Meaning |
| --- | --- | --- |
| `backend` | `http://127.0.0.1:18080` | gostc webui backend to probe / proxy. |
| `username` / `password` | `` | Optional backend HTTP Basic auth. |
| `platformUrl` | `` | Your gostc platform base URL used to download the CLI installer. Empty disables auto-download. |
| `clientPackage` | `` | Exact installer filename; empty auto-picks the package matching your OS/arch. |
| `dataDir` | `` | Install / data directory; empty defaults to `%LOCALAPPDATA%/dsh-gostc`. |
| `webAddr` | `127.0.0.1:18080` | webui listen address used when launching the client. |
| `webUser` / `webPwd` | `` | Optional webui Basic auth set on the launched client. |
| `autoStart` | `true` | Auto-provision and launch on host activation (or reuse a healthy backend). |

See [docs/DETAIL.md](docs/DETAIL.md) for the provisioning flow and full details.

## Screenshots

<!-- Add real panel captures to assets/ and list them in screenshots.json. -->
Screenshots are declared in `screenshots.json` (see `screenshots.example.json`).

## License

MIT — see [LICENSE](LICENSE). gostc itself is Apache-2.0 (see [NOTICE](NOTICE));
this plugin does not include gostc's code.