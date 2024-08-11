---
title: Subtitles
description: 
published: true
date: 2024-08-11T19:17:37.590Z
tags: 
editor: markdown
dateCreated: 2024-08-11T05:04:56.943Z
---

# Subtitles
bazzar replacment


https://github.com/Diaoul/subliminal

```json
    },
    "indexer": {
        "update_interval": 3600
    },
    "database": {
        "host": "postgresql+psycopg2://postgres:postgres@riven-db/riven"
    },
    "notifications": {
        "enabled": false,
        "title": "Riven completed something!",
        "on_item_type": [
            "movie",
            "show",
            "season"
        ],
        "service_urls": []
    },
    "post_processing": {  # Add these settings for subtitles
        "subliminal": {
            "enabled": true,
            "languages": [
                "eng"
            ]
        }
    }
}
```