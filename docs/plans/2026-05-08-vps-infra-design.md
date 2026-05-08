# VPS Infrastructure Design

## Overview
A lightweight, docker-compose based VPS infrastructure stack designed for a 1 vCPU, 1 GB RAM machine. The stack includes Traefik, Forgejo, Uptime Kuma, and Netdata.

## Architecture
The repository uses a monorepo approach separating configuration and persistent data from code. 
- **Reverse Proxy**: Traefik handles all incoming traffic on ports 80/443 and terminates SSL via Let's Encrypt.
- **Git Forge**: Forgejo serves as the private code repository.
- **Monitoring**: Netdata is configured with strict memory constraints to monitor the host.
- **Uptime**: Uptime Kuma provides external service monitoring.

## Components & File Structure
- `.gitignore`: Prevents secrets and state (like `data/` and `acme.json`) from being committed.
- `.env.example`: Public template for environment variables (domains, versions, secrets).
- `compose.yaml`: The Docker Compose specification defining the proxy network and services.
- `config/traefik/traefik.yml`: Static configuration for Traefik to enable Let's Encrypt and basic entrypoints.
- `data/`: The persistent volume directory for all services (ignored by Git).
- `data/netdata/netdataconfig/netdata/netdata.conf`: Required memory limits for Netdata to prevent OOM issues on 1GB RAM.

## Data Flow
1. Incoming traffic hits the host on 80/443.
2. Traefik routes traffic to internal Docker services over a custom `proxy` network based on Host rules.
3. Persistent data is read/written to the `data/` directory mapped as volumes.

## Next Steps
- Implement the scaffolding as defined.
