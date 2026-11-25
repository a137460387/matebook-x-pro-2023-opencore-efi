# Changelog

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