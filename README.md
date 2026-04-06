# Remote Telemetry Plugin

A SimHub plugin that lets endurance racing team members share live dashboards with each other. Only the active driver has real-time telemetry — this plugin lets teammates view that driver's SimHub dashboard remotely, on any device.

## How It Works

1. Each team member runs SimHub with its built-in web server
2. The plugin creates a WebSocket tunnel through a relay server to expose the local web server
3. Players create or join a session via the Remote Telemetry API
4. Teammates view the active driver's dashboard on their browser, monitor, or VoCore screen

## Features

### Session Management
- **Create or Join Sessions** — Create a team or join an existing one
- **Token-Based Identity** — Each player gets a persistent token per team, no passwords needed after first join
- **One-Click Connect** — Save your team name and connect with a single button
- **Active Teams List** — Browse and join other active teams on the server
- **Auto-Reconnect** — Automatically reconnects with exponential backoff if the connection drops
- **Graceful Shutdown** — Clean disconnection when SimHub closes

### Custom Server Support
- **Saved Servers** — Add multiple custom API servers with a dropdown selector
- **Server Validation** — Servers are validated (password + connectivity) before being saved
- **Quick Switch** — Switch between saved servers from the Connection tab
- **Per-Server Token Storage** — Tokens are saved independently for each API server
- **API Password Protection** — Custom servers support password-based access control

### VPS Provisioning (Create Your Own Server)
- **One-Click Install** — Set up your own relay server on any VPS automatically via SSH
- **SSH Key Management** — Generate and manage SSH keys directly from the plugin
- **Server Status** — Real-time health check showing API and Relay status
- **Delete Server** — Uninstall all services from the VPS with one click
- **Auto-Configuration** — Installs Go, API server, and WebSocket relay automatically
- **DNS Records Helper** — Shows the required DNS records with resolved IPs

### Race Engineer (Spectator Mode)
- **Spectator Page** — Web-based Race Engineer page at `/engineer` on the API server
- **Team Selector** — Browse active teams and connect with a token
- **Dashboard Grid** — View dashboards in 1, 2x1, 1x2, or 2x2 grid layouts
- **Multi-Monitor Support** — Launch dashboard windows on specific monitors using the Window Management API
- **Monitor Session Restore** — Automatically restore monitor windows from the last session
- **Auto-Connect** — Shareable URL with team and token for instant connection
- **Per-Cell Controls** — Navigate dashboards and trigger actions per grid cell
- **Fullscreen Mode** — Toggle fullscreen with a single button
- **Not Listed as Member** — Spectators observe without appearing in the team member list
- **Shareable Link** — Copy or open the Race Engineer URL directly from the plugin UI

### Dashboard Sharing
- **Dual Slot System** — Share up to two dashboards simultaneously
- **Dashboard Selection** — Choose which SimHub dashboards to share when you are driving
- **Automatic URL Sharing** — Tunnel URLs are shared with teammates via the API
- **Sharing Toggle** — Start or stop sharing telemetry without leaving the session
- **Button Bindings** — Map physical buttons or keys to control sharing from Settings
- **Viewer Count** — See how many people are watching your telemetry

### Display Options
- **Default Browser** — Opens the dashboard in your default browser
- **Monitor** — Opens fullscreen on a selected monitor
- **VoCore** — Injects the remote dashboard as an overlay on a VoCore screen
- **Dual Display** — Configure two display outputs simultaneously

### Tunneling
- **Pure WebSocket Tunnel** — No external dependencies (no cloudflared/frpc needed)
- **HTTPS with Token Security** — Tunnel access requires a valid token, validated via secure cookie
- **Wildcard Subdomains** — Each player gets a unique subdomain (`name.tunnel.domain.com`)

### Other
- **Auto-Update Check** — Checks for new versions on GitHub Releases
- **Dark Theme** — Matches SimHub's native look with SimHub controls
- **2K/4K Support** — Larger font sizes for high-DPI displays
- **Password Masking** — All password fields use masked input with Show/Hide toggle
- **Debug Logging** — Toggle debug logs from the Settings tab

## Plugin Tabs

### Connection
- API status indicator with latency
- Custom server selector with saved servers
- One-click connect to your team
- Browse and join other active teams
- Race Engineer link with Copy and Open buttons when connected

### Session
- Team members list with latency indicators
- Active driver display and selection
- Play/Stop controls for viewing remote dashboards
- Sharing toggle with viewer count
- Slot swap for teammates with two dashboards

### Settings
- Pilot name and team configuration
- Shared dashboard selection (dual slots)
- Display device selection (monitors + VoCore)
- Web server port configuration
- Tunnel status
- VPS server creation and management
- Debug logging

## Requirements

- [SimHub](https://www.simhubdash.com/) with the built-in web server enabled (default port 8888)
- Windows 10/11
- Internet connection

## Installation

1. Download the latest `RemoteTelemetryPlugin.dll` from [Releases](https://github.com/BrunoSilva1978PT/Remote-Telemetry-Plugin/releases)
2. Close SimHub
3. Copy the DLL to your SimHub installation folder (e.g. `C:\Program Files (x86)\SimHub\`)
4. Open SimHub
5. The plugin appears in the left menu as **Remote Telemetry**

## License

MIT License
