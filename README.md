# MateBook X Pro 2023 OpenCore EFI（公开参考版）

这是华为 MateBook X Pro 2023（酷睿 i7-1360P，Iris Xe）的 OpenCore 参考配置，基于 [1401](https://github.com/nullmoth/1401) 生成。

这里**不含**任何本机序列号、主板号、UUID 或网卡地址。使用前请用 1401 或 `macserial` 为自己生成一套身份信息，填进对应 `config.plist` 的 `PlatformInfo -> Generic`。不要使用、也不要提交别人的序列号。

## 目录

- `efi-sequoia/`：目标系统为 macOS Sequoia **15.8 或更新的 15.x**
- `efi-tahoe/`：目标系统为 macOS Tahoe **26.7 或更新的 26.x**（更早的 26.x 没有这份配置所需的显卡加速支持）

第一次安装更建议先用 Sequoia。两份配置的 USB 端口映射、触摸板和无线方案相同，但 Tahoe 因系统不再自带 AppleHDA，这份参考 EFI 里也没有 AppleALC，内置扬声器和麦克风可能没有声音。

## 使用前请知道

- 这是参考配置，不是开箱即用的完整安装包。缺少身份信息时，不要直接拿去写安装 U 盘后登录 Apple 账号。
- 显卡：第一次进入系统是基础显示。安装 Sequoia 15.8+ 或 Tahoe 26.7+ 后，再用 1401 的 Mac 应用安装 WhateverXe 完整显卡包，才可能打开加速。作者主要验证过同类的 Raptor Lake-U，这台机器上的 Iris Xe（设备 ID A7A0）属于预期可用、尚未广泛验证。
- 无线：Intel AX211 使用 `itlwm`，进系统后需安装 [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases) 才能连 Wi-Fi。安装器里没有 Wi-Fi，请写完整离线安装包，或使用有线网卡。
- 触控板：I2C 触控板可用；触控屏没有 macOS 驱动，不保证能用。
- 本仓库中的 OpenCore 与各 kext 仍遵循各自上游许可证（如 Acidanthera 等），请勿单独改写其许可。

## 不要做的事

- 不要把含有真实序列号的 EFI 推到公开仓库。
- 不要改动华为电脑的内置硬盘来安装。本配置针对外接硬盘安装场景。
