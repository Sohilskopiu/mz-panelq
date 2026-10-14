# MagarZ Cloud

MagarZ Cloud is a self-hosted control panel rebranded as MagarZ Cloud and expanded with web hosting, Discord bot templates, and Lavalink v4 provisioning alongside Minecraft server management.

## Features

- Minecraft server lifecycle management, console, file manager, backups, server settings, plugin/mod tools, player management, and node support retained from the base project.
- Node.js and Python web-app hosting with editable service files and startup commands.
- Discord bot templates for Node.js (`discord.js`) and Python (`discord.py`).
- Lavalink v4 Docker node with a generated private password in `application.yml`.
- Docker container controls, logs, resource limits, and host metrics.
- Admin controls, account/API-key tools, custom panel name/logo/theme settings, and existing authentication flows.

## Install on Ubuntu VPS

Use a fresh Ubuntu 22.04/24.04 VPS with Docker installed or allow the installer to configure dependencies. A practical starting point is 2+ vCPU, 4 GB RAM, and 20 GB free disk if you plan to run several services.

```bash
npm install
npm run build
npm run createuser
npm run start
```

The panel listens on port `6767` by default. For development use `npm run dev` (port `3000`). Configure `JWT_SECRET` to a long random value in the environment before production use, set up HTTPS with a reverse proxy, and restrict exposed service ports in your firewall.

The included `install.sh` provides an interactive install/update flow. Review the script before running it as root and ensure you are in the extracted MagarZ Cloud project directory.

## Create services

1. Open **Deploy Server**.
2. Choose a service type:
   - **Node.js Web App** or **Python Web App** for websites/APIs.
   - **Discord Bot · Node.js** or **Discord Bot · Python** for bot workloads.
   - **Lavalink Audio Node** for a Lavalink v4 audio endpoint.
   - Existing Minecraft software types remain available.
3. Set a unique host port, resources, and owner, then deploy.
4. Use File Manager and Console to upload/edit your source and inspect logs.

### Discord bot token

Bot templates read `DISCORD_TOKEN` from a `.env` file in the service's file directory. Create the file with:

```env
DISCORD_TOKEN=PASTE_YOUR_BOT_TOKEN_HERE
```

Never share or commit this file. Restart the service after saving it. The default template uses only the basic Guilds intent; enable additional intents in code and in the Discord Developer Portal only when needed.

### Lavalink password and port

The panel generates `application.yml` with a random password. Open that file in File Manager and copy the password into your music bot's Lavalink connection configuration. The default Lavalink internal port is `2333`; the panel's selected host port is mapped to it. Restrict that host port to trusted clients. Do not expose Lavalink publicly without network controls.

## Important production notes

- Run behind HTTPS and configure a strong `JWT_SECRET`; do not use development defaults.
- The panel mounts the Docker socket, which effectively grants host-level control to the panel process. Keep admin access tightly restricted and never expose the panel without TLS and security review.
- Custom domains and automatic per-site TLS routing are not implemented in this release; web apps are reached through their configured host ports.
- Disk quota values are currently metadata/planning values; verify actual disk enforcement before offering paid plans.
- Lavalink, bot, and web services require Docker runtime. Ensure the panel's default runtime is Docker.
- Review installer/update scripts before root execution, test upgrades and backups on a disposable VPS, and rotate any secrets that may have been used in previous deployments.

## Validation

The source archive contains the existing npm lockfile. After installing dependencies, run:

```bash
npm run lint
npm run build
```

A successful build is required before deploying to a public VPS.
