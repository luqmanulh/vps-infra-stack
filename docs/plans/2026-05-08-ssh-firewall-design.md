# SSH Firewall and SELinux Design

## Objective
Update the `README.md` to safely guide users through SSH port migration by including necessary Firewall (firewalld/UFW) and SELinux (`semanage`) configurations specific to their Linux distribution.

## Motivation
Changing the SSH port via `sed` in `sshd_config` is insufficient on modern Linux distributions. 
- On RHEL/Rocky Linux, SELinux will block `sshd` from binding to a non-standard port unless explicitly allowed, and `firewalld` will block incoming traffic.
- On Ubuntu/Debian, `ufw` (if active) will block the incoming traffic.
Failing to address these layers will lock the user out of their server when `sshd` restarts.

## Approach
1. **System Packages Update**: Update the RHEL/Rocky Linux prerequisite installation command to include `policycoreutils-python-utils` which provides the `semanage` command.
2. **OS-Specific SSH Migration**: Split the "Host SSH Port Migration" section into two distinct paths:
   - **Rocky Linux/RHEL**: Includes `sed`, `semanage port -a -t ssh_port_t -p tcp $CUSTOM_PORT`, `firewall-cmd --permanent --add-port=$CUSTOM_PORT/tcp`, `firewall-cmd --reload`, and `systemctl restart sshd`.
   - **Ubuntu/Debian**: Includes `sed`, `ufw allow $CUSTOM_PORT/tcp`, and `systemctl restart sshd`.
