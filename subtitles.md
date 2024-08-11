---
title: Subtitles
description: 
published: true
date: 2024-08-11T19:29:04.823Z
tags: 
editor: markdown
dateCreated: 2024-08-11T05:04:56.943Z
---

### Subtitles

> **Note:**  
> These settings are automatically generated when a fresh `settings.json` file is created. If you are working with an existing configuration, make sure these lines are present in your file.
To enable automatic subtitle downloading, you'll need to make some changes to your `settings.json` file. We're using the [Subliminal](https://github.com/Diaoul/subliminal) library for this purpose, which will handle subtitle downloading efficiently.

#### Steps to Update `settings.json`

1. **Locate `settings.json`:**  
   Find the `settings.json` file in your application's configuration directory.

2. **Add the Subliminal Settings:**  
   Insert the following lines under the `"post_processing"` section to enable subtitle downloading:

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
    "post_processing": {  // Add these settings for subtitles
        "subliminal": {
            "enabled": true,
            "languages": [
                "eng"
            ]
        }
    }
    ```

   This configuration enables the Subliminal plugin and sets it to download English subtitles (`"eng"`). You can customize the `"languages"` array to include other languages by using their respective [ISO 639-2 codes](https://en.wikipedia.org/wiki/List_of_ISO_639-2_codes).
> **Additional Note:**  
> The Subliminal library can download subtitles in multiple languages. You can add multiple language codes to the `"languages"` array. For example:
> 
> <details>
>   <summary>Multi-Language Example</summary>
>   
>   ```json
>   "post_processing": {
>       "subliminal": {
>           "enabled": true,
>           "languages": [
>               "eng",  // English
>               "spa",  // Spanish
>               "fre"   // French
>           ]
>       }
>   }
>   ```
> </details>

3. **Save and Restart:**  
   After making these changes, save the file and restart the application to apply the new settings.
   