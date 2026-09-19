# FnDepot

飞牛 fnOS（飞牛私有云）第三方应用源，遵循 [FnDepot V2 规范](https://github.com/EWEDLCM/FnDepot)。

- **源维护者**：[xnkyn](https://github.com/xnkyn) —— 负责本源的收录、更新与维护
- **应用开发者**：[p2pee.com](https://p2pee.com) —— 收录的应用均由其开发

> 按 V2 规范，`source_info.author` 表示**源维护者**（即本仓库的主人），与应用开发者无关；应用的开发者信息记录在每个应用的 `maintainer` / `distributor` 字段中。

## 应用列表

| 应用 | 目录（appname） | 版本 | 平台 | 简介 |
| --- | --- | --- | --- | --- |
| [P2Pee文件共享](p2pee/) | `p2pee` | 1.0.0 | x86 | 通过 P2P 网络实现高效文件共享，浏览器直接下载 NAS 共享文件 |
| [P2Pee内网穿透](p2pee-proxy/) | `p2pee-proxy` | 1.0.0 | x86 | 将本地 HTTP(S) 服务安全暴露到公网，无需公网 IP 和端口映射 |

## 在飞牛 NAS 上添加本源

1. 打开 fnOS 桌面的「应用中心」
2. 进入「设置」→「应用源」，点击添加
3. 填入本仓库地址（二选一）：
   - GitHub 仓库模式：`https://github.com/xnkyn/FnDepot`
   - JSON 直链模式：`https://raw.githubusercontent.com/xnkyn/FnDepot/main/fnpack.json`
4. 保存后刷新应用中心，即可搜索、安装、更新本源内的应用

> 前提：飞牛客户端版本 ≥ 0.0.7，旧版本客户端不支持 V2 规范。

## 手动安装

如果应用源下载较慢，可以直接在本仓库对应应用目录下载 `.fpk` 文件，然后在飞牛应用中心选择「手动安装」上传安装。

## 仓库结构

```
FnDepot/
├── fnpack.json                      # 源索引文件（V2 规范，文件名固定，必须严格 JSON）
├── AGENTS.md                        # 维护指南（添加/更新/下架应用、规范速查、校验清单）
├── p2pee/                           # 应用目录名 = FPK manifest 中的 appname
│   ├── ICON.PNG                     # 图标（全大写文件名，256×256，<500KB）
│   └── p2pee1.0.0.fpk
└── p2pee-proxy/
    ├── ICON.PNG
    └── p2pee-proxy-1.0.0-amd64.fpk
```

## 维护本仓库

添加新应用、更新版本、下架应用的完整操作步骤（SOP）、fnpack.json V2 字段速查表、推送前校验清单，见 [AGENTS.md](AGENTS.md)。该文件同时面向人和 AI 维护助手编写，可直接把仓库交给 AI 按指南操作。
