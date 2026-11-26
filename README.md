# MDC-NGi

MDC-NGi（Next Generation innovation）是一款可自托管的影片刮削与媒体整理工具。它能识别影片番号，从多个来源获取资料，整理视频、字幕和图片，并生成供 Emby 等媒体库读取的 NFO。

[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/li-peifeng/NGi?label=MDC-NGi)](https://github.com/li-peifeng/NGi/releases)
[![Docker Pulls](https://img.shields.io/docker/pulls/leolitaly/mdc-ngi?color=orange)](https://hub.docker.com/r/leolitaly/mdc-ngi/tags)

[最新应用版本](https://github.com/li-peifeng/NGi/releases/latest) · [中文 Wiki](https://github.com/li-peifeng/NGi/wiki) · [更新日志](CHANGELOG.md) · [版本适用范围](https://github.com/li-peifeng/NGi/wiki/版本适用范围) · [Telegram 发布频道](https://t.me/mdc_ngi)

## 功能概览

| 功能 | 可以做什么 |
| --- | --- |
| 影片刮削 | 自动识别番号和分集，按字段选择数据源，获取标题、简介和演员资料，支持翻译与自定义源插件。 |
| 文件整理 | 支持硬链接、软链接、复制、移动、重新整理和原地整理。可创建手动批次，也可监控目录自动处理新文件。 |
| 图片、字幕与 NFO | 下载日亚或 Tenhow 高清图片，裁剪海报、添加水印、匹配字幕并生成 NFO。 |
| 刮削记录 | 查看任务进度、分集状态和日志，按需批量重试、重新整理、重新翻译或原地更新 NFO。 |
| 洗版与网盘 | 按优先级保留旧版或替换资源，支持 Emby 索引查找、CloudDrive2 文件操作和 STRM 原片关联。 |
| Emby 联动 | 通过 [Magic Tools](https://github.com/li-peifeng/NGi/wiki/Emby与Magic-Tools联动刮削) 从 Emby 提交刮削任务，支持整组分集，也可在刮削成功后自动通知 Emby 刷新元数据。 |

PRO 版提供[媒体管理](https://github.com/li-peifeng/NGi/wiki/PRO媒体管理)，可浏览已挂载的影片、编辑 NFO 和图片、原地刮削及下载附属资源。还可在[演员管理](https://github.com/li-peifeng/NGi/wiki/PRO演员管理与Emby自动补齐)查看 Emby 库内演员、作品及写真，手动补齐资料，或配置 Emby Webhook 后自动补齐新入库影片的演员资料。

## 部署

Docker 镜像为 `leolitaly/mdc-ngi`，支持 Linux AMD64 和 ARM64。以下示例用于**新部署**。已有部署升级时，请复用原 Compose 项目、数据挂载和运行用户。

### Docker Compose

新建 `compose.yaml`，把两个示例宿主目录换成实际路径。默认运行用户为 `10001:10001`，需要对 `./data` 和输出目录有写权限，对输入目录有读取和遍历权限。移动视频时还需对输入目录有写权限。

```yaml
services:
  ngi:
    image: leolitaly/mdc-ngi:latest
    user: "10001:10001"
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

在 Compose 文件所在目录运行下列命令。前两条命令用于新建的数据目录。

```sh
mkdir -p data
sudo chown 10001:10001 data
docker compose up -d
```

打开 `http://<宿主机地址>:19218`，创建登录账号。在 NGi 中使用容器内路径 `/downloads` 和 `/movies`。先提交少量文件验证来源、输出路径和 NFO，再开启目录监控。详见[快速开始](https://github.com/li-peifeng/NGi/wiki/快速开始)和[目录挂载与权限](https://github.com/li-peifeng/NGi/wiki/目录挂载与权限)。

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

感谢原版 Movie Data Capture 的作者。MDC-NGi 基于原版工具的使用流程重构，后端使用 Rust，Web 界面使用 Next.js，支持 SQLite 和 PostgreSQL。

<details>
<summary>支持原作者</summary>

<img src="https://user-images.githubusercontent.com/124132602/222636597-f8d48940-a528-41e8-9362-8d15f7517bf6.png" width="200" alt="原作者赞助二维码" />

</details>

[支持 MDC-NGi 维护者](https://ifdian.net/a/leoavdb)
