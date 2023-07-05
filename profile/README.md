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

## Discussions & Help

- [Announcements](https://github.com/orgs/altv-crc/discussions/categories/announcements)
- [Plugins](https://github.com/orgs/altv-crc/discussions/categories/plugins)
- [Q & A](https://github.com/orgs/altv-crc/discussions/categories/q-a)
- [Tutorials](https://github.com/orgs/altv-crc/discussions/categories/tutorials)

## Development Stack

![TypeScript](https://img.shields.io/badge/typescript-%23007ACC.svg?style=for-the-badge&logo=typescript&logoColor=white)
<br />
![Visual Studio Code](https://img.shields.io/badge/Visual%20Studio%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white)
<br />
![MongoDB](https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white)

## Cross Resource Event Specifications

Every resource should have an event system related to its name.

* If your resource is called `crc-my-resource` then your resource should recieve events through it.

```ts
alt.on('crc-my-resource', (functions: { a?: string, b?: string) => {
    alt.log(`got action`);
});
```

* Event parameters should always be a single object that can be deconstructed to point towards specific functions

```ts
function handleActions(functions: { set?: Array<{ text: string; input: string }>; clear?: boolean }) {
    if (typeof functions !== 'object') {
        return;
    }

    // This allows for action properties to clearly be defined and pushed to the right functions
    const { set, clear } = functions;

    if (set) {
        setFunc(set);
    }

    if (clear) {
        clearFunc();
    }
}

alt.on('crc-instructional-buttons', handleActions);
```

* Why this event specification?
  
This ensures that as you add new functionality, the previous events remain always useable. In most cases.

Users can upgrade to new versions of individual resources and still be able to call those events.

This ensures backwards compatability as long as the developer chooses to retain it.

Objects can always add new parameters easily. Arrays cannot.

## Recommended Infrastructure

Below is a general mindmap that shows what resources need to exist to create a login flow that leads a player to the eventual game loop.

Every section with `crc-` should be considered an individual resource that has been built, or needs to be built.

The `*` means the resource has not been built.

```mermaid
graph TD
    category((category))
    not_created[* = Pending]

    %% Initial Category
    altv_crc_core[[altv-crc-core]]
    altv_crc_core --> cat_core
    altv_crc_core --> cat_utility

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
    crc_char_select --> crc_char_preview(crc-character-preview*)
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

    cat_utility --> crc_chat(crc-chat*)

    cat_utility --> crc_voice(crc-voice*)
```

[Built with mermaid.live](https://mermaid.live/)
