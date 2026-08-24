# Rootfs 构建

自动化构建 RISC-V64 架构 rootfs 的流水线和脚本。

## 支持的发行版

| 发行版 | 架构 | 配置文件 |
|--------|------|----------|
| openEuler 24.03 LTS SP3 | riscv64 (RVA20) | `openEuler-24.03-LTS-SP3` (默认) |
| openEuler 24.03 LTS SP2 | riscv64 (RVA20) | `openEuler-24.03-LTS-SP2` |
| openRuyi | riscv64 (RVA23) | `openruyi` |

## 功能

- ✅ 根据 `base.list` 安装指定软件包
- ✅ 生成 `{distro}-rootfs.img.zst`（zstd 压缩的 ext4 镜像）
- ✅ 生成 `{distro}-rootfs.tar.gz`（tar.gz 压缩包）
- ✅ 支持 Docker 构建和本地直接构建
- ✅ GitHub Actions 自动构建

**注意**：本 rootfs 不包含内核，需要单独准备内核镜像和 initrd。

## 目录结构

```
RAVA_ROOTFS/
├── Dockerfile                    # openEuler Docker 镜像
├── Dockerfile.openruyi           # openRuyi Docker 镜像
├── build-rootfs.sh               # rootfs 构建核心脚本
├── config.sh                     # 发行版配置
├── local-build.sh                # Docker 本地构建入口
├── base.list                     # 软件包列表
├── quickstart.sh                 # 快速开始指南
├── .github/workflows/
│   └── build-rootfs.yml          # GitHub Actions 流水线
├── README.md                     # 本文档
└── output/                       # 构建产物（构建后生成）
```

## 快速开始

### 方式 1：Docker 构建（推荐）

```bash
# 构建 openEuler-24.03-LTS-SP3 (默认)
chmod +x local-build.sh
./local-build.sh

# 构建 openEuler-24.03-LTS-SP2
./local-build.sh openEuler-24.03-LTS-SP2

# 构建 openRuyi
./local-build.sh openruyi
```

### 方式 2：本地直接构建

```bash
# openEuler-24.03-LTS-SP3
sudo bash build-rootfs.sh openEuler-24.03-LTS-SP3

# openEuler-24.03-LTS-SP2
sudo bash build-rootfs.sh openEuler-24.03-LTS-SP2

# openRuyi
sudo bash build-rootfs.sh openruyi
```

### 方式 3：GitHub Actions

推送到 `main`/`master` 分支自动触发，或在 Actions 页面手动触发。

## 构建产物

### Latest 下载

稳定下载入口（指向**人工发布**的最新构建，**不会**自动更新）：

- Release 列表：<https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest>
- `/releases/latest` 会重定向到 `latest` tag：<https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/latest>

⚠️ **注意**：主 workflow 只发布带时间戳的 release，**不会**自动写入 `latest` tag。
要在某次构建后把 `latest` 指过去，需要手动运行 [Refresh Latest Release](../../actions/workflows/refresh-latest.yml) workflow。

直接下载指定文件：

```
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP3-RVA20-rootfs.img.zst
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP3-RVA20-rootfs.tar.gz
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP3-RVA20-rootfs-all.tar.gz

https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP3-RVA20-selinux-rootfs.img.zst
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP3-RVA20-selinux-rootfs.tar.gz
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP3-RVA20-selinux-rootfs-all.tar.gz

https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP3-RVA23-rootfs.img.zst
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP3-RVA23-rootfs.tar.gz
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP3-RVA23-rootfs-all.tar.gz

https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP3-RVA23-selinux-rootfs.img.zst
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP3-RVA23-selinux-rootfs.tar.gz
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP3-RVA23-selinux-rootfs-all.tar.gz

https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP2-rootfs.img.zst
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP2-rootfs.tar.gz
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP2-rootfs-all.tar.gz

https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP2-selinux-rootfs.img.zst
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP2-selinux-rootfs.tar.gz
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openEuler-24.03-LTS-SP2-selinux-rootfs-all.tar.gz

https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openruyi-rva23-rootfs.img.zst
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openruyi-rva23-rootfs.tar.gz
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openruyi-rva23-rootfs-all.tar.gz

https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openruyi-rva20-rootfs.img.zst
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openruyi-rva20-rootfs.tar.gz
https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest/download/openruyi-rva20-rootfs-all.tar.gz
```

### 发布 latest

`latest` tag 由人工控制：每次主构建完成后**不会**自动更新。需要从以下三种方式中选一个：

#### 方式 1：推送 `latest` tag（推荐，最轻量）

```bash
# 把当前 HEAD 标记为 latest 并推送
git tag -f latest
git push origin latest --force
```

推送 `latest` tag 不会重新构建 rootfs，只把最近一个 `vYYYYMMDD_HHMMSS` 时间戳 release 发布为 `latest`。

#### 方式 2：手动 Run workflow

1. 进入 Actions 页面，选择 [Refresh Latest Release](../../actions/workflows/refresh-latest.yml) workflow
2. 点 "Run workflow"
3. 默认会把最近一个 `vYYYYMMDD_HHMMSS` 时间戳 release 发布到 `latest`
4. 如需指定其他版本，在 `source_tag` 字段填入具体 tag

#### 方式 3：Repository Dispatch（API 触发）

```bash
curl -X POST \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer ${GITHUB_TOKEN}" \
  https://api.github.com/repos/OERV-RVCI/RAVA_ROOTFS/dispatches \
  -d '{"event_type": "refresh-latest"}'
```

#### 发布完成后

下面的链接才会生效：

- <https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/tag/latest>
- <https://github.com/OERV-RVCI/RAVA_ROOTFS/releases/latest/download/...>

