# Remote Telemetry Plugin

A SimHub plugin that lets endurance racing team members share live dashboards with each other. Only the active driver has real-time telemetry — this plugin lets teammates view that driver's SimHub dashboard remotely, on any device.

## How It Works

1. Each team member runs SimHub with its built-in web server
2. The plugin creates a WebSocket tunnel through a relay server to expose the local web server
3. Players create or join a session via the Remote Telemetry API
4. Teammates view the active driver's dashboard on their browser, monitor, or VoCore screen

## Features

### Session Management
- **Create or Join Sessions** — Create a team with password protection, or join an existing one
- **One-Click Connect** — Save your team name and password, connect with a single button
- **Active Teams List** — Browse and join other active teams on the server
- **Auto-Reconnect** — Automatically reconnects if the connection drops
- **Heartbeat** — Keeps the connection alive with periodic heartbeat messages

### Custom Server Support
- **Saved Servers** — Add multiple custom API servers with a dropdown selector
- **Server Validation** — Servers are validated (password + connectivity) before being saved
- **Quick Switch** — Switch between saved servers from the Connection tab
- **API Password Protection** — Custom servers support password-based access control

### VPS Provisioning (Create Your Own Server)
- **One-Click Install** — Set up your own relay server on any VPS automatically via SSH
- **SSH Key Management** — Generate and manage SSH keys directly from the plugin
- **Server Status** — Real-time health check showing API and Relay status (green/orange/red)
- **Delete Server** — Uninstall all services from the VPS and clear settings with confirmation
- **Auto-Configuration** — Installs Go, Caddy (HTTPS), API server, and WebSocket relay automatically
- **DNS Records Helper** — Shows the required DNS records with resolved IPs

### Dashboard Sharing
- **Dual Slot System** — Share up to two dashboards simultaneously (Slot 1 always on, Slot 2 optional)
- **Dashboard Selection** — Choose which SimHub dashboards to share when you are driving
- **Automatic URL Sharing** — Tunnel URLs are shared with teammates via the API

### Display Options
- **Default Browser** — Opens the dashboard in your default browser
- **Monitor** — Opens fullscreen on a selected monitor
- **VoCore** — Injects the remote dashboard as an overlay on a VoCore screen via CefSharp JS injection
- **Dual Display** — Configure two display outputs simultaneously

### Tunneling
- **Pure WebSocket Tunnel** — No external dependencies (no cloudflared/frpc needed)
- **Auto-Install** — Tunnel client is installed automatically
- **Wildcard Subdomains** — Each player gets a unique subdomain (`name.tunnel.domain.com`)

### UI
- **Three Tabs** — Connection, Session, and Settings
- **Dark Theme** — Matches SimHub's native look with SimHub controls
- **2K/4K Support** — Larger font sizes for high-DPI displays
- **Password Masking** — All password fields use masked input with Show/Hide toggle
- **Themed Modals** — Custom dark-themed confirmation dialogs

## Plugin UI

### Connection Tab
- API status indicator with latency (ping)
- Custom server selector (dropdown with saved servers, add/remove)
- One-click connect to your team
- Browse and join other active teams
- Session info with copy buttons when connected

### Session Tab
- Team members list with latency indicators
- Active driver display
- Slot swap between dashboards
- Play/Stop controls for viewing remote dashboards

### Settings Tab
- Pilot name and team configuration
- Shared dashboard selection (dual slots)
- Display device selection (dual slots with monitors + VoCore)
- Web server port configuration
- Tunnel status and installation
- VPS server creation (SSH, provisioning, DNS)
- Debug logging toggle

## Requirements

- [SimHub](https://www.simhubdash.com/) with the built-in web server enabled (default port 8888)
- Windows 10/11
- Internet connection
- Chromium-based default browser (Chrome, Edge, Brave, etc.) for monitor display mode

## Building

### Prerequisites

- Visual Studio 2022 (Community, Professional, or Enterprise) or Build Tools
- .NET Framework 4.8

### Build

Run `build.bat` from the repository root. It will:

1. Auto-detect MSBuild
2. Copy required SimHub DLLs to `lib/` if not present
3. Build the project in Release mode
4. Copy the output DLL to the SimHub installation folder

```
build.bat
```

Output: `src/RemoteTelemetryPlugin/bin/Release/RemoteTelemetryPlugin.dll`

### Manual Installation

1. Close SimHub
2. Copy `RemoteTelemetryPlugin.dll` to `C:\Program Files (x86)\SimHub\`
3. Open SimHub
4. The plugin appears in the left menu as **Remote Telemetry**

## Architecture

### Infrastructure
- **API Server** (Go) — Session management, team creation, WebSocket coordination
- **Relay Server** (Go) — WebSocket multiplexed tunnel for HTTP traffic on port 80, plugin connections on port 9000
- **Caddy** — HTTPS reverse proxy for the API domain
- **Wildcard DNS** — `*.tunnel.domain.com` points to the VPS for tunnel subdomains

### API Endpoints
- `POST /teams` — Create a team (returns team name)
- `GET /teams` — List active teams
- `GET /ws/{teamName}?name=X&password=Y&token=Z` — WebSocket connection
- `GET /health` — Health check (supports password validation)
- All endpoints protected by `X-API-Password` header when `API_PASSWORD` env var is set

### WebSocket Messages
- **Client → Server:** `tunnel_url`, `set_driver`, `heartbeat`, `latency`
- **Server → Client:** `members` (member list with active driver and tunnel URLs)

## Project Structure

```
src/RemoteTelemetryPlugin/
├── TelemetryPlugin.cs              # Plugin entry point
├── PluginLog.cs                    # File logger
├── Models/
│   ├── ConnectionMode.cs           # Display connection mode enum
│   ├── RecentSession.cs            # Recent session data
│   ├── SavedServer.cs              # Saved custom server (URL + password)
│   ├── SessionInfo.cs              # Session list item
│   ├── TeamMember.cs               # Team member data model
│   └── VoCoreDevice.cs             # VoCore device data model
├── Network/
│   ├── SessionApiClient.cs         # WebSocket API client with password auth
│   ├── TunnelManager.cs            # WebSocket tunnel manager
│   ├── ProcessHelper.cs            # Process execution helper
│   └── VpsProvisioner.cs           # SSH-based VPS provisioning
├── Settings/
│   └── PluginSettings.cs           # Persistent plugin settings
├── UI/
│   ├── SettingsControl.xaml(.cs)   # Tab host shell + welcome modal
│   └── Tabs/
│       ├── ConnectionTab.xaml(.cs) # Custom servers, create/join session
│       ├── SessionTab.xaml(.cs)    # Members, active driver, play/stop
│       └── SettingsTab.xaml(.cs)   # Configuration + VPS provisioning
└── Vocore/
    └── VoCoreManager.cs            # VoCore device detection & JS injection
```

## License

MIT License
