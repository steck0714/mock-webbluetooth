# Mock-WebBluetooth（简体中文）

🇯🇵 [日本語](README.ja.md) | 🇺🇸 [English](README.en.md) | 🇨🇳 [简体中文](README.zh.md)

## 关于

这是一个以 Web Bluetooth API 兼容性为目标的扩展型兼容性 API。

本项目并不旨在直接移植某个特定浏览器的内部实现，而是希望在保持 Web Bluetooth API 兼容性的同时，为不同运行环境提供独立实现和扩展的空间。

## 🌐 实现

### [pyside6-webbluetooth](https://github.com/steck0714/pyside6-webbluetooth)

面向 PySide6 / QtWebEngine 应用程序的 Web Bluetooth API 实现。

- 提供 `navigator.bluetooth`
- 与实际 BLE 设备进行通信
- 原生设备选择对话框
- 实时扫描
- 按 Origin 管理设备权限
- 帧级 Origin 验证
- 访问 GATT Service / Characteristic / Descriptor
- GATT Blocklist
- 支持 `requestDevice()` 的主要过滤处理
- 不阻塞 Qt UI 的异步 BLE Worker
- QtWebEngine / QWebChannel 桥接

## 设计

Web Bluetooth 涉及 BLE 扫描、连接、GATT 探索、read/write/notify 等异步操作，这些操作可能需要较长时间。

因此，实现尽量避免长时间阻塞 QtWebEngine 的 UI 线程。

```text
网页
   │
   │ navigator.bluetooth
   ▼
Web Bluetooth 兼容层
   │
   │ QWebChannel
   ▼
Bluetooth Bridge
   │
   ├── Origin / Frame 验证
   ├── 权限管理
   ├── 设备选择
   ├── GATT Blocklist
   └── API / 参数验证
   │
   ▼
BLE Worker / asyncio
   │
   │ bleak
   ▼
BLE 设备
```

## 兼容性

Chrome / Chromium 的 Web Bluetooth 行为被作为兼容性参考，但本项目不是 Chrome 内部实现的直接移植。

> Web Bluetooth 兼容 ≠ Chrome 克隆

项目重点是 API 兼容性，同时允许针对 Python、PySide6、QtWebEngine 和底层 BLE 后端进行独立实现。

## 安全性

设计考虑了 Web Bluetooth 的安全模型，包括按 Origin 管理权限以及按 Frame 验证 Origin。

通过 GATT Blocklist 限制对受保护 Service、Characteristic 和 Descriptor 的访问。

## 状态

⚠️ 项目处于早期阶段。

实际行为可能因运行环境、浏览器兼容层以及所使用的 BLE 设备而不同。在将其用于硬件控制组件或安全边界之前，请务必检查源代码和运行环境。

## License

MIT License
