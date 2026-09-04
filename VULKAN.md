# Windows Vulkan downloads

This fork keeps Dikte's interface and model catalogues unchanged.  Its only
runtime difference is that Windows x64 machines with a Vulkan loader download
ready-made Vulkan builds of whisper.cpp and llama.cpp from this fork's latest
GitHub release.

The in-app update link also follows `GryAsl/dikte`, so updating does not replace
the Vulkan-aware portable build with the upstream Windows package.

## What the release contains

- `Dikte-<version>-x64-portable.zip`: extract and run `dikte/Dikte.exe`.
- `dikte-whisper-win-vulkan-x64.zip`: `whisper-server.exe` and all adjacent
  runtime DLLs.
- `dikte-llama-win-vulkan-x64.zip`: `llama-server.exe` and all adjacent runtime
  DLLs.

The server archives are downloaded by Dikte itself.  End users do not need
Visual Studio, CMake, Ninja or the Vulkan SDK.  They do need a graphics driver
that provides the Windows Vulkan loader (`vulkan-1.dll`).

## Reproducible builds

`.github/workflows/vulkan-runtimes.yml` runs on GitHub's Windows runner,
installs LunarG Vulkan SDK 1.4.357.0 and builds only the two server targets with
`GGML_VULKAN=ON`.  Source revisions are pinned:

- whisper.cpp: `371b5a7561823ab2bb32142d2751e35e7534727b` (`b4938`)
- llama.cpp: `c1d0e7a004015f23bc0233470b747b596f29b264` (`v0.3.0`)

The ordinary Dikte release workflow calls the runtime workflow and publishes
all three Windows zip files in one release. GitHub publishes a SHA-256 digest
for every release asset; Dikte verifies that digest before extracting anything.

To update a backend, change its pinned commit in the workflow, let CI build the
archives, and publish a new Dikte release. Do not commit compiled binaries to
the repository.
