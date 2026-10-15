# ZFT Narration Library

Shared, mutable **content-only** storage module for ZFT Cinematics Narrate. The library supplies a Foundry JournalEntry compendium and directories for user-created graphics and soundtracks; the editing, library maintenance, playback, and upload UI belongs to ZFT Cinematics. The library itself includes **no scripts, UI, or automatic updater**.

## Release and compatibility

- Stable module source: **v0.1.0**, promoted from the locally installed `v0.1.0-dev.3` content package. The pack definition, GM permissions, and `flags.canUpload` behavior are unchanged from that tested build.
- Primary target: **Foundry VTT V13 Build 351**. Foundry V14 compatibility has **not** been verified.
- Integration: **ZFT Cinematics v0.2.0-dev.25** passed the owner's **9/9 live Foundry V13 tests** on October 15, 2026, including saved-narration Save/Load and soundtrack playback/controls. This does not separately certify cross-server migration or in-app library reinstallation.
- The library is a **persistent user-content store**. Source updates in GitHub do not modify any live data. Installing/upgrading a ZIP over an existing server is a separate, backup-required action.

## On-disk storage contract

```text
Data/modules/zft-narration-library/
  module.json
  packs/
    narrations/             # Foundry-managed JournalEntry/LevelDB data, created on use
  assets/
    graphics/               # user-uploaded narration images
    audio/                  # user-uploaded MP3, OGG, WAV soundtrack files
```

| Resource | Foundry reference |
| --- | --- |
| Saved narration / graphics index | `Compendium.zft-narration-library.narrations` |
| Graphics | `modules/zft-narration-library/assets/graphics/<filename>` |
| Audio | `modules/zft-narration-library/assets/audio/<filename>` |

- Package ID: `zft-narration-library`; compendium collection: `zft-narration-library.narrations` (type `JournalEntry`).
- GM compendium permissions: `GAMEMASTER: OWNER`; `PLAYER`, `TRUSTED`, and `ASSISTANT`: `NONE`. Unlock **ZFT Narrations** as GM to save and update passages or graphics metadata.
- `flags.canUpload: true` allows upload into the library module directory. ZFT Cinematics creates `assets/audio/` when needed. The distribution includes only `assets/graphics/.gitkeep` and `packs/.gitkeep`, **not** real saved passages, images, or soundtracks.
- Saved narration entries contain presentation preferences and media references. The internal `ZFT Graphics Index (Internal)` JournalEntry stores media metadata and is hidden from Cinematics' normal passage picker.
- Data is shared between worlds **using the same Foundry User Data directory**. Different installations/servers are not automatically synchronized.

## Data safety: do not reinstall over existing content

**Treat an installed library as valuable, mutable data, not a disposable package.** Removing/reinstalling/updating the module through a tool that replaces its directory may destroy the compendium or uploaded media. This manifest deliberately does not set `manifest` or `download` URLs; keep the module locked against update/uninstall in Foundry Setup.

To deploy a version change intentionally:

1. **Stop every Foundry process using the relevant User Data directory**, particularly before backing up or copying the LevelDB compendium.
2. Back up the **entire** `Data/modules/zft-narration-library/` directory including `packs/narrations/`, `assets/graphics/`, and `assets/audio/`. Include any legacy content from `Data/assets/zft-narration-library/graphics/` or `Data/worlds/<world-id>/zft-cinematics/narration/` if you are migrating it.
3. Overlay only the necessary package files on the existing directory **without deleting its contents**. Do **not** delete, overwrite with empty folders, or reset `packs/narrations/` or media assets. Verify backup and record counts before/after.
4. Start Foundry after a **module manifest change**; this is different from the client-only Ctrl+F5 procedure used for Cinematics JS/CSS updates.
5. As GM, verify compendium visibility and saved entries, then test Narrate Save/Load, graphic display, and audio selection/playback before resuming normal use.

**Updating the GitHub repository alone requires none of these actions** and does not change your installed server's library version or its data.

## Cleanup and migration

- Images saved in passages, in-use graphics, and graphics marked **Keep** are protected from automated removal.
- Unused graphics older than seven days can be suggested for **manual review**; Cinematics does not automatically delete server files. Unknown-age/untracked graphics are not deletion candidates without review.
- Soundtracks are **never pruned by the graphics cleanup tool**.
- The Cinematics Graphics Library supports manual legacy-image migration from previous locations, preserving originals. Migrate only after backing up.
- GitHub intentionally tracks package definitions and empty directory placeholders, **not actual player-created content or mutable LevelDB database files**. Never assume a GitHub source checkout contains your saved narrations.

## v0.1.0 release notes

- Promotes tested `0.1.0-dev.3` library structure to stable source, with no functional manifest changes beyond the version string.
- Includes `GAMEMASTER: OWNER` compendium access and `flags.canUpload: true` for shared library uploads.
- Retains the shared content layout needed by the tested Cinematics Narrate editor, graphics library, and soundtrack selection.
- Keeps update/uninstall safeguards and excludes saved content from the source distribution.

MIT license; see `LICENSE`.
