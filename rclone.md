---
title: Rclone
description: 
published: true
date: 2024-08-15T10:14:42.274Z
tags: 
editor: markdown
dateCreated: 2024-08-15T10:06:47.808Z
---

# Rclone
&nbsp;
## Setup with SystemD

1. Install the rclone binary
`sudo apt install rclone`
2. Create a rclone config under the path that `rclone config file` outputs. Replace `http://localhost:9999` if zurg is running on a different url.
```toml
[zurg]
type = webdav
url = http://localhost:9999/dav
vendor = other
pacer_min_sleep = 0

[zurghttp]
type = http
url = http://localhost:9999/http
no_head = false
no_slash = false
```
3. Make sure that you have fusermount3 installed `sudo apt install fuse3`
4. Add `user_all_other` to your fuse config file at `/etc/fuse.conf`
```
# user_allow_other - Using the allow_other mount option works fine as root, in
# order to have it work as user you need user_allow_other in /etc/fuse.conf as
# well. (This option allows users to use the allow_other option.) You need
# allow_other if you want users other than the owner to access a mounted fuse.
# This option must appear on a line by itself. There is no value, just the
# presence of the option.

user_allow_other
```
5. Add a file called `rclone.service` to `/etc/systemd/system/rclone.service`.
Tweak the rclone arguments to your liking
```
[Unit]
Description=Rclone Mount Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/rclone mount zurg: /mnt/zurg --allow-other --dir-cache-time 10s --vfs-cache-mode full --vfs-read-chunk-size 8M --vfs-read-chunk-size-limit 2G --buffer-size 16M --vfs-cache-max-age 150h --vfs-cache-max-size 20G --vfs-fast-fingerprint --uid 1000 --gid 1000
ExecStop=/bin/fusermount -u /mnt/zurg
Restart=on-failure
RestartSec=10
User=YOURUSERNAME
Group=YOURUSERNAME

[Install]
WantedBy=default.target
```
6. Reload the systemd daemon: `sudo systemctl daemon-reload`
7. Start the rclone service with `sudo systemctl start rclone` and enable it to run on startup `sudo systemctl enable rclone`