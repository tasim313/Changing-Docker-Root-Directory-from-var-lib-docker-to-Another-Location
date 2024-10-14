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

```bash
docker info | grep 'Docker Root Dir'

# Stop Docker Service
To avoid any conflicts while changing the root directory, stop the Docker service:
- sudo systemctl stop docker

## Create a New Directory for Docker
