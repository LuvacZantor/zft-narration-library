# ZFT Narration Library

A deliberately minimal **content-only Foundry VTT module** for reusable ZFT Cinematics Narrate material. There are no scripts, settings, or UI in this package. The editor, Graphics Library, and maintenance interface belong to ZFT Cinematics.

## Storage contract (Foundry V13 baseline)

| Content | User Data location | Foundry reference |
| --- | --- | --- |
| Saved narration | `Data/modules/zft-narration-library/packs/narrations/` | `Compendium.zft-narration-library.narrations` |
| User-uploaded graphics | `Data/assets/zft-narration-library/graphics/` | `assets/zft-narration-library/graphics/<filename>` |

The **JournalEntry compendium** stores passage text, formatting, narration metadata, and media references. **Image bytes are not embedded in a compendium**: images live in a shared asset directory outside module files. This is still one logical content library. Foundry V13 normally blocks File Picker uploads into installed module directories, so do not change the upload target to `Data/modules/...`.

The `Data/assets/zft-narration-library/graphics/` folder is **not** supplied by this ZIP. It will be created/used by a future ZFT Cinematics build, or can be created manually under the Foundry User Data directory. The existing Cinematics uploads in `Data/worlds/<world-id>/zft-cinematics/narration/` are **not yet migrated**.

## Compendium

- Package ID: `zft-narration-library`
- Pack name: `narrations`
- Document type: `JournalEntry` (no D&D5e system dependency)
- Full collection ID: `zft-narration-library.narrations`
- Starts empty. Foundry V13 has registered the pack and confirmed it visible to a GM in a live test.
- The library package provides no UI or scripts. **ZFT Cinematics** handles Save, Load, image uploads, and future library maintenance features.
- The compendium ownership explicitly grants **GAMEMASTER: OWNER** and denies PLAYER, TRUSTED, and ASSISTANT. A GM can unlock the pack when editing.

## Local install and smoke test

1. Shut down Foundry, install the ZIP beneath `Data/modules/` (one `zft-narration-library/` top-level directory), then restart.
2. Enable **ZFT Narration Library** in a test world.
3. Open Compendium Packs and verify **ZFT Narrations** appears.
4. Unlock the compendium as GM, create or import a test Journal Entry, then lock the pack again.
5. Open a second world on the **same** Foundry User Data directory with the module enabled; confirm that the entry is visible there.
6. Verify no data is lost after restarting Foundry. Do **not** reinstall or update the module as part of this smoke test.

## Protecting content

**This is a mutable data store, not a normal disposable module update.** Updating, uninstalling, or reinstalling an installed content module may overwrite or remove its LevelDB compendium. This package intentionally has no `manifest` or `download` URL, so Foundry does not offer a routine in-app package update for it. **Lock the module against updates/uninstall in Foundry Setup** as a second safeguard.

Before backup, migration, or manual compendium deployment, shut down the affected Foundry process(es) so its LevelDB files are consistent. Back up and transfer **both**:

- `Data/modules/zft-narration-library/packs/`
- `Data/assets/zft-narration-library/graphics/`

The GitHub repository tracks module definitions and whatever *deliberate* snapshots of the compendium are committed. Adding or changing entries while Foundry runs **does not automatically commit them to GitHub**, nor distribute them to a second Foundry User Data directory. Avoid copying a live LevelDB database between running instances. Cross-installation and V13 ↔ V14 migration need separate compatibility testing and a deliberate sync/deployment method.

## Architecture boundary

- **ZFT Narration Library:** content definition, JournalEntry compendium, documented dedicated shared asset namespace.
- **ZFT Cinematics:** editor, Save/Load, image upload/selection, presentation, GM-only maintenance, and any migration/sync helpers.
- No world-level copy is required for ordinary reading and presenting from a shared pack on the same installation.

## Development status

Version `0.1.0-dev.2` fixes GM visibility: Foundry V13 testing confirmed `visible: true` and GM access after setting `GAMEMASTER: OWNER`. ZFT Cinematics `0.2.0-dev.8` provides the first Save/Load integration. V14 is not yet verified.

## v0.1.0-dev.2: GM compendium visibility fix

Foundry V13 previously reported `visible: false` and `permissionLevel: 0` for the GM because the pack omitted the `GAMEMASTER` ownership entry. The manifest now sets `GAMEMASTER: OWNER`, preserving `NONE` for other roles. The user confirmed the pack is now visible in Foundry V13.

For a local update, shut down Foundry, back up `Data/modules/zft-narration-library/packs/` if it contains saved entries, then extract the new library ZIP **over** the module directory without deleting its existing packs database. Restart Foundry after changing `module.json`. Never delete the pack data as part of a normal version update.
