# Huawei MateBook X Pro 2023 — OpenCore EFI

**当前版本：v2.6.0**（2026-11-26）— WiFi 启动自动重连 workaround（LaunchDaemon 修复 IO80211 scan HALTED）；IntelBTPatcher 启用；华为 BLE 鼠标配对成功（重置清 bond）。详见 [CHANGELOG.md](CHANGELOG.md)。

## 适用机型
- Huawei MateBook X Pro 2023（i7-1360P / Iris Xe）
- 其他华为 TGL/RPL 笔记本可能兼容（HWEC EC 路径相同）

## macOS 版本
- macOS Sequoia 15.x（已验证 15.8.3）→ `efi-sequoia/`
- macOS Tahoe 26.x（需 WhateverXe 显卡加速包）→ `efi-tahoe/`

第一次安装更建议先用 Sequoia。Tahoe 因系统不再自带 AppleHDA，参考 EFI 里没有 AppleALC，内置扬声器和麦克风可能没有声音。

## 功能状态
| 功能 | 状态 |
|---|---|
| 电池显示 | ✅ HuaweiECBattery 自研驱动 |
| AC 拔插识别 | ✅ 响应 ≤10s |
| 充电功率显示 | ✅ 90W（已修正，读 EC PD 协商功率） |
| 电池温度 | ✅ 实时读取（EC B0TM，0.1K 单位） |
| 电池 OEM/型号 | ✅ DESAY / HB5781P1EEW-31T |
| 显卡加速 | ✅ WhateverGreen / WhateverXe |
| 声卡 | ✅ AppleALC 自定义 layout 99（CX11970，扬声器+耳机自动切换） |
| WiFi | ✅ AirportItlwm（Intel AX211，启动自动重连 workaround） |
| 蓝牙 | ✅ IntelBluetoothFirmware + BlueToolFixup + IntelBTPatcher（Apple 设备 + 华为 BLE 鼠标均正常） |
| 触控板 | ✅ VoodooI2C 自定义修复（D0上电+polling模式，手势完整） |
| 触屏 | ✅ VoodooI2CTouchscreenHIDEventDriver（绝对坐标，SSDT-TPL1） |
| 启动速度 | ✅ ~30s（reset 2s 超时优化） |
| 睡眠 | ⚠️ 未充分验证 |

## 依赖
- OpenCore 1.0.x
- Lilu
- VirtualSMC
- ECEnabler（诊断增强版，含 ECEDIAG 属性用于调试）
- HuaweiECBattery（自研，基于 VirtualSMC SMCBatteryManager fork）

## 安装方法
1. 将 EFI 目录复制到 ESP 分区
2. 用 GenSMBIOS 或 macserial 生成自己的 Serial/MLB/ROM/SystemUUID 填入 config.plist（发布版中这些值为 `REPLACE_ME`，**必须替换后才能使用**，不要使用或提交别人的序列号）
3. BIOS 设置：Disable Secure Boot、Fast Boot；AHCI mode
4. 从 OpenCore 引导安装或启动 macOS
5. 安装 WiFi 启动自动重连 workaround（解决启动后扫描列表为空）：
   ```bash
   sudo cp scripts/wifi-reset.plist /Library/LaunchDaemons/com.local.wifi-reset.plist
   sudo chown root:wheel /Library/LaunchDaemons/com.local.wifi-reset.plist
   sudo chmod 644 /Library/LaunchDaemons/com.local.wifi-reset.plist
   sudo launchctl bootstrap system /Library/LaunchDaemons/com.local.wifi-reset.plist
   ```
   该 daemon 在启动约 25s 后自动关闭再打开 WiFi，触发 IO80211 扫描状态机恢复并自动重连已知网络。

无线：Intel AX211 使用 AirportItlwm（ForbiddenItlwm fork），原生 WiFi 界面直接连接，无需 HeliPort。安装器里没有 Wi-Fi，请准备完整离线安装包或有线网卡。

## 已知限制
- AC 拔插响应 ≤10s（无 ACPI 通知，靠轮询）
- 非满充充电显示和睡眠唤醒待进一步验证
- USB 扩展坞音频需手动切换输出（macOS 设计行为，非 bug）
- Apple Music 蓝牙耳机播放受 DRM 限制（黑苹果固有限制）

提示：华为 BLE 鼠标首次配对需长按重置键进入配对模式，清除旧 bond 后重新配对即可。
- WiFi 启动后需约 30s 自动重连：IO80211 scan 状态机时序竞争导致开机扫描列表为空，已用 LaunchDaemon 自动重置 workaround（见安装方法第 5 步）

## 推荐工具（非 EFI 组件）
- [Atoll](https://github.com/Ebullioscopic/Atoll)：灵动岛风格菜单栏工具，黑苹果可用

## 许可证
- OpenCore：BSD-3-Clause
- Lilu / VirtualSMC：BSD-3-Clause
- ECEnabler：GPL-3.0
- HuaweiECBattery：BSD-3-Clause（基于 VirtualSMC）
- 其他 kext 见各自上游许可证

## 致谢
- acidanthera 团队（OpenCore / Lilu / VirtualSMC）
- 1Revenger1（ECEnabler）
- [nullmoth/1401](https://github.com/nullmoth/1401)（初始参考配置）
- 各 kext 作者