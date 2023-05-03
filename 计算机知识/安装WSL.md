## 什么是 WSL

WSL（Windows Subsystem for Linux）是在 Windows 平台运行的 linux 子系统。可以在不安装 Linux 虚拟机的情况下获得相对完整的Linux 系统体验，对于深度绑定 Windows 却又偶尔需要 Linux 开发环境的用户十分友好。

## WSL对比虚拟机

**优点**：

- 轻量化，最大程度减少了电脑负担并且可以体验接近原生的Linux环境。
- 可以实现 Linux 与 Windows 系统的文件互通。windows 文件挂载在 wsl 的 /mnt 目录下。

**缺点**：

- IP 地址不固定。
- wsl 基于 Windows 主系统，如果 Windows 系统损伤会直接影响 wsl，但是虚拟机会有快照功能。
- 不属于百分百的 Linux 环境。

## WSL 版本介绍

- **WSL1**：初代版本，WSL1 使用翻译层将 linux 系统调用转化成 windows 系统调用，没有使用的 VM，不支持内核程序。更像是一个轻量化Linux模拟器而非虚拟环境。
- **WSL2**：WSL2 使用了一个轻量级的、无需维护的虚拟机，并在这个虚拟机中运行了一个完整的 linux 内核，可以运行比如Docker等程序。WSL 2 使用一个 VHD 虚拟磁盘文件作为 linux 发行版的根目录，其中使用 ext4 文件系统格式，极大提升了 IO 性能。但是 WSL 2 使用了 Hyper-V，由于兼容性原因不能运行  WSL2 和 VMWare 或 VirtualBox（听说 VMware & Virtualbox 的新版本里解决了此问题，我没测试过。），WSL1 不存在此问题。

**建议安装 WSL2 ，体验更接近虚拟机的 Linux 环境**

## 安装和更新WSL内核

**注意**：WSL 在 Windows10 个别较低版本以及 Windows 以下版本无法安装（大多数Windows10 以及所有 Windows11 电脑都满足安装条件）

### 安装WSL

1. 搜索打开Windows中的“启用或关闭 Windows 功能” ，勾选“适用于 Linux 的 Windows 子系统”和“虚拟机平台”两个选项并确定。重新启动电脑，等待 WSL 组件的安装完成。
2. 重启之后搜索并打开“开发者设置”并进入，之后打开“开发人员模式”。

### 更新WSL2内核

1. 下载内核文件：

- 微软官方渠道：https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

2. 双击安装包根据提示进行安装，Finsh 完成安装。

3. 按下快捷键 Windows键（即窗口键）+ X，在弹出的功能窗口中点击 Windows 终端（管理员），使其以管理员身份打开终端。或者以管理员身份运行 Powershell，在打开的终端输入以下命令对WSL进行更新。

```
wsl --update
```

再之后输入以下命令将WSL2设为默认版本。

```
wsl --set-default-version 2
```

## 安装Linux发行版

**注意：**

- Linux 发行版= Linux（Kernel）内核+（Free Software）自由软件+Tools（工具）+可完整安装程序

- 所有的Linux发行版都使用同样的 Linux 内核（ Linux 内核网站：https://www.kernel.org/），只是具有自己的特色风格而已。

1. 打开Microsoft Store（微软商店），搜索Linux，结果中有几种Linux发行版：Ubuntu(22.04、20.04.4、18.04)、Debian、OpenSUSE、Oracle Linux、Kali Linux.

2. 选中自己需要的发行版本（我选择 Ubuntu），点击获取、安装即可。（如果由于网络问题造成获取或者安装的失败，那就关闭微软商店重新进入，点击重试，多试几次会成功的）

## 初始化WSL子系统

安装完成的 WSL 的图标会出现在最近安装的项目栏目，或者去所有应用里自己查找，点击图标会进入的就是终端命令行了，按照提示设置自己的用户名和密码，完成系统的初始。

至此，一切安装完成，可以在 Windows 资源管理器的侧栏中查看 WSL 的文件结构。