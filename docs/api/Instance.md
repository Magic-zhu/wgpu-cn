# Instance

```rust
pub struct Instance { /* private fields */ }
```

所有其他 wgpu 对象的上下文。 wgpu 实例
这是你在使用 wgpu 时创建的第一个东西。它的主要用途是创建适配器和画布(Surface)。
不一定要 keep alive
对应于 WebGPU GPU

## 实现

### any_backend_feature_enabled

```rust
pub const fn any_backend_feature_enabled() -> bool
```

如果为当前生成配置启用了任何后端特性，则返回 true
哪个特性使得这个方法返回 true 取决于目标平台:
MacOS/iOS: metal, vulkan-portability or angle
Wasm32: webgpu, webgl or Emscripten target.
All other: Always returns true

待办事项: 目前在大多数平台上还不可能选择使用所有功能，见 https://github.com/gfx-rs/wgpu/issues/3514
Windows: 默认使用 Vulkan and GLES
Linux: 默认使用 Vulkan and GLES

### any_backend_feature_enabled

```rust
pub fn new(_instance_desc: InstanceDescriptor) -> Self
```
