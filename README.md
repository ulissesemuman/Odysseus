# Odysseus

Odysseus is a native Windows runtime for Xbox Series X|S titles. The title's
x86-64 code runs in the runtime process, while Odysseus provides the platform
layer and translates the title's console GPU work to Direct3D 12 and DXIL.

Odysseus is based on the open-source
[Marvisak/VIctory](https://github.com/Marvisak/VIctory) project and carries
on its GPL-3.0-or-later licensing.

## Requirements

- 64-bit Windows 10 or 11
- Rust pinned by `rust-toolchain.toml`
- Visual Studio Build Tools with the x64 MSVC toolset and a Windows SDK
- A Direct3D 12 GPU with Shader Model 6.6
- Mesh shaders for 3D world rendering
- An unpacked Xbox Series title that you own

## Configuration

Copy `victory.example.toml` to `victory.toml` and set `game_dir` to the
executable's parent directory outside the repository:

```toml
[paths]
game_dir = 'D:\MyGames\SeriesX'
test_titles = []
```

Run the configuration and build commands:

```powershell
cargo xtask install-hooks
cargo run -p victory -- config
cargo test --workspace
cargo build --release -p victory
```

## Running

```powershell
.\target\release\victory.exe run `
  "D:\MyGames\SeriesX\game_scarlett_master.exe" `
  --execute
```

## Shader cache

Pretranslate shaders into the local cache before running the title:

```powershell
.\target\release\victory.exe warm `
  "D:\MyGames\SeriesX\game_scarlett_master.exe"
```

## Testing

```powershell
cargo test --workspace
cargo xtask policy
```

Tests that need proprietary game files read paths from environment variables
and skip when those variables are unset.

## Data policy

This repository must not contain game executables, archives, assets, dumps,
saves, cache files or derived proprietary data. Game paths, saves and caches
belong outside the repository. See [docs/asset-policy.md](docs/asset-policy.md).

## License

GPL-3.0-or-later. See [LICENSE](LICENSE).
