---
title: Ubuntu install guide
description: 
published: true
date: 2024-08-13T18:35:17.967Z
tags: riven
editor: markdown
dateCreated: 2024-08-13T18:35:17.967Z
---

# Beginner's Guide to Installing Riven on Ubuntu Server

## Introduction

This guide will walk you through the process of installing Riven, an advanced media management and streaming solution, on an Ubuntu server using Docker.

## Prerequisites

Before you begin, ensure you have the following:

1. An Ubuntu server (18.04 LTS or later recommended)
2. Root or sudo access to the server
3. Basic familiarity with command line interface

## Step 1: Update Your System

First, update your system packages:

```
sudo apt update && sudo apt upgrade -y
```

## Step 2: Install Docker and Docker Compose

Install Docker:

```
sudo apt install docker.io -y
```

Install Docker Compose:

```
sudo apt install docker-compose -y
```

## Step 3: Create a Directory for Riven

Create a directory to store Riven's configuration:

```
mkdir ~/riven && cd ~/riven
```

## Step 4: Create Docker Compose File

Create a file named `docker-compose.yml` in the Riven directory:

```
nano docker-compose.yml
```

Copy and paste the following content into the file:

```yaml
services:
  riven-frontend:
    image: spoked/riven-frontend:latest
    container_name: riven-frontend
    restart: unless-stopped
    ports:
      - "3000:3000"
    tty: true
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/New_York
      - ORIGIN=http://localhost:3000
      - BACKEND_URL=http://riven:8080
      - DIALECT=postgres 
      - DATABASE_URL=postgres://postgres:postgres@riven-db:5432/riven 
    depends_on:
      riven:
        condition: service_healthy

  riven:
    image: spoked/riven:latest
    container_name: riven
    restart: unless-stopped
    ports:
      - "8080:8080"
    tty: true
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/New_York
      - RIVEN_FORCE_ENV=true
      - RIVEN_DATABASE_HOST=postgresql://postgres:postgres@riven-db/riven
    healthcheck:
      test: curl -s http://localhost:8080 >/dev/null || exit 1
      interval: 30s
      timeout: 10s
      retries: 10
    volumes:
      - ./data:/riven/data
      - /mnt:/mnt
    depends_on:
      riven_postgres:
        condition: service_healthy

  riven_postgres:
    image: postgres:16.3-alpine3.20
    container_name: riven-db
    environment:
      PGDATA: /var/lib/postgresql/data/pgdata
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: riven
    volumes:
      - ./riven-db:/var/lib/postgresql/data/pgdata
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5
```

Save and exit the file (in nano, press Ctrl+X, then Y, then Enter).

## Step 5: Start Riven

Run the following command to start Riven:

```
docker-compose up -d
```

This command will download the necessary Docker images and start the Riven containers in the background.

## Step 6: Access Riven

Once the containers are up and running, you can access the Riven web interface by opening a web browser and navigating to:

```
http://your_server_ip:3000
```

Replace `your_server_ip` with the actual IP address or domain name of your Ubuntu server.

## Step 7: Configure Riven

On first run, Riven creates a `settings.json` file in the `data` directory. You can edit the settings from the web interface or manually edit the file and restart the container.

## Troubleshooting

If you encounter any issues:

1. Check the logs of the containers:
   ```
   docker-compose logs
   ```

2. Ensure all containers are running:
   ```
   docker-compose ps
   ```

3. If needed, you can restart the containers:
   ```
   docker-compose restart
   ```

## Conclusion

You have now successfully installed Riven on your Ubuntu server. Explore the web interface to set up your media libraries and configure integrations with other services.

For more detailed information and advanced configuration options, please refer to the official Riven documentation.

Would you like me to explain or elaborate on any part of this guide?