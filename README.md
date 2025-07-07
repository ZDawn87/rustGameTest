# rustGameTest

Yes! Fyrox is a 3D game engine for Rust with a specific project structure. Here's the typical file organization for a Fyrox game:

## Basic Project Structure

```
my_game/
├── Cargo.toml
├── src/
│   ├── main.rs           # Entry point
│   ├── lib.rs            # Game logic module
│   ├── player.rs         # Player-specific code
│   ├── weapon.rs         # Weapon systems
│   └── ...               # Other game modules
├── data/                 # Game assets
│   ├── models/           # 3D models (.fbx, .rgs)
│   ├── textures/         # Image files
│   ├── sounds/           # Audio files
│   ├── scenes/           # Scene files (.rgs)
│   ├── materials/        # Material definitions
│   └── scripts/          # Game scripts
├── editor_data/          # FyroxEd editor files
└── target/               # Compiled output
```

## Key Files

**Cargo.toml** - Dependencies typically include:
```toml
[dependencies]
fyrox = "0.34"  # or latest version
```

**main.rs** - Entry point with the game loop:
```rust
use fyrox::{
    core::pool::Handle,
    engine::{Engine, EngineInitParams, SerializationContext},
    event_loop::EventLoop,
    scene::Scene,
};

// Game state structure
pub struct Game {
    scene: Handle<Scene>,
}
```

**Scene Files (.rgs)** - Fyrox scenes are serialized in a custom format and stored in `data/scenes/`. These contain:
- Scene graph with nodes
- Lighting setup
- Physics bodies
- Prefab instances

## Asset Organization

**data/** directory contains all game assets:
- **models/**: 3D models (FBX imports convert to .rgs format)
- **textures/**: Diffuse, normal, roughness maps, etc.
- **sounds/**: Audio files for the sound system
- **materials/**: Material definitions with shader parameters
- **scripts/**: Lua or native scripts for game logic

## Editor Integration

Fyrox games are typically developed using **FyroxEd** (the visual editor), which creates:
- **editor_data/**: Editor-specific files and settings
- Scene files with visual node hierarchies
- Prefab systems for reusable objects

The engine uses a node-based scene graph where everything inherits from a base `Node` type, including meshes, lights, cameras, rigid bodies, and custom game objects.

Would you like me to elaborate on any specific aspect of the Fyrox project structure or architecture?
