# Minecraft Server Configuration Repository

A comprehensive configuration repository for deploying Minecraft servers using Docker and the [itzg/minecraft-server](https://github.com/itzg/docker-minecraft-server) Docker images. This repository provides templates and compose files for various Minecraft server platforms including Vanilla, Fabric, Forge, NeoForge, CurseForge, Modrinth, and FTB.

> **Note:** This repository contains configuration files and templates only. Server management commands can be found in the [EPX](https://github.com/energypatrikhu/epx) repository.

## Overview

This repository is structured to support multiple Minecraft server instances with different configurations and platforms. It uses Docker Compose for container orchestration and provides a templating system for easy server deployment and configuration.

## Features

- **Multi-Platform Support**: Templates for Vanilla, Fabric, Forge, NeoForge, CurseForge, Modrinth, and FTB servers
- **Automated Backups**: Integrated backup solution using itzg/mc-backup
- **Mod Management**: Support for automatic mod downloads from CurseForge and Modrinth
- **Docker-Based**: Containerized deployment for easy management and isolation
- **Templating System**: Pre-configured templates for quick server deployment
- **Flexible Configuration**: Environment-based configuration for different server types

## Docker Compose Services

### Minecraft Server (`itzg-mc.yml`)

The main Minecraft server service with the following features:

- **Container Name**: `mc-${SERVER_DIR}-server`
- **Image**: `itzg/minecraft-server:java${JAVA_VERSION}`
- **Graceful Shutdown**: 30-second stop grace period
- **Auto-Restart**: Unless manually stopped
- **Port Mapping**: Configurable server port (default: 25565)

**Volumes:**
- Server data: `servers/${SERVER_DIR}/data`
- Extras, configs, mods, and plugins directories
- Time synchronization with host

**Secrets:**
- CurseForge API key
- Ops list
- Whitelist

### Backup Service (`itzg-mc-backup.yml`)

Automated backup service for Minecraft servers:

- **Container Name**: `mc-${SERVER_DIR}-backup`
- **Image**: `itzg/mc-backup`
- **Read-Only Access**: Server data mounted as read-only

### Network Configuration (`itzg-config.yml`)

- **Network**: `mc-${SERVER_DIR}-network` with IPv6 support
- **Secrets Management**: Centralized secrets configuration

## Configuration Templates

### Platform Templates

Each platform template includes version-specific configuration:

#### Vanilla
```bash
MEMORY = 6G
JAVA_VERSION = 17
VERSION = 1.12.2
```

#### Fabric
```bash
LOADER_VERSION = 0.16.14
LAUNCHER_VERSION = 1.0.3
MEMORY = 6G
JAVA_VERSION = 21
VERSION = 1.21.5
```

### Server Properties Template

Default server configuration includes:
- **Difficulty**: Hard
- **Game Mode**: Survival
- **Max Players**: 8
- **PVP**: Disabled
- **View Distance**: 8
- **Simulation Distance**: 8
- **Online Mode**: Enabled
- **Network Compression**: 256 threshold

### Backup Configuration

Default backup settings:
- **Interval**: 3 hours
- **Initial Delay**: 15 minutes
- **Retention Count**: 24 backups
- **Default Location**: `../backups`

## Supported Platforms

| Platform | Type | Description |
|----------|------|-------------|
| **Vanilla** | Official | Standard Minecraft server |
| **Fabric** | Modded | Lightweight modding framework |
| **Forge** | Modded | Popular modding platform |
| **NeoForge** | Modded | Modern Forge fork |
| **CurseForge** | Modpack | CurseForge modpack support |
| **Modrinth** | Modpack | Modrinth modpack support |
| **FTB** | Modpack | Feed The Beast modpacks |

## Environment Variables

Key environment variables used across services:

| Variable | Description | Example |
|----------|-------------|---------|
| `SERVER_DIR` | Server instance directory name | `my-server` |
| `SERVER_TYPE` | Platform type | `fabric`, `vanilla`, etc. |
| `SERVER_PORT` | Server port | `25565` |
| `JAVA_VERSION` | Java version for the container | `17`, `21` |
| `VERSION` | Minecraft version | `1.21.5`, `LATEST` |
| `MEMORY` | Server memory allocation | `6G` |
| `BACKUP_DIR` | Backup storage location | `../backups` |

## Secrets Management

The repository uses Docker secrets for sensitive data:

- **curseforge_api_key.txt**: CurseForge API key for mod downloads
- **ops.txt**: Server operators list (per-server)
- **whitelist.txt**: Whitelisted players (per-server)
- **mods.curseforge.txt**: CurseForge mod IDs (per-server)
- **mods.modrinth.txt**: Modrinth mod IDs (per-server)

## Mod Management

### CurseForge Mods
List mod IDs or file IDs in `mods.curseforge.txt`:
```
# One mod ID per line
123456
789012
```

### Modrinth Mods
List mod IDs or file IDs in `mods.modrinth.txt`:
```
# One mod ID per line
fabric-api
sodium
```

## Usage

> **Important:** Server management commands are not included in this repository. Refer to the EPX repository for deployment and management commands.

This repository serves as a configuration and template source for Minecraft server deployments. The actual server instances are created in the `servers/` directory (which is gitignored).

## Server Instance Structure

Each server instance in `servers/${SERVER_DIR}/` follows this structure:
```
servers/my-server/
├── data/              # Main server data (world, configs)
├── extras/
│   ├── data/
│   ├── config/
│   ├── mods/
│   └── plugins/
├── ops.txt
├── whitelist.txt
├── mods.curseforge.txt
└── mods.modrinth.txt
```

## Networking

- Each server instance gets its own Docker network: `mc-${SERVER_DIR}-network`
- IPv6 is enabled by default
- Port mapping is configurable per server instance

## Time Synchronization

Servers automatically sync with the host system time:
- `/etc/localtime` (read-only)
- `/etc/timezone` (read-only)

## References

- [itzg/minecraft-server Documentation](https://docker-minecraft-server.readthedocs.io/)
- [itzg/mc-backup Documentation](https://github.com/itzg/docker-mc-backup)
- [Fabric Server Setup](https://fabricmc.net/use/server/)

## License

Configuration templates and compose files for Minecraft server deployment.

---

**Note**: This is a configuration repository. For server management commands and deployment automation, please refer to the EPX repository.
