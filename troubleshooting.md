---
title: Troubleshooting
description: 
published: true
date: 2024-08-13T19:11:59.994Z
tags: riven
editor: markdown
dateCreated: 2024-08-13T19:11:59.994Z
---

## Troubleshooting

Here are solutions to some common issues you might encounter:

### Connection Issues

**Problem**: Unable to connect to Riven web interface
**Solution**: 
1. Ensure Docker containers are running: `docker ps | grep riven`
2. Check if the correct ports are exposed in your `docker-compose.yml`
3. Verify your firewall settings allow traffic on the specified ports

### Download Service Errors

**Problem**: Downloads failing or stuck
**Solution**:
1. Verify your Real-Debrid API key in Settings > Download Service
2. Ensure your Real-Debrid account has available traffic

### Media Not Appearing in Plex

**Problem**: Downloaded media not showing up in Plex library
**Solution**:
1. Check Riven logs for any symlink creation errors
2. Verify Plex has correct permissions to access the media directory

For more detailed troubleshooting, please refer to our [Extended Troubleshooting Guide](link-to-extended-guide).