# Traefik ACME Environment Variable Design

## Objective
Fix the Traefik SSL certificate resolution failure by ensuring the `ACME_EMAIL` environment variable is successfully passed into the Traefik container.

## Motivation
Currently, Traefik uses a default self-signed certificate ("TRAEFIK DEFAULT CERT") because Let's Encrypt rejects its certificate request. This occurs because the `traefik.yml` configuration file expects the `${ACME_EMAIL}` variable, but this variable is not being injected into the container's environment by Docker Compose. As a result, Traefik sends a literal or blank string as the email.

## Approach
Implement "Approach 1" to inject the environment variable cleanly. We will add an `environment` block to the `traefik` service in `compose.yaml` to explicitly map the host's `ACME_EMAIL` into the container.

This will look like:
```yaml
  traefik:
    image: traefik:${TRAEFIK_TAG}
    container_name: traefik
    restart: unless-stopped
    environment:
      - ACME_EMAIL=${ACME_EMAIL}
```
