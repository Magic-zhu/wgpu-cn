# wgpu

一个跨平台的基于 WebGPU 的图形计算库
要开始使用 API，先创建一个实例

## 后端图形 API

⚠️ WIP: 现在不是所有图形 API 都能手动设定. 在 window 和 linux 下 **Vulkan & GLES 一直都是支持的**   点击这里查看细节[#3514](https://github.com/gfx-rs/wgpu/issues/3514)

- **`dx12`** *(enabled by default)* — 在 windows 上支持 DX12
- **`metal`** *(enabled by default)* — 在苹果生态下支持 metal
- **`webgpu`** *(enabled by default)* — 在 wasm 下支持 webgpu Enables the WebGPU 🔴 在`emscripten`下不支持
- **`angle`** — Enables the GLES backend via [ANGLE](https://github.com/google/angle) on macOS.
- **`vulkan-portability`** — 在苹果生态下支持 vulkan
- **`webgl`** — 在 wasm 环境下支持 GLES.
  - ⚠️ WIP: 目前还将支持 GLES 对任何其他目标的依赖关系

## 支持的 shader 语言

- **`wgsl`**  默认支持
- **`spirv`**
- **`glsl`**
- **`naga-ir`**

## [记录&追踪](https://docs.rs/wgpu/latest/wgpu/#logging--tracing)

以下特性对 WebGPU 后端没有任何作用

- **`strict_asserts`** — 即使在发布版本中，也应用运行时检查。这些是在所有构建中在公共 API 中进行的验证之外的
- **`api_log_info`** — 在 info 而不是跟踪级别记录所有 API 入口点
- **`trace`** — 允许写入跟踪捕获文件 查看  [`Adapter::request_device`](https://docs.rs/wgpu/latest/wgpu/struct.Adapter.html#method.request_device "method wgpu::Adapter::request_device").
- **`replay`** — 允许对使用“ trace”特性编写的跟踪捕获文件进行反序列化。要重播跟踪文件，请使用[wgpu 播放器](https://github.com/gfx-rs/wgpu/tree/trunk/player).

## [其他](https://docs.rs/wgpu/latest/wgpu/#other)

- **`fragile-send-sync-non-atomic-wasm`** — 在 wasm 中执行  `Send` and `Sync`, 但只有在原子不启用的情况下
  WebGL/WebGPU 对象不能在线程之间共享。然而，人为地将它们标记为“发送”和“同步”也是有用的，这样可以更容易地编写跨平台代码。在多线程环境中，这在技术上是非常不安全的，但是在没有使用原子编译的 wasm 二进制文件中，我们知道我们肯定不是在多线程环境中。

## [功能别名](https://docs.rs/wgpu/latest/wgpu/#feature-aliases)

这些功能点并不是真正这个 crates 的功能, 而是复杂案例的方便速记

- **`wgpu_core`** — 在平台上启用任何非 webgpu 后端时启用
- **`naga`** –– 当启用任何非 wgsl 着色器输入时启用
