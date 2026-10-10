<div align="center">

# 📺 IPTV Web Admin

### 繁华视界 IPTV 管理后台 · 客户端定制 APK 一键打包

[![Docker Pulls](https://img.shields.io/docker/pulls/656688515/iptvweb?style=for-the-badge&logo=docker&logoColor=white&color=2496ED)](https://hub.docker.com/r/656688515/iptvweb)
[![Image Size](https://img.shields.io/docker/image-size/656688515/iptvweb/latest?style=for-the-badge&logo=docker&logoColor=white&color=2496ED)](https://hub.docker.com/r/656688515/iptvweb)
[![Version](https://img.shields.io/docker/v/656688515/iptvweb?style=for-the-badge&logo=docker&logoColor=white&label=version&color=2496ED)](https://hub.docker.com/r/656688515/iptvweb)
[![License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge&logo=opensourceinitiative&logoColor=white)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-linux%2Famd64-0078D6?style=for-the-badge&logo=linux&logoColor=white)](https://hub.docker.com/r/656688515/iptvweb)
[![Made with Go](https://img.shields.io/badge/made%20with-Go-00ADD8?style=for-the-badge&logo=go&logoColor=white)](https://go.dev/)

**开箱即用 · 零依赖 · 一键打包定制 APK**

[![QQ 群](https://img.shields.io/badge/QQ%E7%BE%A4-770840454-12B7F5?style=for-the-badge&logo=tencentqq&logoColor=white)](https://qm.qq.com/q/770840454)

---

</div>

## ✨ 功能特性

<div align="center">

| 功能 | 说明 |
|------|------|
| 📱 **客户端定制** | 填写包名/App名/后台地址/上传图标，一键生成定制 APK |
| 🔧 **自动打包链路** | apktool 拆包 → 改 manifest → 替换图标 → 回编译 → jarsigner 签名 |
| 📦 **开箱即用** | 镜像内置 JDK 17 + apktool + base.apk 模板 + 签名证书 |
| 💾 **数据持久化** | 数据库、模板、产物全部 volume 挂载，删容器不丢数据 |
| 🌐 **Web 管理后台** | 完整的 IPTV 管理后台，直播源/设备/授权/EPG 一应俱全 |

</div>

## 🚀 快速开始

### 一行命令启动

```bash
docker run -d \
  --name iptvweb \
  --restart unless-stopped \
  -p 9627:9627 \
  -v iptvweb_data:/app/data \
  -v iptvweb_uploads:/app/uploads \
  656688515/iptvweb:latest
```

打开浏览器访问 **http://服务器IP:9627**，默认账号 `admin` / `admin123`。

### docker-compose 部署

```bash
curl -O https://raw.githubusercontent.com/300737/iptvweb-release/main/docker-compose.yml
docker compose up -d
```

## 📸 界面预览

<div align="center">

> 后台仪表盘

![仪表盘](docs/screenshot-dashboard.png)

> 客户端定制打包工作台

![客户端定制](docs/screenshot-custom.png)

> 打包任务列表

![任务列表](docs/screenshot-tasks.png)

</div>

## 🎬 视频演示

<div align="center">

[![观看演示视频](https://img.shields.io/badge/%F0%9F%8E%AC%20%E6%BC%94%E7%A4%BA%E8%A7%86%E9%A2%91-Click%20to%20Play-red?style=for-the-badge&logo=youtube&logoColor=white)](docs/demo.mp4)

</div>

## 📦 镜像信息

| 项目 | 说明 |
|------|------|
| 镜像地址 | `656688515/iptvweb:latest` |
| 基础镜像 | eclipse-temurin:17-jdk-alpine |
| 镜像大小 | ~520MB |
| 暴露端口 | `9627` |
| 数据卷 | `iptvweb_data` · `iptvweb_uploads` |
| 架构 | linux/amd64 |

## 🛠️ 技术栈

<div align="center">

![Go](https://img.shields.io/badge/Go-1.24-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![Gin](https://img.shields.io/badge/Gin-Web%20Framework-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![GORM](https://img.shields.io/badge/GORM-ORM-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-Database-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Container-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Alpine](https://img.shields.io/badge/Alpine-Linux-0D597F?style=for-the-badge&logo=alpinelinux&logoColor=white)

</div>

## 📂 数据卷说明

| 卷名 | 挂载路径 | 用途 |
|------|----------|------|
| `iptvweb_data` | `/app/data` | SQLite 数据库 + 打包模板（base.apk / apktool.jar / keystore） |
| `iptvweb_uploads` | `/app/uploads` | 定制 APK 产物 + 上传图标 + 日志 + 备份 |

## 📋 客户端定制打包流程

```
┌─────────────┐     ┌──────────────┐     ┌───────────────┐
│  填写参数    │ ──▶ │  apktool 拆包 │ ──▶ │ 修改包名/名称  │
│ 包名/图标等  │     └──────────────┘     └───────┬───────┘
└─────────────┘                                  │
                                                 ▼
┌─────────────┐     ┌──────────────┐     ┌───────────────┐
│  下载 APK    │ ◀── │ jarsigner 签名│ ◀── │  apktool 回编 │
└─────────────┘     └──────────────┘     └───────────────┘
```

## 📊 常用命令

```bash
# 查看日志
docker logs -f iptvweb

# 重启服务
docker restart iptvweb

# 进入容器
docker exec -it iptvweb sh

# 备份数据
docker run --rm -v iptvweb_data:/data -v $(pwd):/backup alpine tar czf /backup/iptvweb_data.tar.gz -C /data .

# 更新镜像
docker pull 656688515/iptvweb:latest
docker compose up -d
```

## ❓ 常见问题

<details>
<summary><b>Q: 打包失败提示 base.apk 不存在？</b></summary>

容器启动时会自动释放模板。如果手动删除过，请重启容器：
```bash
docker restart iptvweb
```
</details>

<details>
<summary><b>Q: 如何修改后台端口？</b></summary>

修改 docker-compose.yml 的端口映射：
```yaml
ports:
  - "8080:9627"   # 宿主机8080 → 容器9627
```
</details>

<details>
<summary><b>Q: 忘记管理员密码？</b></summary>

删除数据库后重启容器会重新初始化默认账号：
```bash
docker exec iptvweb rm -f /app/data/iptv.db
docker restart iptvweb
```
默认账号：`admin` / `admin123`
</details>

## 📝 更新日志

<details>
<summary><b>v20261009f</b></summary>

- ✨ 客户端定制 APK 一键打包功能
- 🔧 内嵌 base.apk / apktool.jar / 签名证书
- 🐳 Docker 镜像发布到 Docker Hub
- 🔒 数据持久化支持
</details>

## 📄 开源协议

[MIT License](LICENSE)

---

## 💬 加入交流

<div align="center">

**繁华视界 · QQ 交流群：770840454**

[![加入 QQ 群](https://img.shields.io/badge/%E5%8A%A0%E5%85%A5QQ%E7%BE%A4-770840454-12B7F5?style=for-the-badge&logo=tencentqq&logoColor=white)](https://qm.qq.com/q/770840454)

> 有问题？想要新功能？进群交流，第一时间获取更新通知和技术支持。

</div>

---

<div align="center">

**如果觉得有用，给个 ⭐ Star 支持一下吧！**

[![GitHub Stars](https://img.shields.io/github/stars/300737/iptvweb-release?style=social)](https://github.com/300737/iptvweb-release)
[![Docker Stars](https://img.shields.io/docker/stars/656688515/iptvweb?style=social)](https://hub.docker.com/r/656688515/iptvweb)

</div>
