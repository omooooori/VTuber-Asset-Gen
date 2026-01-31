# VTuber-Asset-Gen

AI-powered VTuber asset generator — create and customize 2D avatar parts using generative AI, optimized for Spine models.

Generate hair, outfits, accessories, and other avatar parts from text prompts, then export them as Spine-compatible assets ready for use in VTuber applications.

## Motivation

Creating custom VTuber avatar parts traditionally requires illustration skills and manual work. By combining generative AI (Stable Diffusion / ComfyUI) with Spine's skeletal animation system, we can dramatically lower the barrier to VTuber asset creation.

## Features (Planned)

- Text-to-asset generation via ComfyUI server integration
- Style-consistent generation (match existing avatar art style)
- Spine atlas/attachment export format
- Asset preview with Spine skeleton overlay
- Part categories: hair, face parts, outfits, accessories, backgrounds
- Batch generation and variation control
- Mobile-first UI: Jetpack Compose (Android) / SwiftUI (iOS)

## Architecture

```
VTuber-Asset-Gen/
├── shared/                    # KMP shared module
│   └── src/
│       ├── commonMain/
│       │   └── kotlin/
│       │       ├── data/
│       │       │   ├── comfyui/      # ComfyUI API client
│       │       │   ├── civitai/      # CivitAI model browser (optional)
│       │       │   ├── spine/        # Spine atlas/JSON utilities
│       │       │   └── repository/
│       │       ├── domain/
│       │       │   ├── model/        # Asset, Prompt, SpineAttachment
│       │       │   └── usecase/      # GenerateAssetUseCase, ExportUseCase
│       │       └── di/
│       ├── androidMain/
│       └── iosMain/
├── androidApp/                # Jetpack Compose UI
│   └── src/main/kotlin/
│       ├── ui/                # GenerateScreen, PreviewScreen, ExportScreen
│       ├── viewmodel/
│       └── di/
└── iosApp/                    # SwiftUI
```

## Tech Stack

- **Shared**: Kotlin Multiplatform, Ktor Client, Kotlinx Serialization, Koin
- **Android**: Jetpack Compose, Coil (image loading)
- **iOS**: SwiftUI
- **AI Backend**: ComfyUI server (REST + WebSocket API)
- **Asset Format**: Spine JSON/Atlas format
- **Architecture**: Clean Architecture + MVI

## How It Works

```
1. User enters prompt ("gothic lolita dress, anime style")
        ↓
2. App sends request to ComfyUI server with appropriate workflow
        ↓
3. Stable Diffusion generates the asset image
        ↓
4. Post-processing: background removal, edge cleanup
        ↓
5. Preview asset overlaid on Spine skeleton
        ↓
6. Export as Spine-compatible attachment (atlas region + JSON)
```

## Related Projects

- [ComfyUI](https://github.com/Comfy-Org/ComfyUI) — AI image generation backend
- [CivitDeck](https://github.com/omooooori/CivitDeck) — CivitAI mobile client (shares API patterns)
- [spine-runtimes](https://github.com/EsotericSoftware/spine-runtimes) — Spine animation runtime

## Status

**Concept stage** — exploring ComfyUI API integration and Spine asset format.

## Contributing

Contributions are welcome! Please open an issue to discuss your ideas before submitting a PR.

## License

[MIT](LICENSE)
