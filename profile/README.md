# alt:V Cross Resource Community

This is an experimental project that will allow users to quickly drag / drop resources into a structured server infrastructure.

Think of each resource as a micro-service for your gamemode.

## Ideology

- TypeScript
- Very small resources
- Maximum performance with least overhead
- Shared database so credentials automatically are associated with all other resources
- Near one-click solutions for most server components
- Cross resource compatability

## Installation

- [Install on Windows](https://github.com/orgs/altv-crc/discussions/1)
- [Install a Plugin](https://github.com/orgs/altv-crc/discussions/2)

## Discussions & Help

- [Announcements](https://github.com/orgs/altv-crc/discussions/categories/announcements)
- [Plugins](https://github.com/orgs/altv-crc/discussions/categories/plugins)
- [Q & A](https://github.com/orgs/altv-crc/discussions/categories/q-a)

## Development Stack

![TypeScript](https://img.shields.io/badge/typescript-%23007ACC.svg?style=for-the-badge&logo=typescript&logoColor=white)
<br />
![Visual Studio Code](https://img.shields.io/badge/Visual%20Studio%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white)
<br />
![MongoDB](https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white)

## Recommended Infrastructure

Below is a general mindmap that shows what resources need to exist to create a login flow that leads a player to the eventual game loop.

Every section with `crc-` should be considered an individual resource that has been built, or needs to be built.

The `*` means the resource has not been built.

```mermaid
graph TD
    classDef pending fill:#6b384c
    classDef ready fill:#205991

    category((category))
    not_created[* = Pending]:::pending

    %% Initial Category
    altv_crc_core[[altv-crc-core]]:::ready
    altv_crc_core --> cat_core
    altv_crc_core --> cat_utility

    %% Core
    cat_core((Server Flow))
    cat_core --> crc_db:::ready
    cat_core --> crc_db_sql

    %% Database
    crc_db(crc-db):::ready
    crc_db --> crc_login
    crc_db --> crc_discord_login
    crc_db_sql(crc-db-sql*):::pending
    crc_db_sql --> crc_login
    crc_db_sql --> crc_discord_login

    %% Authentication
    crc_login(crc-login):::ready
    crc_discord_login(crc-discord-login):::ready
    crc_login --> crc_ban_handler(crc-ban-handler):::ready
    crc_discord_login --> crc_ban_handler
    crc_login --> cat_character
    crc_discord_login --> cat_character
    
    %% Appearance
    crc_appearance(crc-appearance):::ready
    crc_appearance --> crc_default_clothes(crc-default-clothes):::ready
    crc_char_create --> crc_appearance
    crc_spawn --> crc_appearance
    crc_char_preview --> crc_appearance
    crc_respawn --> crc_appearance

    %% Character
    cat_character((Character))
    cat_character --> crc_char_select(crc-select-character):::ready
    crc_char_select --> crc_char_preview(crc-preview-character):::ready
    crc_char_select --> crc_char_create(crc-create-character):::ready
    crc_char_select --> cat_spawn
    crc_char_create --> cat_spawn
    crc_char_create --> crc_char_preview

    %% Spawn
    cat_spawn((Spawn))
    cat_spawn --> crc_spawn(crc-spawn):::ready
    cat_spawn --> crc_respawn(crc-respawn*):::pending
    crc_spawn --> cat_game_loop
    crc_respawn --> cat_game_loop

    %% Game Loop
    cat_game_loop((Game Loop))
    cat_game_loop --> cat_behavior
    cat_game_loop --> cat_banking
   
    %% Behavior
    cat_behavior((Behavior))
    cat_behavior --> tbd(tbd*):::pending

    %% Banking
    cat_banking((Banking))
    cat_banking --> crc_paychecks(crc-paychecks*):::pending
    cat_banking --> crc_atm(crc-atms*):::pending
    cat_banking --> crc_banks(crc-banks*):::pending
    cat_banking --> cat_jobs

    %% Jobs
    cat_jobs((Jobs))

    %% Utility
    cat_utility((Utility))

    cat_utility --> crc_instructional_buttons(crc-instructional-buttons):::ready
    crc_instructional_buttons --> crc_native_menu(crc-native-menu):::ready

    cat_utility --> crc_chat(crc-chat*):::pending

    cat_utility --> crc_voice(crc-voice*):::pending
```

[Built with mermaid.live](https://mermaid.live/)
