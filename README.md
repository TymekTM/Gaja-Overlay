# Gaja Overlay

**Gaja Overlay** is an optional visual component within the [GAJA Assistant](https://github.com/TymekTM/Gaja) ecosystem, a self-hosted, modular AI home assistant emphasizing privacy, control, and extensibility.

Gaja Overlay provides a transparent, fullscreen visual feedback interface with real-time status updates, animated ball indicator, and response display for voice interactions.

> **⚠️ Currently Under Reconstruction**: This component is actively being redesigned and may not work properly in its current state. The overlay is completely optional - GAJA Client works perfectly without it.

## Table of Contents

- [What is Gaja Overlay?](#what-is-gaja-overlay)
- [Current Status](#current-status)
- [Architecture](#architecture)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Development](#development)
- [Troubleshooting](#troubleshooting)

## What is Gaja Overlay?

Gaja Overlay is a **fullscreen transparent overlay** designed to provide visual feedback during voice interactions with the GAJA Assistant. Built with Rust/Tauri and React, it displays:

- **Animated Status Ball**: Visual indicator for listening, speaking, and wake word detection
- **Real-time Status Updates**: Connection with GAJA Client via HTTP/SSE
- **Response Display**: Text responses with dynamic font sizing
- **Transparent Design**: Non-intrusive fullscreen overlay

## Current Status

### ⚠️ Reconstruction Phase

The overlay is currently being rebuilt and may experience:
- Connection issues with GAJA Client
- UI rendering problems
- Incomplete functionality
- Configuration conflicts

### Working Features
- ✓ Fullscreen transparent overlay
- ✓ Animated Gaja ball indicator
- ✓ Status text display
- ✓ Dynamic response rendering
- ✓ Basic Tauri/React architecture

### Known Issues
- ❌ Inconsistent connection to GAJA Client
- ❌ Configuration management needs update  
- ❌ Auto-hide logic may not work properly
- ❌ Build process may require manual intervention

## Architecture

### Technology Stack

- **Backend**: Rust with Tauri framework
- **Frontend**: React 18 with Vite
- **Communication**: HTTP polling and SSE (Server-Sent Events)
- **Rendering**: Transparent fullscreen overlay with CSS animations

### Components

- **Main Rust Application** (`src/main.rs`): Tauri backend handling window management and client communication
- **React Frontend** (`src/App.jsx`): UI components for status display and animations
- **Status Polling**: Connects to GAJA Client on ports 5000/5001 for real-time updates
- **Window Management**: Fullscreen transparent overlay with click-through support

### Communication Flow

1. **Client Connection**: Overlay connects to GAJA Client HTTP API (`localhost:5000/5001`)
2. **Status Updates**: Real-time status via polling or SSE stream (`/status/stream`)
3. **Visual Feedback**: React components render based on received status data
4. **Auto-hide Logic**: Window visibility managed by Rust backend

## Requirements

### System Requirements

- **Windows 10/11** (primary target)
- **Rust 1.70+** (for building from source)
- **Node.js 18+** (for building from source)
- **GAJA Client running** (for functionality)

### Dependencies

```toml
# Cargo.toml
[dependencies]
tauri = "1.8.3"
tokio = "1.0"
reqwest = "0.11"
serde = "1.0"
serde_json = "1.0"
```

```json
// package.json
{
  "dependencies": {
    "@tauri-apps/api": "^1.5.6",
    "react": "^18.3.1",
    "react-dom": "^18.3.1"
  }
}
```

## Installation

### Build from Source

```bash
# Clone repository
git clone https://github.com/TymekTM/Gaja-Overlay.git
cd Gaja-Overlay

# Install Node.js dependencies
npm install

# Build for development
npm run tauri dev

# Build for production  
npm run tauri build
```

### Prerequisites

1. **Install Rust**: https://rustup.rs/
2. **Install Node.js**: https://nodejs.org/
3. **GAJA Client**: Must be running for overlay functionality

## Configuration

### Tauri Configuration (`tauri.conf.json`)

```json
{
  "tauri": {
    "windows": [
      {
        "fullscreen": false,
        "height": 1080,
        "width": 1920,
        "decorations": false,
        "transparent": true,
        "alwaysOnTop": true,
        "skipTaskbar": true,
        "focus": false,
        "visible": true
      }
    ]
  }
}
```

### Client Connection Settings

The overlay attempts to connect to GAJA Client on:
- **Primary**: `http://localhost:5001`
- **Fallback**: `http://localhost:5000`

### Visual Settings

- **Fullscreen**: 1920x1080 transparent overlay
- **Position**: Centered content with top alignment
- **Animation**: Smooth transitions for status changes
- **Auto-hide**: 30-second timeout for inactivity

## Development

### Development Setup

```bash
# Clone and setup
git clone https://github.com/TymekTM/Gaja-Overlay.git
cd Gaja-Overlay

# Install dependencies
npm install

# Start development mode
npm run tauri dev
```

### Key Files

- **`src/main.rs`**: Tauri backend with window management and client communication
- **`src/App.jsx`**: React frontend with status display and animations
- **`src/style.css`**: CSS with fullscreen overlay styling and animations
- **`tauri.conf.json`**: Window configuration and build settings

### Testing with GAJA Client

1. Start GAJA Client: `python start.py`
2. Verify client is running on port 5001
3. Start overlay: `npm run tauri dev`
4. Check connection in overlay logs

## Troubleshooting

### Common Issues

#### 1. Overlay Not Connecting to Client
```bash
# Check if GAJA Client is running
curl http://localhost:5001/api/status
curl http://localhost:5000/api/status

# Check overlay logs for connection attempts
# Look for "[Rust] Testing connection to CLIENT port" messages
```

#### 2. Build Failures
```bash
# Update Rust toolchain
rustup update

# Clean build cache
cargo clean
npm run tauri build

# Install missing dependencies
npm install
```

#### 3. Transparent Window Issues
- Ensure Windows 10/11 with desktop composition enabled
- Check if other overlays are interfering
- Verify Tauri window configuration

#### 4. CSS Animation Problems
- Clear browser cache if using dev mode
- Check console for CSS import errors
- Verify `style.css` is properly loaded

### Debug Mode

Enable debug logging in development:

```bash
# Development mode shows detailed logs
npm run tauri dev

# Check Rust backend logs for connection status
# Check React frontend logs in dev tools (F12)
```

