# Changelog

## v2.3.0 (2026-11-25)
- feat: 触控板完全修复（Goodix GXTP7863，从未工作到光标+手势完整）
- fix: I2C控制器D3hot→D0上电（PMCSR动态定位，替代硬编码偏移）
- fix: LPSS硬复位清除wedged总线状态机
- fix: polling时序隔离（init期间暂停polling，避免总线竞争）
- fix: post-reset SET_POWER（Linux i2c_hid parity）
- fix: 跳过Goodix不支持的GET_REPORT（避免总线卡死）
- fix: contact_count越界钳位（防panic）
- feat: HID子模块升级（Surface/Button Switch支持）
- 启用SSDT-TPD0
- 启动稳定性改善（PMCSR安全写入）

## v2.2.0 (2026-11-25)
- feat: 自定义 AppleALC layout 99（Conexant CX11970）
- fix: 内置扬声器无声/音量小（正确路径 0x26←0x23 + 0x1d←0x22）
- fix: 耳机插拔自动切换（组合输出组 + unsol verb）
- fix: Apple Music 播放无声（声卡路径修正后恢复）
- 已知限制：USB 扩展坞音频需手动切换；蓝牙耳机 Apple Music DRM 限制

## v2.1.0 (2026-11-25)
- fix: OEM/型号读取（ACPI_OBJECT Buffer 紧凑布局 Length@4/Pointer@8）
- feat: Manufacturer = DESAY, DeviceName = HB5781P1EEW-31T
- 电池信息全链路完成：容量/电压/循环/温度/序列号/OEM/型号/90W 全部真实值

## v2.0.0 (2026-11-25)
- feat: 电池温度实时读取（B0TM, 0.1K 单位）
- feat: 适配器功率修正为 90W（B0PW u24 LE mW）
- feat: 序列号改为 4 位 hex 格式（与固件 ECBM 一致）
- fix: AC 状态判断（adapterCount>0 时 EPWS 回退被跳过）
- fix: 启动卡死（OSDynamicCast(IOACPIPlatformDevice) 跨 kext metaClass 阻塞）
- fix: 启动早期 ACPICA 求值卡死（30s 门闩）
- fix: AC 插回瞬间电量突降（容量防抖）
- perf: AC 拔插响应从 ≤60s 优化到 ≤10s（NormalPollInterval 10s）
- chore: 清理诊断属性，发布干净正式版

已知限制：电池 OEM/型号仍显示 Unknown（EC 数据有效，ACPICA Buffer 求值问题待修复）。

## v1.0.0 (2026-11-25)
- 初始发布：HuaweiECBattery 电池驱动 + OpenCore EFI