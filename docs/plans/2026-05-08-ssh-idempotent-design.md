# SSH Idempotent Design

## Objective
Enhance the reliability of the host SSH port migration script in `README.md` by making the `sed` command idempotent.

## Motivation
The previous `sed` commands (`sudo sed -i "s/^Port 22/Port $CUSTOM_PORT/"`) suffered from a boundary logic flaw. If a user ran the script twice, `sed` would match "Port 22" inside "Port 2222" (the new port), resulting in concatenated junk values like "Port 222222". This could break the `sshd_config` file and lock the user out of their server.

## Approach
Implement an industry-standard, robust regular expression using `sed -E`:
`sudo sed -i -E "s/^#?Port [0-9]+/Port $CUSTOM_PORT/" /etc/ssh/sshd_config`

This single command safely handles both commented (`#Port 22`) and active (`Port 2222`) configuration states by sweeping any existing digit combination and cleanly replacing it with the desired custom port. It guarantees the script can be executed multiple times without corrupting the file.
