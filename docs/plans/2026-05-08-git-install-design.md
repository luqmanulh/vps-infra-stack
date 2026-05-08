# Git Installation Design Modification

## Objective
Update the `README.md` to ensure users running minimal OS distributions like Rocky Linux 10 install required system packages (such as `git`) before proceeding with the deployment.

## Motivation
Rocky Linux 10 and similar minimal VPS templates often do not ship with `git` pre-installed. Since Step 1 of the deployment guide requires `git clone`, the deployment will fail immediately without it.

## Approach
Add a new subsection under the **Prerequisites** section of the `README.md` titled **System Packages**. It will include universal commands for both RHEL-based (like Rocky Linux) and Debian-based systems to install essential packages like `git`.
