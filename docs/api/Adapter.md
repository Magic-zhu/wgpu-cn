# Adapter

"Adapter"（适配器）是指表示图形硬件设备的接口。它是与底层硬件交互的中间层，用于在应用程序和硬件之间进行通信和协调

## Struct wgpu::Adapter

```rust
pub struct Adapter { /* private fields */ }
```

物理图形和/或计算设备的句柄

适配器可用于打开连接到相应的 [`Device`](https://docs.rs/wgpu/latest/wgpu/struct.Device.html "struct wgpu::Device") 在宿主系统上使用[`Adapter::request_device`](https://docs.rs/wgpu/latest/wgpu/struct.Adapter.html#method.request_device "method wgpu::Adapter::request_device").

无需一直保持alive状态

对应 [WebGPU的 `GPUAdapter`](https://gpuweb.github.io/gpuweb/#gpu-adapter).


## 实现

### request_device

```rust
pub fn request_device(
    &self,
    desc: &DeviceDescriptor<'_>,
    trace_path: Option<&Path>
) -> impl Future<Output = Result<(Device, Queue), RequestDeviceError>> + WasmNotSend
```

请求到物理设备的连接，创建逻辑设备。
返回设备以及执行命令缓冲区的队列

#### 参数

`desc` - 对给定设备要求的特性和限制的描述。
`trace _ path` - 可用于 API 调用跟踪，如果 wgpu-core 中启用了该特性的话。

#### 异常

此适配器不支持 desc 指定的特性。
请求适配器时请求了不安全的特性，但没有启用。
请求的限制超过了适配器提供的值。
适配器不支持所有功能 wgpu 要求安全操作。

### create_device_from_hal


```rust
pub unsafe fn create_device_from_hal<A: HalApi>(
    &self,
    hal_device: OpenDevice<A>,
    desc: &DeviceDescriptor<'_>,
    trace_path: Option<&Path>
) -> Result<(Device, Queue), RequestDeviceError>
```

<span style="background:#fff88f">只在`wgpu_core`上可用</span>

将一个回调函数应用于该适配器的底层后端适配器。

如果该适配器由给定的后端API A（如Vulkan、Dx12等）实现，则将 hal_adapter_callback 应用于 Some(&adapter)，其中 adapter 是底层后端适配器类型 A::Adapter。

如果该适配器使用了不同的后端，则将 hal_adapter_callback 应用于 None。

在 hal_adapter_callback 运行期间，适配器被锁定为只读。如果回调函数尝试执行任何需要对适配器进行写访问的 wgpu 操作，将会导致死锁。当回调函数返回时，锁定将自动释放。

`安全性`

传递给回调的原始句柄不能手动销毁。

### is_surface_supported

```rust
pub fn is_surface_supported(&self, surface: &Surface<'_>) -> bool
```

返回此适配器是否可以呈现给传递的Surface。

### features

```rust
pub fn features(&self) -> Features
```
可用于在此适配器上创建设备的特性。

### limits

```rust
pub fn limits(&self) -> Limits
```

可用于在此适配器上创建设备的最佳限制。

### get_info

```rust
pub fn get_info(&self) -> AdapterInfo
```

获取关于适配器本身的信息

### get_downlevel_capabilities

```rust
pub fn get_downlevel_capabilities(&self) -> DownlevelCapabilities
```

获取关于适配器底层的能力

### get_texture_format_features

```rust
pub fn get_texture_format_features(
    &self,
    format: TextureFormat
) -> TextureFormatFeatures
```

返回此适配器对给定纹理格式支持的特性。
请注意，WebGPU 规范进一步限制了可用的用法/特性。要在设备上禁用这些限制，请求 Features: : TEXTURE _ ADAPTER _ SPECIFIC _ FORMAT _ FEATURES 特性。

### get_presentation_timestamp

```rust
pub fn get_presentation_timestamp(&self) -> PresentationTimestamp
```

### global_id

```rust
pub fn global_id(&self) -> Id<Self>
```

返回此适配器的全局唯一标识符。
对同一对象多次调用此方法将始终返回相同的值。对于从同一个实例创建的所有资源，返回的值肯定是不同的。