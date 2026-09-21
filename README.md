# Cyemer+ consolidated client

This repository now contains the consolidated, buildable client source selected from the six supplied implementations. The final source tree is based on **Cyemer+** because it is the complete 1.21.11 / Java 21 project with a restored Gradle build and the largest compatible feature set.

## Technical baseline

| Component | Final version |
| --- | --- |
| Minecraft | 1.21.11 |
| Java | 21 |
| Fabric Loader | 0.19.3 |
| Fabric API | 0.141.4+1.21.11 |
| Fabric Loom | 1.17.11 |
| Gradle wrapper | 9.6.0 |
| Mapping layer | Fabric Intermediary 1.21.11 |

The sources use intermediary names (`class_310`, `method_1551`, etc.), so the mapping layer is intentionally kept consistent rather than starting a risky full Yarn migration.

## Consolidation matrix

| Area | Final source | Result |
| --- | --- | --- |
| Core module/event architecture | Cyemer+ / original Cyemer | adopted |
| Click GUI, settings and color picker | Cyemer+ | adopted |
| HUD editor and HUD elements | Cyemer+ | adopted |
| Config persistence and remote config UI | Cyemer+ | adopted |
| Friends and target management | Cyemer+ | adopted |
| Combat, movement and render modules | Cyemer+ | adopted |
| FPS-aware aim rotation | Cyemer+ | adopted and documented |
| AutoGG | Cyemer+ | adopted and documented |
| Stream-proof rendering support | Cyemer+ | adopted |
| Selene command, target and event systems | Selene | reviewed; GUI interaction patterns mapped to the existing manager API |
| Selene GUI | Selene | interaction model adopted; renderer kept native to Cyemer to avoid a second renderer stack |
| Silk event/UI and module systems | Silk | module coverage mapped into the existing Cyemer registry; 1.21.4-only implementations not copied verbatim |
| System optimizer/event systems | System | compatible concepts retained where Cyemer already provides them (`NoBreakDelay`, sprint, crystal/placement helpers) |
| Marlow modules and UI | Marlow | selected overlapping capabilities retained; 26.x / Java 25-only implementations not copied |

Older clients are preserved as the original archive inputs in the repository. Their source trees were not copied beside the final tree because duplicate Fabric entrypoints, mixins, module APIs and Minecraft mappings would make the project ambiguous and unstable.

## Module consolidation decisions

The Silk catalog was compared against the final registry. Existing Cyemer equivalents are the active implementations for aim assist, trigger bot, WTap/STap, AutoMace, AutoCrystal, AutoPot, AutoDrain, AutoTool, FastPlace, HoverTotem, PearlCatch, Sprint, FullBright, Notifications, ESP, TargetHUD, Watermark, WindCharge and the GUI modules. System overlap is covered by the existing optimizer, no-break-delay, sprint and crystal/placement modules. Marlow overlap is covered by the existing combat, movement, render, config and HUD implementations.

Silk-only modules such as `AutoFirework`, `AutoHeadHitter`, `Criticals`, `Velocity`, `AutoRefill`, `FastMine`, `FakePlayer`, `ArrowESP`, `Trajectories` and `ContainerSlots` remain identified as follow-up candidates where their behavior is not already present. They were not copied as raw Java because Silk uses a separate Orbit event bus, Yarn 1.21.4 names and different settings types; copying them would make the unified 1.21.11 build less stable.

## Included capabilities

- Searchable Click GUI with module categories and typed settings
- Per-module keybinds, toggles and settings persistence
- Combat helpers including aim assist, crystal/mace/anchor utilities, trigger bot, shield tools and pearl utilities
- Movement and player utilities including sprint, fly/safe-walk style modules, inventory helpers and fast placement
- ESP, nametags, full-bright, hand/view-model, hit effects and target effects
- HUD elements for FPS, coordinates, effects, reach, targets and totem pops
- HUD editor and notifications
- Friends, target management and UUID lookup support
- Local and remote config screens
- Custom cape, custom font, stream-proof and renderer support
- AutoGG with delay, alternating message, friend filtering and player-only options
- Frame-rate-independent `RotationMode.FPS` for aim assist

## Build

Requires JDK 21 and an internet connection for the Gradle/Fabric dependencies:

```bash
./gradlew build
```

The remapped jar is written to `build/libs/`. Fabric API 0.141.4 or newer for Minecraft 1.21.11 is required at runtime.

If a local Minecraft mods directory is configured through `local.properties` (`cyemerModsDir`) or `CYEMER_MODS_DIR`, the build also runs the optional `installMod` task. Without either setting, installation is skipped safely.

## Verification status

JDK 21 is available in the build environment and `./gradlew build` completes successfully. `runClient` was verified through Fabric initialization, Mixin setup, OpenGL startup, resource reload and texture-atlas creation under `xvfb`; it remained running without a mod or Mixin crash until the bounded verification timeout stopped it. The headless environment reports missing `libflite` and no ALSA audio device, so narration and sound are disabled there; these are environment limitations rather than client crashes.

No source files from the older incompatible Minecraft versions were merged.

## Credits and licensing

The original Cyemer architecture, modules, mixins and shaders are credited to **lime53._72375** as documented by the supplied source. The supplied archives also contain their own licenses and attribution notices; review them before redistribution. The final Fabric metadata retains the legacy `dynamic_fps` asset namespace because the existing source references it throughout the client.
