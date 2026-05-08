# DNS Setup Documentation Design

## Objective
Update the `README.md` to include a dedicated section for "Domain & DNS Setup" to guide users on the necessary DNS configurations before deployment.

## Motivation
Users need to know which subdomains to point to their VPS IP address to ensure Traefik can correctly route traffic and Let's Encrypt can issue SSL certificates. Providing this information upfront reduces confusion and deployment errors.

## Proposed Changes
Insert a new section "## Domain & DNS Setup" before the "## Deployment Guide" section in `README.md`.

### Content
- A brief introduction explaining the need for DNS A records.
- A table listing the services, their subdomains, and their purposes.
- A note about replacing the example domain with the actual domain used in the `.env` file.

| Service | Subdomain | Purpose |
| :--- | :--- | :--- |
| **Forgejo** | `git.example.com` | Git repository management |
| **Uptime Kuma** | `status.example.com` | Service uptime monitoring |
| **Netdata** | `monitor.example.com` | Host performance metrics |
