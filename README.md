# MDC-NGi

MDC-NGi（Next Generation innovation）是一款可自托管的影片刮削与媒体整理工具。它能识别影片番号，从多个来源获取资料，整理视频、字幕和图片，并生成供 Emby 等媒体库读取的 NFO。

[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/li-peifeng/NGi?label=MDC-NGi)](https://github.com/li-peifeng/NGi/releases)
[![Docker Pulls](https://img.shields.io/docker/pulls/leolitaly/mdc-ngi?color=orange)](https://hub.docker.com/r/leolitaly/mdc-ngi/tags)

[最新应用版本](https://github.com/li-peifeng/NGi/releases/latest) · [中文 Wiki](https://github.com/li-peifeng/NGi/wiki) · [更新日志](CHANGELOG.md) · [版本适用范围](https://github.com/li-peifeng/NGi/wiki/版本适用范围) · [Telegram 发布频道](https://t.me/mdc_ngi)

## Features

- [x] **智能刮削**：自动识别番号和分集，按来源优先级获取资料，支持人脸定位裁剪及日亚高清海报。
- [x] **多种整理模式**：硬链接、复制、移动、软链接、重新整理和原地整理。
- [x] **目录监控**：发现新文件后自动安排刮削，支持性能模式和兼容模式。
- [x] **手动整理**：在 Web 界面浏览和扫描文件、批量创建任务、查看处理结果。
- [x] **图片增强**：可给海报、缩略图和背景图添加 4K／8K 或影片类型水印，并自定义样式和位置。
- [x] **智能翻译**：支持 OpenAI、Google、DeepL 等翻译引擎，也可在番号匹配时采用色花堂中文标题。
- [x] **Web 管理界面**：支持登录认证、深浅主题、安全模式和手机端布局。

## 技术栈

| 组件 | 用途 |
| --- | --- |
| Rust | 后端负责多源采集、并发调度、媒体文件操作和任务状态。 |
| Next.js / React | 提供适配桌面和手机的 Web 管理界面。 |
| SQLite | 默认数据库，随应用运行，适合单机部署。 |
| PostgreSQL | 可选数据库，可使用已有 PostgreSQL 实例保存业务数据。 |

- **并发处理**：可按设备性能和来源访问情况调整刮削线程数。
- **任务持久化**：批次、记录和日志写入数据库，重启后可以继续查阅任务结果。
- **响应式界面**：在桌面和手机上均可管理任务、调整设置和查看日志。
- **Docker 部署**：提供 AMD64 和 ARM64 镜像，通过 Docker Compose 启动。

## 特色功能

| 功能 | 使用场景 |
| --- | --- |
| [多源刮削](https://github.com/li-peifeng/NGi/wiki/数据源与优先级) | 可给标题、演员、海报等字段分别设定来源优先级。自动识别的番号有误时，可在重试时指定番号或来源详情页。 |
| [自定义源插件](https://github.com/li-peifeng/NGi/wiki/源插件使用与规则) | 可导入 JSON 或 JavaScript 规则扩展数据源，在管理页测试搜索和资料获取。 |
| [命名与分类](https://github.com/li-peifeng/NGi/wiki/命名规则) | 用模板将番号、演员、分集和画质写入目录名或文件名。保存前可预览归档路径。 |
| [六种整理模式](https://github.com/li-peifeng/NGi/wiki/整理模式与覆盖策略) | 支持硬链接、软链接、复制、移动、重新整理和原地整理。可手动批量提交，也可为不同监控目录分别配置处理规则。 |
| [高清海报与预告片](https://github.com/li-peifeng/NGi/wiki/图片下载裁剪与水印) | 可用 Tenhow 与日亚查找高清海报，支持人脸定位裁剪、自定义水印及本地 NFO 输出。预告片下载使用对应来源的代理，符合条件的大文件可续传。 |
| [字幕自动补齐](https://github.com/li-peifeng/NGi/wiki/字幕匹配与命名) | 可匹配现有字幕，也可使用 NGi 字幕仓库和迅雷。开启仓库自动补齐后，可为缺字幕的已完成记录补字幕。 |
| [已整理影片维护](https://github.com/li-peifeng/NGi/wiki/重新整理使用指南) | 可从记录重新套用命名规则，或批量原地更新 NFO。只执行「重新翻译」时，仅更新标题、简介译文和原位置的 NFO。 |
| [洗版与 STRM](https://github.com/li-peifeng/NGi/wiki/洗版旧资源管理) | 可通过目录或 Emby 查找旧版，按画质、字幕等条件保留或替换。支持跨媒体库查找、CloudDrive2 路径映射和 STRM 原片联动。 |
| [Emby 与 Magic Tools](https://github.com/li-peifeng/NGi/wiki/Emby与Magic-Tools联动刮削) | 从 Emby 菜单提交影片或整组分集。可开启刮削成功后的合并刷新，集中通知 Emby 更新元数据。 |
| [演员与译名](https://github.com/li-peifeng/NGi/wiki/自定义演员映射与AVDB) | 用演员映射表统一别名或维护自定义显示名，可导出 JSON 或同步到兼容的 AVDB。标题与简介支持按配置翻译。 |

### PRO 功能

| 功能 | 使用场景 |
| --- | --- |
| [媒体管理](https://github.com/li-peifeng/NGi/wiki/PRO媒体管理) | 索引本地及已挂载的影片，按目录或名称筛选。可直接编辑 NFO、海报和字幕，在影片所在目录刮削，或下载缺少的剧照、字幕和预告片。 |
| [Emby 演员管理](https://github.com/li-peifeng/NGi/wiki/PRO演员管理与Emby自动补齐) | 浏览选定 Emby 库的演员、作品和写真。可预览并补齐头像、简介等资料，也可按筛选结果一键补齐无头像演员。 |
| [新入库自动补齐](https://github.com/li-peifeng/NGi/wiki/PRO演员管理与Emby自动补齐#配置-emby-webhook) | 配置 Emby 的「新增媒体」Webhook 后，NGi 会为新入库影片的演员补充缺失资料，可查看回调和队列进度。 |

## 功能介绍

### 🎬 视频刮削整理

| 模式 | 文件处理方式 |
| --- | --- |
| 硬链接 | 在同一文件系统的整理目录创建硬链接，复用视频数据。 |
| 复制 | 复制到整理目录，保留原视频。 |
| 移动 | 把视频移至整理目录，按命名和分类规则归档。 |
| 软链接 | 在整理目录创建指向原视频的链接，播放器需要能访问原视频路径。 |
| 重新整理 | 按当前视频路径查找成功记录，复用已保存的元数据重新命名和整理。 |
| 原地整理 | 保留原视频路径和文件名，在原位置生成刮削资料。 |

**刮削流程**：识别番号和分集 → 从多个来源获取元数据 → 下载并处理图片 → 整理视频及字幕 → 生成 NFO 和其他已启用的附属文件。

**字幕支持**：整理同目录的字幕，按番号、分集匹配本地字幕库，也可启用线上补齐。

### 📁 目录监控

- **性能模式**实时监听文件系统事件，适合本地存储。
- **兼容模式**定时检查文件树，适合 SMB、NFS 或网盘挂载等文件系统事件不稳定的环境。
- 每个目录可独立启停并覆盖整理、命名、下载和数据源等设置。可按扩展名、大小或文件名过滤，移动模式也可按设置清理空目录。[目录监控指南](https://github.com/li-peifeng/NGi/wiki/目录监控)

### 🖼️ 图片处理

- **人脸定位裁剪**辅助选择海报画面，也可按分类使用居中、右侧或不裁剪。
- **日亚和 Tenhow 高清图**用于补充高清海报、缩略图或背景图。
- **水印和独立裁剪工具**支持影片类型、字幕、4K／8K 等标签，可调整样式、大小和位置，也能单独裁剪现有图片。

### 📊 任务管理与记录

- 手动任务和监控刮削都有持久化记录，可按状态筛选，查看各分集进度以及失败步骤。
- 支持批量停止、重试或删除任务记录。自动识别有误时，可指定番号或具体详情页再刮削。
- 详情页提供图集、元数据、各步骤日志及编辑入口，可从候选来源选取或修正资料。[刮削记录与重试](https://github.com/li-peifeng/NGi/wiki/刮削记录与重试)

### 🌐 数据源支持

内置与随应用提供的来源覆盖不同影片类型，还可通过 JSON 或 JavaScript 插件扩展。可以按影片类型设置搜索顺序，并分别调整标题、演员和图片等字段的优先级。部分来源支持配置代理、Cookie 或 FlareSolverr／Trawl，应对网站访问限制。失败任务可从记录页重试。[数据源与优先级](https://github.com/li-peifeng/NGi/wiki/数据源与优先级) · [网络代理与反爬](https://github.com/li-peifeng/NGi/wiki/网络代理与反爬)

## 部署

Docker 镜像为 `leolitaly/mdc-ngi`，支持 Linux AMD64 和 ARM64。可在 [Docker Hub](https://hub.docker.com/r/leolitaly/mdc-ngi/tags) 查看可用版本。以下示例用于**新部署**。已有部署升级时，请复用原 Compose 项目、数据挂载和运行用户。

### Docker Compose

新建 `compose.yaml`，把两个示例宿主目录换成实际路径。此示例使用 `0:0` 在容器内以 root 运行，可减少 NAS 宿主目录用户和组不匹配造成的权限问题。

```yaml
services:
  ngi:
    image: leolitaly/mdc-ngi:latest
    user: "0:0"
    ports:
      - "19218:19218"
    environment:
      TZ: Europe/Rome
    volumes:
      - ./data:/data
      - /volume1/downloads:/downloads
      - /volume1/movies:/movies
    stop_grace_period: 40s
    restart: unless-stopped
    security_opt:
      - no-new-privileges:true
```

在 Compose 文件所在目录运行下列命令。

```sh
mkdir -p data
docker compose up -d
```

打开 `http://<宿主机地址>:19218`，创建登录账号。在 NGi 中使用容器内路径 `/downloads` 和 `/movies`。先提交少量文件验证来源、输出路径和 NFO，再开启目录监控。详见[快速开始](https://github.com/li-peifeng/NGi/wiki/快速开始)和[目录挂载与权限](https://github.com/li-peifeng/NGi/wiki/目录挂载与权限)。

如需以普通用户运行，可将 `user` 改为 `"10001:10001"`（镜像默认值）或 NAS 上有权限的 UID/GID，并为该用户配置数据目录、下载目录和整理目录的访问权限。已有实例切换运行用户前，请先核对现有数据和媒体文件的权限。

### PRO 版设备标识

新激活 PRO 时，NGi 优先读取宿主机硬件 UUID。若「设置 → 许可证」提示无法读取 UUID，将 [compose.license-id.yaml](compose.license-id.yaml) 保存到原 Compose 文件旁，执行下列命令。

```sh
docker compose -f compose.yaml -f compose.license-id.yaml up -d ngi
```

初始化服务会把设备标识保存到 `ngi-license-id` 命名卷。许可证页显示「已检测到挂载的设备标识」后即可激活。重建时沿用同一个 Compose 项目名并保留该卷。后续维护也使用同样的两份 Compose 文件。

首次激活也可手动准备标识。在 Compose 目录没有 `id` 文件时，运行 `openssl rand -hex 16 > ./id` 生成一次，在 `ngi` 的卷挂载中加入 `./id:/ngi-id:ro`，并确保容器用户可读取。已有标识时沿用原值。

已有 PRO 授权的实例应保留原 `/data` 和已绑定的设备标识。若原实例使用 UUID，重建后应继续能读取同一个 UUID。已有 `/run/ngi-host-id` 或 `/ngi-id` 挂载的实例继续沿用原标识。手动挂载和换绑步骤见 [Docker 部署指南](https://github.com/li-peifeng/NGi/wiki/Docker部署#pro-版设备标识)。

## 更新与维护

| 更新方式 | 操作 |
| --- | --- |
| 应用 OTA | 在 NGi 左侧「更新」检查版本、查看摘要并安装。开始前等待活动任务结束。 |
| Docker 基础镜像 | 应用提示需要更新镜像时，沿用原 Compose 文件和项目名执行 `docker compose pull ngi`、`docker compose up -d ngi`。使用固定镜像标签时，先更新 Compose 中的标签。保留原 `/data` 和已绑定的设备标识。 |

应用版本和基础镜像版本分别发布。升级前查看[公开 Release](https://github.com/li-peifeng/NGi/releases)和 [Docker 标签](https://hub.docker.com/r/leolitaly/mdc-ngi/tags)，以[版本适用范围](https://github.com/li-peifeng/NGi/wiki/版本适用范围)和应用内兼容性提示为准。备份及回退步骤见[更新升级与回退](https://github.com/li-peifeng/NGi/wiki/更新升级与回退)与[备份恢复与迁移](https://github.com/li-peifeng/NGi/wiki/备份恢复与迁移)。

## 遇到问题

先查看 [常见问题](https://github.com/li-peifeng/NGi/wiki/常见问题)和[日志与故障排查](https://github.com/li-peifeng/NGi/wiki/日志与故障排查)。提交 [GitHub Issue](https://github.com/li-peifeng/NGi/issues) 时，请附上应用版本、镜像标签、出错步骤及对应日志，并遮盖账号、密钥和私人路径。

## 致谢与支持

感谢原版 MDC-NG 作者。MDC-NGi 基于原版工具的使用流程重构，后端使用 Rust，Web 界面使用 Next.js，支持 SQLite 和 PostgreSQL。

<details>
<summary>支持原作者</summary>

<img src="https://user-images.githubusercontent.com/124132602/222636597-f8d48940-a528-41e8-9362-8d15f7517bf6.png" width="200" alt="原作者赞助二维码" />

</details>

[支持 MDC-NGi 维护者](https://ifdian.net/a/leoavdb)
