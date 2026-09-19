# FnDepot

飞牛 fnOS（飞牛私有云）第三方应用源，遵循 [FnDepot V2 规范](https://github.com/EWEDLCM/FnDepot)。

- **源维护者**：[xnkyn](https://github.com/xnkyn) —— 负责本源的收录、更新与维护
- **应用开发者**：[p2pee.com](https://p2pee.com) —— 收录的应用均由其开发
- **主站**：[Gitee · xnkyn/FnDepot](https://gitee.com/xnkyn/FnDepot)（国内访问快）
- **镜像**：[GitHub · xnkyn/FnDepot](https://github.com/xnkyn/FnDepot)


## 应用列表

| 应用 | 目录（appname） | 版本 | 平台 | 简介 |
| --- | --- | --- | --- | --- |
| [P2Pee文件共享](p2pee/) | `p2pee` | 1.0.0 | x86 | 通过 P2P 网络实现高效文件共享，浏览器直接下载 NAS 共享文件 |
| [P2Pee内网穿透](p2pee-proxy/) | `p2pee-proxy` | 1.0.0 | x86 | 将本地 HTTP(S) 服务安全暴露到公网，无需公网 IP 和端口映射 |

## 在 FnDepot 应用中添加本源

本源通过第三方应用商店客户端 [FnDepot](https://github.com/EWEDLCM/FnDepot)（飞牛论坛 EWEDL 开发）加载。注意：fnOS 官方「应用中心」不支持添加第三方源，第三方源统一在 **FnDepot 应用内**管理和使用。

1. **安装 FnDepot 客户端**（首次使用）：从 FnDepot 官方仓库 / 飞牛论坛发布页下载客户端 `.fpk` 安装包，在 fnOS「应用中心」手动安装
2. 打开 **FnDepot 应用**，进入「应用源 / 源管理」，点击添加源
3. 填入本源地址（二选一）：
   - 主站（Gitee，国内推荐）：`https://gitee.com/xnkyn/FnDepot/raw/main/fnpack.json`
   - 镜像（GitHub）：`https://raw.githubusercontent.com/xnkyn/FnDepot/main/fnpack.json`
4. 保存并刷新，即可在 FnDepot 中浏览、安装、更新本源内的应用

> - FnDepot 客户端需 0.0.7 及以上版本（支持 V2 规范）
> - Gitee raw 直链有约 1~5 分钟缓存，并可能受防盗链影响；若遇到 403 或拉取失败，请改用 GitHub 镜像地址
> - 不想加源、只想装某个应用，见下方「手动安装」

## 手动安装

如果不想添加应用源，或源下载较慢，可以直接在本仓库对应应用目录下载 `.fpk` 文件，然后在 fnOS「应用中心」选择「手动安装」上传安装（注意：这一步才是在官方应用中心操作）。

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

本仓库以 **Gitee 为主站、GitHub 为镜像**：本地 git 已配置双推送 remote，一次 `git push origin main` 同时更新两边（配置方法见 AGENTS.md）。

添加新应用、更新版本、下架应用的完整操作步骤（SOP）、fnpack.json V2 字段速查表、推送前校验清单，见 [AGENTS.md](AGENTS.md)。该文件同时面向人和 AI 维护助手编写，可直接把仓库交给 AI 按指南操作。
