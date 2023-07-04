# alt:V Cross Resource Community

A collection of useful resources, and recommendations to build faster.

## Development Stack

![TypeScript](https://img.shields.io/badge/typescript-%23007ACC.svg?style=for-the-badge&logo=typescript&logoColor=white)
<br />
![Visual Studio Code](https://img.shields.io/badge/Visual%20Studio%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white)
<br />
![MongoDB](https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white)

## Ideology

- TypeScript
- Very small resources
- Maximum performance with least overhead
- Near one-click solutions for most server components
- Cross resource compatability


## Recommended Infrastructure

Below is a general mindmap that shows what resources need to exist to create a login flow that leads a player to the eventual game loop.

Every section with `crc-` should be considered an individual resource that has been built, or needs to be built.

The `*` means the resource has not been built.

```mermaid
graph TD
    category((category))
    not_created[* = Pending]

    %% Initial Category
    altv_typescript[[altv-typescript]]
    altv_typescript --> cat_core
    altv_typescript --> cat_utility

   

    %% Core
    cat_core((Server Flow))
    cat_core --> crc_db
    cat_core --> crc_db_sql

    %% Database
    crc_db(crc-db)
    crc_db --> crc_login
    crc_db --> crc_discord_login
    crc_db_sql(crc-db-sql*)
    crc_db_sql --> crc_login
    crc_db_sql --> crc_discord_login
   

    %% Authentication
    crc_login(crc-login)
    crc_discord_login(crc-discord-login)
    crc_login --> cat_character
    crc_discord_login --> cat_character

    %% Character
    cat_character((Character))
    cat_character --> crc_char_select(crc-select-character*)
    crc_char_select --> crc_char_create(crc-create-character*)
    crc_char_select --> cat_spawn
    crc_char_create --> cat_spawn

    %% Spawn
    cat_spawn((Spawn))
    cat_spawn --> crc_new_spawn(crc-new-spawn*)
    cat_spawn --> crc_restore_spawn(crc-restore-spawn*)
    cat_spawn --> crc_respawn(crc-respawn*)
    crc_new_spawn --> cat_game_loop
    crc_restore_spawn --> cat_game_loop
    crc_respawn --> cat_game_loop

    %% Game Loop
    cat_game_loop((Game Loop))
    cat_game_loop --> cat_behavior
    cat_game_loop --> cat_banking
   
    %% Behavior
    cat_behavior((Behavior))
    cat_behavior --> tbd(tbd*)

    %% Banking
    cat_banking((Banking))
    cat_banking --> crc_paychecks(crc-paychecks*)
    cat_banking --> crc_atm(crc-atms*)
    cat_banking --> crc_banks(crc-banks*)
    cat_banking --> cat_jobs

    %% Jobs
    cat_jobs((Jobs))

    %% Utility
    cat_utility((Utility))
    cat_utility --> crc_instructional_buttons(crc-instructional-buttons)
    crc_instructional_buttons --> crc_native_menu(crc-native-menu)
```
