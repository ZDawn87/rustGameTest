
# Fyrox Game Development Setup Guide

This guide helps you set up a complete development environment for building games with the Fyrox engine in Rust using VS Code.

## Quick Setup

Run the setup script to automatically configure VS Code:

```bash
curl -sSL https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/setup-vscode.sh | bash
```

Or clone this repository and run:

```bash
./setup-vscode.sh
```

## Manual Setup

### Prerequisites

1. **Install Rust** (if not already installed):
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   source ~/.cargo/env
   ```

2. **Install VS Code**: Download from [code.visualstudio.com](https://code.visualstudio.com/)

3. **Enable VS Code CLI**: 
   - **macOS**: Open VS Code → Command Palette (⌘+⇧+P) → "Shell Command: Install 'code' command in PATH"
   - **Linux**: Usually available after package installation
   - **Windows**: Check "Add to PATH" during installation

### Essential VS Code Extensions

Install these extensions manually or use the setup script:

#### Core Extensions
```bash
code --install-extension rust-lang.rust-analyzer
code --install-extension vadimcn.vscode-lldb
code --install-extension usernamehw.errorlens
code --install-extension serayuzgur.crates
code --install-extension tamasfe.even-better-toml
```

#### Recommended Extensions
```bash
code --install-extension eamodio.gitlens
code --install-extension pkief.material-icon-theme
code --install-extension ms-vscode.vscode-todo-highlight
code --install-extension gruntfuggly.todo-tree
```

## Project Structure

A typical Fyrox project structure:

```
your-game/
├── .vscode/                 # VS Code configuration
│   ├── settings.json        # Editor settings
│   ├── launch.json          # Debug configuration
│   ├── tasks.json           # Build tasks
│   └── extensions.json      # Recommended extensions
├── src/                     # Rust source code
│   ├── main.rs             # Game entry point
│   ├── lib.rs              # Game modules
│   ├── player.rs           # Player systems
│   └── ...                 # Other game modules
├── data/                   # Game assets
│   ├── models/             # 3D models (.fbx, .rgs)
│   ├── textures/           # Textures and materials
│   ├── sounds/             # Audio files
│   ├── scenes/             # Fyrox scene files (.rgs)
│   └── scripts/            # Game scripts
├── editor_data/            # FyroxEd editor files
├── Cargo.toml              # Project configuration
├── .gitignore              # Git ignore rules
└── README.md               # Project documentation
```

## VS Code Configuration

The setup script creates optimized VS Code settings for Fyrox development:

### Key Features
- **rust-analyzer** with clippy integration
- **Format on save** with automatic import organization
- **Debugging support** for both applications and tests
- **Build tasks** for common cargo commands
- **File associations** for Fyrox-specific formats
- **Performance optimizations** for large projects

### Debugging

Launch configurations are pre-configured for:
- **Debug executable**: Debug your game with breakpoints
- **Debug unit tests**: Debug individual test functions

Use `F5` to start debugging or `Ctrl+Shift+P` → "Debug: Start Debugging"

### Build Tasks

Available tasks (`Ctrl+Shift+P` → "Tasks: Run Task"):
- **Rust: cargo build** - Build the project
- **Rust: cargo run** - Run the game
- **Rust: cargo test** - Run tests
- **Rust: cargo check** - Quick syntax check
- **Rust: cargo clippy** - Code linting

## Development Workflow

### 1. Create a New Fyrox Project

```bash
# Create new Rust project
cargo new my-game --bin
cd my-game

# Add Fyrox dependency
cargo add fyrox

# Run the setup script
curl -sSL https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/setup-vscode.sh | bash
```

### 2. Basic Game Structure

Create a minimal Fyrox game in `src/main.rs`:

```rust
use fyrox::{
    core::pool::Handle,
    engine::{Engine, EngineInitParams},
    event::Event,
    event_loop::{ControlFlow, EventLoop},
    scene::Scene,
    window::WindowBuilder,
};

struct Game {
    scene: Handle<Scene>,
}

impl Game {
    pub fn new() -> Self {
        Self {
            scene: Handle::NONE,
        }
    }
}

fn main() {
    let event_loop = EventLoop::new();
    let window_builder = WindowBuilder::new().with_title("My Fyrox Game");
    
    let mut engine = Engine::new(EngineInitParams {
        window_builder,
        resource_manager: Default::default(),
        serialization_context: Default::default(),
    }).unwrap();
    
    let mut game = Game::new();
    
    event_loop.run(move |event, _, control_flow| {
        match event {
            Event::RedrawRequested(_) => {
                engine.update(1.0 / 60.0, control_flow, &mut game, Default::default());
            }
            Event::WindowEvent { event, .. } => {
                if let Some(os_event) = engine.window_event_handler(&event) {
                    engine.user_interface.process_os_event(&os_event);
                }
            }
            _ => *control_flow = ControlFlow::Poll,
        }
    });
}
```

### 3. Development Commands

```bash
# Check code without building
cargo check

# Build the project
cargo build

# Run the game
cargo run

# Run with optimizations (release mode)
cargo run --release

# Run tests
cargo test

# Lint code
cargo clippy

# Format code
cargo fmt
```

### 4. Using FyroxEd Editor

Install and use the Fyrox Editor for visual scene creation:

```bash
cargo install fyroxed
fyroxed
```

## Troubleshooting

### Common Issues

**Extension not working**: Reload VS Code window (`Ctrl+Shift+P` → "Developer: Reload Window")

**rust-analyzer slow**: Exclude target directory in settings (done automatically by setup script)

**Debug not working**: Ensure CodeLLDB extension is installed and working

**Build errors**: Check that Rust is properly installed: `rustc --version`

### Performance Tips

- Use `cargo check` for quick syntax validation
- Use `cargo clippy` for code quality suggestions
- Enable "format on save" for consistent code style
- Use `.gitignore` to exclude `target/` and `editor_data/` directories

## Useful Resources

- [Fyrox Book](https://fyrox-book.github.io/) - Official documentation
- [Fyrox Examples](https://github.com/FyroxEngine/Fyrox/tree/master/examples) - Code examples
- [Rust Book](https://doc.rust-lang.org/book/) - Learn Rust programming
- [VS Code Rust Guide](https://code.visualstudio.com/docs/languages/rust) - VS Code Rust support

## Contributing

To contribute to this setup:

1. Fork this repository
2. Make your changes
3. Test with a fresh Fyrox project
4. Submit a pull request

## License

This setup guide and scripts are provided under the MIT License.
