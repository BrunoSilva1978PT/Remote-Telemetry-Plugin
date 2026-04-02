# Remote Telemetry Plugin for SimHub

A SimHub plugin that lets endurance racing team members share live dashboards with each other. When one driver is on track, teammates can view their SimHub dashboard remotely in real-time.

## How It Works

1. Each team member runs SimHub with this plugin installed
2. The plugin creates a secure tunnel (via Cloudflare) to expose your SimHub web server
3. Team members connect to the same session using a team name and password
4. Select a teammate to watch their live dashboard on your monitor or VoCore display

The plugin handles everything automatically: tunnel creation, URL sharing, and session management.

## Features

- **Team Sessions** -- Create or join a team session with a name and password
- **Live Dashboard Sharing** -- View any teammate's SimHub dashboard in real-time
- **Dual Slot Support** -- Share up to 2 dashboards simultaneously (e.g., pit wall + timing)
- **Multiple Display Options** -- View dashboards on a monitor (fullscreen) or VoCore screen
- **Automatic Tunneling** -- Cloudflare tunnels handle connectivity, no port forwarding needed
- **Self-Hosted Fallback** -- Host sessions locally if the central server is unavailable
- **Session Discovery** -- Browse active teams and join with one click
- **Persistent Settings** -- Team name, password, and display preferences are saved

## Requirements

- [SimHub](https://www.simhubdash.com/) (with web server enabled on port 8888)
- Windows 10 or later
- Internet connection

## Installation

1. Download `RemoteTelemetryPlugin.dll` from the [Releases](https://github.com/BrunoSilva1978PT/Remote-Telemetry-Plugin/releases) page
2. Copy the DLL to your SimHub installation folder (default: `C:\Program Files (x86)\SimHub\`)
3. Restart SimHub
4. The plugin appears in the left menu as **Remote Telemetry**

## First Time Setup

1. Open SimHub and go to **Remote Telemetry** in the left menu
2. Go to the **Settings** tab:
   - Set your **Pilot Name** (identifies you in sessions)
   - Set your **Team Name** and **Password**
   - Choose which **dashboard** to share (Slot 1, optionally Slot 2)
   - Choose your **display device** for viewing teammates (monitor or VoCore)
3. Go to the **Settings** tab and click **Install Cloudflared** (one-time setup)
4. Go to the **Connection** tab and click **Create Team** or **Join Team**

## Usage

### Creating a Session (Team Leader)

1. Go to the **Connection** tab
2. Your team name and password are pre-filled from Settings
3. Click **Create Team**
4. Share the team name and password with your teammates

### Joining a Session (Teammates)

1. Go to the **Connection** tab
2. Active teams appear in the list -- click **Refresh** to update
3. Select your team from the list
4. Enter the password and click **Join Team**

### Watching a Teammate

1. Go to the **Session** tab after connecting
2. You'll see all connected team members
3. Click **Watch** next to the driver you want to view
4. Their dashboard opens on your configured display
5. Click **Stop** to close the remote view

### Offline Mode (Central Server Unavailable)

If the central server is down, you have three fallback options:

- **Host Team** -- Run the API locally and share it via tunnel
- **Browse Registry** -- Find locally-hosted teams automatically
- **Manual URL** -- Connect directly to a known API URL

## Plugin Tabs

| Tab | Purpose |
|-----|---------|
| **Connection** | Create, join, or host team sessions |
| **Session** | View connected members, watch/stop remote dashboards |
| **Settings** | Configure pilot name, team credentials, dashboards, display devices, cloudflared, and local API |

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Cloudflared not installed | Go to Settings tab and click **Install Cloudflared** |
| Can't see active teams | Click **Refresh** on the Connection tab |
| Teammate's dashboard not loading | Ensure they have SimHub's web server enabled (port 8888) |
| Team name already taken | Change the team name in Settings or on the Connection tab |
| Central API offline | Use **Host Team** to run a local session |

## License

MIT

## Author

Bruno Silva
