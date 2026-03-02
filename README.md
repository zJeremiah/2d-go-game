This code lives in codeberg.
Please see https://codeberg.org/zjeremiah/2d-go-game

# 2D Go Game Framework

A JSON-driven 2D game framework for Go, built on [Ebitengine](https://ebitengine.org). Define scenes, props, animations, and triggers in JSON files and let the engine handle rendering, input, audio, and state management.

## Status

Work in progress. See [docs/PLAN.md](https://codeberg.org/zjeremiah/2d-go-game/src/branch/main/docs/PLAN.md) for the full architecture and build plan.

## Features (planned)

- **Scene system** -- JSON-defined scenes with props, player characters, and triggers
- **Spritesheet animation** -- Aseprite JSON import with generic types extensible to other tools
- **Props** -- unified scene elements (backgrounds, decorations, interactables) with Y-sorted rendering for top-down depth
- **Player character** -- free-form pixel movement with directional animations
- **Event/trigger system** -- declarative cause-and-effect chains defined in JSON
- **Audio** -- sound pool for overlapping SFX, music playback, wired to events
- **Save/load** -- delta-based scene state persistence via SQLite (CGo-free)
- **UI** -- data-driven menus and dialogs using the same trigger/action system
- **Debug console** -- in-game overlay for inspecting state and firing events at runtime

## Requirements

- Go 1.25+
- [Ebitengine system dependencies](https://ebitengine.org/en/documents/install.html) (varies by platform)
- [Ebitengine repo](https://github.com/hajimehoshi/ebiten)
## Install

```bash
go get codeberg.org/zjeremiah/2d-go-game/engine
```

## Quick Start

```go
package main

import (
    "log"
    "codeberg.org/zjeremiah/2d-go-game/engine"
)

func main() {
    e := engine.New(engine.Config{
        Title:        "My Game",
        ScreenWidth:  640,
        ScreenHeight: 480,
    })
    if err := e.Run(); err != nil {
        log.Fatal(err)
    }
}
```

## Development

Requires [just](https://github.com/casey/just) for task running. Install it via your package manager or `cargo install just`, or `brew install just`

### Available recipes

```bash
just test                # run all tests with coverage and race detection
just test-pkg engine     # run tests for a specific package (e.g. engine)
just build               # build all packages
just vet                 # run go vet
just tidy                # run go mod tidy
```

Run `just --list` to see all available recipes.

## Architecture

The framework is a single importable package (`engine/`) with these core components:

| File | Purpose |
|------|---------|
| `types.go` | Shared types (`Rect`, `Size`) |
| `sprite.go` | Spritesheet types, `Sprite` playback, `LoadImageData` |
| `aseprite.go` | Aseprite JSON format parser |
| `assets.go` | Asset cache for images, spritesheets, audio |
| `engine.go` | Core game loop wrapping Ebitengine |
| `scene.go` | Scene manager and JSON scene loading |
| `prop.go` | Props with Y-sorted three-pass rendering |
| `player.go` | Player character with movement and animation |
| `camera.go` | Camera following the player |
| `input.go` | Configurable key bindings and action mapping |
| `trigger.go` | Event bus and JSON-defined trigger/action system |
| `audio.go` | Sound pool and music playback |
| `store.go` | SQLite key-value data store |
| `save.go` | Save/load system with delta-based scene state |
| `ui.go` | Menus and dialogs |
| `console.go` | In-game debug console |

## License

TBD
