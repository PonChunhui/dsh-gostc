# Changelog

All notable changes to the **dsh-gostc** DeepSeek Harness plugin are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/); versions follow [SemVer](https://semver.org/).

## [1.4.1] - 2026-11-26

### Changed
- Redesigned About page layout: a hero header (name + version badge + tagline + official
  link) with quick-action chips, then a responsive two-column card grid — guide / FAQ / tips
  on the left, self test / environment / contact / links on the right. Collapses to one
  column on narrow panels.

## [1.4.0] - 2026-11-26

### Added
- Official-site deep links on the About tab: sign up / sign in, get connection key,
  download client, manage tunnels (real routes of the gostc platform).
- One-click **self test**: probes the local backend health and platform reachability,
  shows green / red verdicts plus next-step guidance.
- **Copy diagnostics** button: plugin / backend version, phase, OS / arch, data dir,
  platform URL and timestamp, ready to paste into an Issue.
- Collapsible **FAQ** (cannot connect, tunnel not effective, where is the key, boot auto-start).
- **Changelog / Roadmap** links so users can see the plugin is maintained.
- Optional operator **contact channels** (email / QQ group / WeChat QR image URL),
  configurable in Settings and rendered on the About tab.

## [1.3.0] - 2026-11-26

### Added
- About tab: clickable official homepage, quick-start guide, docs / source links,
  contact & feedback, environment info and tips.
### Changed
- Default platform URL is now the official `https://gostc.linux66.cn` (still overridable).

## [1.2.0] - 2026-11-26

### Added
- In-panel **Settings** form on the Backend tab (platform URL, package, web listen
  address / auth, auto-start), persisted to `<dataDir>/settings.json` and overriding
  the cordis defaults — no hand-editing of YAML required.

## [1.1.0] - 2026-11-26

### Added
- Plugin-managed lifecycle: reuse a healthy backend, otherwise download the command-line
  client from your platform (OS + CPU-arch aware) and launch it as a managed child process
  with bounded auto-restart.
- Windows-service install / uninstall (UAC) for boot-time, plugin-independent operation.
- Backend process tab with start / stop / restart / download / update controls and log tail.

## [1.0.0] - 2026-11-25

### Added
- Initial release: same-origin proxy to a running gostc webui backend plus a global panel
  (clients / tunnels / P2P / logs) with create / edit / delete / start / stop.