# redot-physx-runtime

Runtime binaries for the [Redot Engine](https://github.com/Redot-Engine/redot-engine) **PhysX module**: the NVIDIA PhysX GPU runtime, the Blast destruction DLLs and the NVIDIA Flow runtime DLLs, packaged so the editor can download them on demand instead of shipping them with every build.

## Layout

- `manifest.json` — the download manifest the engine consumes: runtime version, bundle URL, and the SHA-256 + size of the bundle and of every file inside it.
- Bundles themselves live as **GitHub Release assets** (GitHub blocks files over 100 MB in git, and the Windows bundle is ~110 MB). The manifest's `platforms.<platform>.bundle.url` points at the release asset.

## How the engine uses this

When a project selects `PhysX` as its 3D physics engine, the editor checks for the runtime files next to its executable. If they are missing, it offers to download the bundle listed in `manifest.json`, verifies every SHA-256, installs the files beside the editor executable, and restarts.

## Updating the runtime

1. Build/test the new SDK runtime.
2. Attach the new bundle zip to a release tagged after the SDK version (e.g. `v5.12.0` → `physx-runtime-5.12.0-windows.zip`).
3. Update `manifest.json` (version, URL, hashes, sizes) and commit.

The engine's manifest URL constant then moves to the new tag — no other code change needed.

## Status

- Windows x86_64: **available** (PhysX SDK 5.12.0 — GPU runtime, Blast 5.0.6, Flow 2.2.0).
- Linux x86_64: **available** (same SDK set as shared libraries; the nine Blast libraries are hard-linked into the Linux editor, so they must be present before it can start — the in-editor download offer can only top up the lazily-loaded PhysXGpu/Flow runtimes, the bundle otherwise serves manual installs).
