<div align="center">

# 📺 IPTV Web Admin

### 繁华视界 IPTV 管理后台 · 直播源治理 · 设备授权 · 客户端定制 APK 一键打包

[![Docker Pulls](https://img.shields.io/docker/pulls/656688515/iptvweb?style=for-the-badge&logo=docker&logoColor=white&color=2496ED)](https://hub.docker.com/r/656688515/iptvweb)
[![Image Size](https://img.shields.io/docker/image-size/656688515/iptvweb/latest?style=for-the-badge&logo=docker&logoColor=white&color=2496ED)](https://hub.docker.com/r/656688515/iptvweb)
[![Version](https://img.shields.io/badge/version-v20261011b-2496ED?style=for-the-badge)](CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge&logo=opensourceinitiative&logoColor=white)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-amd64%20%7C%20arm64-0078D6?style=for-the-badge&logo=linux&logoColor=white)](https://hub.docker.com/r/656688515/iptvweb)
[![Made with Go](https://img.shields.io/badge/made%20with-Go%201.26-00ADD8?style=for-the-badge&logo=go&logoColor=white)](https://go.dev/)

**一行 Docker 命令跑起来 · 内置完整的 IPTV 直播源管理后台 · 自带 APK 定制打包工具链**

[English](README_EN.md) · [更新日志](CHANGELOG.md) · [QQ 交流群](https://qm.qq.com/q/770840454)

</div>

---

## 🧭 目录

- [功能总览](#-功能总览)
- [快速开始](#-快速开始)
- [首次登录](#-首次登录)
- [界面预览](#-界面预览)
- [客户端定制打包](#-客户端定制打包)
- [部署进阶](#-部署进阶)
- [运维命令](#-运维命令)
- [常见问题](#-常见问题)
- [技术栈](#-技术栈)
- [参与建设](#-参与建设)

---

## ✨ 功能总览

| 模块 | 能力 |
|------|------|
| 📡 **直播源管理** | 直播源分组、批量导入 m3u、可用性探测、自定义 UA、线路优先级 |
| 📺 **频道管理** | 频道增删改查、分组归类、EPG 节目单挂载、批量操作 |
| 📅 **EPG 节目单** | 电子节目单数据源配置、定时拉取、节目回看展示 |
| 📱 **设备管理** | 设备激活码授权、心跳在线状态、最后上线时间、IP 归属地解析 |
| 🌍 **数据统计** | 设备数/在线数趋势、地区分布 TOP 10（基于 ip2region，省级粒度）、活跃度看板 |
| 🔑 **API 密钥** | 多密钥管理、按通道分发、便于对接第三方 |
| 📢 **公告通知** | 客户端公告发布（含弹窗公告），APK 启动时拉取 |
| 🛠 **客户端定制** | 填包名/App 名/后台地址/上传图标，一键生成定制 APK |
| 💾 **备份恢复** | 后台一键备份数据库，出问题可回滚 |
| 🔐 **安全与日志** | 操作日志、登录鉴权（JWT）、后台改密入口 |

---

## 🚀 快速开始

### 方式一：一行命令

```bash
docker run -d \
  --name iptvweb \
  --restart unless-stopped \
  -p 9627:9627 \
  -v iptvweb_data:/app/data \
  -v iptvweb_uploads:/app/uploads \
  656688515/iptvweb:latest
```

打开浏览器访问 **http://服务器IP:9627** 即可进入后台。

### 方式二：docker compose

```bash
curl -O https://raw.githubusercontent.com/300737/iptvweb-release/main/docker-compose.yml
docker compose up -d
```

> **端口被占用？** 改映射即可，例如 `-p 8080:9627`（宿主机 8080 → 容器 9627）。
>
> **arm64 机器（飞腾/鲲鹏/树莓派/甲骨文 ARM）** 直接跑同一条命令即可，
> 镜像已提供 **amd64 + arm64 原生多架构**，无需 QEMU 转译。

---

## 🔑 首次登录

| 项目 | 值 |
|------|-----|
| 后台地址 | `http://服务器IP:9627` |
| 默认账号 | `admin` |
| 默认口令 | `admin123` |

想在首次初始化就换成自己的账号口令，启动时加两个环境变量即可（**仅首次初始化、库里还没有管理员时生效**）：

```bash
docker run -d \
  --name iptvweb --restart unless-stopped \
  -p 9627:9627 \
  -v iptvweb_data:/app/data \
  -v iptvweb_uploads:/app/uploads \
  -e IPTV_SYSTEM_DEFAULT_ADMIN_USERNAME=myadmin \
  -e IPTV_SYSTEM_DEFAULT_ADMIN_PASSWORD='My$tr0ngP@ss' \
  656688515/iptvweb:latest
```

> ⚠️ 上线后请第一时间在 **右上角头像 → 改密码** 里改掉默认口令。

---

## 📸 界面预览

<div align="center">

**后台仪表盘**

![仪表盘](docs/screenshot-dashboard.png)

**客户端定制打包工作台**

![客户端定制](docs/screenshot-custom.png)

**打包任务列表**

![任务列表](docs/screenshot-tasks.png)

</div>

---

## 📦 客户端定制打包

不用装 Android Studio，不用配 JDK，后台点点鼠标就能出一版带自己 Logo 和后台地址的 APK。

```
┌─────────────┐     ┌──────────────┐     ┌───────────────┐
│  填写参数    │ ──▶ │  apktool 拆包 │ ──▶ │ 修改包名/名称  │
│ 包名/图标等  │     └──────────────┘     └───────┬───────┘
└─────────────┘                                  │
                                                 ▼
┌─────────────┐     ┌──────────────┐     ┌───────────────┐
│  下载 APK    │ ◀── │  apksigner   │ ◀── │  apktool 回编 │
└─────────────┘     │   签名对齐    │     └───────────────┘
                    └──────────────┘
```

- 镜像内置 **JDK 17 + apktool + zipalign + 签名证书 + base.apk 模板**，开箱即用
- 支持自定义：包名、应用名、版本号、后台服务器地址、应用图标
- 打包产物落在 `iptvweb_uploads` 卷里，后台列表直接下载
- **arm64 机器上自动使用原生 zipalign**，不会因为架构不对导致打包失败

---

## 🗂 部署进阶

### 数据卷说明

| 卷名 | 挂载路径 | 用途 |
|------|----------|------|
| `iptvweb_data` | `/app/data` | SQLite 数据库 + 打包模板（base.apk / apktool.jar / keystore） |
| `iptvweb_uploads` | `/app/uploads` | 定制 APK 产物 + 上传图标 + 日志 + 备份 |

### 镜像信息

| 项目 | 说明 |
|------|------|
| 镜像地址 | `656688515/iptvweb:latest` |
| 基础镜像 | `eclipse-temurin:17-jdk-jammy` |
| 支持架构 | `linux/amd64` · `linux/arm64` |
| 暴露端口 | `9627` |
| 健康检查 | `GET /api/health` |
| 持久化 | `iptvweb_data` · `iptvweb_uploads` |

### 反向代理（HTTPS）

用 Nginx / 宝塔面板反代 `127.0.0.1:9627` 即可，WebSocket 不需要。若要把后台挂在子路径下，建议直接用二级域名，避免静态资源路径问题。

---

## 🧰 运维命令

```bash
# 查看日志
docker logs -f iptvweb

# 只看启动时的关键提示
docker logs iptvweb | head -40

# 重启 / 停止 / 启动
docker restart iptvweb
docker stop iptvweb
docker start iptvweb

# 进入容器
docker exec -it iptvweb sh

# 升级到最新版
docker pull 656688515/iptvweb:latest
docker rm -f iptvweb
docker run -d --name iptvweb --restart unless-stopped \
  -p 9627:9627 \
  -v iptvweb_data:/app/data \
  -v iptvweb_uploads:/app/uploads \
  656688515/iptvweb:latest
# 数据卷不动，升级不丢数据

# 备份数据卷
docker run --rm -v iptvweb_data:/data -v $(pwd):/backup alpine \
  tar czf /backup/iptvweb_data_$(date +%Y%m%d).tar.gz -C /data .

# 恢复数据卷
docker run --rm -v iptvweb_data:/data -v $(pwd):/backup alpine \
  tar xzf /backup/iptvweb_data_20261011.tar.gz -C /data
```

> 备份前建议先 `docker stop iptvweb`，恢复完再 `docker start iptvweb`，
> 避免 SQLite 在写入过程中被打包导致文件不完整。

---

## ❓ 常见问题

<details>
<summary><b>Q：打包失败，提示 base.apk 不存在？</b></summary>

容器启动时会自动释放模板。如果手动删过，重启容器即可：

```bash
docker restart iptvweb
```
</details>

<details>
<summary><b>Q：如何修改后台端口？</b></summary>

改映射，容器里的 9627 不用动：

```bash
docker run ... -p 8080:9627 ...
```
</details>

<details>
<summary><b>Q：忘记管理员密码怎么办？</b></summary>

登录页进不去、又不想动数据的情况下，可以去容器里连 SQLite 改 —— 但更省事的做法是 **重置整个后台账号**。
⚠️ 注意：下面这条会清空全部业务数据（设备、频道、EPG、打包任务），务必先备份。

```bash
docker stop iptvweb
docker run --rm -v iptvweb_data:/data -v $(pwd):/backup alpine \
  tar czf /backup/iptvweb_data_backup.tar.gz -C /data .   # 先备份
docker exec iptvweb rm -f /app/data/iptv.db               # 删库（此时容器已停，改用下面的方式）
# 容器停止状态下无法 exec，直接挂载卷删除：
docker run --rm -v iptvweb_data:/data alpine rm -f /data/iptv.db
docker start iptvweb
```

容器重启后会重新初始化，账号口令回到 `admin` / `admin123`。
</details>

<details>
<summary><b>Q：arm64 机器能跑吗？</b></summary>

能。镜像从 `v20261009k` 起原生支持 `linux/amd64` 与 `linux/arm64`，同一条 `docker run` 命令即可，Docker 会自动拉取匹配你机器架构的镜像层。
</details>

<details>
<summary><b>Q：升级镜像会丢数据吗？</b></summary>

不会。数据库和产物都在数据卷里，只要用同一个卷名重新 `docker run`（或 `docker compose up -d`），数据就还在。
真正会丢数据的情况只有一种：你手动 `docker volume rm` 了卷。
</details>

---

## 🛠 技术栈

<div align="center">

![Go](https://img.shields.io/badge/Go-1.26-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![Gin](https://img.shields.io/badge/Gin-v1.12-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![GORM](https://img.shields.io/badge/GORM-v1.31-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-Pure%20Go%20%2F%20no%20CGO-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-v5-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)
![ip2region](https://img.shields.io/badge/ip2region-v3.18-4B8BBE?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-Multi--arch-2496ED?style=for-the-badge&logo=docker&logoColor=white)

</div>

后端纯 Go 实现（SQLite 走 `glebarez/sqlite`，无需 CGO），单个二进制 + 静态资源即可运行；
Docker 镜像额外打包了 JDK 17 与 apktool 工具链，用来支撑客户端 APK 定制。

---

## 📝 近期更新

| 版本 | 要点 |
|------|------|
| `v20261011b` | 种子数据幂等，清空管理员账号后重启不再崩溃；系统配置不再被覆盖 |
| `v20261011a` | 默认管理员恢复固定口令 `admin` / `admin123`（去掉随机口令加固），支持环境变量自定义初始账号 |
| `v20261009q` | 地区分布排除内网设备，统计粒度维持省级 |
| `v20261009p` | 接入 ip2region 归属地库，地区分布有真实数据 |
| `v20261009o` | 数据统计页空态兜底 + 记录设备 IP |
| `v20261009m` | 修复操作日志中间件从未挂载，系统日志终于有内容 |
| `v20261009k` | Docker 镜像支持 amd64 + arm64 原生多架构 |
| `v20261009b` | 客户端定制 APK 一键打包 |

完整历史见 [CHANGELOG.md](CHANGELOG.md)。

---

## 🤝 参与建设

欢迎提 Issue 和 PR。提交前麻烦注意：

- Bug 请附上版本号、docker 镜像 digest、以及 `docker logs iptvweb` 的关键片段
- 需求请说清楚使用场景，IPTV 场景碎片化，越具体越容易落地
- PR 请保持代码风格一致（`gofmt`），并在描述里写清楚动机

源码仓库为私有仓库（插件和教育敏感配置未开源），Docker 镜像与本文档仓库是公开交付物。
如果你希望获取源码协作权限，请通过下方 QQ 群联系作者。

---

## 📄 开源协议

本项目基于 **[MIT License](LICENSE)** 开源，可自由使用、修改与二次分发。

---

<div align="center">

## 💬 加入交流

**繁华视界 · QQ 交流群：770840454**

[![加入 QQ 群](https://img.shields.io/badge/%E5%8A%A0%E5%85%A5QQ%E7%BE%A4-770840454-12B7F5?style=for-the-badge&logo=tencentqq&logoColor=white)](https://qm.qq.com/q/770840454)

有问题？想要新功能？进群交流，第一时间获取更新通知和技术支持。

---

**如果觉得有用，给个 ⭐ Star 支持一下吧！**

[![GitHub Stars](https://img.shields.io/github/stars/300737/iptvweb-release?style=social)](https://github.com/300737/iptvweb-release)
[![Docker Stars](https://img.shields.io/docker/stars/656688515/iptvweb?style=social)](https://hub.docker.com/r/656688515/iptvweb)

</div>
