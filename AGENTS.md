# AGENTS.md

This file provides guidance to AI coding agents working with this repository.

## Project Overview

VTuber-Asset-Gen is a KMP mobile app that uses generative AI (ComfyUI / Stable Diffusion) to create 2D avatar parts for Spine-based VTuber models. Users describe what they want in text, and the app generates, previews, and exports Spine-compatible assets.

## Commands

```bash
# Android
./gradlew :androidApp:installDebug    # Build & install Android debug
./gradlew :androidApp:assembleRelease # Build Android release APK
./gradlew :shared:test                # Run shared module tests

# iOS
cd iosApp && pod install              # Install iOS dependencies
open VTuberAssetGen.xcworkspace       # Open in Xcode

# Code Quality
./gradlew ktlintCheck                 # Lint check
./gradlew ktlintFormat                # Auto-format
./gradlew detekt                      # Static analysis
```

## Architecture

### Tech Stack
- Kotlin Multiplatform (KMP) — shared logic
- Ktor Client — ComfyUI REST + WebSocket API
- Kotlinx Serialization — JSON parsing
- Koin — dependency injection
- Jetpack Compose (Android) / SwiftUI (iOS) — UI
- Clean Architecture + MVI pattern

### Module Structure

```
VTuber-Asset-Gen/
├── shared/
│   └── src/
│       ├── commonMain/
│       │   └── kotlin/
│       │       ├── data/
│       │       │   ├── comfyui/      # ComfyUI API client
│       │       │   │   ├── ComfyUIClient.kt       # REST + WebSocket
│       │       │   │   ├── WorkflowTemplate.kt    # Predefined workflows
│       │       │   │   └── dto/                    # API DTOs
│       │       │   ├── spine/        # Spine format utilities
│       │       │   │   ├── SpineAtlasWriter.kt    # Atlas file generation
│       │       │   │   └── SpineJsonEditor.kt     # Skeleton JSON modification
│       │       │   └── repository/
│       │       ├── domain/
│       │       │   ├── model/        # Asset, GenerationConfig, SpineAttachment
│       │       │   ├── repository/
│       │       │   └── usecase/
│       │       └── di/
│       ├── androidMain/
│       └── iosMain/
├── androidApp/
│   └── src/main/kotlin/
│       ├── ui/
│       │   ├── generate/      # Prompt input, style selection
│       │   ├── preview/       # Generated asset preview with skeleton overlay
│       │   └── export/        # Export settings and confirmation
│       ├── viewmodel/
│       └── di/
└── iosApp/
```

### Key Design Decisions

**ComfyUI Integration**
- Connect to user's ComfyUI server (local network or remote)
- Use predefined workflow templates for each asset type (hair, outfit, etc.)
- WebSocket for real-time generation progress
- Server address configurable in settings

**Spine Asset Format**
- Generated images are processed into Spine atlas regions
- Attachment definitions written to Spine JSON format
- Support for mesh attachments (for deformable parts)
- Preview uses a reference skeleton to show placement

**Image Post-Processing**
- Background removal (alpha channel)
- Edge cleanup and anti-aliasing
- Resolution/DPI adjustment for target Spine skeleton

## Development Guidelines

- ComfyUI workflow templates are stored as JSON in resources
- Spine format logic goes in `commonMain` — no platform dependencies
- Image processing may need platform-specific implementations (androidMain/iosMain)
- Keep ComfyUI client generic — it should work with any ComfyUI workflow
- Test Spine format output against actual Spine editor imports
- CivitDeck shares similar KMP patterns — reference its architecture for consistency

## Language

Communicate in English for this project.