### 历史 release

| 发行版 | 镜像文件 | 压缩包 |
|--------|----------|--------|
| openEuler-24.03-LTS-SP3 | `openEuler-24.03-LTS-SP3-rootfs.img.zst` | `openEuler-24.03-LTS-SP3-rootfs.tar.gz` |
| openEuler-24.03-LTS-SP2 | `openEuler-24.03-LTS-SP2-rootfs.img.zst` | `openEuler-24.03-LTS-SP2-rootfs.tar.gz` |
| openRuyi | `openruyi-rootfs.img.zst` | `openruyi-rootfs.tar.gz` |

## 使用 rootfs

### QEMU 启动

```bash
# 解压镜像
zstd -d output/openEuler-24.03-LTS-SP3-rootfs.img.zst -o output/openEuler-24.03-LTS-SP3-rootfs.img
zstd -d output/openEuler-24.03-LTS-SP2-rootfs.img.zst -o output/openEuler-24.03-LTS-SP2-rootfs.img
zstd -d output/openruyi-rootfs.img.zst -o output/openruyi-rootfs.img

# 启动 openEuler-24.03-LTS-SP3 (需要内核 Image)
qemu-system-riscv64 \
    -machine virt -cpu rv64 -nographic \
    -net nic,model=virtio,macaddr=52:54:00:12:34:58 \
    -net user \
    -m 512 \
    -smp 8 -m 8G \
    -device virtio-blk-device,drive=hd0,if=none \
    -drive file=openEuler-24.03-LTS-SP3-rootfs.img,format=raw,id=hd0,if=none \
    -append "root=/dev/vda rw console=ttyS0 selinux=0" \
    -device virtio-net-device,netdev=usernet \
    -netdev user,id=usernet,hostfwd=tcp::16615-:22 \
    -kernel openEuler-24.03-LTS-SP3/Image

# 启动 openEuler-24.03-LTS-SP2 (需要内核 Image)
qemu-system-riscv64 \
    -machine virt -cpu rv64 -nographic \
    -net nic,model=virtio,macaddr=52:54:00:12:34:57 \
    -net user \
    -m 512 \
    -smp 8 -m 8G \
    -device virtio-blk-device,drive=hd0,if=none \
    -drive file=openEuler-24.03-LTS-SP2-rootfs.img,format=raw,id=hd0,if=none \
    -append "root=/dev/vda rw console=ttyS0 selinux=0" \
    -device virtio-net-device,netdev=usernet \
    -netdev user,id=usernet,hostfwd=tcp::16614-:22 \
    -kernel openEuler-24.03-LTS-SP2/Image

# 启动 openRuyi (需要内核 Image)
qemu-system-riscv64 \
    -machine virt -cpu rv64 -nographic \
    -net nic,model=virtio,macaddr=52:54:00:12:34:59 \
    -net user \
    -m 512 \
    -smp 8 -m 8G -cpu rva23s64 \
    -device virtio-blk-device,drive=hd0,if=none \
    -drive file=openruyi-rootfs.img,format=raw,id=hd0,if=none \
    -append "root=/dev/vda rw console=ttyS0 selinux=0" \
    -device virtio-net-device,netdev=usernet \
    -netdev user,id=usernet,hostfwd=tcp::16616-:22 \
    -kernel openruyi/Image
```

### 真机启动

```bash
zstd -d output/openEuler-24.03-LTS-SP3-rootfs.img.zst -o output/openEuler-24.03-LTS-SP3-rootfs.img
dd if=output/openEuler-24.03-LTS-SP3-rootfs.img of=/dev/sdX bs=1M status=progress
```

## 默认配置

| 项目 | openEuler (RVA20) | openRuyi (RVA23) |
|------|-----------|----------|
| 主机名 | openeuler-rva20 | openruyi-rva23 |
| root 密码 | openEuler12#$ | openEuler12#$ |
| SSH | 允许 root 密码登录 | 允许 root 密码登录 |
| 网络 | DHCP 自动获取 | DHCP 自动获取 |
| 分区 | 单一 root 分区 (/dev/vda) | 单一 root 分区 (/dev/vda) |
| 时间同步 | systemd-timesyncd | systemd-timesyncd |
| 代理 | 已配置 (/etc/profile.d/proxy.sh) | 已配置 |
| 预装文件 | /root/stream.c | /root/stream.c |

## 自定义

### 修改软件包列表

编辑 `base.list`，每行一个包名：

```
NetworkManager
openssh-server
vim-enhanced
```

### 修改发行版配置

编辑 `config.sh`，添加或修改发行版配置：

```bash
mydistro)
    DISTRO_NAME="MyDistro"
    DISTRO_VERSION="1.0"
    ARCH="riscv64"
    PROFILE="myprofile"
    CONTAINER_IMAGE="myrepo/myimage:tag"
    REPO_URL="https://myrepo.example.com/path"
    ...
    ;;
```

## 常见问题

**Q: 构建失败，网络问题？**

A: 替换为国内镜像源，在 `config.sh` 中修改 `REPO_URL`。

**Q: Docker 权限问题？**

A: 确保使用 `--privileged` 标志运行容器。

**Q: 镜像大小过大？**

A: 在 `build-rootfs.sh` 中添加清理步骤：
```bash
rm -rf ${ROOTFS_DIR}/usr/share/locale/*
rm -rf ${ROOTFS_DIR}/usr/share/doc/*
```

**Q: 内核相关？**

A: 本 rootfs 不包含内核，请自行准备内核和 initrd。

## 环境要求

- Docker（方式 1）或 root 权限（方式 2）
- 磁盘空间 > 5GB
- 网络连接（访问软件源）

## License

MIT
