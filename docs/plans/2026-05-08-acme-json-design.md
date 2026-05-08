# Acme.json Persistence Design

## Objective
Update the `README.md` to explicitly instruct users to create the `data/acme.json` file before running `docker compose up -d`.

## Motivation
The `data/` directory is deliberately ignored by `.gitignore` to prevent committing sensitive state (databases, repositories, SSL certificates). When a user runs `git clone`, the `data/` directory is not created.
If `docker compose up -d` is executed before `data/acme.json` exists, Docker's bind mount behavior will automatically create a *directory* named `acme.json`. Traefik expects a file for its certificate storage and will immediately crash if it encounters a directory.

## Approach
Modify "Step 3" of the deployment guide in `README.md` to include `mkdir -p data` and `touch data/acme.json` prior to the `chmod 600` command, ensuring the file exists before Docker runs.
