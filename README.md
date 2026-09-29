# dev-lifehacks

**Page:** [github.com/jimilchoksi/dev-lifehacks](https://github.com/jimilchoksi/dev-lifehacks)

Practical **developer lifehacks**—small scripts and automations that save time across everyday machines and servers. Not limited to one OS: expect helpers for **macOS**, **Linux**, **Windows**, **servers**, CI/ops, and general tooling as the collection grows.

Each script aims to be:

- Documented and easy to install
- Idempotent where possible (safe to re-run)
- Easy to pause or remove without breaking the apps it manages

## Scripts

| Script | Platforms | Description | Documentation |
|--------|-----------|-------------|---------------|
| **Cursor auto-updater** | macOS | Daily Cursor updates via Homebrew + LaunchAgent (default 10:00 local). | [docs/mac-cursor-updater-script.md](docs/mac-cursor-updater-script.md) |

*More scripts for other OS and server workflows will be listed here as they are added.*

## Quick start — Cursor updater (macOS)

Run in Terminal as your normal user (not `sudo`):

```bash
curl -fsSL https://gist.githubusercontent.com/jimilchoksi/e21c23e16f08f59dbe1f13f2af701d60/raw/mac_install_cursor_updater.sh \
  -o /tmp/mac_install_cursor_updater.sh \
  && bash /tmp/mac_install_cursor_updater.sh
```

Choose **`1) Enable / Install`** in the menu.

## Layout

```text
dev-lifehacks/
├── README.md
├── LICENSE
├── docs/           # how-to guides per script
└── scripts/        # installers / utilities (as they are added)
```

## Contributing

Issues and PRs welcome. Prefer clear docs, one concern per script, and install/remove flows that don’t uninstall unrelated software.

## Author

[@jimilchoksi](https://github.com/jimilchoksi)

## License

[MIT](LICENSE)
