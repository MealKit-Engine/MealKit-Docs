# Requirements

## Running

- Windows 10 1803 or later.
- **A GPU with Direct3D 12 Shader Model 6.6 and resource binding tier 3** (Intel 11th generation (Xe) integrated graphics and newer have it). This is what the renderer's bindless design needs, not a performance target: there is no minimum spec beyond it. The game checks this at startup and, if the GPU falls short, exits with the reason and the driver version. Bindless resources (Shader Model 6.6) are used from the texture work onward.
- A CPU with SSE4.2.

How fast a game must run is the game developer's choice: the default settings stay light, and heavier features are turned on per project (reasons in the {engine}`decision log <docs/DECISIONS.md>`, 2026-11-25). `sandbox.exe --system-info` prints the CPU and GPU features and exits with code 2 when the GPU or CPU lacks a requirement above.

## Building

- Visual Studio 2022 or Build Tools 2022 with the "Desktop development with C++" workload (includes CMake).
- Python 3, only to regenerate the sample models.
- For the `ClangCL` comparison build: the Visual Studio component "C++ Clang tools for Windows".
- An internet connection for the first build, which downloads the DXC shader compiler (about 32 MB) once.
- For the `Vulkan` and `VulkanEditor` builds: a GPU driver with Vulkan 1.3. The Vulkan SDK is only needed for the validation layer (`--debug`).
