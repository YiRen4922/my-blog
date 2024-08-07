---
title: windows11安装wsl2
date: 2024-08-07 22:47:41
tags:
---

在 Windows 11 上安装 WSL2（Windows Subsystem for Linux 2）可以通过以下步骤完成：

### 步骤1：启用 WSL 和虚拟机平台

1. **打开 PowerShell 以管理员身份运行**：

   - 右键单击开始按钮，选择“Windows PowerShell（管理员）”。

2. **启用 WSL 功能**：

   ```shell
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   ```

3. **启用虚拟机平台功能**：

   ```shell
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```

4. **重启计算机**：执行完上述命令后，重启计算机以使更改生效。

### 步骤2：安装 WSL2 内核更新包

1. 下载 WSL2 内核更新包

   ：

   - 访问 WSL2 内核更新包 下载页面，下载并安装该包。

### 步骤3：将 WSL 设置为 WSL2

1. **打开 PowerShell 以管理员身份运行**：

   - 再次以管理员身份运行 PowerShell。

2. **设置 WSL2 为默认版本**：

   ```shell
   wsl --set-default-version 2
   ```

### 步骤4：安装 Linux 发行版

1. **打开 Microsoft Store**：
   - 在开始菜单中搜索并打开 Microsoft Store。
2. **搜索 Linux 发行版**：
   - 搜索你喜欢的 Linux 发行版，例如 Ubuntu。
3. **安装 Linux 发行版**：
   - 选择你想要的发行版并点击“获取”进行安装。
4. **启动 Linux 发行版**：
   - 安装完成后，点击“启动”或在开始菜单中找到并启动该发行版。首次启动时会提示进行一些初始设置（如创建用户和密码）。

### 步骤5：验证 WSL 版本

1. **检查安装的 Linux 发行版版本**：

   - 打开 PowerShell，运行以下命令检查你安装的 Linux 发行版是否使用 WSL2：

     ```shell
     wsl -l -v
     ```

2. **将特定的 Linux 发行版设置为 WSL2**（如果默认版本没有生效）：

   ```shell
   wsl --set-version <Distro> 2
   ```

   替换 `<Distro>` 为你的 Linux 发行版名称，例如 `Ubuntu-22.04`。

### 完成

现在，你已经在 Windows 11 上成功安装并配置了 WSL2，并且可以在 Windows 上运行 Linux 命令行和工具。
