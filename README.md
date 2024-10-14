# Changing Docker Root Directory from `/var/lib/docker` to Another Location

This guide explains how to change the Docker root directory from its default location `/var/lib/docker` to another directory. This is useful when your system partition is running out of space or when you want to move Docker data to a separate disk or partition.

## Prerequisites

- **Docker Installed**: Ensure Docker is installed and running on your system.
- **Root Access**: You need root or sudo access to make these changes.
- **New Directory**: Decide on a new location for Docker’s root directory (e.g., `/mnt/docker-data`).

---

## Table of Contents

1. [Check Current Docker Root Directory](#check-current-docker-root-directory)
2. [Stop Docker Service](#stop-docker-service)
3. [Create a New Directory for Docker](#create-a-new-directory-for-docker)
4. [Move Docker Data to the New Directory](#move-docker-data-to-the-new-directory)
5. [Update Docker Configuration](#update-docker-configuration)
6. [Restart Docker and Verify](#restart-docker-and-verify)
7. [Optional: Disk Space and Performance Considerations](#optional-disk-space-and-performance-considerations)
8. [Conclusion](#conclusion)

---

## 1. Check Current Docker Root Directory

Before changing anything, it's a good idea to verify Docker’s current root directory:


- docker info | grep 'Docker Root Dir'

# Stop Docker Service
To avoid any conflicts while changing the root directory, stop the Docker service:
- sudo systemctl stop docker

## Create a New Directory for Docker
Create the new directory where Docker will store its data. Ensure that the directory is on a disk or partition with enough space.
Example: Moving Docker to /mnt/docker-data:
- sudo mkdir -p /mnt/docker-data
Set the correct ownership for the new directory:
- sudo chown -R root:root /mnt/docker-data
- sudo chmod -R 755 /mnt/docker-data
## Move Docker Data to the New Directory
Now, move the existing Docker data from /var/lib/docker to the new directory (/mnt/docker-data):
- sudo rsync -aP /var/lib/docker/ /mnt/docker-data/
This will ensure that all files are copied, preserving permissions and symlinks.
After confirming the copy, you may choose to rename or delete the original directory to avoid confusion:
- sudo mv /var/lib/docker /var/lib/docker.old

## Update Docker Configuration
Next, we need to tell Docker to use the new directory as its root. This can be done by editing the Docker service configuration.
Open the Docker configuration file for editing:
- sudo nano /etc/docker/daemon.json
Add the following configuration to set the new root directory (or modify if it exists):
```bash
{
  "data-root": "/mnt/docker-data"
}                                                                                                
```
If the file is empty, add the entire JSON block. If it already has content, just append the data-root option.

## Restart Docker and Verify
- sudo systemctl start docker
Verify that Docker is now using the new directory:
- docker info | grep 'Docker Root Dir'
You should now see the new directory (e.g., /mnt/docker-data) listed as the Docker root directory.
Additionally, run docker ps to ensure everything is functioning properly, and containers are still available.

# Optional: Disk Space and Performance Considerations
Check Disk Space: Ensure the new directory is mounted on a disk with enough space for Docker data.
- df -h /mnt/docker-data

blog [(https://linuxconfig.org/how-to-move-docker-s-default-var-lib-docker-to-another-directory-on-ubuntu-debian-linux)]
