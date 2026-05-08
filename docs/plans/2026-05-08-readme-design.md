# README.md Design

## Objective
Create a highly detailed, English-language `README.md` for the lightweight VPS infrastructure project. The document will guide users on deploying the stack on a severely constrained VPS (1 vCPU, 1 GB RAM).

## Structure

### 1. Title & Overview
A brief introduction explaining that this is a lightweight, dockerized infrastructure stack designed for low-resource VPS environments.

### 2. Architecture & Components
Explaining the purpose of each service:
*   **Traefik**: Reverse proxy with automatic Let's Encrypt SSL.
*   **Forgejo**: Lightweight Git forge.
*   **Uptime Kuma**: Service monitoring.
*   **Netdata**: Host monitoring.

### 3. Critical Prerequisites (The 1GB RAM Survival Guide)
A highlighted section explaining why a 2GB Swap file is mandatory to prevent OOM (Out of Memory) kills, and the specific memory constraints we placed on Netdata in `netdata.conf`.

### 4. Security & Configuration
Explaining the separation of concerns: why `data/` and `.env` are excluded from version control, and how the `.env.example` acts as a safe template.

### 5. Deployment Guide
Step-by-step instructions:
1. Cloning the repo
2. Creating the swap file (with commands)
3. Copying `.env.example` to `.env` and editing it
4. Booting the stack with `docker compose up -d`
