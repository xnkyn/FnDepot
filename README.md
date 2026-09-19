# FnDepot

飞牛 fnOS（飞牛私有云）第三方应用源，收录 [p2pee](https://p2pee.com) 系列应用，遵循 [FnDepot V2 规范](https://github.com/EWEDLCM/FnDepot)。

- **应用官网**：[p2pee.com](https://p2pee.com)（无需安装客户端，打开浏览器即可使用）
- **应用开发者**：[p2pee.com](https://p2pee.com) —— 收录的应用均由其开发
- **源维护者**：[xnkyn](https://github.com/xnkyn) —— 负责本源的收录、更新与维护
- **代码主站**：[Gitee · xnkyn/FnDepot](https://gitee.com/xnkyn/FnDepot)（维护者推送代码处）
- **镜像仓库**：[GitHub · xnkyn/FnDepot](https://github.com/xnkyn/FnDepot)（自动同步自 Gitee）

## 应用列表

| 应用 | 目录（appname） | 版本 | 架构 | 简介 |
| --- | --- | --- | --- | --- |
| [P2Pee文件共享](p2pee/) | `p2pee` | 1.0.0 | x86 | 通过 P2P 网络实现高效文件共享，浏览器直接下载 NAS 共享文件 |
| [P2Pee内网穿透](p2pee-proxy/) | `p2pee-proxy` | 1.0.0 | x86 | 将本地 HTTP(S) 服务安全暴露到公网，无需公网 IP 和端口映射 |

## 在 FnDepot 应用中添加本源

本源通过第三方应用商店客户端 [FnDepot](https://github.com/EWEDLCM/FnDepot)（飞牛论坛 EWEDL 开发）加载。注意：fnOS 官方「应用中心」不支持添加第三方源，第三方源统一在 **FnDepot 应用内**管理和使用。

1. **安装 FnDepot 客户端**（首次使用）：从 FnDepot 官方仓库 / 飞牛论坛发布页下载客户端 `.fpk` 安装包，在 fnOS「应用中心」手动安装
2. 打开 **FnDepot 应用**，进入「应用源 / 源管理」，点击添加源
3. 填入本源地址（二选一，推荐主源）：
   - 主源（GitHub，稳定推荐）：`https://raw.githubusercontent.com/xnkyn/FnDepot/main/fnpack.json`
     （也可填仓库地址 `https://github.com/xnkyn/FnDepot`，客户端会自动识别）
   - 备用源（Gitee，国内直连快）：`https://gitee.com/xnkyn/FnDepot/raw/main/fnpack.json`
4. 保存并刷新，即可在 FnDepot 中浏览、安装、更新本源内的应用

> - FnDepot 客户端需 0.0.7 及以上版本（支持 V2 规范）
> - Gitee 备用直链有约 1~5 分钟缓存，且可能受防盗链影响偶发 403，拉取失败时请改回 GitHub 主源
> - 不想加源、只想装某个应用，见下方「手动安装」

## 手动安装（免加源，直接下载 fpk）

本仓库就是 P2Pee 系列飞牛应用的 fpk 安装包分发处，点击下面的直链即可直接下载安装包：

| 应用 | 版本 | 主源下载（GitHub） | 备用下载（Gitee，国内快） |
| --- | --- | --- | --- |
| P2Pee文件共享 | 1.0.0 | [p2pee-1.0.0-amd64.fpk](https://raw.githubusercontent.com/xnkyn/FnDepot/main/p2pee/p2pee-1.0.0-amd64.fpk) | [p2pee-1.0.0-amd64.fpk](https://gitee.com/xnkyn/FnDepot/raw/main/p2pee/p2pee-1.0.0-amd64.fpk) |
| P2Pee内网穿透 | 1.0.0 | [p2pee-proxy-1.0.0-amd64.fpk](https://raw.githubusercontent.com/xnkyn/FnDepot/main/p2pee-proxy/p2pee-proxy-1.0.0-amd64.fpk) | [p2pee-proxy-1.0.0-amd64.fpk](https://gitee.com/xnkyn/FnDepot/raw/main/p2pee-proxy/p2pee-proxy-1.0.0-amd64.fpk) |

### 安装步骤

1. 点击上表直链，把 `.fpk` 安装包下载到电脑（当前仅提供 **x86_64** 架构包，适用于飞牛 x86 机型）
2. 打开 fnOS 桌面的「应用中心」，进入「手动安装」（入口一般在应用中心的设置 / 更多菜单里）
3. 选择刚下载的 `.fpk` 文件上传，按安装向导确认完成
4. 「P2Pee文件共享」首次安装会提示安装依赖 `nodejs_v22`，按提示确认即可
5. 安装完成后打开应用，填入你的 p2pee Key（前往 [p2pee.com](https://p2pee.com) 注册获取）即可使用

## ARM 架构支持说明

当前仓库收录的两个应用均只提供 **x86（x86_64）** 架构安装包，飞牛 ARM 机型暂时无法安装（ARM 版发布后本仓库会第一时间收录，并同步更新此说明和下载直链）。想第一时间获知 ARM 版动态，请关注 [p2pee.com](https://p2pee.com) 

维护者视角的 ARM 包接入操作步骤见 [AGENTS.md](AGENTS.md) 的「接入 ARM 架构安装包」。

## 关于 p2pee

[p2pee.com](https://p2pee.com) 提供两大核心服务，与本仓库收录的两个应用一一对应：

| 官网服务 | 对应应用 | 一句话介绍 |
| --- | --- | --- |
| 文件共享 | P2Pee文件共享（`p2pee`） | 像云盘一样简单，但数据始终在你自己的 NAS 手里 |
| 内网穿透 | P2Pee内网穿透（`p2pee-proxy`） | 把本地 HTTP(S) 服务安全暴露到公网，无需公网 IP、无需端口映射 |

- 基于 **WebRTC 点对点直连**，数据不经过中转服务器，局域网内满速传输，支持大文件
- **网页版无需安装任何客户端**，打开 [p2pee.com](https://p2pee.com) 即可使用
- 多端支持：Windows / macOS / Linux / NAS，另有 CLI 命令行版和 Docker 版
- 交流反馈：官方 QQ 群 876628967

## 仓库结构

```
FnDepot/
├── fnpack.json                      # 源索引文件（V2 规范，文件名固定，必须严格 JSON）
├── AGENTS.md                        # 维护指南（添加/更新/下架应用、ARM 接入、规范速查、校验清单）
├── .github/workflows/
│   └── sync-from-gitee.yml          # GitHub 镜像自动同步（每小时从 Gitee 拉取）
├── p2pee/                           # 应用目录名 = FPK manifest 中的 appname
│   ├── ICON.PNG                     # 图标（全大写文件名，256×256，<500KB）
│   └── p2pee-1.0.0-amd64.fpk
└── p2pee-proxy/
    ├── ICON.PNG
    └── p2pee-proxy-1.0.0-amd64.fpk
```

## 维护本仓库

- **代码流向**：维护者只推 Gitee 主站（`git push origin main`），GitHub 镜像自动同步（配置方法见 AGENTS.md）
- **用户流向**：添加源 / 下载 fpk 以 GitHub 直链为主（稳定），Gitee 直链为国内备用
- 添加新应用、更新版本、下架应用、接入 ARM 安装包的完整操作步骤（SOP）、fnpack.json V2 字段速查表、推送前校验清单，见 [AGENTS.md](AGENTS.md)。该文件同时面向人和 AI 维护助手编写，可直接把仓库交给 AI 按指南操作。
