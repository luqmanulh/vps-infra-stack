# Lightweight VPS Infrastructure Stack

A highly optimized, dockerized infrastructure stack designed for severely constrained Virtual Private Servers (VPS), specifically tailored for environments with **1 vCPU and 1 GB of RAM**.

This repository contains the Infrastructure as Code (IaC) to deploy a secure, automatically-provisioned routing and monitoring stack alongside a private Git forge.

## Architecture & Components

The stack uses a single Docker network (`proxy`) to isolate internal traffic from the host network. Only Traefik exposes ports to the internet.

- **Traefik (Reverse Proxy)**: Handles incoming traffic on ports `80` and `443`. It automatically negotiates and renews SSL/TLS certificates via Let's Encrypt and routes traffic to the appropriate containers based on their hostnames.
- **Forgejo (Git Forge)**: A lightweight, self-hosted Git service (a soft fork of Gitea) for private repository management.
- **Uptime Kuma (Service Monitoring)**: A self-hosted monitoring tool to keep track of service uptime and response times.
- **Netdata (Host Monitoring)**: Real-time performance and health monitoring for the VPS host system, severely constrained to prevent memory exhaustion.

---

## ⚠️ Critical Prerequisites (The 1GB RAM Survival Guide)

If you deploy this stack on a 1GB RAM VPS without prior configuration, the Linux OOM (Out of Memory) killer **will** terminate your containers during spikes (especially when Forgejo runs heavy Git operations or Netdata compiles metrics).

### 1. Essential System Packages
Minimal distributions like Rocky Linux 10 do not ship with Git pre-installed. You need it to clone this repository.
Run the appropriate command for your OS:

- **Rocky Linux / RHEL / CentOS**:
  ```bash
  sudo dnf install git -y
  ```
- **Ubuntu / Debian**:
  ```bash
  sudo apt update && sudo apt install git -y
  ```

### 2. Mandatory Swap File Configuration
You **must** configure a swap file of at least 2GB. Run these commands on your VPS host before deploying:

```bash
# Create a 2GB swap file
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Make the swap file persistent across reboots
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

### 3. Host SSH Port Migration (Mandatory)
Since Forgejo needs port 22 to provide standard Git SSH clone URLs, you must move your host's SSH daemon to a different port (e.g., 2222) to avoid conflicts and increase security.
Run these commands on your VPS host before deploying:

```bash
# Change SSH port to 2222
sudo sed -i 's/^#Port 22/Port 2222/' /etc/ssh/sshd_config
sudo sed -i 's/^Port 22/Port 2222/' /etc/ssh/sshd_config

# Restart SSH service
sudo systemctl restart sshd

# Important: Keep your current SSH session open, open a new terminal, and try connecting with 'ssh -p 2222 user@your_vps_ip' to verify it works before closing the original session!
```

### 4. Netdata Constraints
Netdata is extremely memory-intensive by default. This repository includes a pre-configured `netdata.conf` located in `data/netdata/netdataconfig/netdata/netdata.conf` which heavily restricts its page cache size and database engine disk space to keep its footprint minimal.

---

## Security & Configuration Model

This repository adheres strictly to the **separation of configuration and secrets**:
- The `.gitignore` is configured to block any sensitive files, meaning `.env` files and the persistent `data/` volume (which holds the SSL certificates in `acme.json`, databases, and Git repositories) will never be committed to Git.
- We provide `.env.example` as a safe, public template.

---

## Deployment Guide

### Step 1: Clone the Repository
Clone this repository to your VPS.
```bash
git clone https://github.com/luqmanulh/vps-infra-stack vps-infra
cd vps-infra
```

### Step 2: Configure Environment Variables
Copy the template environment file to create your actual secrets file.
```bash
cp .env.example .env
```
Edit the `.env` file using your preferred text editor (`nano`, `vim`, etc.) and populate it with your actual domain name, email address (for Let's Encrypt), and secure passwords.

### Step 3: Set Strict Permissions on acme.json
Traefik requires the Let's Encrypt certificate storage file to have strict permissions, otherwise it will refuse to start.
```bash
chmod 600 data/acme.json
```

### Step 4: Boot the Stack
Once your swap file is active and your environment variables are configured, bring up the stack:
```bash
docker compose up -d
```

### Step 5: Secure Netdata (Action Required)
By default, the `compose.yaml` exposes Netdata on a subdomain. **You must protect this route.** Since Netdata exposes deep system metrics, it should not be publicly accessible. 
Update the Netdata labels in `compose.yaml` to include a Traefik BasicAuth middleware, or restrict access via a VPN/IP whitelist.

---

## Accessing Your Services

Assuming you set `DOMAIN_MAIN=example.com` in your `.env` and pointed your DNS A-records to your VPS IP address, your services will be automatically provisioned with Let's Encrypt SSL certificates at:
- **Forgejo**: `https://git.example.com`
- **Uptime Kuma**: `https://status.example.com`
- **Netdata**: `https://monitor.example.com`
