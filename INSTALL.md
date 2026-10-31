# Install the Waev web companion

The Waev web companion is the openHop Repeater plugin `waev.companion`. The
install needs no configuration: nothing goes in the plugin's `config.json`.

- [Prerequisites](#prerequisites)
- [Install](#install)
- [Check it](#check-it)
- [Upgrade](#upgrade)
- [Reinstall the same version](#reinstall-the-same-version)
- [Move from `waev.push`](#move-from-waevpush)
- [Bluetooth handhelds](#bluetooth-handhelds)
- [Uninstall](#uninstall)
- [Troubleshooting](#troubleshooting)

## Prerequisites

- **openHop Repeater 1.1.2 or later**, the first with the plugin manager.
  Supported hosts:
  - Raspberry Pi OS 64-bit (arm64);
  - Raspberry Pi OS 32-bit (armhf);
  - Debian x64 or arm64 in an LXC container or Docker.

  Luckfox and other Buildroot images have no plugin manager. If your
  repeater's interface has no **Plugins** page, upgrade the repeater first.
- **Python 3.10 or later**, as for openHop. PyPI must be reachable while the
  wheel installs: the plugin manager fetches `httpx` and `cryptography`.
- **Port 8792** reachable on your network. Browsers reach the Waev engine
  there.
- **A console on plain HTTP.** A page served over HTTPS, including through a
  reverse proxy, cannot open the engine's `ws://` connection.
- **Port 8791** reachable from the phones, only if you turn push on.

## Install

1. **Download** `waev_companion-<version>-py3-none-any.whl` from the
   [latest release](https://github.com/Treehouse-00/waev-companion/releases/latest).
   Check it against the release's `SHA256SUMS`, in the folder holding both:

   ```sh
   sha256sum -c --ignore-missing SHA256SUMS          # Linux
   shasum -a 256 -c --ignore-missing SHA256SUMS      # macOS
   ```

2. **Install.** In the repeater's built-in interface, open **System →
   Plugins**, choose **Install plugin wheel**, pick the `.whl`, then
   **Enable** `waev.companion`. The plugin manager creates the plugin's venv,
   fetches its dependencies from PyPI, extracts the UI and seeds
   `data/config.json` from the defaults.
3. **Open Waev** with the plugin's **Open UI** action, or at
   `http://<repeater>:8000/plugins/waev.companion/`.
4. **Sign in** with the repeater's console password. On the first sign-in the
   page makes the engine's own API key, named `waev.companion`, with your
   session and hands it to the engine once. **Settings › Connection › Engine
   key** then reads **Present**.
5. **Choose a companion** on **Settings › Connection**:
   - one of **Companions on this repeater**, confirmed with **Use**. A
     companion another app holds shows **In use**, and Waev stays read-only
     until that app lets go;
   - **Create a companion**. The confirmation names the new MeshCore
     identity. It gets its own key and sends no advert until you send one;
   - a handheld radio, **Over the network** or **Over Bluetooth** (see
     [Bluetooth handhelds](#bluetooth-handhelds)).

   Nothing is chosen for you, not even the only companion.

## Check it

`GET /api/plugins/runtime?id=waev.companion`, or the plugin's
`data/runtime.json`, should show:

- `"engine": "running"`;
- `"bridgePort": 8792`;
- `"apiToken": "present"`;
- `"testBuild": false`.

The plugin's **Logs** action shows its logs.

## Upgrade

Install the new wheel the same way (**Install plugin wheel**), then
**Restart** the plugin. A newer version replaces the launcher and the engine.
The plugin stays enabled, and the running process keeps the old release until
it restarts.

Everything under `data/` stays: the engine's history, its key, the companion
choice (`link.json`) and push. Moving from a test build works the same way.

## Reinstall the same version

Installing a wheel whose version is already installed changes only the UI
files. To reinstall the same version and keep your data:

1. In **Plugins**, **Disable** then **Uninstall** `waev.companion`. Leave
   **Also delete persistent data directory** unticked. The repeater removes
   the release and its venv, and keeps `data/` and `logs/`.
2. Install the wheel again, **Enable** it, and open Waev. The engine's
   history, its key, `link.json` and push are where they were.

## Move from `waev.push`

Push is part of `waev.companion` and is off by default. A guard keeps the two
plugins from sending one push twice.

1. Install and set up `waev.companion` as above. While `waev.push` is
   enabled, **Settings › This repeater › Push for Waev apps** says **Blocked:
   Waev Push is on**. While the engine key is missing it says **Blocked:
   can't check for Waev Push**.
2. In **Plugins**, **Disable** `waev.push`. Do not uninstall it yet.
3. In Waev, on **Push for Waev apps**, choose **Import from Waev Push**. It
   copies the old database and its keys, then verifies the copy. It never
   changes the old plugin's data. Phones keep the same port and secret, so
   they keep working without registering again.
4. Choose Apple and Android. The platforms that have registered devices are
   already selected.
5. Check that a phone receives a push. Then **Uninstall** `waev.push`. Tick
   **Also delete persistent data directory** only when you no longer want its
   data.

To go back, turn both switches off in Waev first, then enable `waev.push`
again.

## Bluetooth handhelds

- BlueZ 5.66 or later. Raspberry Pi OS Bookworm ships it; check with
  `bluetoothctl --version`.
- The plugin's service user in the `bluetooth` group, which Debian's D-Bus
  policy requires:

  ```sh
  sudo usermod -aG bluetooth repeater
  sudo systemctl restart openhop-plugin-manager
  ```

  A group change reaches only processes started after it.

Pairing keeps its bond in BlueZ, not in Waev. Removing the plugin leaves the
bond; remove it with `bluetoothctl remove <address>`.

## Uninstall

- In **Plugins**, **Disable** then **Uninstall** `waev.companion`, or call
  `DELETE /api/plugins/waev.companion`. This keeps `data/` and `logs/`: the
  config, the engine's history, its key, `link.json` and push's database.
- To remove everything, tick **Also delete persistent data directory**, or add
  `?delete_data=true`.
- Revoke the engine's key in **System → Configuration → Access → API Tokens**
  (`waev.companion`). Waev revokes a key only when it replaces one it made
  itself, so uninstalling leaves the key in place.
- A companion created through Waev is an openHop companion. It stays in the
  repeater's configuration until you remove it there.

## Troubleshooting

| What you see | What to check |
|---|---|
| The page loads but never connects | Port 8792 must be reachable from your browser, and the console must be on plain HTTP. |
| The install fails while fetching packages | The repeater must reach PyPI during the install. |
| **Engine key** reads missing | Sign in again: the page makes the key with your session. |
| A companion shows **In use** | Another app holds it. Waev stays read-only until that app lets go. |
| **Over Bluetooth** cannot connect | Check the BlueZ version and the `bluetooth` group above, then restart the plugin manager. |
| No **Plugins** page | The repeater predates the plugin manager. Upgrade the repeater first. |

Still stuck? Open an [issue](https://github.com/Treehouse-00/waev-companion/issues)
with the plugin version, your repeater's version and the plugin's logs.
