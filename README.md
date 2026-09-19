# FnDepot

飞牛 fnOS（飞牛私有云）第三方应用源，遵循 [FnDepot V2 规范](https://github.com/EWEDLCM/FnDepot)。

## 应用列表

| 目录（appname） | 应用 | 版本 | 平台 | 简介 |
| --- | --- | --- | --- | --- |
| `p2pee` | P2Pee文件共享 | 1.0.0 | x86 | 通过 P2P 网络实现高效文件共享，浏览器直接下载 NAS 共享文件 |
| `p2pee-proxy` | P2Pee内网穿透 | 1.0.0 | x86 | 将本地 HTTP(S) 服务安全暴露到公网，无需公网 IP 和端口映射 |

## 在飞牛 NAS 上使用

1. fnOS 桌面打开「应用中心」→「设置」→「应用源」→ 添加
2. 填入本仓库地址：`https://github.com/<你的GitHub用户名>/FnDepot`
3. 刷新后即可搜索并安装（飞牛客户端需 0.0.7 及以上版本，旧版不支持 V2 规范）

## 目录结构

```
FnDepot/
├── fnpack.json                      # 源索引文件（V2 规范，文件名固定，必须严格 JSON）
├── p2pee/                           # 应用目录名 = FPK manifest 中的 appname
│   ├── ICON.PNG                     # 图标（全大写文件名，建议 256×256，<500KB）
│   └── p2pee1.0.0.fpk
└── p2pee-proxy/
    ├── ICON.PNG
    └── p2pee-proxy-1.0.0-amd64.fpk
```

## 如何添加新应用

1. 新建应用目录：仅小写字母、数字、连字符（如 `mytool`、`another-app`），目录名必须与 FPK 内 manifest 的 `appname` 完全一致。
2. 将 FPK 安装包和 `ICON.PNG`（全大写）放入该目录。
3. 在 `fnpack.json` 的 `apps` 对象中新增条目，键名与目录名完全一致，可套用下面的模板：

```json
"mytool": {
  "display_name": "MyTool",
  "desc": "应用简介",
  "platform": ["all"],
  "categories": ["系统工具"],
  "icon_url": "./mytool/ICON.PNG",
  "maintainer": "开发者",
  "maintainer_url": "https://example.com",
  "distributor": "发布者",
  "distributor_url": "https://example.com",
  "run_as": "package",
  "install_type": "",
  "is_docker": false,
  "releases": {
    "1.0.0": {
      "changelog": "首个版本",
      "updated_at": "2026-09-19T12:00:00+08:00",
      "packages": {
        "all": {
          "download_url": "./mytool/mytool_1.0.0.fpk",
          "sha256": "<64位十六进制>",
          "size": 0
        }
      }
    }
  }
}
```

4. 计算校验值和文件大小（Git Bash）：

```bash
sha256sum mytool/mytool_1.0.0.fpk
stat -c '%s' mytool/mytool_1.0.0.fpk
```

5. 校验 JSON 并提交推送：

```bash
jq empty fnpack.json
git add -A
git commit -m "feat: add mytool 1.0.0"
git push
```

### 注意事项

- `platform` 只能是 `x86`、`arm`、`all`，不要写 `x86_64` 或 `arm64`
- `categories` 只能使用官方九分类（最多 2 个）：影音娱乐、系统工具、编程开发、AI赋能、生活服务、智能智控、教育学习、游戏地带、硬件驱动
- `install_type` 留空 `""` 表示安装到存储空间，`"root"` 表示系统空间
- 已发布的「版本号 + 架构」安装包视为不可变：更新内容必须发布新版本号（新文件）
- `fnpack.json` 必须是严格 JSON：无注释、无尾逗号，提交前务必用 `jq empty` 校验
