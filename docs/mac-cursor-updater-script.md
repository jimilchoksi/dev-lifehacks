# macOS Cursor auto-updater

**Repository:** [dev-lifehacks](https://github.com/jimilchoksi/dev-lifehacks)

Keeps the [Cursor](https://cursor.com) editor up to date on macOS using **Homebrew** (`cursor` cask) and a **LaunchAgent** (launchd). Updates run on a **daily schedule** (default **10:00** local time), with periodic checks so a missed run can **catch up** if the Mac was asleep or offline.

- **Per-user** — installs for the account that runs the installer (not system-wide)
- **Apple Silicon & Intel**
- **macOS 12+** (Homebrew cask requirement)
- **Pause or remove** the automation without uninstalling Cursor or Homebrew

**Installer source (gist):** [mac_install_cursor_updater.sh](https://gist.github.com/jimilchoksi/e21c23e16f08f59dbe1f13f2af701d60)

---

## Install (one command)

Run in **Terminal** as your normal user. Do **not** use `sudo`.

```bash
curl -fsSL https://gist.githubusercontent.com/jimilchoksi/e21c23e16f08f59dbe1f13f2af701d60/raw/mac_install_cursor_updater.sh \
  -o /tmp/mac_install_cursor_updater.sh \
  && bash /tmp/mac_install_cursor_updater.sh
```

When the menu appears, select **`1) Enable / Install`** and follow the prompts.

**Security tip:** You can review the script before running:

```bash
curl -fsSL https://gist.githubusercontent.com/jimilchoksi/e21c23e16f08f59dbe1f13f2af701d60/raw/mac_install_cursor_updater.sh \
  -o /tmp/mac_install_cursor_updater.sh
open -e /tmp/mac_install_cursor_updater.sh   # or: less /tmp/mac_install_cursor_updater.sh
bash /tmp/mac_install_cursor_updater.sh
```

---

## Interactive menu

| Option | Action |
|--------|--------|
| **1) Enable / Install** | Install or manage Cursor via Homebrew and register the daily update schedule |
| **2) Disable / Pause** | Stop background updates; keeps configuration files |
| **3) Delete / Remove** | Remove LaunchAgent, runner, logs, and state (**Cursor and Homebrew stay installed**) |
| **4) Exit** | Quit without changes |

Re-run the same `curl` command anytime to open the menu again.

---

## How updates work

1. A runner script lives under `~/Scripts/cursor-auto-updater/`.
2. launchd loads `~/Library/LaunchAgents/local.cursor.autoupdater.plist`.
3. The runner checks whether an update is **due** (once per local calendar day, after the configured time).
4. If the Mac was off at the scheduled time, a later periodic check can perform a **catch-up** update.
5. Activity is appended to `~/Library/Logs/cursor_update.log`.

Default schedule: **10:00** local time.

---

## Requirements

| Requirement | Details |
|-------------|---------|
| macOS | **12 or newer** |
| User | Normal login session — **not** `root` |
| Shell | **bash 3.2+** (macOS default bash is supported) |
| Homebrew | Installed or installed by the manager during setup |
| Network | Required when an update actually runs |

---

## Paths after installation

| Path | Purpose |
|------|---------|
| `~/Scripts/cursor-auto-updater/update_cursor.sh` | Update runner invoked by launchd |
| `~/Library/LaunchAgents/local.cursor.autoupdater.plist` | User LaunchAgent |
| `~/Library/Logs/cursor_update.log` | Log file |
| `~/Library/Application Support/CursorAutoUpdater/` | State, lock directory, install grace period |

**Legacy label:** Older installs may have used `com.jimilchoksi.cursor-autoupdater`; the installer handles migration toward `local.cursor.autoupdater`.

---

## Troubleshooting

### “Run this as your normal user, not root”

Run the installer without `sudo`. It only installs a LaunchAgent for **your** user.

### Homebrew not found

Install [Homebrew](https://brew.sh), then run **Enable / Install** again.

### Updates not happening

1. Read the log: `tail -50 ~/Library/Logs/cursor_update.log`
2. Confirm the agent is loaded (example):

   ```bash
   launchctl print "gui/$(id -u)/local.cursor.autoupdater" 2>/dev/null || echo "Agent not loaded"
   ```

3. Check for network errors in the log; the runner retries on later checks.

### Invalid menu choice

Enter **1**, **2**, **3**, or **4** only.

---

## Uninstall automation only

1. Run the installer (same `curl` command as install).
2. Choose **`3) Delete / Remove`**.
3. Confirm when prompted.

Cursor remains in `/Applications` (or `~/Applications`). Homebrew is unchanged.

---

## Related links

- [dev-lifehacks repository](https://github.com/jimilchoksi/dev-lifehacks)
- [Installer gist](https://gist.github.com/jimilchoksi/e21c23e16f08f59dbe1f13f2af701d60)
