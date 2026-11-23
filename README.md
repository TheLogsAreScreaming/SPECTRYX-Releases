# SPECTRYX Releases

Downloads, release notes and issue tracker for **SPECTRYX**, a self-hosted, private dashboard for your
Meshtastic mesh. SPECTRYX connects to a Meshtastic radio over USB serial, Wi‑Fi (TCP) or Bluetooth and
gives you a web dashboard with live nodes, packets, maps, messages, scheduled tasks and 33 bundled
plugins, all running on your own machine.

**Current version: 1.0.0.** Get it from the [Releases](https://github.com/TheLogsAreScreaming/SPECTRYX-Releases/releases) page.

Each release attaches:

| File | What it is |
|---|---|
| `SPECTRYX-mac.zip` | The macOS app (Intel and Apple Silicon) |
| `spectryx-<version>.zip` | The app source tree, for Linux, Raspberry Pi, WSL2 and Docker |
| `SHA256SUMS` | Checksums for the files above |

## Install

### macOS (app)

1. Download **SPECTRYX‑mac.zip**, unzip it and drag **SPECTRYX** to Applications.
2. Open it. The app is not notarized yet: the first time, right‑click it and choose **Open**.
3. The first launch sets everything up (a few minutes), then shows the setup wizard where you create
   your admin login and pick your radio.

- Needs macOS 13 or newer and Python 3.10 or newer ([python.org](https://www.python.org/downloads/) or
  Homebrew).
- Your data and settings live in `~/Library/Application Support/SPECTRYX`.
- The dashboard listens on `127.0.0.1:8181` only. Closing the window hides it; **Quit** (⌘Q) stops it.
- To update, replace the app with the newer one. Your data, settings and plugin choices are kept.

### Linux / Raspberry Pi / WSL2 (from the source zip)

```bash
sudo apt install -y python3 python3-venv python3-pip build-essential python3-dev libxml2-dev libxslt-dev unzip
unzip spectryx-1.0.0.zip -d spectryx && cd spectryx
python3 -m venv venv
./venv/bin/pip install -r requirements.txt
./venv/bin/python meshtastic_dashboard.py
```

Then open `http://<this-machine>:8181/` and follow the setup wizard, which creates your admin login. For USB radios, add your user to the `dialout` group (`sudo usermod -aG dialout $USER`, then log
out and back in).

### Docker

From the unzipped source folder:

```bash
docker build -t spectryx .
docker run -d --name spectryx --restart unless-stopped --network host --privileged \
  -v /dev:/dev -v spectryx_data:/app/data spectryx
```

Then open `http://localhost:8181/`. USB passthrough works on Linux hosts only.

## Updates

SPECTRYX checks this repository's latest release shortly after it starts and then every 12 hours, and
tells you when a newer version is out. Source and Docker installs can install it from the dashboard:
the download is checked against `SHA256SUMS`, and the old version is put back by itself if the new one
fails to start. Mac users replace the app.

## Privacy

- No telemetry, no heartbeat, no relay. SPECTRYX does not report your radio, nodes, messages or usage to
  anyone.
- The update check is an anonymous request for this repository's latest release. It carries no node ID,
  key or version.
- The web page loads some libraries and fonts from public CDNs and map tiles from OpenStreetMap, CARTO
  or Esri. Place names on the Overview come from OpenStreetMap Nominatim.
- Features you turn on yourself (weather, translation, notifications, hosted AI models, MQTT and so on)
  talk only to the service they name.

## Issues and security

- Bugs and requests: [Issues](https://github.com/TheLogsAreScreaming/SPECTRYX-Releases/issues).
- Security problems: please don't open a public issue. Use **Security → Report a vulnerability** on
  this repository.

## Credits and license

SPECTRYX is a rebranded fork of **[MeshDash](https://meshdash.co.uk)** by **Rusjp**, based on MeshDash
R3.1.14 and released under the GNU General Public License v3.0 (**GPL‑3.0‑only**). The bundled plugins
are from the MeshDash project. Each source zip includes the license, the NOTICE file listing what
changed, and the changelogs.

SPECTRYX is an independent project. It is not affiliated with or endorsed by the MeshDash project or by
Meshtastic LLC. *Meshtastic®* is a registered trademark of Meshtastic LLC.
