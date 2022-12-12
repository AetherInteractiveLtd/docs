---
icon: material/file-document-outline
---

## :material-file-document-outline: Manifest

The Manifest is a file you need to have in your workspace whenever working with Tina.
This file should always be in the root of your project.

```yaml title="manifest.tina.yml"
name: "Hello World" # (1)!
version: "0.0.1" # (2)!
description: "My First Ever Project!" # (3)!
config:
    tickrate: 20 # (6)!
    net:
        compression: true # (4)!
    supported_languages: # (5)!
        - fr-fr # French
        - cs-cz # Czech
        - en-us # English
    max_players: 10 # (7)!
tina: "dev" # IMPORTANT (8)
```

1. The name of your project, this can always be changed!
2. The version of your project, we **highly** recommend that you use a versioning standard such as [SemVer](https://semver.org).
3. Have some fun, this field isn't really necessary, but it's nice to write an elevator pitch for a game before you've even begun writing it!
4. Whether or not to use compressed networking.
5. All officially supported languages. You can find language codes [here](https://create.roblox.com/docs/production/localization/language-codes).
6. The amount of times Server-Side Processes will run every second. We recommend using 20, 30, or 60, this is capped at 60.
7. The maximum amount of players a single game server will accept; if another tries to join over the cap, they will be kicked and their data will never load.
8.   **IMPORTANT** The current deployment environment.
   
    | Value | Description |
    | - | - |
    | `#!typescript "dev"` | Active development, this will leave any `.developmentOnly` endpoints on and will allow native console access. |
    | `#!typescript "stable"` | The game is currently published, all `.developmentOnly` endpoints are off and native console access is no longer allowed. |