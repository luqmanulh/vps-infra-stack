# SSH Variable Design

## Objective
Enhance the security of the `README.md` documentation by removing hardcoded sensitive port numbers (like `2222`) and replacing them with a bash variable.

## Motivation
Hardcoding specific port numbers in a public repository's README can inadvertently leak an administrator's intended security configuration (like their custom SSH port) to potential attackers.

## Approach
Update the "Host SSH Port Migration" section in `README.md` to use a bash variable (`CUSTOM_PORT`) at the start of the command block. This forces the user to consciously set their port and prevents accidental copy-pasting of a default "secure" port which might be easily guessed.

The new command block will look like:
```bash
# Set your secure custom port here!
CUSTOM_PORT=54321

sudo sed -i "s/^#Port 22/Port $CUSTOM_PORT/" /etc/ssh/sshd_config
sudo sed -i "s/^Port 22/Port $CUSTOM_PORT/" /etc/ssh/sshd_config
sudo systemctl restart sshd
```
