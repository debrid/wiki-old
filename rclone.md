---
title: Rclone
description: 
published: true
date: 2024-08-15T10:10:10.150Z
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
5. Start the rclone service with `sudo systemctl start rclone` and enable it to run on startup `sudo systemctl enable rclone`