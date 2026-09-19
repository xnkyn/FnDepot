# AGENTS.md — FnDepot 仓库维护指南

本文件是仓库的维护手册，面向维护者和 AI 助手。任何对本仓库的修改请先阅读本文件并遵守其中的规范，保证 `fnpack.json` 始终是可被飞牛客户端正确解析的 FnDepot V2 源索引。

- 仓库：`https://github.com/xnkyn/FnDepot`（公开，main 分支，直接推送到 main 即可生效）
- 规范来源：[EWEDLCM/FnDepot](https://github.com/EWEDLCM/FnDepot)（官方示例源 `fnpack.json` 为准）
- 角色约定：`source_info.author` 填**源维护者 xnkyn**（规范定义它与应用开发者无关）；应用开发者写在每个应用的 `maintainer` 字段（当前为 p2pee.com），**不要**把 p2pee.com 写进 `source_info.author`

## 目录结构约定

```
FnDepot/
├── fnpack.json          # 源索引，文件名固定，仓库根目录
├── <appname>/           # 每个应用一个目录，目录名 = appname
│   ├── ICON.PNG         # 全大写！建议 256×256 PNG，< 500KB
│   └── <文件名>.fpk     # 安装包，文件名可任意
└── ...
```

- 目录名：仅小写字母、数字、连字符（如 `mytool`、`another-app`），必须与 FPK 内 manifest 的 `appname` 完全一致，并与 `fnpack.json` 中 `apps` 的键名完全一致（区分大小写）
- 图标必须命名为 `ICON.PNG`（全大写），优先从 FPK 内提取 `ICON_256.PNG` 改名使用

## fnpack.json V2 字段速查

顶层结构：

```json
{
  "schema_version": "2",
  "source_info": { "name": "FnDepot", "author": "xnkyn", "homepage": "...", "description": "..." },
  "apps": { "<appname>": { ... } }
}
```

应用级字段（`apps.<appname>` 下）：

| 字段 | 必填 | 要求 |
| --- | --- | --- |
| `display_name` | 是 | 显示名称 |
| `desc` | 是 | 简介，支持简单 HTML 文本 |
| `platform` | 是 | 数组，取值只能是 `x86`、`arm`、`all`，**禁止** `x86_64` / `arm64` |
| `categories` | 是 | 只能用官方九分类，最多 2 个：影音娱乐、系统工具、编程开发、AI赋能、生活服务、智能智控、教育学习、游戏地带、硬件驱动 |
| `icon_url` | 是 | 图标地址，相对路径写 `./<appname>/ICON.PNG` |
| `run_as` | 是 | `package`（以应用账户运行）或 `root`；看 FPK 内 `config/privilege` 的 `run-as` |
| `install_type` | 是 | `""`（安装到存储空间）或 `"root"`（系统空间），一般留空 |
| `is_docker` | 是 | 布尔值，是否为 Docker 应用 |
| `maintainer` / `maintainer_url` | 建议 | 应用开发者（p2pee.com） |
| `distributor` / `distributor_url` | 建议 | 发布者，与开发者相同可省略 |
| `service_port` | 可选 | 端口号，字符串；无则省略 |
| `releases` | 是 | 版本节点，见下 |

版本级字段（`releases.<version>` 下）：

| 字段 | 必填 | 要求 |
| --- | --- | --- |
| 版本键名 | 是 | 可比较的 SemVer（如 `1.0.0`、`2.0.0-beta.1`），**禁止** `latest` 或日期 |
| `changelog` | 建议 | 更新日志 |
| `updated_at` | 建议 | ISO 8601 带时区，如 `2026-09-19T12:00:00+08:00` |
| `os_min_version` / `os_max_version` | 可选 | fnOS 版本范围 |
| `packages` | 是 | 架构 → 安装包映射 |

安装包字段（`packages.<arch>` 下）：

| 字段 | 必填 | 要求 |
| --- | --- | --- |
| 架构键 | 是 | 只能是 `all`、`x86`、`arm`（其他键会导致整个源校验失败） |
| `download_url` | 是 | 相对路径写 `./<appname>/<文件名>.fpk`；不能跨版本继承 |
| `sha256` | 强烈建议 | 64 位十六进制 |
| `size` | 建议 | 字节数（整数），**禁止** `"8 MB"` 这类字符串 |

全局硬性规则：

- `fnpack.json` 必须是**严格 JSON**：无注释、无尾逗号，UTF-8
- 已发布的「版本号 + 架构」安装包视为**不可变**：文件内容变了必须换新版本号（或新文件名）
- 已发布版本对应的文件不要删除/改名，除非同时移除对应的 release 节点
- 图标单张 < 500KB；若加 `preview_urls` 预览图单张 < 2MB（最多 8 张）

## 添加新应用 SOP

以下命令均在 Git Bash（Windows）下执行，仓库根目录为当前目录。

1. **获取元数据**：解出 FPK 的 manifest（权威来源，以此为准填字段，不要凭文件名猜）

   ```bash
   tar -xzOf mytool/mytool_1.0.0.fpk manifest
   ```

2. **建目录放文件**：目录名 = manifest 里的 `appname`

   ```bash
   mkdir mytool
   cp mytool_1.0.0.fpk mytool/
   # 图标：从 FPK 里提取 256 图标改名
   tar -xzOf mytool/mytool_1.0.0.fpk ICON_256.PNG > mytool/ICON.PNG
   ```

3. **算校验值和大小**

   ```bash
   sha256sum mytool/mytool_1.0.0.fpk
   stat -c '%s' mytool/mytool_1.0.0.fpk
   ```

4. **编辑 fnpack.json**：在 `apps` 中新增键名 = 目录名的条目，模板（按 manifest 实际值替换）：

   ```json
   "mytool": {
     "display_name": "MyTool",
     "desc": "应用简介",
     "platform": ["x86"],
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
           "x86": {
             "download_url": "./mytool/mytool_1.0.0.fpk",
             "sha256": "<64位十六进制>",
             "size": 0
           }
         }
       }
     }
   }
   ```

5. **校验 JSON**（本机没有 jq/python，用 node）

   ```bash
   node -e "JSON.parse(require('fs').readFileSync('fnpack.json','utf8')); console.log('OK')"
   ```

6. **过一遍推送前校验清单**（见下节）

7. **提交推送**

   ```bash
   git add -A
   git commit -m "feat: add mytool 1.0.0"
   git push
   ```

8. **更新 README.md** 的应用列表表格，保持同步

## 更新已有应用版本

1. 新 FPK 放进对应应用目录（保留旧 FPK 或删除均可，但文件名变了就要改 `download_url`）
2. 在该应用的 `releases` 下**新增**版本节点（不动旧节点，除非旧文件已删除），`updated_at` 填新时间
3. 重算 sha256 和 size
4. 同步更新 README 应用列表的版本号，校验 JSON，提交推送（建议提交信息 `feat: bump mytool to 1.1.0`）

## 下架应用

1. 删除 `apps` 中对应键
2. 删除对应应用目录
3. README 应用列表移除该行，校验 JSON，提交推送
4. 客户端在源同步成功后会自动清理该应用缓存；**不要**用把 JSON 改空/改错的方式来"下架"

## 推送前校验清单

- [ ] `node -e "JSON.parse(...)"` 通过（严格 JSON）
- [ ] `apps` 键名 = 目录名 = FPK manifest 的 `appname`（三者完全一致）
- [ ] `platform` 与 `packages` 架构键只用了 `x86` / `arm` / `all`
- [ ] `categories` 只用了官方九分类
- [ ] `sha256`、`size` 与实际文件一致（用 sha256sum / stat 复核）
- [ ] `download_url`、`icon_url` 的相对路径与实际文件位置一致（`./<appname>/...`）
- [ ] `schema_version` 是字符串 `"2"`
- [ ] 已发布版本对应的文件没有被删除或改名
- [ ] README.md 应用列表已同步
