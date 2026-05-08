# SSH Port Design Modification

## Objective
Modify the VPS infrastructure so that the Forgejo container handles SSH traffic on the standard port 22. The VPS host's default SSH port will be moved to a custom port (e.g., 2222) to avoid conflicts and increase security.

## Motivation
- **Security**: Moving the host's SSH daemon away from port 22 mitigates automated scanner attacks.
- **Usability**: Users can clone and push Git repositories using standard SSH URLs (`git@git.domain.com:repo.git`) without appending custom ports.

## Approach
1. **Host Configuration Guide**: Update `README.md` to include explicit instructions on changing the host's `/etc/ssh/sshd_config` to use a custom port, and restarting the SSH daemon. This is a strict prerequisite before booting the stack.
2. **Container Port Mapping**: Update `compose.yaml` to map host port 22 to container port 22 (`"22:22"`) within the `forgejo` service block.
