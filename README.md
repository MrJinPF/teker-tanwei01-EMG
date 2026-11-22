# TEKER sEMG 上位机 v1.1.6

TEKER 表面肌电（surface EMG, sEMG）采集与处理上位机，Windows x64 发行版归档。

## 版本信息

| 项目 | 内容 |
|---|---|
| 版本号 | 1.1.6 |
| 构建类型 | SSL 修复版 |
| 构建日期 | 2026-10-29 |
| 目标平台 | Windows 10 / 11 x64 |
| 打包方式 | PyInstaller onedir（`_internal/` 依赖目录） |
| 归档日期 | 2026-11-22 |

## 运行方式

1. 下载本仓库全部文件（或从 Releases 下载原始 zip 解压）；
2. 双击 `TEKER_sEMG.exe` 启动。

无需安装 Python 或任何运行库。**`_internal/` 目录必须与 `TEKER_sEMG.exe` 保持同级**，不可分离或重命名，否则程序无法启动。

## 功能范围

- 无线 sEMG 设备扫描与连接（BLE，基于 `bleak`）
- 多通道肌电信号实时波形显示（`PyQt5`）
- 数字滤波与信号处理（`numpy` / `scipy`）
- 采集数据记录与导出
- 设备固件升级（BFU，AES 加密传输）
- 授权验证（联网校验机器码）

## 目录结构

```
TEKER_sEMG_1.1.6/
├── TEKER_sEMG.exe                  主程序入口
└── _internal/                      运行时依赖（Python 3.10 运行时 + 第三方库 + 自有模块）
    ├── server_cert.pem             服务端 TLS 证书（自签）
    ├── auth_manager.*.pyd          授权验证模块
    ├── bfu_updater.*.pyd           固件升级模块
    ├── bfu_updater_crypto.*.pyd    固件加解密模块
    ├── PyQt5/  numpy/  scipy/  ... 第三方库
    └── *.dll  *.pyd                Python 运行时与扩展
```

关键自有模块位于 `_internal/`：

| 模块 | 作用 |
|---|---|
| `auth_manager.cp310-win_amd64.pyd` | 授权验证、机器码生成、授权对话框 |
| `bfu_updater.cp310-win_amd64.pyd` | 固件升级流程控制 |
| `bfu_updater_crypto.cp310-win_amd64.pyd` | 固件包加解密与校验 |

## 第三方组件与许可

本发行版内含以下开源组件，版权归各自作者，遵循各自许可协议：

| 组件 | 版本 | 许可 |
|---|---|---|
| PyQt5 / Qt5 | 5.15.x | GPL v3（或商业许可） |
| numpy | — | BSD-3-Clause |
| scipy | — | BSD-3-Clause |
| bleak | — | MIT |
| cryptography | 49.0.0 | Apache-2.0 / BSD-3-Clause |
| Pillow | — | HPND |
| certifi | — | MPL-2.0 |
| pyserial | — | BSD-3-Clause |

完整许可文本见各组件 `*.dist-info/LICENSE` 文件。

> **合规提示**：PyQt5 以 GPL v3 分发。若以公开仓库形式分发由 PyQt5 构建的二进制，GPL v3 要求向接收者提供对应源代码。若不便开源源码，应改用 LGPL 的 PySide2/PySide6 重新构建，或取得 Qt 商业许可。

## 说明

- 本仓库为**发行版二进制归档**，不含应用源码；
- 程序启动与授权校验需联网；
- 仓库内容按现状提供，仅供持有授权的用户部署使用。

## 版本历史

- **v1.1.6**（2026-10-29）：修复 TLS/SSL 连接问题，随包附带自签服务端证书。
